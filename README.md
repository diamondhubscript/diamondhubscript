-- Create ScreenGui
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "DiamondHub"
screenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

-- Main Frame (Movable)
local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0.5, 0, 0.6, 0) -- Larger size
mainFrame.Position = UDim2.new(0.25, 0, 0.2, 0)
mainFrame.BackgroundColor3 = Color3.fromRGB(15, 30, 60) -- Dark Blue
mainFrame.BorderSizePixel = 0
mainFrame.Active = true
mainFrame.Draggable = true -- Makes the frame movable
mainFrame.Parent = screenGui

-- Frame Corner
local frameCorner = Instance.new("UICorner")
frameCorner.CornerRadius = UDim.new(0, 12)
frameCorner.Parent = mainFrame

-- Title
local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0.2, 0)
title.BackgroundTransparency = 1
title.Text = "Diamond Hub"
title.Font = Enum.Font.SourceSansBold
title.TextSize = 32 -- Larger text
title.TextColor3 = Color3.fromRGB(255, 215, 0) -- Gold
title.Parent = mainFrame

-- Variables for Auto Farm toggle
local autoFarm = false

-- Function to start and stop auto farming
local function toggleAutoFarm()
    autoFarm = not autoFarm
    if autoFarm then
        autoFarmButton.Text = "Auto Farm: ON"
        while autoFarm do
            -- Auto farm functionality here
            teleportAndBreakCoin() -- Uses the coin function defined below
            wait(1) -- Delay between each teleport and break
        end
    else
        autoFarmButton.Text = "Auto Farm: OFF"
    end
end

-- Function to find and teleport to nearest coin
local function teleportAndBreakCoin()
    local character = game.Players.LocalPlayer.Character or game.Players.LocalPlayer.CharacterAdded:Wait()
    local nearestCoin = nil
    local shortestDistance = math.huge

    for _, coin in pairs(workspace.Coins:GetChildren()) do
        if coin:IsA("BasePart") then
            local distance = (coin.Position - character.HumanoidRootPart.Position).magnitude
            if distance < shortestDistance then
                nearestCoin = coin
                shortestDistance = distance
            end
        end
    end

    if nearestCoin then
        -- Teleport to the nearest coin and break it
        character.HumanoidRootPart.CFrame = CFrame.new(nearestCoin.Position)
        wait(0.1)
        
        -- Trigger "BreakPrompt" if available on the coin
        local breakPrompt = nearestCoin:FindFirstChild("BreakPrompt")
        if breakPrompt and breakPrompt:IsA("ProximityPrompt") then
            fireproximityprompt(breakPrompt)
        end
    end
end

-- Auto Farm Button
local autoFarmButton = Instance.new("TextButton")
autoFarmButton.Size = UDim2.new(0.8, 0, 0.15, 0)
autoFarmButton.Position = UDim2.new(0.1, 0, 0.5, 0)
autoFarmButton.BackgroundColor3 = Color3.fromRGB(70, 130, 180) -- Sky Blue
autoFarmButton.TextColor3 = Color3.fromRGB(255, 255, 255)
autoFarmButton.Font = Enum.Font.SourceSansBold
autoFarmButton.TextSize = 22
autoFarmButton.Text = "Auto Farm: OFF"
autoFarmButton.Parent = mainFrame

-- Button Corner for Auto Farm Button
local buttonCorner = Instance.new("UICorner")
buttonCorner.CornerRadius = UDim.new(0, 10)
buttonCorner.Parent = autoFarmButton

-- Connect the toggle function to the button click
autoFarmButton.MouseButton1Click:Connect(toggleAutoFarm)
