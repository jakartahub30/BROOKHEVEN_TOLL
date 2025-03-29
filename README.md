
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local UserInputService = game:GetService("UserInputService")

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Parent = LocalPlayer.PlayerGui

local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 300, 0, 400)
MainFrame.Position = UDim2.new(0.5, -150, 0.5, -200)
MainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
MainFrame.BorderSizePixel = 0
MainFrame.Parent = ScreenGui

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, 0, 0, 30)
Title.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
Title.Text = "JAKARTA HUB"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.Font = Enum.Font.SourceSansBold
Title.TextSize = 20
Title.Parent = MainFrame

local ScrollingFrame = Instance.new("ScrollingFrame")
ScrollingFrame.Size = UDim2.new(1, 0, 1, -30)
ScrollingFrame.Position = UDim2.new(0, 0, 0, 30)
ScrollingFrame.CanvasSize = UDim2.new(0, 0, 0, 600)
ScrollingFrame.ScrollBarThickness = 8
ScrollingFrame.Parent = MainFrame

local function createButton(name, callback)
    local Button = Instance.new("TextButton")
    Button.Size = UDim2.new(1, -10, 0, 40)
    Button.Position = UDim2.new(0, 5, 0, 0)
    Button.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    Button.TextColor3 = Color3.fromRGB(255, 255, 255)
    Button.Text = name
    Button.Parent = ScrollingFrame
    Button.MouseButton1Click:Connect(callback)
end

local function createSlider(name, min, max, callback)
    local SliderFrame = Instance.new("Frame")
    SliderFrame.Size = UDim2.new(1, -10, 0, 40)
    SliderFrame.Position = UDim2.new(0, 5, 0, 0)
    SliderFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    SliderFrame.Parent = ScrollingFrame

    local Slider = Instance.new("Slider")
    Slider.Size = UDim2.new(1, 0, 0, 20)
    Slider.Position = UDim2.new(0, 0, 0, 10)
    Slider.Min = min
    Slider.Max = max
    Slider.Parent = SliderFrame

    local SliderLabel = Instance.new("TextLabel")
    SliderLabel.Size = UDim2.new(1, 0, 0, 20)
    SliderLabel.Position = UDim2.new(0, 0, 0, 0)
    SliderLabel.Text = name .. " (" .. tostring(min) .. "-" .. tostring(max) .. ")"
    SliderLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    SliderLabel.Parent = SliderFrame

    Slider.ValueChanged:Connect(function(value)
        SliderLabel.Text = name .. " (" .. tostring(math.floor(value)) .. ")"
        callback(value)
    end)
end

local killAA = false
local noClip = false
local infinityJump = false
local speed = 16
local esp = false
local invisible = false
local jumpPower = 50
local fly = 0

createButton("Kill AA", function()
    killAA = not killAA
end)

createButton("No Clip", function()
    noClip = not noClip
    if noClip then
        LocalPlayer.Character.HumanoidRootPart.CanCollide = false
    else
        LocalPlayer.Character.HumanoidRootPart.CanCollide = true
    end
end)

createButton("Infinity Jump", function()
    infinityJump = not infinityJump
end)

createSlider("Speed", 1, 500, function(value)
    speed = value
end)

createButton("ESP", function()
    esp = not esp
end)

createButton("Invisible", function()
    invisible = not invisible
    if invisible then
        LocalPlayer.Character.HumanoidRootPart.Transparency = 1
    else
        LocalPlayer.Character.HumanoidRootPart.Transparency = 0
    end
end)

createSlider("Jump Power", 1, 10000, function(value)
    jumpPower = value
end)

create