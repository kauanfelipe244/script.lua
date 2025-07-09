local Players = game:GetService("Players")
local player = Players.LocalPlayer
local Backpack = player:WaitForChild("Backpack")

-- Função para criar uma ferramenta personalizada
local function criarItem(nome, aoEquipar)
    local tool = Instance.new("Tool")
    tool.RequiresHandle = false
    tool.Name = nome
    tool.Parent = Backpack

    tool.Activated:Connect(aoEquipar)
end

-- Item 1: Noclip (ativa noclip ao clicar com o item)
criarItem("Noclip", function()
    local char = player.Character
    if char then
        for _, part in pairs(char:GetDescendants()) do
            if part:IsA("BasePart") then
                part.CanCollide = false
            end
        end
    end
end)

-- Item 2: Horse fly
criarItem("Horse fly", function()
    loadstring(game:HttpGet('https://raw.githubusercontent.com/GhostPlayer352/Test4/refs/heads/main/Vehicle%20Fly%20Gui'))()
end)

-- Item 3: Admin hub
criarItem("Admin hub", function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/gumanba/Scripts/main/DeadRails"))()
end)

-- Item 4: Gojo
criarItem("Gojo", function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/LolnotaKid/SCRIPTSBYVEUX/refs/heads/main/LALALALALALAGOJOOO.lua.txt"))()
end)

-- Item 5: Speed Hack (ativa velocidade 49)
criarItem("Speed Hack", function()
    local humanoid = player.Character and player.Character:FindFirstChildWhichIsA("Humanoid")
    if humanoid then
        humanoid.WalkSpeed = 49
    end
end)