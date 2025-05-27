
local ConfigsKaitun = {
    ["Safe Mode"] = false,
    ["Berry Collect"] = true,
    ["Melee"] = {
        ["Death Step"] = true,
        ["Electric Claw"] = true,
        -- adicione outros ataques se desejar
    },
    ["Sword"] = {
        ["Saber"] = true,
        ["Midnight Blade"] = true,
        -- adicione outros
    },
    ["Gun"] = {
        ["Kabucha"] = true,
        -- adicione outros
    },
    ["Fruit"] = {
        ["Main Fruit"] = {"Dough-Dough"},
        ["Sec Fruit"] = {"buddha-buddha", "Ice-Ice", "dragon-dragon"},
    },
    ["Quest"] = {
        ["Shark Anchor"] = {
            ["Enable"] = true,
            ["Level"] = 2600,
            ["MaxMoney"] = 25000000,
            ["MinMoney"] = 22000000,
        }
    },
    -- outras configurações
}

-- Funções auxiliares
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
local HRP = Character:WaitForChild("HumanoidRootPart")

-- Função para mover personagem até uma posição
local function moveTo(position)
    HRP.CFrame = CFrame.new(position)
    wait(0.5) -- tempo para o personagem chegar
end

-- Função para coletar berries
local function collectBerries()
    if not ConfigsKaitun["Berry Collect"] then return end
    local berriesFolder = workspace:FindFirstChild("Berries")
    if not berriesFolder then return end
    for _, berry in pairs(berriesFolder:GetChildren()) do
        if berry and berry:FindFirstChild("Position") then
            moveTo(berry.Position)
            -- aqui você pode simular clique ou coleta
            -- por exemplo, ativar uma ferramenta de coleta
            print("Coletando berry em", berry.Position)
            wait(0.2)
        end
    end
end

-- Função para atacar inimigos
local function attackEnemies()
    local enemiesFolder = workspace:FindFirstChild("Enemies")
    if not enemiesFolder then return end
    for _, enemy in pairs(enemiesFolder:GetChildren()) do
        if enemy and enemy:FindFirstChild("Humanoid") and enemy.Humanoid.Health > 0 then
            moveTo(enemy.HumanoidRootPart.Position)
            -- Aqui você pode usar habilidades ou ataques
            print("Atacando inimigo em", enemy.HumanoidRootPart.Position)
            -- Exemplo: ativar ataque de melee
            -- Você pode usar ferramentas ou comandos específicos
            wait(1)
        end
    end
end

-- Função para usar armas (exemplo simples)
local function usarArmas()
    -- Aqui você pode ativar armas específicas dependendo da configuração
    for weaponType, weapons in pairs({["Melee"] = ConfigsKaitun["Melee"], ["Sword"] = ConfigsKaitun["Sword"], ["Gun"] = ConfigsKaitun["Gun"]}) do
        for weaponName, enabled in pairs(weapons) do
            if enabled then
                print("Usando arma:", weaponName, "do tipo:", weaponType)
                -- Aqui você pode simular ativação de arma
                -- Como: tool = workspace.Tools:FindFirstChild(weaponName)
                -- Se encontrado, ativar
                -- Exemplo:
                -- local tool = workspace.Tools:FindFirstChild(weaponName)
                -- if tool then
                --     tool.Parent = LocalPlayer.Character
                --     -- usar a arma
                -- end
                wait(0.5)
            end
        end
    end
end

-- Função para usar frutas (exemplo)
local function usarFrutas()
    local fruits = ConfigsKaitun["Fruit"]
    for _, fruitName in ipairs(fruits["Main Fruit"]) do
        print("Usando fruta:", fruitName)
        -- Aqui você pode ativar o uso da fruta
        wait(1)
    end
end

-- Função para completar quests
local function completarQuests()
    local quests = ConfigsKaitun["Quest"]
    for questName, questConfig in pairs(quests) do
        if questConfig["Enable"] then
            print("Completar quest:", questName)
            -- lógica para completar quests
            -- exemplo: mover até pontos específicos, ativar ações
            wait(2)
        end
    end
end

-- Loop principal
while true do
    -- Executa funções
    collectBerries()
    attackEnemies()
    usarArmas()
    usarFrutas()
    completarQuests()

    -- Controlar FPS ou modo seguro
    if not ConfigsKaitun["Safe Mode"] then
        -- modo normal
        wait(1)
    else
        -- modo seguro (mais lento)
        wait(3)
    end
end
