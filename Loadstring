local UserInputService = game:GetService("UserInputService")
local VirtualInputManager = game:GetService("VirtualInputManager")
local LocalPlayer = game.Players.LocalPlayer
local attach = true

-- Функция для создания кнопок
local function createButton(parent, text, position)
    local button = Instance.new("TextButton")
    button.Parent = parent
    button.Size = UDim2.new(0, 250, 0, 40)
    button.Position = position
    button.Text = text
    button.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    button.TextColor3 = Color3.new(1, 1, 1)
    button.Font = Enum.Font.SourceSansBold
    button.TextSize = 18
    button.BorderSizePixel = 0
    button.AutoButtonColor = true

    local UICorner = Instance.new("UICorner")
    UICorner.CornerRadius = UDim.new(0, 10)
    UICorner.Parent = button

    button.MouseButton1Down:Connect(function()
        button.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
    end)

    button.MouseButton1Up:Connect(function()
        button.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    end)

    return button
end

-- GUI
local Gui = Instance.new("ScreenGui")
Gui.Name = "DeathBallGui"
Gui.Parent = LocalPlayer:WaitForChild("PlayerGui")
Gui.ResetOnSpawn = false

local Panel = Instance.new("Frame")
Panel.Parent = Gui
Panel.Size = UDim2.new(0, 300, 0, 240)
Panel.Position = UDim2.new(0.5, -150, 0, 50)
Panel.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
Panel.BackgroundTransparency = 0.5
Panel.Active = true
Panel.Draggable = true

local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(0, 10)
UICorner.Parent = Panel

-- Название
local title = Instance.new("TextLabel")
title.Parent = Panel
title.Size = UDim2.new(0, 300, 0, 40)
title.Position = UDim2.new(0.5, -150, 0, 10)
title.Text = "DeathBall V2"
title.TextSize = 24
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.BackgroundTransparency = 1
title.Font = Enum.Font.SourceSansBold
title.TextStrokeTransparency = 0.6

-- Кнопка Auto
local AutoButton = createButton(Panel, "Auto", UDim2.new(0.5, -125, 0, 60))

-- Кнопка Telegram
local TelegramButton = createButton(Panel, "Telegram: @En1gmaWq", UDim2.new(0.5, -125, 0, 110))

TelegramButton.MouseButton1Click:Connect(function()
    setclipboard("https://t.me/En1gmaWq")
    print("Ссылка на Telegram скопирована!")
end)

-- Подсказка F1 (неактивная кнопка)
local F1Info = Instance.new("TextButton")
F1Info.Parent = Panel
F1Info.Size = UDim2.new(0, 250, 0, 40)
F1Info.Position = UDim2.new(0.5, -125, 0, 160)
F1Info.Text = "Скрыть / показать окно (F1)"
F1Info.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
F1Info.TextColor3 = Color3.fromRGB(200, 200, 200)
F1Info.Font = Enum.Font.SourceSansBold
F1Info.TextSize = 17
F1Info.AutoButtonColor = false
F1Info.BorderSizePixel = 0
F1Info.Active = false

local UICornerF1 = Instance.new("UICorner")
UICornerF1.CornerRadius = UDim.new(0, 10)
UICornerF1.Parent = F1Info

-- Переключение отображения GUI на F1
UserInputService.InputBegan:Connect(function(input)
    if input.KeyCode == Enum.KeyCode.F1 then
        Panel.Visible = not Panel.Visible
    end
end)

-- Логика Auto
local autoMode = false
AutoButton.MouseButton1Click:Connect(function()
    autoMode = not autoMode
    AutoButton.BackgroundColor3 = autoMode and Color3.fromRGB(0, 255, 0) or Color3.fromRGB(255, 0, 0)
    AutoButton.Text = autoMode and "Auto: ON" or "Auto: OFF"
end)

-- Функция нажатия F
local function CLC()
    if attach then
        VirtualInputManager:SendKeyEvent(true, "F", false, game)
        attach = false
        task.delay(0.1, function() attach = true end)
    end
end

-- Автоудар
local RB = Color3.new(1, 0, 0)
local previousBallPosition = nil
local speedThreshold = 50
local minRadius = 20

local function calculateDistance(ball)
    local playerPos = LocalPlayer.Character.HumanoidRootPart.Position
    return (playerPos - ball.Position).Magnitude
end

local function calculateSpeed(ball)
    if previousBallPosition then
        local deltaPosition = ball.Position - previousBallPosition
        local velocity = deltaPosition.Magnitude / 0.05
        previousBallPosition = ball.Position
        return velocity
    else
        previousBallPosition = ball.Position
        return 0
    end
end

task.spawn(function()
    while true do
        task.wait(0.05)
        if not autoMode then continue end
        local ball = workspace:FindFirstChild("Part")
        if ball and ball:FindFirstChild("Highlight") and ball.Highlight.FillColor == RB then
            local distance = calculateDistance(ball)
            local speed = calculateSpeed(ball)
            if distance < minRadius and speed > speedThreshold then
                CLC()
            end
        end
    end

