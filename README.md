local laserAktif = false

local function buatLaser(player)
    if player.Character then
        local character = player.Character
        local humanoidRootPart = character:WaitForChild("HumanoidRootPart")
        
        local laser = Instance.new("Part")
        laser.Size = Vector3.new(1, 20, 1)
        laser.Position = humanoidRootPart.Position - Vector3.new(0, 5, 0)
        laser.Anchored = true
        laser.CanCollide = false
        laser.Color = Color3.fromRGB(255, 0, 0)
        laser.Material = Enum.Material.Neon
        laser.Parent = workspace

        local beam = Instance.new("Beam")
        beam.Attachment0 = humanoidRootPart:FindFirstChild("Attachment") or Instance.new("Attachment", humanoidRootPart)
        beam.Attachment1 = Instance.new("Attachment", laser)
        beam.Color = ColorSequence.new(Color3.fromRGB(255, 0, 0))
        beam.Parent = laser

        laser.Touched:Connect(function(hit)
            local hitPlayer = game.Players:GetPlayerFromCharacter(hit.Parent)
            if hitPlayer and hitPlayer ~= player then
                local humanoid = hit.Parent:FindFirstChild("Humanoid")
                if humanoid then
                    humanoid.Health = 0
                end
            end
        end)

        game:GetService("Debris"):AddItem(laser, 5)
    end
end

local function toggleLaser(player)
    laserAktif = not laserAktif
    if laserAktif then
        buatLaser(player)
    end
end

game.Players.PlayerAdded:Connect(function(player)
    player.CharacterAdded:Connect(function()
        wait(1)
        -- Panggil toggleLaser untuk mengaktifkan atau menonaktifkan laser
        toggleLaser(player)
    end)
end)