local Players = game:GetService("Players")
local StarterGui = game:GetService("StarterGui")
local player = Players.LocalPlayer

local screenGui = Instance.new("ScreenGui", player.PlayerGui)
local button = Instance.new("TextButton", screenGui)
local enabled = false

button.Size = UDim2.new(0, 100, 0, 50)
button.Position = UDim2.new(0.5, -50, 0.5, -25)
button.Text = "Rusuh: OFF"
button.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
button.TextColor3 = Color3.fromRGB(255, 255, 255)
button.Font = Enum.Font.SourceSansBold
button.TextSize = 20
button.BorderSizePixel = 2
button.BorderColor3 = Color3.fromRGB(0, 0, 0)
button.Draggable = true
button.Active = true

local function rusuh()
    if enabled then
        for _, target in ipairs(Players:GetPlayers()) do
            if target ~= player then
                local character = target.Character
                if character and character:FindFirstChild("HumanoidRootPart") then
                    character.HumanoidRootPart.CFrame = character.HumanoidRootPart.CFrame * CFrame.new(0, 100, 0)
                    
                    StarterGui:SetCore("SendNotification", {
                        Title = "Rusuh Mode",
                        Text = target.Name .. " dilempar ke atas!",
                        Duration = 2
                    })
                end
            end
        end
    end
end

button.MouseButton1Click:Connect(function()
    enabled = not enabled
    if enabled then
        button.Text = "Rusuh: ON"
        button.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
        rusuh()
    else
        button.Text = "Rusuh: OFF"
        button.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
    end
end)