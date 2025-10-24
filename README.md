-- Anti-Lag Hub - Script Local para Roblox
local Players = game:GetService("Players")
local TeleportService = game:GetService("TeleportService")
local UserInputService = game:GetService("UserInputService")
local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- Criar ScreenGui
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "AntiLagHub"
screenGui.ResetOnSpawn = false
screenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
screenGui.Parent = playerGui

-- Frame Principal (Móvel)
local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.Size = UDim2.new(0, 300, 0, 200)
mainFrame.Position = UDim2.new(0.5, -150, 0.5, -100)
mainFrame.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
mainFrame.BorderSizePixel = 0
mainFrame.Active = true
mainFrame.Draggable = true
mainFrame.Parent = screenGui

-- Adicionar cantos arredondados
local mainCorner = Instance.new("UICorner")
mainCorner.CornerRadius = UDim.new(0, 10)
mainCorner.Parent = mainFrame

-- Barra de Título
local titleBar = Instance.new("Frame")
titleBar.Name = "TitleBar"
titleBar.Size = UDim2.new(1, 0, 0, 40)
titleBar.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
titleBar.BorderSizePixel = 0
titleBar.Parent = mainFrame

local titleCorner = Instance.new("UICorner")
titleCorner.CornerRadius = UDim.new(0, 10)
titleCorner.Parent = titleBar

-- Texto do Título
local titleLabel = Instance.new("TextLabel")
titleLabel.Name = "Title"
titleLabel.Size = UDim2.new(1, -50, 1, 0)
titleLabel.Position = UDim2.new(0, 10, 0, 0)
titleLabel.BackgroundTransparency = 1
titleLabel.Text = "Anti-Lag Hub"
titleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
titleLabel.TextSize = 18
titleLabel.Font = Enum.Font.GothamBold
titleLabel.TextXAlignment = Enum.TextXAlignment.Left
titleLabel.Parent = titleBar

-- Botão Fechar
local closeButton = Instance.new("TextButton")
closeButton.Name = "CloseButton"
closeButton.Size = UDim2.new(0, 30, 0, 30)
closeButton.Position = UDim2.new(1, -35, 0, 5)
closeButton.BackgroundColor3 = Color3.fromRGB(255, 50, 50)
closeButton.BorderSizePixel = 0
closeButton.Text = "X"
closeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
closeButton.TextSize = 16
closeButton.Font = Enum.Font.GothamBold
closeButton.Parent = titleBar

local closeCorner = Instance.new("UICorner")
closeCorner.CornerRadius = UDim.new(0, 6)
closeCorner.Parent = closeButton

closeButton.MouseButton1Click:Connect(function()
    screenGui:Destroy()
end)

-- Container de Conteúdo
local contentFrame = Instance.new("Frame")
contentFrame.Name = "Content"
contentFrame.Size = UDim2.new(1, -20, 1, -60)
contentFrame.Position = UDim2.new(0, 10, 0, 50)
contentFrame.BackgroundTransparency = 1
contentFrame.Parent = mainFrame

-- Botão Anti-Lag
local antiButton = Instance.new("TextButton")
antiButton.Name = "AntiButton"
antiButton.Size = UDim2.new(1, 0, 0, 50)
antiButton.Position = UDim2.new(0, 0, 0, 20)
antiButton.BackgroundColor3 = Color3.fromRGB(50, 150, 255)
antiButton.BorderSizePixel = 0
antiButton.Text = "Anti"
antiButton.TextColor3 = Color3.fromRGB(255, 255, 255)
antiButton.TextSize = 20
antiButton.Font = Enum.Font.GothamBold
antiButton.Parent = contentFrame

local antiCorner = Instance.new("UICorner")
antiCorner.CornerRadius = UDim.new(0, 8)
antiCorner.Parent = antiButton

-- Efeito hover no botão
antiButton.MouseEnter:Connect(function()
    antiButton.BackgroundColor3 = Color3.fromRGB(70, 170, 255)
end)

antiButton.MouseLeave:Connect(function()
    antiButton.BackgroundColor3 = Color3.fromRGB(50, 150, 255)
end)

-- Função para reiniciar o jogo
antiButton.MouseButton1Click:Connect(function()
    antiButton.Text = "Reiniciando..."
    antiButton.BackgroundColor3 = Color3.fromRGB(150, 150, 150)
    wait(0.5)
    
    -- Teleporta o jogador de volta para o mesmo jogo
    local placeId = game.PlaceId
    TeleportService:Teleport(placeId, player)
end)

-- Texto informativo
local infoLabel = Instance.new("TextLabel")
infoLabel.Name = "Info"
infoLabel.Size = UDim2.new(1, 0, 0, 40)
infoLabel.Position = UDim2.new(0, 0, 1, -50)
infoLabel.BackgroundTransparency = 1
infoLabel.Text = "Clique em 'Anti' para reiniciar o jogo"
infoLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
infoLabel.TextSize = 12
infoLabel.Font = Enum.Font.Gotham
infoLabel.TextWrapped = true
infoLabel.Parent = contentFrame

print("Anti-Lag Hub carregado com sucesso!")
