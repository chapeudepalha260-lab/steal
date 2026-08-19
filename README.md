-- LIB OFICIAL
local OrionLib = loadstring(game:HttpGet("https://raw.githubusercontent.com/shlexware/Orion/main/source"))()
local Window = OrionLib:MakeWindow({
    Name = "Base Farm V2 | by IA",
    HidePremium = false, 
    SaveConfig = true, 
    ConfigFolder = "BaseFarmConfig"
})

local plr = game.Players.LocalPlayer
local TweenService = game:GetService("TweenService")

-- VARIAVEIS
local _G = getgenv()
_G.AutoColetar = false
_G.AutoComprar = false
_G.VelocidadeTP = 0.3

-- FUNÇÃO TP COM TWEEN - MAIS SUAVE
function _tp(cf)
    if plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
        local tween = TweenService:Create(plr.Character.HumanoidRootPart, TweenInfo.new(_G.VelocidadeTP), {CFrame = cf})
        tween:Play()
    end
end

-- ABA 1: FARM
local FarmTab = Window:AddTab({Name = "🚀Farm"})
FarmTab:AddSection({Name = "Auto Farm"})

FarmTab:AddToggle({
    Name = "Auto Coletar Dinheiro",
    Default = false,
    Callback = function(v)
        _G.AutoColetar = v
    end
})

FarmTab:AddToggle({
    Name = "Auto Comprar na Máquina",
    Default = false,
    Callback = function(v)
        _G.AutoComprar = v
    end
})

FarmTab:AddSlider({
    Name = "Velocidade do TP",
    Min = 0.1, Max = 1, Default = 0.3,
    Callback = function(v)
        _G.VelocidadeTP = v
    end
})

-- ABA 2: TELEPORT
local TPTab = Window:AddTab({Name = "🗺️Travel"})
TPTab:AddButton({Name = "TP Loja", Callback = function()
    local loja = workspace:FindFirstChild("Loja")
    if loja then _tp(loja.CFrame + Vector3.new(0,5,0)) end
end})

TPTab:AddButton({Name = "TP Máquina", Callback = function()
    local maquina = workspace:FindFirstChild("Máquina de Criação")
    if maquina then _tp(maquina.CFrame + Vector3.new(0,5,0)) end
end})

TPTab:AddButton({Name = "TP Minha Base", Callback = function()
    for _,base in pairs(workspace:WaitForChild("Bases"):GetChildren()) do
        if base:FindFirstChild("Owner") and base.Owner.Value == plr.Name then
            _tp(base:WaitForChild("Porta").CFrame + Vector3.new(0,5,0))
        end
    end
end})

-- LOOP PRINCIPAL
task.spawn(function()
    while task.wait(0.3) do
        -- AUTO COLETAR
        if _G.AutoColetar then
            for _,base in pairs(workspace.Bases:GetChildren()) do
                if base:FindFirstChild("Owner") and base.Owner.Value == plr.Name then
                    if base:FindFirstChild("MoneyPad") then
                        _tp(base.MoneyPad.CFrame + Vector3.new(0,5,0))
                        task.wait(0.1)
                        game.ReplicatedStorage:WaitForChild("ColetarDinheiro"):FireServer()
                    end
                end
            end
        end

        -- AUTO COMPRAR
        if _G.AutoComprar then
            local maquina = workspace:FindFirstChild("Máquina de Criação")
            if maquina then
                _tp(maquina.CFrame + Vector3.new(0,5,0))
                task.wait(0.3)
                game.ReplicatedStorage:WaitForChild("ComprarItem"):FireServer()
            end
        end
    end
end)

OrionLib:Init()
OrionLib:MakeNotification({Name = "GUI Carregada", Content = "Base Farm V2 Ativado", Time = 3})
