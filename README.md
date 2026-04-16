-- znHub - Snowy RPG [VERSÃO FINAL + FLY VELOZ + NOCLIP]
-- Velocidade otimizada: rápida mas controlável

wait(2)

-- Variáveis
local farming = false
local useSkillR = true
local useSkillT = true
local attackDistance = 50
local lastAttackTime = 0
local attackDelay = 0.5

local flying = false
local bodyVelocity = nil
local noclipActive = false
local flySpeed = 180
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

-- Criar UI
local gui = Instance.new("ScreenGui")
gui.Name = "ZnHub"
gui.ResetOnSpawn = false

local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 300, 0, 480)
frame.Position = UDim2.new(0.5, -150, 0.5, -240)
frame.BackgroundColor3 = Color3.fromRGB(10, 10, 18)
frame.BackgroundTransparency = 0.1
frame.BorderSizePixel = 0

local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0, 10)
corner.Parent = frame

local stroke = Instance.new("UIStroke")
stroke.Color = Color3.fromRGB(0, 150, 255)
stroke.Thickness = 1.5
stroke.Parent = frame

-- Título
local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0, 40)
title.BackgroundColor3 = Color3.fromRGB(0, 80, 160)
title.BackgroundTransparency = 0.4
title.Text = "znHub - Snowy RPG"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.TextSize = 18
title.Font = Enum.Font.GothamBold
title.Parent = frame

local titleCorner = Instance.new("UICorner")
titleCorner.Parent = title

-- Fechar
local close = Instance.new("TextButton")
close.Size = UDim2.new(0, 28, 0, 28)
close.Position = UDim2.new(1, -34, 0, 6)
close.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
close.BackgroundTransparency = 0.3
close.Text = "X"
close.TextColor3 = Color3.fromRGB(255, 255, 255)
close.TextSize = 16
close.Parent = title

local closeCorner = Instance.new("UICorner")
closeCorner.CornerRadius = UDim.new(1, 0)
closeCorner.Parent = close

-- Status
local status = Instance.new("TextLabel")
status.Size = UDim2.new(0.9, 0, 0, 30)
status.Position = UDim2.new(0.05, 0, 0.13, 0)
status.BackgroundTransparency = 1
status.Text = "● PARADO"
status.TextColor3 = Color3.fromRGB(255, 80, 80)
status.TextSize = 14
status.Font = Enum.Font.GothamBold
status.Parent = frame

-- Alvo info
local targetInfo = Instance.new("TextLabel")
targetInfo.Size = UDim2.new(0.9, 0, 0, 25)
targetInfo.Position = UDim2.new(0.05, 0, 0.2, 0)
targetInfo.BackgroundTransparency = 1
targetInfo.Text = "Alvo: ---"
targetInfo.TextColor3 = Color3.fromRGB(180, 180, 180)
targetInfo.TextSize = 11
targetInfo.Parent = frame

-- Botão Farm
local farmBtn = Instance.new("TextButton")
farmBtn.Size = UDim2.new(0.8, 0, 0, 42)
farmBtn.Position = UDim2.new(0.1, 0, 0.3, 0)
farmBtn.BackgroundColor3 = Color3.fromRGB(0, 100, 200)
farmBtn.Text = "▶ INICIAR"
farmBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
farmBtn.TextSize = 15
farmBtn.Font = Enum.Font.GothamSemibold
farmBtn.Parent = frame

local btnCorner = Instance.new("UICorner")
btnCorner.CornerRadius = UDim.new(0, 6)
btnCorner.Parent = farmBtn

-- Botão Skill R
local skillRBtn = Instance.new("TextButton")
skillRBtn.Size = UDim2.new(0.8, 0, 0, 35)
skillRBtn.Position = UDim2.new(0.1, 0, 0.44, 0)
skillRBtn.BackgroundColor3 = Color3.fromRGB(0, 70, 130)
skillRBtn.Text = "SKILL R: ON"
skillRBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
skillRBtn.TextSize = 13
skillRBtn.Font = Enum.Font.GothamSemibold
skillRBtn.Parent = frame

local btnCorner2 = Instance.new("UICorner")
btnCorner2.CornerRadius = UDim.new(0, 6)
btnCorner2.Parent = skillRBtn

-- Botão Skill T
local skillTBtn = Instance.new("TextButton")
skillTBtn.Size = UDim2.new(0.8, 0, 0, 35)
skillTBtn.Position = UDim2.new(0.1, 0, 0.55, 0)
skillTBtn.BackgroundColor3 = Color3.fromRGB(0, 70, 130)
skillTBtn.Text = "SKILL T: ON"
skillTBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
skillTBtn.TextSize = 13
skillTBtn.Font = Enum.Font.GothamSemibold
skillTBtn.Parent = frame

local btnCorner3 = Instance.new("UICorner")
btnCorner3.CornerRadius = UDim.new(0, 6)
btnCorner3.Parent = skillTBtn

-- Botão Distância
local distBtn = Instance.new("TextButton")
distBtn.Size = UDim2.new(0.8, 0, 0, 30)
distBtn.Position = UDim2.new(0.1, 0, 0.66, 0)
distBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 60)
distBtn.Text = "DIST: 50"
distBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
distBtn.TextSize = 12
distBtn.Parent = frame

local btnCorner4 = Instance.new("UICorner")
btnCorner4.CornerRadius = UDim.new(0, 6)
btnCorner4.Parent = distBtn

-- Botão Delay
local delayBtn = Instance.new("TextButton")
delayBtn.Size = UDim2.new(0.8, 0, 0, 30)
delayBtn.Position = UDim2.new(0.1, 0, 0.78, 0)
delayBtn.BackgroundColor3 = Color3.fromRGB(40, 60, 40)
delayBtn.Text = "⏱ DELAY: 0.5s"
delayBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
delayBtn.TextSize = 12
delayBtn.Parent = frame

local btnCorner5 = Instance.new("UICorner")
btnCorner5.CornerRadius = UDim.new(0, 6)
btnCorner5.Parent = delayBtn

-- Botão Fly
local flyBtn = Instance.new("TextButton")
flyBtn.Size = UDim2.new(0.8, 0, 0, 35)
flyBtn.Position = UDim2.new(0.1, 0, 0.88, 0)
flyBtn.BackgroundColor3 = Color3.fromRGB(100, 50, 150)
flyBtn.Text = "🕊️ FLY: OFF"
flyBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
flyBtn.TextSize = 13
flyBtn.Font = Enum.Font.GothamSemibold
flyBtn.Parent = frame

local btnCorner6 = Instance.new("UICorner")
btnCorner6.CornerRadius = UDim.new(0, 6)
btnCorner6.Parent = flyBtn

-- Slider de Velocidade
local speedFrame = Instance.new("Frame")
speedFrame.Size = UDim2.new(0.8, 0, 0, 40)
speedFrame.Position = UDim2.new(0.1, 0, 0.95, 0)
speedFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 30)
speedFrame.BackgroundTransparency = 0.3
speedFrame.BorderSizePixel = 0
speedFrame.Parent = frame

local speedCorner = Instance.new("UICorner")
speedCorner.CornerRadius = UDim.new(0, 6)
speedCorner.Parent = speedFrame

local speedLabel = Instance.new("TextLabel")
speedLabel.Size = UDim2.new(1, 0, 0, 18)
speedLabel.Position = UDim2.new(0, 0, 0, 2)
speedLabel.BackgroundTransparency = 1
speedLabel.Text = "⚡ VELOCIDADE DO FLY: 180"
speedLabel.TextColor3 = Color3.fromRGB(200, 200, 255)
speedLabel.TextSize = 11
speedLabel.Font = Enum.Font.Gotham
speedLabel.Parent = speedFrame

local speedMinus = Instance.new("TextButton")
speedMinus.Size = UDim2.new(0, 25, 0, 20)
speedMinus.Position = UDim2.new(0, 5, 1, -24)
speedMinus.BackgroundColor3 = Color3.fromRGB(200, 60, 60)
speedMinus.Text = "-"
speedMinus.TextColor3 = Color3.fromRGB(255, 255, 255)
speedMinus.TextSize = 14
speedMinus.Font = Enum.Font.GothamBold
speedMinus.Parent = speedFrame

local speedMinusCorner = Instance.new("UICorner")
speedMinusCorner.CornerRadius = UDim.new(1, 0)
speedMinusCorner.Parent = speedMinus

local speedValue = Instance.new("TextLabel")
speedValue.Size = UDim2.new(0, 50, 0, 20)
speedValue.Position = UDim2.new(0.5, -25, 1, -24)
speedValue.BackgroundColor3 = Color3.fromRGB(30, 30, 40)
speedValue.Text = "180"
speedValue.TextColor3 = Color3.fromRGB(255, 255, 255)
speedValue.TextSize = 12
speedValue.Font = Enum.Font.GothamBold
speedValue.Parent = speedFrame

local speedValueCorner = Instance.new("UICorner")
speedValueCorner.CornerRadius = UDim.new(1, 0)
speedValueCorner.Parent = speedValue

local speedPlus = Instance.new("TextButton")
speedPlus.Size = UDim2.new(0, 25, 0, 20)
speedPlus.Position = UDim2.new(1, -30, 1, -24)
speedPlus.BackgroundColor3 = Color3.fromRGB(60, 200, 60)
speedPlus.Text = "+"
speedPlus.TextColor3 = Color3.fromRGB(255, 255, 255)
speedPlus.TextSize = 14
speedPlus.Font = Enum.Font.GothamBold
speedPlus.Parent = speedFrame

local speedPlusCorner = Instance.new("UICorner")
speedPlusCorner.CornerRadius = UDim.new(1, 0)
speedPlusCorner.Parent = speedPlus

-- ============================================
-- [NOVO] BORDA RGB ANIMADA (glow colorido)
-- ============================================
local rgbTime = 0
RunService.RenderStepped:Connect(function(dt)
    rgbTime = rgbTime + dt * 0.8 -- velocidade do ciclo RGB
    local r = math.abs(math.sin(rgbTime)) * 255
    local g = math.abs(math.sin(rgbTime + 2.094)) * 255  -- 120° defasado
    local b = math.abs(math.sin(rgbTime + 4.189)) * 255  -- 240° defasado
    stroke.Color = Color3.fromRGB(r, g, b)
end)

-- ============================================
-- [NOVO] EFEITO SONORO DE CLIQUE (usando TweenService para animação)
-- ============================================
local TweenService = game:GetService("TweenService")

local function playClickSound()
    pcall(function()
        local sound = Instance.new("Sound")
        sound.SoundId = "rbxassetid://6042053626" -- clique curto
        sound.Volume = 0.5
        sound.Parent = gui
        sound:Play()
        game:GetService("Debris"):AddItem(sound, 1)
    end)
end

local function animateClick(btn)
    local originalSize = btn.Size
    TweenService:Create(btn, TweenInfo.new(0.07, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {
        Size = UDim2.new(
            originalSize.X.Scale, originalSize.X.Offset - 6,
            originalSize.Y.Scale, originalSize.Y.Offset - 4
        )
    }):Play()
    task.delay(0.07, function()
        TweenService:Create(btn, TweenInfo.new(0.1, Enum.EasingStyle.Bounce, Enum.EasingDirection.Out), {
            Size = originalSize
        }):Play()
    end)
end

-- Conectar efeito de clique em todos os botões
local allButtons = {farmBtn, skillRBtn, skillTBtn, distBtn, delayBtn, flyBtn, speedMinus, speedPlus, close}
for _, btn in pairs(allButtons) do
    btn.MouseButton1Click:Connect(function()
        playClickSound()
        animateClick(btn)
    end)
end

-- ============================================
-- [NOVO] TOGGLE UI COM CTRL
-- ============================================
local uiVisible = true
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    if input.KeyCode == Enum.KeyCode.LeftControl then
        uiVisible = not uiVisible
        if uiVisible then
            frame.Visible = true
            TweenService:Create(frame, TweenInfo.new(0.2, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {
                BackgroundTransparency = 0.1
            }):Play()
        else
            TweenService:Create(frame, TweenInfo.new(0.15, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {
                BackgroundTransparency = 1
            }):Play()
            task.delay(0.15, function()
                frame.Visible = false
            end)
        end
        playClickSound()
    end
end)

-- ============================================
-- DETECÇÃO DE NPCs
-- ============================================
local function FindNearestNPC()
    local player = game.Players.LocalPlayer
    local char = player.Character
    if not char then return nil end
    
    local root = char:FindFirstChild("HumanoidRootPart") or char:FindFirstChild("Torso")
    if not root then return nil end
    
    local nearest = nil
    local minDist = attackDistance
    
    for _, obj in pairs(workspace:GetDescendants()) do
        if obj:IsA("Model") and obj:FindFirstChild("Humanoid") then
            local hum = obj.Humanoid
            local objRoot = nil
            if obj:FindFirstChild("HumanoidRootPart") then
                objRoot = obj.HumanoidRootPart
            elseif obj:FindFirstChild("Torso") then
                objRoot = obj.Torso
            elseif obj:FindFirstChild("UpperTorso") then
                objRoot = obj.UpperTorso
            end
            
            if hum and hum.Health and hum.Health > 0 and objRoot and objRoot:IsA("BasePart") then
                if obj ~= char then
                    local isPlayer = game.Players:GetPlayerFromCharacter(obj)
                    if not isPlayer then
                        local dist = (root.Position - objRoot.Position).Magnitude
                        if dist < minDist then
                            nearest = obj
                            minDist = dist
                            local npcName = obj.Name or "NPC"
                            if string.len(npcName) > 20 then
                                npcName = string.sub(npcName, 1, 17) .. "..."
                            end
                            targetInfo.Text = "Alvo: " .. npcName .. " (" .. math.floor(dist) .. "m)"
                            targetInfo.TextColor3 = Color3.fromRGB(100, 255, 100)
                        end
                    end
                end
            end
        end
    end
    
    return nearest, minDist
end

-- ============================================
-- FUNÇÕES DE ATAQUE
-- ============================================
local function ClickAttack()
    pcall(function()
        local vu = game:GetService("VirtualUser")
        vu:ClickButton1(Vector2.new(0, 0))
    end)
end

local function UseSkill(key)
    pcall(function()
        local vim = game:GetService("VirtualInputManager")
        vim:SendKeyEvent(true, key, false, game)
        wait(0.05)
        vim:SendKeyEvent(false, key, false, game)
    end)
end

local function UseEquippedTool()
    local char = game.Players.LocalPlayer.Character
    if char then
        local tool = char:FindFirstChildOfClass("Tool")
        if tool then
            pcall(function()
                tool:Activate()
            end)
        end
    end
end

local function AttackNPC(npc)
    if not npc then return false end
    
    local player = game.Players.LocalPlayer
    local char = player.Character
    if not char then return false end
    
    local charRoot = char:FindFirstChild("HumanoidRootPart") or char:FindFirstChild("Torso")
    
    local npcRoot = nil
    if npc:FindFirstChild("HumanoidRootPart") then
        npcRoot = npc.HumanoidRootPart
    elseif npc:FindFirstChild("Torso") then
        npcRoot = npc.Torso
    elseif npc:FindFirstChild("UpperTorso") then
        npcRoot = npc.UpperTorso
    end
    
    if not charRoot or not npcRoot or not npcRoot:IsA("BasePart") then return false end
    
    charRoot.CFrame = CFrame.new(charRoot.Position, Vector3.new(npcRoot.Position.X, charRoot.Position.Y, npcRoot.Position.Z))
    
    ClickAttack()
    UseEquippedTool()
    
    wait(0.1)
    
    if useSkillR then
        UseSkill("R")
    end
    
    wait(0.1)
    
    if useSkillT then
        UseSkill("T")
    end
    
    return true
end

-- ============================================
-- LOOP PRINCIPAL
-- ============================================
local farmConnection = nil
local attackCooldown = false

local function StartFarm()
    if farmConnection then return end
    
    farmConnection = game:GetService("RunService").Stepped:Connect(function()
        if farming and not attackCooldown then
            attackCooldown = true
            
            local npc = FindNearestNPC()
            if npc then
                AttackNPC(npc)
            else
                targetInfo.Text = "Alvo: ---"
                targetInfo.TextColor3 = Color3.fromRGB(180, 180, 180)
            end
            
            task.wait(attackDelay)
            attackCooldown = false
        end
    end)
end

local function StopFarm()
    if farmConnection then
        farmConnection:Disconnect()
        farmConnection = nil
    end
    targetInfo.Text = "Alvo: ---"
    targetInfo.TextColor3 = Color3.fromRGB(180, 180, 180)
end

-- ============================================
-- NOCLIP
-- ============================================
local function enableNoClip()
    noclipActive = true
    local function noclipLoop()
        while noclipActive and flying do
            local char = game.Players.LocalPlayer.Character
            if char then
                for _, part in pairs(char:GetDescendants()) do
                    if part:IsA("BasePart") then
                        part.CanCollide = false
                    end
                end
            end
            wait(0.1)
        end
    end
    spawn(noclipLoop)
end

local function disableNoClip()
    noclipActive = false
    local char = game.Players.LocalPlayer.Character
    if char then
        for _, part in pairs(char:GetDescendants()) do
            if part:IsA("BasePart") then
                part.CanCollide = true
            end
        end
    end
end

-- ============================================
-- SISTEMA DE FLY
-- ============================================
local function startFly()
    local player = game.Players.LocalPlayer
    local char = player.Character
    if not char then return end
    
    local humanoid = char:FindFirstChild("Humanoid")
    local rootPart = char:FindFirstChild("HumanoidRootPart")
    
    if humanoid and rootPart then
        humanoid.PlatformStand = true
        
        bodyVelocity = Instance.new("BodyVelocity")
        bodyVelocity.MaxForce = Vector3.new(100000, 100000, 100000)
        bodyVelocity.Velocity = Vector3.new(0, 0, 0)
        bodyVelocity.Parent = rootPart
        
        enableNoClip()
        
        local flyConnection
        flyConnection = RunService.RenderStepped:Connect(function()
            if not flying or not player.Character then
                if flyConnection then flyConnection:Disconnect() end
                return
            end
            
            local moveDirection = Vector3.new()
            local camera = workspace.CurrentCamera
            
            if UserInputService:IsKeyDown(Enum.KeyCode.W) then
                moveDirection = moveDirection + camera.CFrame.LookVector
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.S) then
                moveDirection = moveDirection - camera.CFrame.LookVector
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.D) then
                moveDirection = moveDirection + camera.CFrame.RightVector
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.A) then
                moveDirection = moveDirection - camera.CFrame.RightVector
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.Space) then
                moveDirection = moveDirection + Vector3.new(0, 1, 0)
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.LeftControl) then
                moveDirection = moveDirection - Vector3.new(0, 1, 0)
            end
            
            if moveDirection.Magnitude > 0 then
                moveDirection = moveDirection.Unit * flySpeed
            end
            
            if bodyVelocity then
                bodyVelocity.Velocity = moveDirection
            end
        end)
    end
end

local function stopFly()
    local player = game.Players.LocalPlayer
    local char = player.Character
    if char then
        local humanoid = char:FindFirstChild("Humanoid")
        if humanoid then
            humanoid.PlatformStand = false
        end
    end
    if bodyVelocity then
        bodyVelocity:Destroy()
        bodyVelocity = nil
    end
    disableNoClip()
end

-- ============================================
-- CONFIGURAR BOTÕES
-- ============================================
farmBtn.MouseButton1Click:Connect(function()
    farming = not farming
    
    if farming then
        farmBtn.Text = "⏸ PARAR"
        farmBtn.BackgroundColor3 = Color3.fromRGB(200, 60, 60)
        status.Text = "● FARMANDO"
        status.TextColor3 = Color3.fromRGB(80, 255, 80)
        StartFarm()
        print("znHub: ✅ Farm INICIADO")
    else
        farmBtn.Text = "▶ INICIAR"
        farmBtn.BackgroundColor3 = Color3.fromRGB(0, 100, 200)
        status.Text = "● PARADO"
        status.TextColor3 = Color3.fromRGB(255, 80, 80)
        StopFarm()
        print("znHub: ⏹ Farm PARADO")
    end
end)

skillRBtn.MouseButton1Click:Connect(function()
    useSkillR = not useSkillR
    skillRBtn.Text = useSkillR and "SKILL R: ON" or "SKILL R: OFF"
    skillRBtn.BackgroundColor3 = useSkillR and Color3.fromRGB(0, 70, 130) or Color3.fromRGB(50, 50, 70)
end)

skillTBtn.MouseButton1Click:Connect(function()
    useSkillT = not useSkillT
    skillTBtn.Text = useSkillT and "SKILL T: ON" or "SKILL T: OFF"
    skillTBtn.BackgroundColor3 = useSkillT and Color3.fromRGB(0, 70, 130) or Color3.fromRGB(50, 50, 70)
end)

distBtn.MouseButton1Click:Connect(function()
    attackDistance = attackDistance + 5
    if attackDistance > 70 then attackDistance = 30 end
    distBtn.Text = "DIST: " .. attackDistance
end)

delayBtn.MouseButton1Click:Connect(function()
    if attackDelay >= 1.0 then
        attackDelay = 0.3
    elseif attackDelay >= 0.5 then
        attackDelay = 1.0
    else
        attackDelay = 0.5
    end
    delayBtn.Text = "⏱ DELAY: " .. attackDelay .. "s"
end)

-- ============================================
-- CONTROLES DE VELOCIDADE DO FLY
-- ============================================
local function updateFlySpeedDisplay()
    speedValue.Text = tostring(flySpeed)
    speedLabel.Text = "⚡ VELOCIDADE DO FLY: " .. flySpeed
    if flying then
        print("znHub: Velocidade do fly alterada para: " .. flySpeed)
    end
end

speedMinus.MouseButton1Click:Connect(function()
    flySpeed = math.max(120, flySpeed - 10)
    updateFlySpeedDisplay()
end)

speedPlus.MouseButton1Click:Connect(function()
    flySpeed = math.min(350, flySpeed + 10)
    updateFlySpeedDisplay()
end)

flyBtn.MouseButton1Click:Connect(function()
    flying = not flying
    if flying then
        flyBtn.Text = "🛸 FLY: ON (RÁPIDO)"
        flyBtn.BackgroundColor3 = Color3.fromRGB(150, 50, 200)
        startFly()
        print("znHub: 🕊️ Fly ATIVADO - Velocidade: " .. flySpeed)
    else
        flyBtn.Text = "🕊️ FLY: OFF"
        flyBtn.BackgroundColor3 = Color3.fromRGB(100, 50, 150)
        stopFly()
        print("znHub: 🕊️ Fly DESATIVADO")
    end
end)

-- ============================================
-- ARRASTAR UI
-- ============================================
local dragging = false
local dragStart
local startPos

title.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        dragStart = input.Position
        startPos = frame.Position
    end
end)

game:GetService("UserInputService").InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement and dragging then
        local delta = input.Position - dragStart
        frame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end
end)

game:GetService("UserInputService").InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = false
    end
end)

close.MouseButton1Click:Connect(function()
    farming = false
    flying = false
    StopFarm()
    stopFly()
    gui:Destroy()
end)

-- Finalizar UI
frame.Parent = gui
gui.Parent = game:GetService("CoreGui")

-- Comandos manuais
_G.znFarm = function()
    farming = not farming
    if farming then
        farmBtn.Text = "⏸ PARAR"
        status.Text = "● FARMANDO"
        StartFarm()
    else
        farmBtn.Text = "▶ INICIAR"
        status.Text = "● PARADO"
        StopFarm()
    end
    return "Farm: " .. (farming and "LIGADO" or "DESLIGADO")
end

_G.znFly = function()
    flying = not flying
    if flying then
        flyBtn.Text = "🛸 FLY: ON (RÁPIDO)"
        startFly()
    else
        flyBtn.Text = "🕊️ FLY: OFF"
        stopFly()
    end
    return "Fly: " .. (flying and "LIGADO" or "DESLIGADO")
end

_G.znFlySpeed = function(value)
    if value then
        flySpeed = math.clamp(value, 120, 350)
        updateFlySpeedDisplay()
        return "Velocidade ajustada para: " .. flySpeed
    end
    return "Velocidade atual: " .. flySpeed
end

print("========================================")
print("znHub - Snowy RPG [FLY VELOZ]")
print("========================================")
print("✅ Farm funcionando")
print("✅ Fly com velocidade AJUSTÁVEL (120 a 350)")
print("✅ NoClip automático")
print("✅ Borda RGB animada")
print("✅ Efeitos de clique e animação")
print("✅ Toggle UI com CTRL")
print("")
print("📌 VELOCIDADES:")
print("   120 = Rápido")
print("   180 = Muito rápido (padrão)")
print("   250 = Cruzando ilhas em segundos")
print("   350 = Extremo (use com cuidado)")
print("")
print("📌 CONTROLES:")
print("   CTRL = Esconder/Mostrar UI")
print("   Botões +/- para ajustar velocidade")
print("   W/A/S/D = Movimento")
print("   Espaço = Subir | Control = Descer")
print("========================================")

game:GetService("StarterGui"):SetCore("SendNotification", {
    Title = "znHub - Snowy RPG",
    Text = "Fly VELOZ! CTRL para esconder UI | RGB ativo",
    Duration = 4
})
