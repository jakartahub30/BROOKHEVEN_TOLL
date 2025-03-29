local ScreenGui = Instance.new("ScreenGui")
local MainFrame = Instance.new("Frame")
local ScrollingFrame = Instance.new("ScrollingFrame")
local UIListLayout = Instance.new("UIListLayout")
local LogoButton = Instance.new("TextButton")
local CloseButton = Instance.new("TextButton")
local TitleBar = Instance.new("TextLabel")

ScreenGui.Name = "JakartaScript"
ScreenGui.Parent = game.CoreGui

MainFrame.Name = "MainFrame"
MainFrame.Parent = ScreenGui
MainFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
MainFrame.Size = UDim2.new(0, 250, 0, 350)
MainFrame.Position = UDim2.new(0.5, -125, 0.5, -150)
MainFrame.Active = true
MainFrame.Draggable = true

TitleBar.Name = "TitleBar"
TitleBar.Parent = MainFrame
TitleBar.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
TitleBar.Size = UDim2.new(1, 0, 0, 25)
TitleBar.Font = Enum.Font.SourceSansBold
TitleBar.Text = "Jakarta Script"
TitleBar.TextColor3 = Color3.fromRGB(255, 255, 255)
TitleBar.TextSize = 16

ScrollingFrame.Parent = MainFrame
ScrollingFrame.Size = UDim2.new(1, 0, 1, -30)
ScrollingFrame.Position = UDim2.new(0, 0, 0, 30)
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
LogoButton.Font = Enum.Font.SourceSansBold
LogoButton.Text = "JKT"
LogoButton.TextSize = 20
LogoButton.TextColor3 = Color3.fromRGB(0, 0, 0)
LogoButton.Visible = false

local isVisible = true
local toggles = {}

local function toggleGui()
    isVisible = not isVisible
    MainFrame.Visible = isVisible
    LogoButton.Visible = not isVisible
end

local function createButton(name, callback)
    local Button = Instance.new("TextButton")
    Button.Parent = ScrollingFrame
    Button.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    Button.Size = UDim2.new(1, 0, 0, 30)
    Button.Font = Enum.Font.SourceSans
    Button.Text = name .. " [OFF]"
    Button.TextColor3 = Color3.fromRGB(255, 255, 255)
    Button.TextSize = 20
    Button.MouseButton1Click:Connect(function()
        toggles[name] = not toggles[name]
        Button.Text = name .. (toggles[name] and " [ON]" or " [OFF]")
        callback(toggles[name])
    end)
end

CloseButton.Parent = MainFrame
CloseButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
CloseButton.Size = UDim2.new(0, 30, 0, 30)
CloseButton.Position = UDim2.new(1, -35, 0, -35)
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
                if (player.Character.HumanoidRootPart.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).magnitude < 10 then
                    game:GetService("ReplicatedStorage").Events.Damage:FireServer(player.Character.Humanoid)
                end
            end
        end
        wait(0.1)
    end
end)

createButton("Fly", function(state)
    local flyEnabled = state
    local player = game.Players.LocalPlayer
    local character = player.Character

    if flyEnabled then
        local bodyGyro = Instance.new("BodyGyro")
        local bodyVelocity = Instance.new("BodyVelocity")
        bodyGyro.P = 9e4
        bodyGyro.Parent = character.HumanoidRootPart
        bodyVelocity.Velocity = Vector3.new(0, 0, 0)
        bodyVelocity.Parent = character.HumanoidRootPart

        local function fly()
            if flyEnabled then
                bodyVelocity.Velocity = character.HumanoidRootPart.CFrame.LookVector * 50
                bodyGyro.CFrame = workspace.CurrentCamera.CFrame
                wait()
                fly()
            else
                bodyGyro:Destroy()
                bodyVelocity:Destroy()
            end
        end

        fly()
    end
end)

createButton("Teleport", function(state)
    if state then
        local player = game.Players.LocalPlayer
        local mouse = player:GetMouse()

        mouse.Button1Down:Connect(function()
            if mouse.Target then
                player.Character.HumanoidRootPart.CFrame = CFrame.new(mouse.Hit.p)
            end
        end)
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

LogoButton.MouseButton1Click:Connect(toggleGui)