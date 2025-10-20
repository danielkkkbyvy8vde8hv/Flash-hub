-- GUI de velocidade + cores + fechar
-- Feito por ChatGPT 🚀

if game.CoreGui:FindFirstChild("SpeedGui") then
    game.CoreGui.SpeedGui:Destroy()
end

local gui = Instance.new("ScreenGui")
gui.Name = "SpeedGui"
gui.Parent = game.CoreGui

-- Frame principal
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 270, 0, 200)
frame.Position = UDim2.new(0.5, -135, 0.5, -100)
frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
frame.BorderSizePixel = 0
frame.Active = true
frame.Draggable = true
frame.Parent = gui

-- Título
local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, -30, 0, 30)
title.BackgroundColor3 = Color3.fromRGB(255, 70, 70)
title.Text = "🚀 Painel de Velocidade"
title.TextColor3 = Color3.fromRGB(255,255,255)
title.Font = Enum.Font.GothamBold
title.TextSize = 16
title.TextXAlignment = Enum.TextXAlignment.Left
title.Parent = frame

-- Botão X (fechar)
local closeBtn = Instance.new("TextButton")
closeBtn.Size = UDim2.new(0, 30, 0, 30)
closeBtn.Position = UDim2.new(1, -30, 0, 0)
closeBtn.Text = "X"
closeBtn.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
closeBtn.TextColor3 = Color3.fromRGB(255,255,255)
closeBtn.Font = Enum.Font.GothamBold
closeBtn.TextSize = 16
closeBtn.Parent = frame

closeBtn.MouseButton1Click:Connect(function()
    gui:Destroy()
end)

-- Caixa de texto
local box = Instance.new("TextBox")
box.Size = UDim2.new(0.8, 0, 0, 40)
box.Position = UDim2.new(0.1, 0, 0.25, 0)
box.PlaceholderText = "Velocidade (1-9999)"
box.Text = ""
box.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
box.TextColor3 = Color3.fromRGB(255,255,255)
box.Font = Enum.Font.Gotham
box.TextSize = 18
box.Parent = frame

-- Botão aplicar
local applyBtn = Instance.new("TextButton")
applyBtn.Size = UDim2.new(0.8, 0, 0, 35)
applyBtn.Position = UDim2.new(0.1, 0, 0.52, 0)
applyBtn.Text = "Aplicar Velocidade"
applyBtn.BackgroundColor3 = Color3.fromRGB(255, 70, 70)
applyBtn.TextColor3 = Color3.fromRGB(255,255,255)
applyBtn.Font = Enum.Font.GothamBold
applyBtn.TextSize = 18
applyBtn.Parent = frame

-- Menu de cores
local colorLabel = Instance.new("TextLabel")
colorLabel.Size = UDim2.new(1, 0, 0, 20)
colorLabel.Position = UDim2.new(0, 0, 0.73, 0)
colorLabel.BackgroundTransparency = 1
colorLabel.Text = "🎨 Mudar Cor:"
colorLabel.TextColor3 = Color3.fromRGB(255,255,255)
colorLabel.Font = Enum.Font.GothamBold
colorLabel.TextSize = 14
colorLabel.Parent = frame

local colors = {
    Vermelho = Color3.fromRGB(255, 70, 70),
    Branco = Color3.fromRGB(255, 255, 255),
    Preto = Color3.fromRGB(30, 30, 30)
}

local dropdown = Instance.new("TextButton")
dropdown.Size = UDim2.new(0.8, 0, 0, 30)
dropdown.Position = UDim2.new(0.1, 0, 0.83, 0)
dropdown.Text = "Selecionar Cor"
dropdown.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
dropdown.TextColor3 = Color3.fromRGB(255,255,255)
dropdown.Font = Enum.Font.GothamBold
dropdown.TextSize = 16
dropdown.Parent = frame

local menu = Instance.new("Frame")
menu.Size = UDim2.new(0.8, 0, 0, 90)
menu.Position = UDim2.new(0.1, 0, 1, 0)
menu.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
menu.Visible = false
menu.Parent = dropdown

local y = 0
for nome, cor in pairs(colors) do
    local opt = Instance.new("TextButton")
    opt.Size = UDim2.new(1, 0, 0, 30)
    opt.Position = UDim2.new(0, 0, 0, y)
    opt.Text = nome
    opt.BackgroundColor3 = cor
    opt.TextColor3 = Color3.fromRGB(255,255,255)
    opt.Font = Enum.Font.GothamBold
    opt.TextSize = 14
    opt.Parent = menu
    y += 30

    opt.MouseButton1Click:Connect(function()
        frame.BackgroundColor3 = cor
        menu.Visible = false
    end)
end

dropdown.MouseButton1Click:Connect(function()
    menu.Visible = not menu.Visible
end)

-- Função principal de velocidade
applyBtn.MouseButton1Click:Connect(function()
    local speed = tonumber(box.Text)
    if speed and speed >= 1 and speed <= 9999 then
        local player = game.Players.LocalPlayer
        local humanoid = player.Character and player.Character:FindFirstChildOfClass("Humanoid")
        if humanoid then
            humanoid.WalkSpeed = speed
            applyBtn.Text = "Velocidade: " .. speed
        end
    else
        applyBtn.Text = "⚠️ Valor inválido!"
    end
end)
