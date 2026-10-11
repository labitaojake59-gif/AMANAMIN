local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

local function new(class, props)
	local obj = Instance.new(class)
	if props then
		for k,v in pairs(props) do
			obj[k] = v
		end
	end
	return obj
end

local clickSound = Instance.new("Sound")
clickSound.SoundId = "rbxassetid://315416964"
clickSound.Volume = 0.5
clickSound.Parent = playerGui

local currentTheme = "WHITE"
local themes = {
	WHITE = {primary = Color3.fromRGB(0,0,0), accent = Color3.fromRGB(200,200,200)},
	RED   = {primary = Color3.fromRGB(0,0,0), accent = Color3.fromRGB(255,50,50)},
	BLUE  = {primary = Color3.fromRGB(0,0,0), accent = Color3.fromRGB(50,150,255)},
	GRAY  = {primary = Color3.fromRGB(0,0,0), accent = Color3.fromRGB(200,200,200)},
	PINK  = {primary = Color3.fromRGB(0,0,0), accent = Color3.fromRGB(255,100,200)},
}

local screenGui = new("ScreenGui", {
	Name = "MONSY_GUI",
	ResetOnSpawn = false,
	Parent = playerGui,
	DisplayOrder = 1000,
})

local main = new("Frame", {
	Name = "MainFrame",
	Size = UDim2.new(0, 860, 0, 520),
	Position = UDim2.new(0.5, -430, 0.5, -260),
	BackgroundColor3 = Color3.fromRGB(255,255,255),
	BorderSizePixel = 0,
	Parent = screenGui,
})
main.ClipsDescendants = true
new("UICorner", {CornerRadius = UDim.new(0,12), Parent = main})

-- Make draggable
local dragging, dragInput, dragStart, startPos
local function update(input)
	local delta = input.Position - dragStart
	main.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X,
		startPos.Y.Scale, startPos.Y.Offset + delta.Y)
end

main.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
		dragging = true
		dragStart = input.Position
		startPos = main.Position
		input.Changed:Connect(function()
			if input.UserInputState == Enum.UserInputState.End then
				dragging = false
			end
		end)
	end
end)

main.InputChanged:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
		dragInput = input
	end
end)

UserInputService.InputChanged:Connect(function(input)
	if input == dragInput and dragging then
		update(input)
	end
end)

-- Left Panel
local leftPanel = new("Frame", {
	Name = "LeftPanel",
	Size = UDim2.new(0, 220, 1, 0),
	BackgroundColor3 = Color3.fromRGB(255,255,255),
	BorderSizePixel = 0,
	Parent = main,
})
new("UICorner", {CornerRadius = UDim.new(0,12), Parent = leftPanel})

local divider = new("Frame", {
	Parent = leftPanel,
	Size = UDim2.new(0, 1, 1, -24),
	Position = UDim2.new(1, -1, 0, 12),
	BackgroundColor3 = Color3.fromRGB(255,255,255),
	BorderSizePixel = 0,
