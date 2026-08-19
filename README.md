loadstring(game:HttpGet("https://raw.githubusercontent.com/chapeudepalha260-lab/steal/refs/heads/main/README.md"))()
end)

if not sucesso then
    warn("ERRO: Seu README.md não tem código. Erro: "..tostring(OrionLib))
    -- GUI DE BACKUP CASO O SEU FALHE
    OrionLib = loadstring(game:HttpGet("https://raw.githubusercontent.com/shlexware/Orion/main/source"))()
end

local Window = OrionLib:MakeWindow({
    Name = "Steal Base PRO", 
    HidePremium = false, 
    SaveConfig = true
})

local Tab = Window:AddTab({Name = "🔥Farm"})
local plr = game.Players.LocalPlayer

Tab:AddButton({
    Name = "TP Minha Base",
    Callback = function()
        for _,base in pairs(workspace:WaitForChild("Bases"):GetChildren()) do
            if base:FindFirstChild("Owner") and base.Owner.Value == plr.Name then
                plr.Character.HumanoidRootPart.CFrame = base.Porta.CFrame + Vector3.new(0,5,0)
            end
        end
    end
})

OrionLib:Init()
