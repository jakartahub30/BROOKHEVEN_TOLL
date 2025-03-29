local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local player = Players.LocalPlayer
local stuckEvent = ReplicatedStorage:WaitForChild("StuckEvent")

local screenGui = Instance.new("ScreenGui")
screenGui.Parent = player.PlayerGui

local button = Instance.new("TextButton")
button.Size = UDim2.new(0, 200, 0, 50)
button.Position = UDim2.new(0.5, -100, 0.1, 0)
button.Text = "Stuck: OFF"
button.Parent = screenGui
button.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
button.TextColor3 = Color3.fromRGB(255, 255, 255)
button.Font = Enum.Font.SourceSansBold
button.TextSize = 20

local stuckEnabled = false

button.MouseButton1Click:Connect(function()
    stuckEnabled = not stuckEnabled
    if stuckEnabled then
        button.Text = "Stuck: ON"
        button.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
    else
        button.Text = "Stuck: OFF"
        button.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
    end
end)

local function teleportAndFreeze(targetPlayer, distance)
    if targetPlayer and targetPlayer.Character then
        local targetCharacter = targetPlayer.Character
        local newPosition = player.Character.HumanoidRootPart.Position + player.Character.HumanoidRootPart.CFrame.LookVector * distance
        stuckEvent:FireServer(targetPlayer, newPosition)

        local humanoid = targetCharacter:FindFirstChildOfClass("Humanoid")
        if humanoid then
            humanoid.WalkSpeed = 0
            humanoid.JumpPower = 0
            humanoid.PlatformStand = true
        end
    end
end

game:GetService("RunService").Heartbeat:Connect(function()
    if stuckEnabled then
        for _, targetPlayer in ipairs(Players:GetPlayers()) do
            if targetPlayer ~= player and targetPlayer.Character then
                teleportAndFreeze(targetPlayer, 5)
            end
        end
    end
end)