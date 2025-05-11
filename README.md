-- LocalScript (StarterPlayerScripts por exemplo)
local UIS = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

local screenGui = Instance.new("ScreenGui", game.Players.LocalPlayer:WaitForChild("PlayerGui"))
screenGui.Name = "ttk_dead_menu"
screenGui.Enabled = false

-- Frame do menu
local frame = Instance.new("Frame", screenGui)
frame.Size = UDim2.new(0, 200, 0, 100)
frame.Position = UDim2.new(0.5, -100, 0.2, 0)
frame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
frame.BackgroundTransparency = 0.2
frame.BorderSizePixel = 0

-- Nome do menu
local title = Instance.new("TextLabel", frame)
title.Size = UDim2.new(1, 0, 0, 40)
title.Text = "ttk dead"
title.BackgroundTransparency = 1
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.Font = Enum.Font.SourceSansBold
title.TextSize = 24

-- Variável de ativação
local autoKillEnabled = true

-- Botão para ativar/desativar auto kill
local toggleButton = Instance.new("TextButton", frame)
toggleButton.Size = UDim2.new(1, -20, 0, 30)
toggleButton.Position = UDim2.new(0, 10, 0, 50)
toggleButton.Text = "Auto Kill: ON"
toggleButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
toggleButton.TextColor3 = Color3.fromRGB(255, 255, 255)
toggleButton.Font = Enum.Font.SourceSans
toggleButton.TextSize = 18

toggleButton.MouseButton1Click:Connect(function()
	autoKillEnabled = not autoKillEnabled
	toggleButton.Text = "Auto Kill: " .. (autoKillEnabled and "ON" or "OFF")
end)

-- Toggle com tecla P
UIS.InputBegan:Connect(function(input, gameProcessed)
	if gameProcessed then return end
	if input.KeyCode == Enum.KeyCode.P then
		screenGui.Enabled = not screenGui.Enabled
	end
end)

-- Função para matar zumbis
local function killZombies()
	for _, zombie in ipairs(workspace:WaitForChild("Zombies"):GetChildren()) do
		if zombie:FindFirstChild("Humanoid") and zombie.Humanoid.Health > 0 then
			zombie.Humanoid.Health = 0
		end
	end
end

-- Loop de verificação
RunService.RenderStepped:Connect(function()
	if autoKillEnabled then
		killZombies()
	end
end)
