-- LocalScript (coloque dentro do StarterPlayerScripts)

local UIS = game:GetService("UserInputService")
local player = game.Players.LocalPlayer
local char = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = char:WaitForChild("HumanoidRootPart")

local flying = false
local speed = 50
local bodyGyro
local bodyVelocity

-- Função para ativar/desativar o fly
local function toggleFly()
	flying = not flying

	if flying then
		-- Criar BodyGyro e BodyVelocity
		bodyGyro = Instance.new("BodyGyro")
		bodyGyro.P = 9e4
		bodyGyro.maxTorque = Vector3.new(9e9, 9e9, 9e9)
		bodyGyro.cframe = humanoidRootPart.CFrame
		bodyGyro.Parent = humanoidRootPart

		bodyVelocity = Instance.new("BodyVelocity")
		bodyVelocity.velocity = Vector3.new(0, 0, 0)
		bodyVelocity.maxForce = Vector3.new(9e9, 9e9, 9e9)
		bodyVelocity.Parent = humanoidRootPart

		-- Loop de movimento
		while flying and player.Character and humanoidRootPart do
			task.wait()
			bodyGyro.CFrame = workspace.CurrentCamera.CFrame
			bodyVelocity.Velocity = workspace.CurrentCamera.CFrame.LookVector * speed
		end
	else
		-- Remover componentes
		if bodyGyro then bodyGyro:Destroy() end
		if bodyVelocity then bodyVelocity:Destroy() end
	end
end

-- Detectar tecla
UIS.InputBegan:Connect(function(input, gameProcessed)
	if gameProcessed then return end
	if input.KeyCode == Enum.KeyCode.F then
		toggleFly()
	end
end)
