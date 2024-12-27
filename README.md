-- تم إنشاء السكربت بواسطة عبدالله او دارك ريبر (الحقوق لي + تحت بالسطر الثالث الديسكورد حقي كدليل)
-- The script was made it by Abdullh or Dark Reaper
-- Discord: .w9k
local ScreenGui = Instance.new("ScreenGui")
local MainFrame = Instance.new("Frame")
local BlurEffect = Instance.new("BlurEffect")
local Title = Instance.new("TextLabel")
local CloseButton = Instance.new("ImageButton")
local InputBox = Instance.new("TextBox")

-- إعداد التأثير الزجاجي
BlurEffect.Size = 10
BlurEffect.Parent = game.Lighting

-- تعيين الخصائص الأساسية
ScreenGui.Parent = game:GetService("CoreGui")
ScreenGui.Name = "ModernPointsEditor"

MainFrame.Name = "MainFrame"
MainFrame.Parent = ScreenGui
MainFrame.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
MainFrame.BackgroundTransparency = 0.85
MainFrame.Position = UDim2.new(0.5, -200, 0.5, -250)
MainFrame.Size = UDim2.new(0, 400, 0, 500)
MainFrame.ClipsDescendants = true

-- إضافة تأثير الخلفية المتحركة
local BackgroundPattern = Instance.new("ImageLabel")
BackgroundPattern.Name = "Pattern"
BackgroundPattern.Parent = MainFrame
BackgroundPattern.BackgroundTransparency = 1
BackgroundPattern.Size = UDim2.new(1.5, 0, 1.5, 0)
BackgroundPattern.Position = UDim2.new(-0.25, 0, -0.25, 0)
BackgroundPattern.Image = "rbxassetid://6073763717"
BackgroundPattern.ImageTransparency = 0.9
BackgroundPattern.ImageColor3 = Color3.fromRGB(126, 140, 255)

-- تأثير حركة الخلفية
spawn(function()
    while wait(0.05) do
        BackgroundPattern.Rotation = (BackgroundPattern.Rotation + 0.1) % 360
    end
end)

-- تزيين الزوايا بشكل أكثر نعومة
local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(0, 20)
UICorner.Parent = MainFrame

-- العنوان بتصميم عصري
Title.Name = "Title"
Title.Parent = MainFrame
Title.BackgroundTransparency = 1
Title.Position = UDim2.new(0, 30, 0, 20)
Title.Size = UDim2.new(1, -60, 0, 40)
Title.Font = Enum.Font.GothamBlack
Title.Text = "✨ محرر النقاط المتطور"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.TextSize = 24
Title.TextStrokeTransparency = 0.8

-- مربع الإدخال بتصميم عصري
InputBox.Name = "InputBox"
InputBox.Parent = MainFrame
InputBox.Position = UDim2.new(0.1, 0, 0.15, 0)
InputBox.Size = UDim2.new(0.8, 0, 0, 50)
InputBox.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
InputBox.BackgroundTransparency = 0.9
InputBox.Font = Enum.Font.GothamSemibold
InputBox.PlaceholderText = "✏️ أدخل القيمة هنا"
InputBox.Text = ""
InputBox.TextColor3 = Color3.fromRGB(255, 255, 255)
InputBox.TextSize = 18

-- دالة إنشاء الأزرار المتطورة
local function CreateModernButton(name, position, gradientColors)
    local Button = Instance.new("TextButton")
    Button.Name = name
    Button.Parent = MainFrame
    Button.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
    Button.BackgroundTransparency = 0.9
    Button.Position = position
    Button.Size = UDim2.new(0.8, 0, 0, 45)
    Button.Font = Enum.Font.GothamBold
    Button.Text = name
    Button.TextColor3 = Color3.fromRGB(255, 255, 255)
    Button.TextSize = 16
    Button.AnchorPoint = Vector2.new(0.5, 0)

    -- إضافة تأثير التوهج
    local UIGradient = Instance.new("UIGradient")
    UIGradient.Color = ColorSequence.new(gradientColors)
    UIGradient.Parent = Button

    -- حركة التدرج
    spawn(function()
        local offset = 0
        while wait() do
            offset = (offset + 0.001) % 1
            UIGradient.Offset = Vector2.new(offset, 0)
        end
    end)

    -- تأثير عند الضغط
    Button.MouseButton1Down:Connect(function()
        game:GetService("TweenService"):Create(Button, TweenInfo.new(0.1), {
            BackgroundTransparency = 0.7,
            TextSize = 14
        }):Play()
    end)

    Button.MouseButton1Up:Connect(function()
        game:GetService("TweenService"):Create(Button, TweenInfo.new(0.1), {
            BackgroundTransparency = 0.9,
            TextSize = 16
        }):Play()
    end)

    local ButtonCorner = Instance.new("UICorner")
    ButtonCorner.CornerRadius = UDim.new(0, 12)
    ButtonCorner.Parent = Button

    return Button
end

-- إنشاء الأزرار بألوان متدرجة جذابة
local MultiVersionButton = CreateModernButton("🌟 النسخة المتعددة", 
    UDim2.new(0.5, 0, 0.3, 0),
    {Color3.fromRGB(147, 112, 219), Color3.fromRGB(180, 130, 255)}
)

local ApplyAllButton = CreateModernButton("💫 تطبيق على الكل", 
    UDim2.new(0.5, 0, 0.43, 0),
    {Color3.fromRGB(45, 120, 200), Color3.fromRGB(83, 160, 255)}
)

local HighestButton = CreateModernButton("⭐ تغيير العدد الأعلى", 
    UDim2.new(0.5, 0, 0.56, 0),
    {Color3.fromRGB(45, 180, 45), Color3.fromRGB(100, 255, 100)}
)

local PointsButton = CreateModernButton("💎 تغيير النقاط", 
    UDim2.new(0.5, 0, 0.69, 0),
    {Color3.fromRGB(200, 45, 45), Color3.fromRGB(255, 100, 100)}
)

local ResetButton = CreateModernButton("🔄 إعادة التعيين", 
    UDim2.new(0.5, 0, 0.82, 0),
    {Color3.fromRGB(150, 150, 150), Color3.fromRGB(200, 200, 200)}
)

-- [باقي الكود للوظائف يبقى كما هو]

-- إضافة تأثير عائم للإطار الرئيسي
spawn(function()
    local originalY = MainFrame.Position.Y.Scale
    while wait() do
        local offset = math.sin(tick()) * 0.005
        MainFrame.Position = UDim2.new(
            MainFrame.Position.X.Scale,
            MainFrame.Position.X.Offset,
            originalY + offset,
            MainFrame.Position.Y.Offset
        )
    end
end)


-- وظائف الأزرار والتحديث
local updateConnections = {}

-- وظيفة إيقاف جميع التحديثات
local function StopAllUpdates()
    for _, connection in pairs(updateConnections) do
        if connection then
            connection:Disconnect()
        end
    end
    updateConnections = {}
end

-- وظيفة تحديث القيم
local function UpdateStats(statName, value)
    local player = game.Players.LocalPlayer
    if player and player:FindFirstChild("leaderstats") then
        local stat = player.leaderstats:FindFirstChild(statName)
        if stat then
            stat.Value = tonumber(value) or stat.Value
        end
    end
end

-- وظائف الأزرار
MultiVersionButton.MouseButton1Click:Connect(function()
    local success, error = pcall(function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/IdkDarkReaper/Files-script/refs/heads/main/.gitignore"))()
    end)
    if not success then
        print("خطأ في تحميل السكربت:", error)
    end
end)

ApplyAllButton.MouseButton1Click:Connect(function()
    StopAllUpdates()
    local value = tonumber(InputBox.Text)
    if value then
        local connection = game:GetService("RunService").Heartbeat:Connect(function()
            UpdateStats("Cash", value)
            UpdateStats("Credit", value)
        end)
        table.insert(updateConnections, connection)
    end
end)

HighestButton.MouseButton1Click:Connect(function()
    StopAllUpdates()
    local value = tonumber(InputBox.Text)
    if value then
        local connection = game:GetService("RunService").Heartbeat:Connect(function()
            UpdateStats("Credit", value)
        end)
        table.insert(updateConnections, connection)
    end
end)

PointsButton.MouseButton1Click:Connect(function()
    StopAllUpdates()
    local value = tonumber(InputBox.Text)
    if value then
        local connection = game:GetService("RunService").Heartbeat:Connect(function()
            UpdateStats("Cash", value)
        end)
        table.insert(updateConnections, connection)
    end
end)

ResetButton.MouseButton1Click:Connect(function()
    StopAllUpdates()
    UpdateStats("Cash", 0)
    UpdateStats("Credit", 0)
    InputBox.Text = ""
end)

-- إضافة زر الإغلاق
local CloseButton = Instance.new("TextButton")
CloseButton.Name = "CloseButton"
CloseButton.Parent = MainFrame
CloseButton.Position = UDim2.new(1, -40, 0, 10)
CloseButton.Size = UDim2.new(0, 30, 0, 30)
CloseButton.BackgroundTransparency = 1
CloseButton.Text = "×"
CloseButton.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseButton.TextSize = 24
CloseButton.Font = Enum.Font.GothamBold

CloseButton.MouseButton1Click:Connect(function()
    StopAllUpdates()
    ScreenGui:Destroy()
    if BlurEffect then
        BlurEffect:Destroy()
    end
end)

-- إضافة تأثيرات صوتية
local clickSound = Instance.new("Sound")
clickSound.Parent = game:GetService("SoundService")
clickSound.SoundId = "rbxassetid://6895079853"
clickSound.Volume = 0.5

local function PlayClickSound()
    clickSound:Play()
end

-- إضافة الصوت لكل الأزرار
MultiVersionButton.MouseButton1Click:Connect(PlayClickSound)
ApplyAllButton.MouseButton1Click:Connect(PlayClickSound)
HighestButton.MouseButton1Click:Connect(PlayClickSound)
PointsButton.MouseButton1Click:Connect(PlayClickSound)
ResetButton.MouseButton1Click:Connect(PlayClickSound)
CloseButton.MouseButton1Click:Connect(PlayClickSound)
