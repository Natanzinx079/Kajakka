-- NATAN DEAD REAILS 1.0 | Script atualizado
-- Script exclusivo com teleporte, autofarm, noclip e HUD estiloso

-- Carrega biblioteca de interface
local OrionLib = loadstring(game:HttpGet("https://raw.githubusercontent.com/shlexware/Orion/main/source"))()

-- Cria janela com nome vermelho
local Window = OrionLib:MakeWindow({
    Name = "NATAN DEAD REAILS 1.0",
    HidePremium = false,
    SaveConfig = false,
    ConfigFolder = "NatanHub",
    IntroText = "NATAN DEAD REAILS 1.0",
    IntroIcon = "",
})

-- Pegando jogador e personagem
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()

-- Função de Teleporte
local function teleportTo(pos)
    if character and character:FindFirstChild("HumanoidRootPart") then
        character:MoveTo(pos)
    end
end

-- Abas
local tpTab = Window:MakeTab({
    Name = "Teleporte",
    Icon = "rbxassetid://4483345998",
    PremiumOnly = false
})

local funcTab = Window:MakeTab({
    Name = "Funções",
    Icon = "rbxassetid://6035047409",
    PremiumOnly = false
})

-- TELEPORTES ATUALIZADOS
tpTab:AddButton({
    Name = "TP para Banco do Trem",
    Callback = function()
        teleportTo(Vector3.new(10, 5, -80))
    end
})

tpTab:AddButton({
    Name = "TP para Base Final",
    Callback = function()
        teleportTo(Vector3.new(105, 110, 340))
    end
})

tpTab:AddButton({
    Name = "TP para Cima da Torre Final",
    Callback = function()
        teleportTo(Vector3.new(105, 130, 340))
    end
})

tpTab:AddButton({
    Name = "TP para Tesla",
    Callback = function()
        teleportTo(Vector3.new(340, 25, -120))
    end
})

-- AUTO FARM
_G.AutoFarm = false

funcTab:AddToggle({
    Name = "Auto Farm",
    Default = false,
    Callback = function(val)
        _G.AutoFarm = val
        while _G.AutoFarm do
            for _, item in pairs(workspace:GetDescendants()) do
                if item:IsA("TouchTransmitter") and item.Parent then
                    firetouchinterest(character.HumanoidRootPart, item.Parent, 0)
                    firetouchinterest(character.HumanoidRootPart, item.Parent, 1)
                end
            end
            task.wait(1)
        end
    end
})

-- NOCLIP
getgenv().noclip = false

funcTab:AddToggle({
    Name = "Noclip",
    Default = false,
    Callback = function(state)
        getgenv().noclip = state
        game:GetService("RunService").Stepped:Connect(function()
            if getgenv().noclip and character then
                for _, p in pairs(character:GetDescendants()) do
                    if p:IsA("BasePart") then
                        p.CanCollide = false
                    end
                end
            end
        end)
    end
})

-- ATIVAR TUDO
funcTab:AddToggle({
    Name = "Ativar Tudo (Auto Farm + Noclip)",
    Default = false,
    Callback = function(val)
        _G.AutoFarm = val
        getgenv().noclip = val
        while _G.AutoFarm do
            for _, item in pairs(workspace:GetDescendants()) do
                if item:IsA("TouchTransmitter") and item.Parent then
                    firetouchinterest(character.HumanoidRootPart, item.Parent, 0)
                    firetouchinterest(character.HumanoidRootPart, item.Parent, 1)
                end
            end
            task.wait(1)
        end
    end
})

-- Inicializa UI
OrionLib:Init()
