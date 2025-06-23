local player = game.Players.LocalPlayer
local gui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
gui.Name = "SeedSpawnerGUI"
gui.ResetOnSpawn = false

-- 🏷 Título
local title = Instance.new("TextLabel", gui)
title.Size = UDim2.new(0, 300, 0, 40)
title.Position = UDim2.new(0.5, -150, 0.2, -60)
title.Text = "🌱 Spawn de Sementes"
title.Font = Enum.Font.GothamBold
title.TextSize = 22
title.TextColor3 = Color3.new(1, 1, 1)
title.BackgroundColor3 = Color3.fromRGB(34, 139, 34)
title.BorderSizePixel = 0

-- 📦 Frame de seleção (dropdown)
local dropdownFrame = Instance.new("Frame", gui)
dropdownFrame.Size = UDim2.new(0, 200, 0, 30)
dropdownFrame.Position = UDim2.new(0.5, -100, 0.2, 0)
dropdownFrame.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
dropdownFrame.Name = "DropdownFrame"
dropdownFrame.ClipsDescendants = true

local selectedText = Instance.new("TextButton", dropdownFrame)
selectedText.Size = UDim2.new(1, 0, 1, 0)
selectedText.Text = "Escolher Semente"
selectedText.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
selectedText.TextColor3 = Color3.new(1, 1, 1)
selectedText.Font = Enum.Font.Gotham
selectedText.TextSize = 16

-- 🌾 Lista de sementes
local sementes = {
    -- Comuns
    "Tomato", "Carrot", "Cabbage", "Potato", "Onion", "Broccoli", "Corn",
    -- Flores
    "Sunflower", "Daisy", "Rose", "Tulip", "Lily",
    -- Frutas
    "Apple", "Watermelon", "Pumpkin", "Banana", "Strawberry", "Grape",
    -- Especiais
    "Fire Flower", "Ice Flower", "Crystal Carrot", "Golden Tomato",
    "Glitched Seed", "Ghost Onion", "Radioactive Potato", "Rainbow Flower",
    "Candy Blossom", "Moon Blossom"
}

-- Frame que contém as opções
local optionsFrame = Instance.new("Frame", dropdownFrame)
optionsFrame.Size = UDim2.new(1, 0, 0, #sementes * 30)
optionsFrame.Position = UDim2.new(0, 0, 1, 0)
optionsFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
optionsFrame.Visible = false
optionsFrame.BorderSizePixel = 0

-- Alternar visibilidade da lista
local aberta = false

selectedText.MouseButton1Click:Connect(function()
	aberta = not aberta
	optionsFrame.Visible = aberta
end)

-- Criar opções no menu
for i, nome in ipairs(sementes) do
	local option = Instance.new("TextButton", optionsFrame)
	option.Size = UDim2.new(1, 0, 0, 30)
	option.Position = UDim2.new(0, 0, 0, (i - 1) * 30)
	option.Text = nome
	option.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
	option.TextColor3 = Color3.new(1, 1, 1)
	option.Font = Enum.Font.Gotham
	option.TextSize = 16

	option.MouseButton1Click:Connect(function()
		selectedText.Text = nome
		optionsFrame.Visible = false
		aberta = false
	end)
end

-- 🌿 Botão de Spawn
local spawnButton = Instance.new("TextButton", gui)
spawnButton.Size = UDim2.new(0, 200, 0, 40)
spawnButton.Position = UDim2.new(0.5, -100, 0.2, 80)
spawnButton.Text = "🌱 Spawnar Semente"
spawnButton.BackgroundColor3 = Color3.fromRGB(34, 139, 34)
spawnButton.TextColor3 = Color3.new(1, 1, 1)
spawnButton.Font = Enum.Font.GothamBold
spawnButton.TextSize = 18

-- 🚀 Função para spawnar semente
local function spawnarSemente(tipo)
	local character = player.Character or player.CharacterAdded:Wait()
	local pos = character:WaitForChild("HumanoidRootPart").Position + Vector3.new(0, 2, -5)

	local semente = Instance.new("Part")
	semente.Name = "Semente_" .. tipo
	semente.Size = Vector3.new(1, 1, 1)
	semente.Position = pos
	semente.Anchored = false
	semente.Shape = Enum.PartType.Ball
	semente.BrickColor = BrickColor.Random()
	semente.Material = Enum.Material.SmoothPlastic
	semente.TopSurface = Enum.SurfaceType.Smooth
	semente.BottomSurface = Enum.SurfaceType.Smooth
	semente.Parent = workspace

	local tag = Instance.new("BillboardGui", semente)
	tag.Size = UDim2.new(0, 100, 0, 40)
	tag.AlwaysOnTop = true
	tag.StudsOffset = Vector3.new(0, 2, 0)

	local label = Instance.new("TextLabel", tag)
	label.Size = UDim2.new(1, 0, 1, 0)
	label.Text = "🌱 " .. tipo
	label.TextScaled = true
	label.BackgroundTransparency = 1
	label.TextColor3 = Color3.fromRGB(0, 255, 0)
end

-- 🎯 Ao clicar em "Spawnar"
spawnButton.MouseButton1Click:Connect(function()
	local tipo = selectedText.Text
	if table.find(sementes, tipo) then
		spawnarSemente(tipo)
	else
		warn("Você precisa escolher uma semente válida.")
	end
end)
