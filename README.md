-- سكربت النقاط 
local ScreenGui = Instance.new("ScreenGui")
local MainFrame = Instance.new("Frame")
local UIGradient = Instance.new("UIGradient")
local Title = Instance.new("TextLabel")
local CloseButton = Instance.new("TextButton")
local InputBox = Instance.new("TextBox")
local ButtonsHolder = Instance.new("Frame")

-- Sound Effects
local function CreateSound()
    local sound = Instance.new("Sound")
    sound.Parent = game:GetService("SoundService")
    sound.SoundId = "rbxassetid://6895079853" -- صوت نقرة خفيف
    sound.Volume = 0.5
    return sound
end

local clickSound = CreateSound()

-- تعيين الخصائص الأساسية
ScreenGui.Parent = game:GetService("CoreGui")
ScreenGui.Name = "PointsEditor"

MainFrame.Name = "MainFrame"
MainFrame.Parent = ScreenGui
MainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
MainFrame.Position = UDim2.new(0.5, -150, 0.5, -125)
MainFrame.Size = UDim2.new(0, 300, 0, 250)
MainFrame.BorderSizePixel = 0
MainFrame.ClipsDescendants = true

-- إضافة التدرج اللوني
UIGradient.Color = ColorSequence.new{
    ColorSequenceKeypoint.new(0, Color3.fromRGB(40, 40, 40)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(25, 25, 25))
}
UIGradient.Parent = MainFrame

-- تزيين الزوايا
local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(0, 10)
UICorner.Parent = MainFrame

-- العنوان
Title.Name = "Title"
Title.Parent = MainFrame
Title.BackgroundTransparency = 1
Title.Position = UDim2.new(0, 10, 0, 10)
Title.Size = UDim2.new(1, -50, 0, 30)
Title.Font = Enum.Font.GothamBold
Title.Text = "💎 تعديل النقاط"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.TextSize = 20
Title.TextXAlignment = Enum.TextXAlignment.Left

-- زر الإغلاق
CloseButton.Name = "CloseButton"
CloseButton.Parent = MainFrame
CloseButton.BackgroundTransparency = 1
CloseButton.Position = UDim2.new(1, -40, 0, 10)
CloseButton.Size = UDim2.new(0, 30, 0, 30)
CloseButton.Font = Enum.Font.GothamBold
CloseButton.Text = "×"
CloseButton.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseButton.TextSize = 24

-- مربع إدخال الأرقام
InputBox.Name = "InputBox"
InputBox.Parent = MainFrame
InputBox.Position = UDim2.new(0.1, 0, 0.2, 0)
InputBox.Size = UDim2.new(0.8, 0, 0, 40)
InputBox.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
InputBox.BorderSizePixel = 0
InputBox.Font = Enum.Font.GothamSemibold
InputBox.PlaceholderText = "أدخل العدد"
InputBox.Text = ""
InputBox.TextColor3 = Color3.fromRGB(255, 255, 255)
InputBox.TextSize = 16

local InputBoxCorner = Instance.new("UICorner")
InputBoxCorner.CornerRadius = UDim.new(0, 6)
InputBoxCorner.Parent = InputBox

-- دالة إنشاء الأزرار
local function CreateButton(name, position, color)
    local Button = Instance.new("TextButton")
    Button.Name = name
    Button.Parent = MainFrame
    Button.BackgroundColor3 = color
    Button.Position = position
    Button.Size = UDim2.new(0.8, 0, 0, 35)
    Button.Font = Enum.Font.GothamSemibold
    Button.Text = name
    Button.TextColor3 = Color3.fromRGB(255, 255, 255)
    Button.TextSize = 14
    Button.BorderSizePixel = 0
    Button.AnchorPoint = Vector2.new(0.5, 0)
    Button.Position = position

    local ButtonCorner = Instance.new("UICorner")
    ButtonCorner.CornerRadius = UDim.new(0, 6)
    ButtonCorner.Parent = Button

    -- تأثير التحويم
    Button.MouseEnter:Connect(function()
        game:GetService("TweenService"):Create(Button,
            TweenInfo.new(0.3),
            {BackgroundColor3 = color:Lerp(Color3.fromRGB(255, 255, 255), 0.2)}
        ):Play()
    end)

    Button.MouseLeave:Connect(function()
        game:GetService("TweenService"):Create(Button,
            TweenInfo.new(0.3),
            {BackgroundColor3 = color}
        ):Play()
    end)

    return Button
end

-- إنشاء الأزرار
local ApplyAllButton = CreateButton("تطبيق لكل الرمزين النقاط والعدد الأعلى", UDim2.new(0.5, 0, 0.45, 0), Color3.fromRGB(45, 120, 200))
local HighestButton = CreateButton("تغيير العدد الأعلى فقط", UDim2.new(0.5, 0, 0.6, 0), Color3.fromRGB(45, 180, 45))
local PointsButton = CreateButton("تغيير عدد النقاط", UDim2.new(0.5, 0, 0.75, 0), Color3.fromRGB(200, 45, 45))
local ResetButton = CreateButton("إعادة التعيين والإيقاف", UDim2.new(0.5, 0, 0.9, 0), Color3.fromRGB(150, 150, 150))

-- المتغيرات
local updateConnections = {}

-- الوظائف
local function StopAllUpdates()
    for _, connection in pairs(updateConnections) do
        if connection then
            connection:Disconnect()
        end
    end
    updateConnections = {}
end

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
ApplyAllButton.MouseButton1Click:Connect(function()
    clickSound:Play()
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
    clickSound:Play()
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
    clickSound:Play()
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
    clickSound:Play()
    StopAllUpdates()
    -- إعادة القيم الأصلية (يمكن تعديلها حسب احتياجك)
    UpdateStats("Cash", 0)
    UpdateStats("Credit", 0)
end)

CloseButton.MouseButton1Click:Connect(function()
    clickSound:Play()
    StopAllUpdates()
    ScreenGui:Destroy()
end)

-- خاصية السحب
local UserInputService = game:GetService("UserInputService")
local isDragging = false
local dragStart = nil
local startPos = nil

MainFrame.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        isDragging = true
        dragStart = input.Position
        startPos = MainFrame.Position
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement and isDragging then
        local delta = input.Position - dragStart
        MainFrame.Position = UDim2.new(
            startPos.X.Scale,
            startPos.X.Offset + delta.X,
            startPos.Y.Scale,
            startPos.Y.Offset + delta.Y
        )
    end
end)

UserInputService.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        isDragging = false
    end
end)

-- رسالة الترحيب
print("✨ تم تشغيل محرر النقاط")
