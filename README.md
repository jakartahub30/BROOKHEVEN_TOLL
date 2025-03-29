local laserAktif = false
local laserPart

local function buatLaser()
    local player = game.Players.LocalPlayer
    if player.Character then
        local character = player.Character
        local humanoidRootPart = character:WaitForChild("HumanoidRootPart")
        if laserAktif then
            laserPart = Instance.new("Part")
            laserPart.Size = Vector3.new(1, 20, 1)
            laserPart.Position = humanoidRootPart.Position - Vector3.new(0, 5, 0)
            laserPart.Anchored = true
            laserPart.CanCollide = false
            laserPart.Color = Color3.fromRGB(255, 0, 0)
            laserPart.Material = Enum.Material.Neon
            laserPart.Parent = workspace
            local beam = Instance.new("Beam")
            beam.Attachment0 = humanoidRootPart:FindFirstChild("Attachment") or Instance.new("Attachment", humanoidRootPart)
            beam.Attachment1 = Instance.new("Attachment", laserPart)
            beam.Color = ColorSequence.new(Color3.fromRGB(255, 0, 0))
            beam.Parent = laserPart
            laserPart.Touched:Connect(function(hit)
                local hitPlayer = game.Players:GetPlayerFromCharacter(hit.Parent)
                if hitPlayer and hitPlayer ~= player then
                    local humanoid = hit.Parent:FindFirstChild("Humanoid")
                    if humanoid then
                        humanoid.Health = 0
                    end
                end
            end)
            game:GetService("Debris"):AddItem(laserPart, 5)
        end
    end
end

local function toggleLaser()
    laserAktif = not laserAktif
    if laserAktif then
        buatLaser()
    else
        if laserPart then
            laserPart:Destroy()
        end
    end
end

toggleLaser()