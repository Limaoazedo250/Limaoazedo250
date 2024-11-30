
local Players = game:GetService("Players")
local Workspace = game:GetService("Workspace")
local RunService = game:GetService("RunService")

-- Função para criar o ESP
local function criarESP(jogador)
    -- Acessa a parte do personagem do jogador (geralmente a parte "HumanoidRootPart")
    local character = jogador.Character or jogador.CharacterAdded:Wait()
    local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

    -- Criar uma caixa (BillboardGui) acima do jogador
    local espGui = Instance.new("BillboardGui")
    espGui.Adornee = humanoidRootPart
    espGui.Size = UDim2.new(0, 200, 0, 50)  -- Tamanho da caixa
    espGui.StudsOffset = Vector3.new(0, 3, 0)  -- Posição da caixa (acima do jogador)
    espGui.Parent = character

    -- Caixa de texto (TextLabel) dentro do BillboardGui
    local espTexto = Instance.new("TextLabel")
    espTexto.Size = UDim2.new(1, 0, 1, 0)
    espTexto.BackgroundTransparency = 1
    espTexto.TextColor3 = Color3.fromRGB(255, 0, 0)  -- Cor do texto (vermelho)
    espTexto.TextStrokeTransparency = 0.5
    espTexto.TextSize = 16
    espTexto.Text = jogador.Name
    espTexto.Parent = espGui
end

-- Função para remover o ESP quando o jogador sair
local function removerESP(jogador)
    if jogador.Character then
        for _, gui in pairs(jogador.Character:GetChildren()) do
            if gui:IsA("BillboardGui") then
                gui:Destroy()
            end
        end
    end
end

-- Criação do ESP para jogadores que já estão no jogo
for _, jogador in pairs(Players:GetPlayers()) do
    jogador.CharacterAdded:Connect(function()
        criarESP(jogador)
    end)
    -- Cria o ESP imediatamente se o jogador já estiver no jogo
    if jogador.Character then
        criarESP(jogador)
    end
end

-- Atualiza a criação do ESP para novos jogadores
Players.PlayerAdded:Connect(function(jogador)
    jogador.CharacterAdded:Connect(function()
        criarESP(jogador)
    end)
end)

-- Remove o ESP quando o jogador sair
Players.PlayerRemoving:Connect(function(jogador)
    removerESP(jogador)
end)

-- Atualiza a posição do ESP se o jogador se mover
RunService.Heartbeat:Connect(function()
    for _, jogador in pairs(Players:GetPlayers()) do
        if jogador.Character then
            local humanoidRootPart = jogador.Character:FindFirstChild("HumanoidRootPart")
            if humanoidRootPart then
                local espGui = humanoidRootPart:FindFirstChildOfClass("BillboardGui")
                if espGui then
                    espGui.Adornee = humanoidRootPart  -- Garante que o ESP se mova com o jogador
                end
            end
        end
    end
end)
