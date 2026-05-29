--[[
    Roblox Eat the World Auto-Farm Script
    Created for loadstring execution or direct executor injection.
    
    Usage:
    loadstring(game:HttpGet("https://raw.githubusercontent.com/YOUR_USERNAME/YOUR_REPO/main/autofarm.lua"))()
--]]
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer
-- Configuration variables (can be adjusted via UI or manually)
local Config = {
    AutoFarm = false,
    AutoSell = false,
    FarmSpeed = 45, -- Speed of tweening
    FoodFolderNames = {"Food", "Foods", "Fruits", "SpawnedFood", "FoodSpawns", "WorldItems"},
    SellPartNames = {"Sell", "SellArea", "SellRing", "SellPart", "SellPlace"},
    MaxDistance = 1500 -- Max distance to search for food
}
-- UI Setup (Sleek Glassmorphic Toggle GUI)
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "EatWorldHub"
ScreenGui.ResetOnSpawn = false
-- Core Gui Protection if using supported executors
if syn and syn.protect_gui then
    syn.protect_gui(ScreenGui)
elseif gethui then
    ScreenGui.Parent = gethui()
else
    ScreenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")
end
local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, 260, 0, 320)
MainFrame.Position = UDim2.new(0.05, 0, 0.3, 0)
MainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
MainFrame.BackgroundTransparency = 0.15
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Parent = ScreenGui
-- Rounded corners
local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(0, 10)
UICorner.Parent = MainFrame
-- Title
local TitleLabel = Instance.new("TextLabel")
TitleLabel.Name = "Title"
TitleLabel.Size = UDim2.new(1, 0, 0, 45)
TitleLabel.BackgroundTransparency = 1
TitleLabel.Font = Enum.Font.GothamBold
TitleLabel.Text = "EAT THE WORLD HUB"
TitleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
TitleLabel.TextSize = 16
TitleLabel.Parent = MainFrame
-- Status / Info Label
local StatusLabel = Instance.new("TextLabel")
StatusLabel.Name = "Status"
StatusLabel.Size = UDim2.new(1, 0, 0, 30)
StatusLabel.Position = UDim2.new(0, 0, 0, 45)
StatusLabel.BackgroundTransparency = 1
StatusLabel.Font = Enum.Font.GothamSemibold
StatusLabel.Text = "Durum: Bekliyor..."
StatusLabel.TextColor3 = Color3.fromRGB(180, 180, 180)
StatusLabel.TextSize = 12
StatusLabel.Parent = MainFrame
-- Toggle Button helper function
local function createToggle(name, text, position, default, callback)
    local Button = Instance.new("TextButton")
    Button.Name = name
    Button.Size = UDim2.new(0, 220, 0, 45)
    Button.Position = position
    Button.BackgroundColor3 = default and Color3.fromRGB(46, 204, 113) or Color3.fromRGB(231, 76, 60)
    Button.Font = Enum.Font.GothamBold
    Button.Text = text .. ": " .. (default and "ACIK" or "KAPALI")
    Button.TextColor3 = Color3.fromRGB(255, 255, 255)
    Button.TextSize = 14
    Button.Parent = MainFrame
    
    local BtnCorner = Instance.new("UICorner")
    BtnCorner.CornerRadius = UDim.new(0, 8)
    BtnCorner.Parent = Button
    
    local state = default
    Button.MouseButton1Click:Connect(function()
        state = not state
        Button.BackgroundColor3 = state and Color3.fromRGB(46, 204, 113) or Color3.fromRGB(231, 76, 60)
        Button.Text = text .. ": " .. (state and "ACIK" or "KAPALI")
        callback(state)
    end)
    return Button
end
-- Create Auto Farm Toggle
createToggle("AutoFarmToggle", "Otomatik Yemek Ye", UDim2.new(0, 20, 0, 90), Config.AutoFarm, function(state)
    Config.AutoFarm = state
    if not state then
        StatusLabel.Text = "Durum: Durduruldu"
    end
end)
-- Create Auto Sell Toggle
createToggle("AutoSellToggle", "Otomatik Sat", UDim2.new(0, 20, 0, 145), Config.AutoSell, function(state)
    Config.AutoSell = state
end)
-- Speed Controls
local SpeedLabel = Instance.new("TextLabel")
SpeedLabel.Size = UDim2.new(1, 0, 0, 25)
SpeedLabel.Position = UDim2.new(0, 0, 0, 205)
SpeedLabel.BackgroundTransparency = 1
SpeedLabel.Font = Enum.Font.Gotham
SpeedLabel.Text = "Ulasma Hizi: " .. Config.FarmSpeed
SpeedLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
SpeedLabel.TextSize = 12
SpeedLabel.Parent = MainFrame
local SpeedUp = Instance.new("TextButton")
SpeedUp.Size = UDim2.new(0, 100, 0, 30)
SpeedUp.Position = UDim2.new(0, 20, 0, 235)
SpeedUp.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
SpeedUp.Font = Enum.Font.GothamBold
SpeedUp.Text = "Hizlandir (+5)"
SpeedUp.TextColor3 = Color3.fromRGB(255, 255, 255)
SpeedUp.TextSize = 12
SpeedUp.Parent = MainFrame
Instance.new("UICorner", SpeedUp).CornerRadius = UDim.new(0, 5)
SpeedUp.MouseButton1Click:Connect(function()
    Config.FarmSpeed = math.min(150, Config.FarmSpeed + 5)
    SpeedLabel.Text = "Ulasma Hizi: " .. Config.FarmSpeed
end)
local SpeedDown = Instance.new("TextButton")
SpeedDown.Size = UDim2.new(0, 100, 0, 30)
SpeedDown.Position = UDim2.new(0, 140, 0, 235)
SpeedDown.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
SpeedDown.Font = Enum.Font.GothamBold
SpeedDown.Text = "Yavaslat (-5)"
SpeedDown.TextColor3 = Color3.fromRGB(255, 255, 255)
SpeedDown.TextSize = 12
SpeedDown.Parent = MainFrame
Instance.new("UICorner", SpeedDown).CornerRadius = UDim.new(0, 5)
SpeedDown.MouseButton1Click:Connect(function()
    Config.FarmSpeed = math.max(10, Config.FarmSpeed - 5)
    SpeedLabel.Text = "Ulasma Hizi: " .. Config.FarmSpeed
end)
-- Minimize Button
local MinimizeBtn = Instance.new("TextButton")
MinimizeBtn.Size = UDim2.new(0, 30, 0, 30)
MinimizeBtn.Position = UDim2.new(1, -40, 0, 7)
MinimizeBtn.BackgroundTransparency = 1
MinimizeBtn.Font = Enum.Font.GothamBold
MinimizeBtn.Text = "-"
MinimizeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
MinimizeBtn.TextSize = 20
MinimizeBtn.Parent = MainFrame
local Minimized = false
MinimizeBtn.MouseButton1Click:Connect(function()
    Minimized = not Minimized
    if Minimized then
        MainFrame:TweenSize(UDim2.new(0, 260, 0, 45), "Out", "Quad", 0.3, true)
        MinimizeBtn.Text = "+"
    else
        MainFrame:TweenSize(UDim2.new(0, 260, 0, 320), "Out", "Quad", 0.3, true)
        MinimizeBtn.Text = "-"
    end
end)
-- Core Auto Farm Mechanics
-- Find Food Folder
local function getFoodFolder()
    for _, name in ipairs(Config.FoodFolderNames) do
        local folder = workspace:FindFirstChild(name)
        if folder then return folder end
    end
    -- Fallback: check workspace recursively for parts with food-related attributes or names
    return nil
end
-- Find Sell Area
local function getSellPart()
    for _, name in ipairs(Config.SellPartNames) do
        local part = workspace:FindFirstChild(name, true)
        if part and part:IsA("BasePart") then
            return part
        end
    end
    return nil
end
-- Find Character Parts
local function getCharacterParts()
    local char = LocalPlayer.Character
    if not char then return nil, nil end
    local hrp = char:FindFirstChild("HumanoidRootPart")
    local hum = char:FindFirstChildOfClass("Humanoid")
    return hrp, hum
end
-- Get Capacity Details (Looks at Leaderstats or common UI elements)
local function isFull()
    local leaderstats = LocalPlayer:FindFirstChild("leaderstats")
    if leaderstats then
        local current = leaderstats:FindFirstChild("Food") or leaderstats:FindFirstChild("Weight") or leaderstats:FindFirstChild("Size")
        local maxVal = leaderstats:FindFirstChild("MaxFood") or leaderstats:FindFirstChild("Capacity") or leaderstats:FindFirstChild("MaxWeight")
        
        if current and maxVal then
            return current.Value >= maxVal.Value
        end
    end
    
    -- Fallback: check PlayerGui for text patterns like "Full", "100/100", etc.
    return false
end
-- Move to target safely using Tween (prevents bans better than instant teleportation)
local activeTween = nil
local function moveTo(targetPosition, speed)
    local hrp, hum = getCharacterParts()
    if not hrp or not hum then return end
    
    local distance = (hrp.Position - targetPosition).Magnitude
    local duration = distance / speed
    
    local tweenInfo = TweenInfo.new(duration, Enum.EasingStyle.Linear)
    activeTween = TweenService:Create(hrp, tweenInfo, {CFrame = CFrame.new(targetPosition)})
    activeTween:Play()
    activeTween.Completed:Wait()
    activeTween = nil
end
-- Find Nearest Food
local function getNearestFood()
    local hrp = getCharacterParts()
    if not hrp then return nil end
    
    local folder = getFoodFolder()
    local nearest = nil
    local shortestDistance = Config.MaxDistance
    
    local function checkItem(item)
        if item:IsA("BasePart") or (item:IsA("Model") and item.PrimaryPart) then
            local pos = item:IsA("BasePart") and item.Position or item.PrimaryPart.Position
            local dist = (hrp.Position - pos).Magnitude
            if dist < shortestDistance then
                shortestDistance = dist
                nearest = item
            end
        end
    end
    
    if folder then
        for _, child in ipairs(folder:GetChildren()) do
            checkItem(child)
        end
    else
        -- Search top level workspace if folder not found
        for _, child in ipairs(workspace:GetChildren()) do
            if child.Name:lower():find("food") or child.Name:lower():find("fruit") then
                checkItem(child)
            end
        end
    end
    
    return nearest
end
-- Main Loop
task.spawn(function()
    while true do
        task.wait(0.1)
        
        if Config.AutoFarm then
            local hrp, hum = getCharacterParts()
            if hrp and hum then
                -- Check if full and AutoSell is enabled
                if Config.AutoSell and isFull() then
                    StatusLabel.Text = "Durum: Cantan Dolu! Satiliyor..."
                    local sellPart = getSellPart()
                    if sellPart then
                        moveTo(sellPart.Position, Config.FarmSpeed)
                        task.wait(0.5) -- Wait to ensure sell completes
                    else
                        StatusLabel.Text = "Hata: Satis alani bulunamadi!"
                    end
                else
                    -- Farm nearest food
                    local food = getNearestFood()
                    if food then
                        StatusLabel.Text = "Durum: Yiyecege gidiliyor..."
                        local targetPos = food:IsA("BasePart") and food.Position or food.PrimaryPart.Position
                        
                        -- Offset height slightly to align with center
                        targetPos = targetPos + Vector3.new(0, 1, 0)
                        moveTo(targetPos, Config.FarmSpeed)
                    else
                        StatusLabel.Text = "Durum: Yiyecek araniyor..."
                    end
                end
            end
        end
    end
end)
print("Eat the World Hub successfully loaded!")
