local Player = game.Players.LocalPlayer
local Mouse = Player:GetMouse()
local ScreenGui = Instance.new("ScreenGui")
local MainFrame = Instance.new("Frame")
local EspButton = Instance.new("TextButton")
local EspLineButton = Instance.new("TextButton")
local EspBoxButton = Instance.new("TextButton")
local ESPActive = false

-- Configurações do GUI
ScreenGui.Parent = Player:WaitForChild("PlayerGui")

MainFrame.Size = UDim2.new(0, 200, 0, 200)
MainFrame.Position = UDim2.new(0.5, -100, 0.5, -100)
MainFrame.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
MainFrame.Visible = false
MainFrame.Parent = ScreenGui

EspButton.Size = UDim2.new(1, 0, 0, 50)
EspButton.Position = UDim2.new(0, 0, 0, 0)
EspButton.BackgroundColor3 = Color3.new(0.2, 0.5, 0.8)
EspButton.Text = "Toggle ESP"
EspButton.Parent = MainFrame

EspLineButton.Size = UDim2.new(1, 0, 0, 50)
EspLineButton.Position = UDim2.new(0, 0, 0, 60)
EspLineButton.BackgroundColor3 = Color3.new(0.2, 0.8, 0.2)
EspLineButton.Text = "Ativar Linha ESP"
EspLineButton.Parent = MainFrame

EspBoxButton.Size = UDim2.new(1, 0, 0, 50)
EspBoxButton.Position = UDim2.new(0, 0, 0, 120)
EspBoxButton.BackgroundColor3 = Color3.new(0.8, 0.2, 0.2)
EspBoxButton.Text = "Ativar Caixa ESP"
EspBoxButton.Parent = MainFrame

-- Função para criar ESP
function createESP(player, color, type)
    local adornment
    if type == "Box" then
        adornment = Instance.new("BoxHandleAdornment")
        adornment.Size = Vector3.new(4, 6, 4)
    else
        adornment = Instance.new("LineHandleAdornment")
        adornment.Length = 10
    end
    adornment.Color3 = color
    adornment.AlwaysOnTop = true
    adornment.ZIndex = 10
    adornment.Adornee = player.Character:WaitForChild("HumanoidRootPart")
    adornment.Parent = player.Character
end

-- Função para ativar/desativar ESP
function toggleESP()
    ESPActive = not ESPActive
    for _, player in ipairs(game.Players:GetPlayers()) do
        if player ~= Player then
            if ESPActive then
                if player.Team.Name == "Inimigos" then
                    createESP(player, Color3.fromRGB(255, 0, 0), "Box") -- Vermelho para inimigos
                elseif player.Team.Name == "Aliados" then
                    createESP(player, Color3.fromRGB(255, 255, 255), "Box") -- Branco para aliados
                end
            else
                for _, adornment in ipairs(player.Character:GetChildren()) do
                    if adornment:IsA("BoxHandleAdornment") or adornment:IsA("LineHandleAdornment") then
                        adornment:Destroy()
                    end
                end
            end
        end
    end
end

-- Conexões dos botões
EspButton.MouseButton1Click:Connect(function()
    MainFrame.Visible = not MainFrame.Visible
end)

EspLineButton.MouseButton1Click:Connect(function()
    for _, player in ipairs(game.Players:GetPlayers()) do
        if player ~= Player and player.Character then
            createESP(player, Color3.fromRGB(255, 0, 0), "Line") -- Vermelho para inimigos
        end
    end
end)

EspBoxButton.MouseButton1Click:Connect(function()
    toggleESP()
end)

-- Atualiza ESP para jogadores existentes
for _, player in ipairs(game.Players:GetPlayers()) do
    if player.Character then
        if player.Team.Name == "Inimigos" then
            createESP(player, Color3.fromRGB(255, 0, 0), "Box")
        elseif player.Team.Name == "Aliados" then
            createESP(player, Color3.fromRGB(255, 255, 255), "Box")
        end
    end
end
