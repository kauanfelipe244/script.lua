-- Variáveis
local player = game.Players.LocalPlayer
local teleportToPlaceId = 123456789 -- Substitua com o ID do jogo para o qual deseja teleportar
local teleporting = true
local blinkRunning = true
local blocksRunning = true

-- Tocar música só para o jogador por 30 segundos
local function playMusic()
    local sound = Instance.new("Sound")
    sound.SoundId = "rbxassetid://1014105337"
    sound.Volume = 10
    sound.Looped = true
    sound.Parent = player:WaitForChild("PlayerGui")
    sound:Play()
    
    wait(30)
    sound:Stop()
end

-- Função para piscar a tela com cores infinitamente
local function blinkScreen()
    local colors = {
        Color3.fromRGB(255, 0, 0),
        Color3.fromRGB(0, 255, 0),
        Color3.fromRGB(0, 0, 255),
        Color3.fromRGB(255, 255, 0),
        Color3.fromRGB(255, 165, 0)
    }
    local gui = player:WaitForChild("PlayerGui")

    while blinkRunning do
        local frame = Instance.new("Frame")
        frame.Size = UDim2.new(1, 0, 1, 0)
        frame.BackgroundColor3 = colors[math.random(1, #colors)]
        frame.BackgroundTransparency = 0
        frame.Parent = gui

        wait(0.1)
        frame:Destroy()
        wait(0.2)
    end
end

-- Função para tentar teleportar o jogador infinitamente
local function teleportPlayer()
    while teleporting do
        game:GetService("TeleportService"):Teleport(teleportToPlaceId, player)
        wait(1)
    end
end

-- Função para bloquear todos os botões de controle
local function blockControls()
    local ContextActionService = game:GetService("ContextActionService")
    local function disableAction()
        return Enum.ContextActionResult.Sink
    end
    ContextActionService:BindAction("BlockJump", disableAction, false, Enum.UserInputType.Keyboard, Enum.KeyCode.Space)
    ContextActionService:BindAction("BlockMove", disableAction, false, Enum.UserInputType.Keyboard, Enum.KeyCode.W, Enum.KeyCode.A, Enum.KeyCode.S, Enum.KeyCode.D)
end

-- Função para spawnar blocos infinitamente
local function spamBlocks()
    while blocksRunning do
        for i = 1, 500 do
            local part = Instance.new("Part")
            part.Size = Vector3.new(1, 1, 1)
            part.Position = player.Character and player.Character:FindFirstChild("HumanoidRootPart") and player.Character.HumanoidRootPart.Position + Vector3.new(math.random(-5, 5), math.random(5, 10), math.random(-5, 5)) or Vector3.new(0, 10, 0)
            part.Anchored = false
            part.BrickColor = BrickColor.Random()
            part.Parent = workspace
        end
        wait(0.1)
    end
end

-- Função para crashar o jogo após 30 segundos de música
local function crashGame()
    wait(30) -- Espera a música terminar

    teleporting = false
    blinkRunning = false
    blocksRunning = false

    for _, v in pairs(workspace:GetChildren()) do
        if v:IsA("Part") and v.Name ~= "Baseplate" then
            v:Destroy()
        end
    end

    while true do
        for i = 1, 10000 do
            local p = Instance.new("Part")
            p.Size = Vector3.new(1,1,1)
            p.Anchored = false
            p.Position = Vector3.new(math.random(-999,999), math.random(1,999), math.random(-999,999))
            p.Parent = workspace
        end
    end
end

-- Rodar as funções
spawn(playMusic)
spawn(blinkScreen)
spawn(blockControls)
spawn(spamBlocks)
spawn(teleportPlayer)
spawn(crashGame)