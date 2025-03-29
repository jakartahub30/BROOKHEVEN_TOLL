local ScreenGui = Instance.new("ScreenGui")
local MainFrame = Instance.new("Frame")
local ScrollingFrame = Instance.new("ScrollingFrame")
local UIListLayout = Instance.new("UIListLayout")
local LogoButton = Instance.new("ImageButton") -- Ganti TextButton dengan ImageButton
local CloseButton = Instance.new("TextButton")
local TitleBar = Instance.new("TextLabel")

ScreenGui.Name = "JakartaScript"
ScreenGui.Parent = game.CoreGui

MainFrame.Name = "MainFrame"
MainFrame.Parent = ScreenGui
MainFrame.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
MainFrame.BorderSizePixel = 0
MainFrame.Size = UDim2.new(0, 300, 0, 400)
MainFrame.Position = UDim2.new(0.5, -150, 0.5, -200)
MainFrame.Active = true
MainFrame.Draggable = true

local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(0, 10)
UICorner.Parent = MainFrame

local UIStroke = Instance.new("UIStroke")
UIStroke.Thickness = 2
UIStroke.Color = Color3.fromRGB(30, 30, 30)
UIStroke.Parent = MainFrame

TitleBar.Name = "TitleBar"
TitleBar.Parent = MainFrame
TitleBar.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
TitleBar.Size = UDim2.new(1, 0, 0, 30)
TitleBar.Font = Enum.Font.SourceSansBold
TitleBar.Text = "Jakarta Script"
TitleBar.TextColor3 = Color3.fromRGB(255, 255, 255)
TitleBar.TextSize = 20

local TitleBarCorner = Instance.new("UICorner")
TitleBarCorner.CornerRadius = UDim.new(0, 10)
TitleBarCorner.Parent = TitleBar

ScrollingFrame.Parent = MainFrame
ScrollingFrame.Size = UDim2.new(1, 0, 1, -40)
ScrollingFrame.Position = UDim2.new(0, 0, 0, 40)
ScrollingFrame.CanvasSize = UDim2.new(0, 0, 0, 0)
ScrollingFrame.ScrollBarThickness = 8
ScrollingFrame.BackgroundTransparency = 1
ScrollingFrame.AutomaticCanvasSize = Enum.AutomaticSize.Y

UIListLayout.Parent = ScrollingFrame
UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
UIListLayout.Padding = UDim.new(0, 5)

LogoButton.Name = "LogoButton"
LogoButton.Parent = ScreenGui
LogoButton.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
LogoButton.Position = UDim2.new(0, 10, 0, 10)
LogoButton.Size = UDim2.new(0, 50, 0, 50)
LogoButton.Image = ""

-- Tambahkan latar belakang merah putih silang
local BackgroundFrame = Instance.new("Frame")
BackgroundFrame.Size = UDim2.new(1, 0, 1, 0)
BackgroundFrame.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
BackgroundFrame.Parent = LogoButton

local WhiteCross1 = Instance.new("Frame")
WhiteCross1.Size = UDim2.new(0.1, 0, 1, 0)
WhiteCross1.Position = UDim2.new(0.45, 0, 0, 0)
WhiteCross1.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
WhiteCross1.Parent = BackgroundFrame

local WhiteCross2 = Instance.new("Frame")
WhiteCross2.Size = UDim2.new(1, 0, 0.1, 0)
WhiteCross2.Position = UDim2.new(0, 0, 0.45, 0)
WhiteCross2.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
WhiteCross2.Parent = BackgroundFrame

LogoButton.Visible = true
LogoButton.Active = true
LogoButton.Draggable = true -- Menjadikan logo dapat digerakkan

local isVisible = true
local toggles = {}
local teleportConnection = nil

local function toggleGui()
    isVisible = not isVisible
    MainFrame.Visible = isVisible
    LogoButton.Visible = not isVisible
end

local function createButton(name, callback)
    local Button = Instance.new("TextButton")
    Button.Parent = ScrollingFrame
    Button.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
    Button.Size = UDim2.new(1, 0, 0, 40)
    Button.Font = Enum.Font.SourceSans
    Button.Text = name .. " [OFF]"
    Button.TextColor3 = Color3.fromRGB(255, 255, 255)
    Button.TextSize = 20

    local ButtonCorner = Instance.new("UICorner")
    ButtonCorner.CornerRadius = UDim.new(0, 10)
    ButtonCorner.Parent = Button

    local ButtonStroke = Instance.new("UIStroke")
    ButtonStroke.Thickness = 2
    ButtonStroke.Color = Color3.fromRGB(45, 45, 45)
    ButtonStroke.Parent = Button

    Button.MouseButton1Click:Connect(function()
        toggles[name] = not toggles[name]
        Button.Text = name .. (toggles[name] and " [ON]" or " [OFF]")
        callback(toggles[name])
    end)
end

CloseButton.Parent = MainFrame
CloseButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
CloseButton.Size = UDim2.new(0, 30, 0, 30)
CloseButton.Position = UDim2.new(1, -35, 0, 5)
CloseButton.Font = Enum.Font.SourceSansBold
CloseButton.Text = "X"
CloseButton.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseButton.TextSize = 20
CloseButton.MouseButton1Click:Connect(toggleGui)

createButton("Super Speed", function(state)
    game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = state and 500 or 16
end)

createButton("Invisible", function(state)
    for _, part in pairs(game.Players.LocalPlayer.Character:GetChildren()) do
        if part:IsA("BasePart") then
            part.Transparency = state and 1 or 0
        end
    end
end)

createButton("Jump Power", function(state)
    game.Players.LocalPlayer.Character.Humanoid.JumpPower = state and 200 or 50
end)

createButton("Infinite Jump", function(state)
    local InfiniteJumpEnabled = state
    game:GetService("UserInputService").JumpRequest:Connect(function()
        if InfiniteJumpEnabled then
            game.Players.LocalPlayer.Character:FindFirstChildOfClass("Humanoid"):ChangeState("Jumping")
        end
    end)
end)

createButton("NoClip", function(state)
    local noclip = state
    game:GetService("RunService").Stepped:Connect(function()
        if noclip then
            for _, part in pairs(game.Players.LocalPlayer.Character:GetChildren()) do
                if part:IsA("BasePart") then
                    part.CanCollide = false
                end
            end
        end
    end)
end)

createButton("ESP", function(state)
    local espEnabled = state
    while espEnabled do
        for _, player in pairs(game.Players:GetPlayers()) do
            if player ~= game.Players.LocalPlayer then
                if not player.Character:FindFirstChild("ESPBox") then
                    local esp = Instance.new("BoxHandleAdornment")
                    esp.Name = "ESPBox"
                    esp.Adornee = player.Character
                    esp.Size = player.Character:GetExtentsSize()
                    esp.Color3 = Color3.fromRGB(255, 0, 0)
                    esp.AlwaysOnTop = true
                    esp.ZIndex = 10
                    esp.Transparency = 0.7
                    esp.Parent = player.Character
                end
            end
        end
        wait(0.1)
    end
    for _, player in pairs(game.Players:GetPlayers()) do
        if player.Character:FindFirstChild("ESPBox") then
            player.Character:FindFirstChild("ESPBox"):Destroy()
        end
    end
end)

createButton("Gravity", function(state)
    game.Workspace.Gravity = state and 50 or 196.2
end)

createButton("Aim Lock", function(state)
    local aimlockEnabled = state
    local aimlockTarget = nil
    game:GetService("UserInputService").InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton2 then
            aimlockEnabled = not aimlockEnabled
            if aimlockEnabled then
                aimlockTarget = game.Players:GetPlayers()[math.random(1, #game.Players:GetPlayers())]
            else
                aimlockTarget = nil
            end
        end
    end)
    game:GetService("RunService").RenderStepped:Connect(function()
        if aimlockEnabled and aimlockTarget then
            local mouse = game.Players.LocalPlayer:GetMouse()
            mouse.TargetFilter = aimlockTarget.Character
            mouse.Hit = aimlockTarget.Character.Head.CFrame
        end
    end)
end)

createButton("God Mode", function(state)
    local godModeEnabled = state
    while godModeEnabled do
        game.Players.LocalPlayer.Character.Humanoid.Health = 100
        wait(0.1)
    end
end)

createButton("Anti AFK", function(state)
    local vu = game:GetService("VirtualUser")
    game:GetService("Players").LocalPlayer.Idled:Connect(function()
        if state then
            vu:Button2Down(Vector2.new(0, 0), workspace.CurrentCamera.CFrame)
            wait(1)
            vu:Button2Up(Vector2.new(0, 0), workspace.CurrentCamera.CFrame)
        end
    end)
end)

createButton("Kill Aura", function(state)
    local killAuraEnabled = state
    while killAuraEnabled do
        for _, player in pairs(game.Players:GetPlayers()) do
            if player ~= game.Players.LocalPlayer and player.Character and player.Character:FindFirstChildOfClass("Humanoid") then
                local distance = (player.Character.HumanoidRootPart.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude
                if distance < 10 then
                    pcall(function()
                        game:GetService("ReplicatedStorage").Events.Damage:FireServer(player.Character.Humanoid)
                    end)
                end
            end
        end
        wait(0.1)
    end
end)

createButton("Teleport", function(state)
    local player = game.Players.LocalPlayer
    local mouse = player:GetMouse()

    if state then
        teleportConnection = mouse.Button1Down:Connect(function()
            if mouse.Target then
                local targetPosition = mouse.Hit.p + Vector3.new(0, 5, 0) -- Adjust the teleport position to be slightly above the ground
                local humanoidRootPart = player.Character:FindFirstChild("HumanoidRootPart")
                if humanoidRootPart then
                    humanoidRootPart.CFrame = CFrame.new(targetPosition)
                end
            end
        end)
    else
        if teleportConnection then
            teleportConnection:Disconnect()
            teleportConnection = nil
        end
    end
end)

createButton("Speed Boost", function(state)
    local speedBoostEnabled = state
    local player = game.Players.LocalPlayer
    local character = player.Character

    if speedBoostEnabled then
        character.Humanoid.WalkSpeed = 100
        wait(5)
        character.Humanoid.WalkSpeed = 16
    end
end)

createButton("Heal", function(state)
    if state then
        local player = game.Players.LocalPlayer
        player.Character.Humanoid.Health = player.Character.Humanoid.MaxHealth
    end
end)

createButton("Force Field", function(state)
    local forceFieldEnabled = state
    local player = game.Players.LocalPlayer

    if forceFieldEnabled then
        local forceField = Instance.new("ForceField")
        forceField.Parent = player.Character
    else
        for _, child in pairs(player.Character:GetChildren()) do
            if child:IsA("ForceField") then
                child:Destroy()
            end
        end
    end
end)

createButton("Bunny Hop", function(state)
    local bunnyHopEnabled = state
    local player = game.Players.LocalPlayer
    local character = player.Character

    while bunnyHopEnabled do
        character.Humanoid:ChangeState("Jumping")
        wait(0.3)
    end
end)

createButton("Item Float", function(state)
    local floatEnabled = state
    local player = game.Players.LocalPlayer

    if floatEnabled then
        local item = player.Backpack:FindFirstChildOfClass("Tool") atau player.Character:FindFirstChildOfClass("Tool")
        if item then
            local part = item.Handle atau item:FindFirstChildWhichIsA("BasePart")
            if part then
                local bodyPosition = Instance.new("BodyPosition")
                bodyPosition.Position = part.Position + Vector3.new(0, 5, 0)
                bodyPosition.D = 1000
                bodyPosition.P = 3000
                bodyPosition.MaxForce = Vector3.new(4000, 4000, 4000)
                bodyPosition.Parent = part

                local bodyGyro = Instance.new("BodyGyro")
                bodyGyro.CFrame = part.CFrame
                bodyGyro.D = 500
                bodyGyro.P = 3000
                bodyGyro.MaxTorque = Vector3.new(4000, 4000, 4000)
                bodyGyro.Parent = part

                part.CanCollide = true
            end
        end
    else
        -- Remove floating properties if disabled
        for _, item in pairs(player.Backpack:GetChildren()) do
            if item:IsA("Tool") then
                local part = item.Handle atau item:FindFirstChildWhichIsA("BasePart")
                if part then
                    if part:FindFirstChildOfClass("BodyPosition") then
                        part:FindFirstChildOfClass("BodyPosition"):Destroy()
                    end
                    if part:FindFirstChildOfClass("BodyGyro") then
                        part:FindFirstChildOfClass("BodyGyro"):Destroy()
                    end
                    part.CanCollide = false
                end
            end
        end
    end
end)

LogoButton.MouseButton1Click:Connect(toggleGui)