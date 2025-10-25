-- Premium Hub Script
local Players = game:GetService("Players")
local TeleportService = game:GetService("TeleportService")
local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- Criar ScreenGui
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "PremiumHub"
screenGui.ResetOnSpawn = false
screenGui.Parent = playerGui

-- Frame principal
local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.Size = UDim2.new(0, 400, 0, 300)
mainFrame.Position = UDim2.new(0.5, -200, 0.5, -150)
mainFrame.BackgroundColor3 = Color3.fromRGB(35, 35, 45)
mainFrame.BorderSizePixel = 0
mainFrame.Parent = screenGui

-- Adicionar corner arredondado
local mainCorner = Instance.new("UICorner")
mainCorner.CornerRadius = UDim.new(0, 10)
mainCorner.Parent = mainFrame

-- Título
local titleLabel = Instance.new("TextLabel")
titleLabel.Name = "Title"
titleLabel.Size = UDim2.new(1, 0, 0, 50)
titleLabel.Position = UDim2.new(0, 0, 0, 0)
titleLabel.BackgroundColor3 = Color3.fromRGB(25, 25, 35)
titleLabel.BorderSizePixel = 0
titleLabel.Text = "PREMIUM HUB"
titleLabel.TextColor3 = Color3.fromRGB(255, 215, 0)
titleLabel.TextSize = 24
titleLabel.Font = Enum.Font.GothamBold
titleLabel.Parent = mainFrame

-- Corner do título
local titleCorner = Instance.new("UICorner")
titleCorner.CornerRadius = UDim.new(0, 10)
titleCorner.Parent = titleLabel

-- Botão de fechar
local closeButton = Instance.new("TextButton")
closeButton.Name = "CloseButton"
closeButton.Size = UDim2.new(0, 30, 0, 30)
closeButton.Position = UDim2.new(1, -40, 0, 10)
closeButton.BackgroundColor3 = Color3.fromRGB(220, 50, 50)
closeButton.Text = "X"
closeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
closeButton.TextSize = 18
closeButton.Font = Enum.Font.GothamBold
closeButton.Parent = mainFrame

local closeCorner = Instance.new("UICorner")
closeCorner.CornerRadius = UDim.new(0, 5)
closeCorner.Parent = closeButton

-- Container de abas
local tabContainer = Instance.new("Frame")
tabContainer.Name = "TabContainer"
tabContainer.Size = UDim2.new(1, -20, 1, -70)
tabContainer.Position = UDim2.new(0, 10, 0, 60)
tabContainer.BackgroundTransparency = 1
tabContainer.Parent = mainFrame

-- Botão da aba Private
local privateTab = Instance.new("TextButton")
privateTab.Name = "PrivateTab"
privateTab.Size = UDim2.new(0, 360, 0, 50)
privateTab.Position = UDim2.new(0, 10, 0, 10)
privateTab.BackgroundColor3 = Color3.fromRGB(50, 50, 65)
privateTab.Text = "🔒 PRIVATE SERVER"
privateTab.TextColor3 = Color3.fromRGB(255, 255, 255)
privateTab.TextSize = 20
privateTab.Font = Enum.Font.GothamBold
privateTab.Parent = tabContainer

local privateCorner = Instance.new("UICorner")
privateCorner.CornerRadius = UDim.new(0, 8)
privateCorner.Parent = privateTab

-- Label de informação
local infoLabel = Instance.new("TextLabel")
infoLabel.Name = "InfoLabel"
infoLabel.Size = UDim2.new(1, -20, 0, 100)
infoLabel.Position = UDim2.new(0, 10, 0, 70)
infoLabel.BackgroundTransparency = 1
infoLabel.Text = "Clique no botão acima para\nentrar em um servidor privado\nonde apenas você pode entrar!"
infoLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
infoLabel.TextSize = 16
infoLabel.Font = Enum.Font.Gotham
infoLabel.TextWrapped = true
infoLabel.TextYAlignment = Enum.TextYAlignment.Top
infoLabel.Parent = tabContainer

-- Função de arrastar
local dragging
local dragInput
local dragStart
local startPos

local function update(input)
    local delta = input.Position - dragStart
    mainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
end

titleLabel.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragging = true
        dragStart = input.Position
        startPos = mainFrame.Position
        
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)

titleLabel.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
        dragInput = input
    end
end)

game:GetService("UserInputService").InputChanged:Connect(function(input)
    if input == dragInput and dragging then
        update(input)
    end
end)

-- Função do botão fechar
closeButton.MouseButton1Click:Connect(function()
    screenGui:Destroy()
end)

-- Função do servidor privado
privateTab.MouseButton1Click:Connect(function()
    privateTab.Text = "⏳ TELEPORTANDO..."
    privateTab.BackgroundColor3 = Color3.fromRGB(70, 70, 85)
    
    local success, err = pcall(function()
        local placeId = game.PlaceId
        local reservedCode = TeleportService:ReserveServer(placeId)
        
        TeleportService:TeleportToPrivateServer(placeId, reservedCode, {player})
    end)
    
    if not success then
        privateTab.Text = "❌ ERRO AO TELEPORTAR"
        privateTab.BackgroundColor3 = Color3.fromRGB(180, 50, 50)
        wait(2)
        privateTab.Text = "🔒 PRIVATE SERVER"
        privateTab.BackgroundColor3 = Color3.fromRGB(50, 50, 65)
        
        warn("Erro ao criar servidor privado: " .. tostring(err))
    end
end)

-- Efeitos hover
privateTab.MouseEnter:Connect(function()
    privateTab.BackgroundColor3 = Color3.fromRGB(70, 70, 85)
end)

privateTab.MouseLeave:Connect(function()
    if privateTab.Text == "🔒 PRIVATE SERVER" then
        privateTab.BackgroundColor3 = Color3.fromRGB(50, 50, 65)
    end
end)

closeButton.MouseEnter:Connect(function()
    closeButton.BackgroundColor3 = Color3.fromRGB(255, 70, 70)
end)

closeButton.MouseLeave:Connect(function()
    closeButton.BackgroundColor3 = Color3.fromRGB(220, 50, 50)
end)

print("Premium Hub carregado com sucesso!")
