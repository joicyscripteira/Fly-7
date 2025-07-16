local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")
local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local hrp = character:WaitForChild("HumanoidRootPart")
local spawnLocation = Workspace:FindFirstChild("SpawnLocation") or Workspace:FindFirstChild("Spawn") -- ajuste se o nome for diferente

local flyButton = Instance.new("TextButton")
flyButton.Size = UDim2.new(0, 100, 0, 50)
flyButton.Position = UDim2.new(1, -110, 0.5, -25)
flyButton.Text = "Fly to Spawn"
flyButton.BackgroundColor3 = Color3.fromRGB(30, 144, 255)
flyButton.TextColor3 = Color3.new(1, 1, 1)
flyButton.Parent = game:GetService("Players").LocalPlayer:WaitForChild("PlayerGui"):WaitForChild("ScreenGui") or Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))

local flying = false
local flightConnection

local function startFlying()
	if not spawnLocation then
		warn("Spawn não encontrado!")
		return
	end

	if flying then return end
	flying = true

	local goalPos = spawnLocation.Position + Vector3.new(0, 60, 0)

	local direction = (goalPos - hrp.Position).Unit
	local speed = 60
	local distance = (goalPos - hrp.Position).Magnitude
	local duration = distance / speed

	local tweenInfo = TweenInfo.new(duration, Enum.EasingStyle.Linear)
	local tween = TweenService:Create(hrp, tweenInfo, {CFrame = CFrame.new(goalPos)})
	tween:Play()

	tween.Completed:Connect(function()
		flying = false
	end)
end

flyButton.MouseButton1Click:Connect(startFlying)
