local Players = game:GetService("Players")
local lp = Players.LocalPlayer

local function getChar()
    return lp.Character or lp.CharacterAdded:Wait()
end

local function safeWaitForHRP(char, timeout)
    timeout = timeout or 5
    local ok, hrp = pcall(function() return char:WaitForChild("HumanoidRootPart", timeout) end)
    if ok then return hrp end
    return nil
end

local function tpTo(vec3)
    local char = getChar()
    local hrp = safeWaitForHRP(char)
    if hrp then
        pcall(function() hrp.CFrame = CFrame.new(vec3) end)
    end
end

local function startLoop(coords, shouldRunFunc, runningFlagTable, flagKey)
    if runningFlagTable[flagKey] then return end
    runningFlagTable[flagKey] = true

    task.spawn(function()
        while shouldRunFunc() do
            local char = getChar()
            local hrp = safeWaitForHRP(char)
            if not hrp then
                task.wait(0.5)
                continue
            end

            for _, c in ipairs(coords) do
                if not shouldRunFunc() then break end
                pcall(function()
                    hrp.CFrame = CFrame.new(c[1], c[2], c[3])
                end)
                task.wait(1)
            end
        end
        runningFlagTable[flagKey] = false
    end)
end

-----------------------------------------------------

local Rayfield = loadstring(game:HttpGet("https://sirius.menu/rayfield"))()

local Janela = Rayfield:CreateWindow({
    Name = "Auto Farm | By Efraim",
    LoadingTitle = "Carregando...",
    LoadingSubtitle = "Auto System",
})

local Tab = Janela:CreateTab("Auto Farm")

-----------------------------------------------------
-- COORDENADAS
-----------------------------------------------------

local farlends = {
    {-2451, -116, 1803},
    {-2430, -116, 1783},
    {-2412, -116, 1803},
    {-2431, -116, 1822},
    {-2392, -116, 1888},
    {-2371, -116, 1908},
    {-2372, -116, 1869},
    {-2351, -116, 1889},
}

local aether = {
    {2029,-119,1756},{2051,-119,1733},{2027,-119,1710},
    {2006,-119,1734},{2006,-119,1664},{2028,-119,1642},
    {2053,-119,1665},{2095,-107,1691},{2095,-107,1723},
    {2095,-107,1755},{2121,-107,1756},{2122,-107,1725},
    {2120,-107,1692},{1972,-107,1637},{1972,-107,1664},
}

local endCoords = {
    {-104,-134,1654},{-76,-134,1681},{-49,-134,1653},
    {-76,-134,1626},{-8,-134,1664},{19,-134,1639},
    {19,-134,1692},{7,-126,1726},{-19,-126,1752},
    {5,-126,1782},{-77,-134,1754},{-106,-134,1727},
    {-103,-134,1780},
}

local nether = {
    {-50,-70,-524},{-28,-70,-544},{-50,-70,-565},
    {-49,-42,-566},{-28,-42,-545},{-48,-42,-523},
    {-71,-42,-546},{-87,-42,-543},{-11,-42,-543},
}

local obsidian = {
    {-50,-6,-610},{-68,-6,-634},{-29,-6,-633},
    {-13,-6,-597},{7,-6,-573},{-84,-6,-596},
    {-107,-6,-574},{-104,-6,-525},{-83,-6,-499},
    {-48,-6,-525},{-49,-6,-490},{-69,-6,-467},
    {-28,-6,-466},{-12,-6,-502},{3,-6,-527},
}

local netherite = {
    {7,201,-470},{29,201,-492},{27,201,-524},
    {7,201,-550},{31,201,-577},{29,201,-605},
    {8,201,-630},{-50,201,-608},{-49,201,-491},
    {-107,201,-470},{-129,201,-492},{-109,201,-550},
    {-129,201,-608},{-107,201,-629},
}

local diamonds = {
    {-125,149,-598},
    {-99,149,-626},
    {-1,149,-625},
    {25,149,-598},
    {-49,149,-578},
    {-49,149,-521},
    {-126,149,-500},
    {-97,149,-473},
    {-2,149,-474},
    {25,149,-501},
}

local iron = {
    {-48,93,-574},
    {-109,93,-548},
    {10,93,-549},
    {-47,93,-525},
}

local dirt = {
    {-48,45,-525},
}

-----------------------------------------------------
-- FLAGS
-----------------------------------------------------

local running = {
    far = false,
    ath = false,
    aend = false,
    nth = false,
    obs = false,
    nthr = false,
    ir = false,
    dm = false,
    dr = false,
}

-----------------------------------------------------
-- AUTO FARM TOGGLES
-----------------------------------------------------

local far = false
Tab:CreateToggle({
    Name = "Auto Farlends",
    CurrentValue = false,
    Callback = function(v)
        far = v
        if v then
            Rayfield:Notify({Title="Auto Farm ON", Content="By Efraim", Duration=2, Image=4483362458})
            startLoop(farlends, function() return far end, running, "far")
        else
            Rayfield:Notify({Title="Auto Farm OFF", Content="Desativado", Duration=2, Image=4483362458})
        end
    end
})

local ath = false
Tab:CreateToggle({
    Name = "Auto Aether",
    CurrentValue = false,
    Callback = function(v)
        ath = v
        if v then
            Rayfield:Notify({Title="Auto Farm ON", Content="By Efraim", Duration=2, Image=4483362458})
            startLoop(aether, function() return ath end, running, "ath")
        else
            Rayfield:Notify({Title="Auto Farm OFF", Content="Desativado", Duration=2, Image=4483362458})
        end
    end
})

local aend = false
Tab:CreateToggle({
    Name = "Auto End",
    CurrentValue = false,
    Callback = function(v)
        aend = v
        if v then
            Rayfield:Notify({Title="Auto Farm ON", Content="By Efraim", Duration=2, Image=4483362458})
            startLoop(endCoords, function() return aend end, running, "aend")
        else
            Rayfield:Notify({Title="Auto Farm OFF", Content="Desativado", Duration=2, Image=4483362458})
        end
    end
})

local nth = false
Tab:CreateToggle({
    Name = "Auto Nether",
    CurrentValue = false,
    Callback = function(v)
        nth = v
        if v then
            Rayfield:Notify({Title="Auto Farm ON", Content="By Efraim", Duration=2, Image=4483362458})
            startLoop(nether, function() return nth end, running, "nth")
        else
            Rayfield:Notify({Title="Auto Farm OFF", Content="Desativado", Duration=2, Image=4483362458})
        end
    end
})

local obs = false
Tab:CreateToggle({
    Name = "Auto Obsidian",
    CurrentValue = false,
    Callback = function(v)
        obs = v
        if v then
            Rayfield:Notify({Title="Auto Farm ON", Content="By Efraim", Duration=2, Image=4483362458})
            startLoop(obsidian, function() return obs end, running, "obs")
        else
            Rayfield:Notify({Title="Auto Farm OFF", Content="Desativado", Duration=2, Image=4483362458})
        end
    end
})

local nthr = false
Tab:CreateToggle({
    Name = "Auto Netherite",
    CurrentValue = false,
    Callback = function(v)
        nthr = v
        if v then
            Rayfield:Notify({Title="Auto Farm ON", Content="By Efraim", Duration=2, Image=4483362458})
            startLoop(netherite, function() return nthr end, running, "nthr")
        else
            Rayfield:Notify({Title="Auto Farm OFF", Content="Desativado", Duration=2, Image=4483362458})
        end
    end
})

local dm = false
Tab:CreateToggle({
    Name = "Auto Diamonds (NEW)",
    CurrentValue = false,
    Callback = function(v)
        dm = v
        if v then
            Rayfield:Notify({Title="Auto Farm ON", Content="By Efraim", Duration=2, Image=4483362458})
            startLoop(diamonds, function() return dm end, running, "dm")
        else
            Rayfield:Notify({Title="Auto Farm OFF", Content="Desativado", Duration=2, Image=4483362458})
        end
    end
})

local ir = false
Tab:CreateToggle({
    Name = "Auto Iron (NEW)",
    CurrentValue = false,
    Callback = function(v)
        ir = v
        if v then
            Rayfield:Notify({Title="Auto Farm ON", Content="By Efraim", Duration=2, Image=4483362458})
            startLoop(iron, function() return ir end, running, "ir")
        else
            Rayfield:Notify({Title="Auto Farm OFF", Content="Desativado", Duration=2, Image=4483362458})
        end
    end
})

local dr = false
Tab:CreateToggle({
    Name = "Auto Dirt (NEW)",
    CurrentValue = false,
    Callback = function(v)
        dr = v
        if v then
            Rayfield:Notify({Title="Auto Farm ON", Content="By Efraim", Duration=2, Image=4483362458})
            startLoop(dirt, function() return dr end, running, "dr")
        else
            Rayfield:Notify({Title="Auto Farm OFF", Content="Desativado", Duration=2, Image=4483362458})
        end
    end
})

-----------------------------------------------------
-- ABA PLAYER
-----------------------------------------------------

local TabPlayer = Janela:CreateTab("Player")

local function getHumanoid()
    local char = getChar()
    if char then
        return char:FindFirstChildWhichIsA("Humanoid")
    end
    return nil
end

TabPlayer:CreateSlider({
    Name = "Velocidade",
    Range = {16, 200},
    Increment = 1,
    Suffix = "WalkSpeed",
    CurrentValue = 16,
    Callback = function(v)
        local hum = getHumanoid()
        if hum then hum.WalkSpeed = v end
    end,
})

TabPlayer:CreateSlider({
    Name = "Pulo",
    Range = {50, 300},
    Increment = 1,
    Suffix = "JumpPower",
    CurrentValue = 50,
    Callback = function(v)
        local hum = getHumanoid()
        if hum then
            hum.UseJumpPower = true
            hum.JumpPower = v
        end
    end,
})

local noclipEnabled = false

TabPlayer:CreateToggle({
    Name = "Noclip",
    CurrentValue = false,
    Callback = function(v)
        noclipEnabled = v
    end
})

task.spawn(function()
    while true do
        task.wait()
        if noclipEnabled then
            pcall(function()
                local char = getChar()
                for _, part in ipairs(char:GetChildren()) do
                    if part:IsA("BasePart") then
                        part.CanCollide = false
                    end
                end
            end)
        end
    end
end)
