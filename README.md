-- SUA LIB AQUI
local OrionLib = loadstring(game:HttpGet("https://raw.githubusercontent.com/chapeudepalha260-lab/steal/refs/heads/main/README.md"))()
local Window = OrionLib:MakeWindow({
    Name = "Steal Base PRO", 
    HidePremium = false, 
    SaveConfig = true
})

local plr = game.Players.LocalPlayer
getgenv().AutoRoubar = false

-- ABA
local Tab = Window:AddTab({Name = "🚀Farm"})

Tab:AddToggle({
    Name = "Auto Roubar + TP Base",
    Default = false,
    Callback = function(v)
        getgenv().AutoRoubar = v
    end
})

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

-- LOOP
task.spawn(function()
    while task.wait(0.3) do
        if getgenv().AutoRoubar then
            -- aqui você coloca o resto do farm
        end
    end
end)

OrionLib:Init()
