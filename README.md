-- ====================================================================
-- RETRO 3D MINECRAFT RAYCASTER & LIVE FPS TRACKER
-- ====================================================================

local RunService = game:GetService("RunService")
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local PlayerGui = LocalPlayer:WaitForChild("PlayerGui")

-- 1. Create Retro Viewport GUI
local mcGui = Instance.new("ScreenGui")
mcGui.Name = "MCRetroGui"
mcGui.ResetOnSpawn = false
mcGui.Parent = PlayerGui

local screenFrame = Instance.new("Frame")
screenFrame.Size = UDim2.new(0, 320, 0, 200)
screenFrame.Position = UDim2.new(0.5, -160, 0.5, -100)
screenFrame.BackgroundColor3 = Color3.fromRGB(135, 206, 235) -- Sky Blue
screenFrame.BorderSizePixel = 2
screenFrame.BorderColor3 = Color3.fromRGB(50, 50, 50)
screenFrame.Active = true
screenFrame.Draggable = true
screenFrame.Parent = mcGui

-- 2. Live Performance & FPS Counter
local fpsLabel = Instance.new("TextLabel")
fpsLabel.Size = UDim2.new(0, 120, 0, 20)
fpsLabel.Position = UDim2.new(0, 5, 0, 5)
fpsLabel.BackgroundTransparency = 1
fpsLabel.TextColor3 = Color3.fromRGB(0, 255, 100)
fpsLabel.Font = Enum.Font.Code
fpsLabel.TextSize = 11
fpsLabel.TextXAlignment = Enum.TextXAlignment.Left
fpsLabel.Parent = screenFrame

-- Performance calculation loop
local lastTick = tick()
local frameCount = 0
local currentFPS = 60

RunService.RenderStepped:Connect(function()
    frameCount = frameCount + 1
    local now = tick()
    if now - lastTick >= 1 then
        currentFPS = math.floor(frameCount / (now - lastTick))
        frameCount = 0
        lastTick = now
        
        -- Dynamic performance adaptation
        local perfColor = (currentFPS >= 30) and Color3.fromRGB(0, 255, 100) or Color3.fromRGB(255, 50, 50)
        fpsLabel.TextColor3 = perfColor
        fpsLabel.Text = "FPS: " .. currentFPS .. " | Voxel: OK"
    end
end)

-- 3. Ground / Floor Horizon Split
local groundFrame = Instance.new("Frame")
groundFrame.Size = UDim2.new(1, 0, 0.5, 0)
groundFrame.Position = UDim2.new(0, 0, 0.5, 0)
groundFrame.BackgroundColor3 = Color3.fromRGB(85, 140, 40) -- Grass Green
groundFrame.BorderSizePixel = 0
groundFrame.Parent = screenFrame
