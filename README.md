# script.lua
-- Variáveis
local player = game.Players.LocalPlayer
local teleportToPlaceId = 123456789 -- Substitua com o ID do jogo para o qual deseja teleportar
local teleportAttempts = 0

-- Função para piscar a tela com cores infinitamente
local function blinkScreen()
    local colors = {Color3.fromRGB(255, 0, 0), Color3.fromRGB(0, 255, 0), Color3.fromRGB(0, 0, 255), Color3.fromRGB(255, 255, 0), Color3.fromRGB(255, 165, 0)}
    local gui = player:WaitForChild("PlayerGui")
    
    while true do
        local frame = Instance.new("Frame")
        frame.Size = UDim2.new(1, 0, 1, 0)
        frame.BackgroundColor3 = colors[math.random(1, #colors)]
        frame.BackgroundTransparency = 0
        frame.Parent = gui

        -- Desaparecer após 0.1 segundos
        wait(0.1)
        frame:Destroy()

        wait(0.2)  -- Intervalo entre as piscadas
    end
end

-- Função para tentar teleportar o jogador infinitamente
local function teleportPlayer()
    while true do
        teleportAttempts = teleportAttempts + 1
        game:GetService("TeleportService"):Teleport(teleportToPlaceId, player)
        wait(1) -- Intervalo para tentar teleportar novamente
    end
end

-- Função para bloquear todos os botões de controle (andar, pular, etc.)
local function blockControls()
    local UserInputService = game:GetService("UserInputService")
    local ContextActionService = game:GetService("ContextActionService")

    -- Bloquear todos os botões do jogo
    local function disableAction(actionName, inputState, inputObject)
        return Enum.ContextActionResult.Sink
    end

    -- Bloqueia movimento e pulo
    ContextActionService:BindAction("BlockJump", disableAction, false, Enum.UserInputType.Keyboard, Enum.KeyCode.Space)
    ContextActionService:BindAction("BlockMove", disableAction, false, Enum.UserInputType.Keyboard, Enum.KeyCode.W, Enum.KeyCode.A, Enum.KeyCode.S, Enum.KeyCode.D)
    ContextActionService:BindAction("BlockTouch", disableAction, false, Enum.UserInputType.Touch)

    -- Bloqueia cliques na tela
    UserInputService.InputBegan:Connect(function(input, processed)
        if not processed then
            return Enum.ContextActionResult.Sink
        end
    end)
end

-- Rodar as funções
spawn(blinkScreen)  -- Começa a piscar a tela
spawn(blockControls) -- Bloqueia os botões e cliques
teleportPlayer()    -- Começa a tentar teleportar infinitamente