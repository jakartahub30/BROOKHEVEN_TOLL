local Players = game:GetService("Players") local LocalPlayer = Players.LocalPlayer local Mouse = LocalPlayer:GetMouse()

local function teleportToPlayer(player) if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then LocalPlayer.Character.HumanoidRootPart.CFrame = player.Character.HumanoidRootPart.CFrame end end

local function createTeleportButton(player) local button = Instance.new("TextButton") button.Text = "Teleport ke " .. player.Name button.Size = UDim2.new(1, 0, 0, 30) button.BackgroundColor3 = Color3.fromRGB(50, 50, 50) button.TextColor3 = Color3.fromRGB(255, 255, 255) button.MouseButton1Click:Connect(function() teleportToPlayer(player) end) button.Parent = scrollingFrame end

local gui = Instance.new("ScreenGui") local frame = Instance.new("Frame") local scrollingFrame = Instance.new("ScrollingFrame")

frame.Size = UDim2.new(0, 300, 0, 400) frame.Position = UDim2.new(0.5, -150, 0.5, -200) frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30) frame.Parent = gui

scrollingFrame.Size = UDim2.new(1, 0, 1, 0) scrollingFrame.CanvasSize = UDim2.new(0, 0, 5, 0) scrollingFrame.Parent = frame

for _, player in pairs(Players:GetPlayers()) do if player ~= LocalPlayer then createTeleportButton(player) end end

Players.PlayerAdded:Connect(function(player) createTeleportButton(player) end)

Players.PlayerRemoving:Connect(function(player) for _, button in pairs(scrollingFrame:GetChildren()) do if button:IsA("TextButton") and button.Text == "Teleport ke " .. player.Name then button:Destroy() end end end)

gui.Parent = game.CoreGui

