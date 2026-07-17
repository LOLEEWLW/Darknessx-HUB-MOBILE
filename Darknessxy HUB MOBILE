--[[
    DARK PREMIUM - MOBILE EDITION (BLOX FRUITS)
    Fly GUI V3 integrado com controle por analógico
]]

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local CoreGui = game:GetService("CoreGui")
local Debris = game:GetService("Debris")
local StarterGui = game:GetService("StarterGui")
local VirtualInputManager = game:GetService("VirtualInputManager")
local TweenService = game:GetService("TweenService")
local Lighting = game:GetService("Lighting")
local Workspace = game:GetService("Workspace")

local player = Players.LocalPlayer
local camera = Workspace.CurrentCamera

-- ==========================================
-- SISTEMA DE KEY (LOGIN)
-- ==========================================
local CorrectKey = "DRKPREMIUM"

local guiParent = (gethui and gethui()) or CoreGui

if guiParent:FindFirstChild("DARKHUB") then
    guiParent.DARKHUB:Destroy()
end

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "DARKHUB"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = guiParent
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

-- ==========================================
-- VARIÁVEIS GLOBAIS
-- ==========================================
local uiVisible = true
local espEnabled = true

getgenv().GlideEnabled = false
getgenv().GlideSpeed = 350  
getgenv().GlideTime = 0.20   
local JumpForce = 300       
local LastShootTime = 0

-- Variáveis do FLY GUI V3
local nowe = false           -- fly ativado?
local speeds = 1             -- multiplicador de velocidade
local tpwalking = false      -- speed hack loop
local FlyBodyGyro = nil
local FlyBodyVelocity = nil
local flyMaxSpeed = 50       -- base, multiplicado por speeds

getgenv().HitboxSize = 15
getgenv().HitboxTransparency = 0.9
getgenv().HitboxStatus = false
getgenv().TeamCheck = false

local aimbotActive = false
local aimbotTarget = nil

local reduceParticles = false
local removeFog = false
local maxFPS = 60
local removeDecorations = false
local removeWaterEffects = false
local disableShadows = false
local disableAnimations = false

local chamsEnabled = false
local chamsColor = Color3.fromRGB(255, 0, 0)
local wireframeEnabled = false
local zoomLevel = 1
local fovLevel = 70

local antiAFK = false
local autoFarm = false
local godMode = false
local infiniteJump = false
local speedHack = false
local speedHackValue = 50

local autoClickActive = false
local autoClickInterval = 0.1
local fastAttackActive = false
local fastAttackInterval = 0.15

local pingDisplay = 0

-- ==========================================
-- FUNDO ANIMADO (ROSTO FELIZ + PARTÍCULAS)
-- ==========================================

local Background = Instance.new("Frame")
Background.Size = UDim2.new(1, 0, 1, 0)
Background.BackgroundColor3 = Color3.fromRGB(8, 0, 0)
Background.BackgroundTransparency = 0.4
Background.Parent = ScreenGui

local gradient = Instance.new("ImageLabel")
gradient.Size = UDim2.new(1, 0, 1, 0)
gradient.BackgroundTransparency = 1
gradient.Image = "rbxassetid://1461692487"
gradient.ImageColor3 = Color3.fromRGB(50, 0, 0)
gradient.ImageTransparency = 0.5
gradient.Parent = Background

-- Rosto feliz (simplificado)
local FaceFrame = Instance.new("Frame")
FaceFrame.Size = UDim2.new(0, 80, 0, 80)
FaceFrame.Position = UDim2.new(0.5, -40, 0.5, -40)
FaceFrame.BackgroundTransparency = 1
FaceFrame.Parent = Background

local EyeL = Instance.new("Frame")
EyeL.Size = UDim2.new(0, 12, 0, 12)
EyeL.Position = UDim2.new(0.25, -6, 0.3, -6)
EyeL.BackgroundColor3 = Color3.fromRGB(200, 200, 200)
EyeL.BorderSizePixel = 0
EyeL.Parent = FaceFrame
Instance.new("UICorner", EyeL).CornerRadius = UDim.new(1, 0)

local EyeR = Instance.new("Frame")
EyeR.Size = UDim2.new(0, 12, 0, 12)
EyeR.Position = UDim2.new(0.75, -6, 0.3, -6)
EyeR.BackgroundColor3 = Color3.fromRGB(200, 200, 200)
EyeR.BorderSizePixel = 0
EyeR.Parent = FaceFrame
Instance.new("UICorner", EyeR).CornerRadius = UDim.new(1, 0)

local PupilL = Instance.new("Frame")
PupilL.Size = UDim2.new(0, 5, 0, 5)
PupilL.Position = UDim2.new(0.3, -2.5, 0.35, -2.5)
PupilL.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
PupilL.BorderSizePixel = 0
PupilL.Parent = EyeL
Instance.new("UICorner", PupilL).CornerRadius = UDim.new(1, 0)

local PupilR = Instance.new("Frame")
PupilR.Size = UDim2.new(0, 5, 0, 5)
PupilR.Position = UDim2.new(0.3, -2.5, 0.35, -2.5)
PupilR.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
PupilR.BorderSizePixel = 0
PupilR.Parent = EyeR
Instance.new("UICorner", PupilR).CornerRadius = UDim.new(1, 0)

local Mouth = Instance.new("Frame")
Mouth.Size = UDim2.new(0, 35, 0, 15)
Mouth.Position = UDim2.new(0.5, -17.5, 0.65, 0)
Mouth.BackgroundColor3 = Color3.fromRGB(180, 180, 180)
Mouth.BorderSizePixel = 0
Mouth.Parent = FaceFrame
Instance.new("UICorner", Mouth).CornerRadius = UDim.new(0, 20)

task.spawn(function()
    while Background and Background.Parent do
        TweenService:Create(EyeL, TweenInfo.new(0.1), {Size = UDim2.new(0, 12, 0, 2)}):Play()
        TweenService:Create(EyeR, TweenInfo.new(0.1), {Size = UDim2.new(0, 12, 0, 2)}):Play()
        task.wait(0.15)
        TweenService:Create(EyeL, TweenInfo.new(0.1), {Size = UDim2.new(0, 12, 0, 12)}):Play()
        TweenService:Create(EyeR, TweenInfo.new(0.1), {Size = UDim2.new(0, 12, 0, 12)}):Play()
        task.wait(3)
        TweenService:Create(Mouth, TweenInfo.new(0.3), {Size = UDim2.new(0, 45, 0, 20)}):Play()
        task.wait(0.5)
        TweenService:Create(Mouth, TweenInfo.new(0.3), {Size = UDim2.new(0, 35, 0, 15)}):Play()
        task.wait(2.5)
    end
end)

-- Partículas
local Particles = Instance.new("Folder")
Particles.Parent = Background
for i = 1, 15 do
    local star = Instance.new("Frame")
    star.Size = UDim2.new(0, math.random(3, 7), 0, math.random(3, 7))
    star.Position = UDim2.new(math.random() * 0.95, 0, math.random() * 0.95, 0)
    star.BackgroundColor3 = Color3.fromRGB(200, 200, 200)
    star.BackgroundTransparency = 0.2 + math.random() * 0.6
    star.BorderSizePixel = 0
    star.Parent = Particles
    Instance.new("UICorner", star).CornerRadius = UDim.new(1, 0)
    task.spawn(function()
        local startX = star.Position.X.Scale
        local startY = star.Position.Y.Scale
        local speed = 0.5 + math.random() * 0.5
        local t = 0
        while Background and Background.Parent do
            t = t + 0.02 * speed
            local x = startX + math.sin(t) * 0.05
            local y = startY + math.cos(t * 0.7) * 0.05
            star.Position = UDim2.new(math.clamp(x, 0, 1), 0, math.clamp(y, 0, 1), 0)
            task.wait(0.03)
        end
    end)
end

-- ==========================================
-- FUNÇÕES DE ESTILO
-- ==========================================

local function createBorder(frame, thickness, color)
    thickness = thickness or 2
    color = color or Color3.fromRGB(150, 150, 150)
    local stroke = Instance.new("UIStroke")
    stroke.Thickness = thickness
    stroke.Color = color
    stroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
    stroke.Parent = frame
    return stroke
end

local function animateIn(instance, duration)
    duration = duration or 0.3
    instance.Size = UDim2.new(0, 0, 0, 0)
    instance.BackgroundTransparency = 1
    local tween = TweenService:Create(instance, TweenInfo.new(duration, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {
        Size = UDim2.new(0, 360, 0, 200),
        Position = UDim2.new(0.5, -180, 0.5, -100),
        BackgroundTransparency = 0.15
    })
    tween:Play()
    return tween
end

-- ==========================================
-- SISTEMA DE LOGIN (KEY)
-- ==========================================

local KeyFrame = Instance.new("Frame")
KeyFrame.Size = UDim2.new(0, 0, 0, 0)
KeyFrame.Position = UDim2.new(0.5, 0, 0.5, 0)
KeyFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
KeyFrame.BackgroundTransparency = 0.15
KeyFrame.BorderSizePixel = 0
KeyFrame.Active = true
KeyFrame.Draggable = true
KeyFrame.Parent = ScreenGui
Instance.new("UICorner", KeyFrame).CornerRadius = UDim.new(0, 12)
createBorder(KeyFrame, 2)
animateIn(KeyFrame, 0.5)

local KeyTitle = Instance.new("TextLabel")
KeyTitle.Size = UDim2.new(1, 0, 0, 30)
KeyTitle.Position = UDim2.new(0, 0, 0, 15)
KeyTitle.BackgroundTransparency = 1
KeyTitle.Text = "⚡ DARK PREMIUM ⚡"
KeyTitle.TextColor3 = Color3.fromRGB(255, 255, 255)
KeyTitle.Font = Enum.Font.GothamBlack
KeyTitle.TextSize = 20
KeyTitle.Parent = KeyFrame

local SubTitle = Instance.new("TextLabel")
SubTitle.Size = UDim2.new(1, 0, 0, 20)
SubTitle.Position = UDim2.new(0, 0, 0, 45)
SubTitle.BackgroundTransparency = 1
SubTitle.Text = "🔒 KEY DO DARKNEESXY HUB"
SubTitle.TextColor3 = Color3.fromRGB(200, 200, 200)
SubTitle.Font = Enum.Font.GothamMedium
SubTitle.TextSize = 13
SubTitle.Parent = KeyFrame

local KeyInput = Instance.new("TextBox")
KeyInput.Size = UDim2.new(0.8, 0, 0, 30)
KeyInput.Position = UDim2.new(0.1, 0, 0, 75)
KeyInput.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
KeyInput.Text = ""
KeyInput.PlaceholderText = "Digite a Key aqui..."
KeyInput.PlaceholderColor3 = Color3.fromRGB(150, 150, 150)
KeyInput.TextColor3 = Color3.fromRGB(255, 255, 255)
KeyInput.Font = Enum.Font.GothamMedium
KeyInput.TextSize = 14
KeyInput.Parent = KeyFrame
Instance.new("UICorner", KeyInput).CornerRadius = UDim.new(0, 6)
createBorder(KeyInput, 2)

local LoginBtn = Instance.new("TextButton")
LoginBtn.Size = UDim2.new(0.8, 0, 0, 30)
LoginBtn.Position = UDim2.new(0.1, 0, 0, 115)
LoginBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
LoginBtn.Text = "🔓 ACESSAR"
LoginBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
LoginBtn.Font = Enum.Font.GothamBold
LoginBtn.TextSize = 14
LoginBtn.Parent = KeyFrame
Instance.new("UICorner", LoginBtn).CornerRadius = UDim.new(0, 6)
createBorder(LoginBtn, 2)

LoginBtn.MouseEnter:Connect(function()
    TweenService:Create(LoginBtn, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(100, 100, 100)}):Play()
end)
LoginBtn.MouseLeave:Connect(function()
    TweenService:Create(LoginBtn, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(60, 60, 60)}):Play()
end)

local StatusText = Instance.new("TextLabel")
StatusText.Size = UDim2.new(1, 0, 0, 20)
StatusText.Position = UDim2.new(0, 0, 0, 155)
StatusText.BackgroundTransparency = 1
StatusText.Text = "🔑 Aguardando chave..."
StatusText.TextColor3 = Color3.fromRGB(200, 200, 200)
StatusText.Font = Enum.Font.Gotham
StatusText.TextSize = 12
StatusText.Parent = KeyFrame

-- ==========================================
-- PAINEL PRINCIPAL
-- ==========================================

local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 0, 0, 0)
MainFrame.Position = UDim2.new(0.02, 0, 0.05, 0)
MainFrame.BackgroundColor3 = Color3.fromRGB(10, 0, 0)
MainFrame.BackgroundTransparency = 0.1
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Visible = false
MainFrame.Parent = ScreenGui
Instance.new("UICorner", MainFrame).CornerRadius = UDim.new(0, 10)
createBorder(MainFrame, 2)

local PANEL_WIDTH = 380
local PANEL_HEIGHT = 500

local TitleTop = Instance.new("TextLabel")
TitleTop.Size = UDim2.new(1, 0, 0, 30)
TitleTop.Position = UDim2.new(0, 0, 0, 5)
TitleTop.BackgroundTransparency = 1
TitleTop.Text = "⚡ DARKNESSXY HUB ⚡"
TitleTop.TextColor3 = Color3.fromRGB(255, 255, 255)
TitleTop.Font = Enum.Font.GothamBlack
TitleTop.TextSize = 22
TitleTop.Parent = MainFrame

local SubTitleMain = Instance.new("TextLabel")
SubTitleMain.Size = UDim2.new(1, 0, 0, 16)
SubTitleMain.Position = UDim2.new(0, 0, 0, 35)
SubTitleMain.BackgroundTransparency = 1
SubTitleMain.Text = "⚡ PREMIUM FOR PLAYERS ⚡"
SubTitleMain.TextColor3 = Color3.fromRGB(200, 200, 200)
SubTitleMain.Font = Enum.Font.GothamBold
SubTitleMain.TextSize = 11
SubTitleMain.Parent = MainFrame

local CloseBtn = Instance.new("TextButton")
CloseBtn.Size = UDim2.new(0, 25, 0, 25)
CloseBtn.Position = UDim2.new(1, -30, 0, 5)
CloseBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
CloseBtn.Text = "✕"
CloseBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.TextSize = 14
CloseBtn.Parent = MainFrame
Instance.new("UICorner", CloseBtn).CornerRadius = UDim.new(0, 6)
createBorder(CloseBtn, 1)

CloseBtn.MouseEnter:Connect(function()
    TweenService:Create(CloseBtn, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(80, 80, 80)}):Play()
end)
CloseBtn.MouseLeave:Connect(function()
    TweenService:Create(CloseBtn, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(40, 40, 40)}):Play()
end)
CloseBtn.MouseButton1Click:Connect(function()
    uiVisible = false
    MainFrame.Visible = false
end)

local function animateMainIn()
    MainFrame.Visible = true
    MainFrame.Size = UDim2.new(0, 0, 0, 0)
    MainFrame.BackgroundTransparency = 1
    local tween = TweenService:Create(MainFrame, TweenInfo.new(0.5, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {
        Size = UDim2.new(0, PANEL_WIDTH, 0, PANEL_HEIGHT),
        BackgroundTransparency = 0.1
    })
    tween:Play()
    return tween
end

-- ==========================================
-- BARRA DE ABAS
-- ==========================================

local TabBar = Instance.new("Frame")
TabBar.Size = UDim2.new(0.95, 0, 0, 35)
TabBar.Position = UDim2.new(0.025, 0, 0, 58)
TabBar.BackgroundTransparency = 1
TabBar.Parent = MainFrame

local function createTabButton(text, xPos)
    local Btn = Instance.new("TextButton")
    Btn.Size = UDim2.new(0.12, 0, 1, 0)
    Btn.Position = UDim2.new(xPos, 0, 0, 0)
    Btn.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    Btn.Text = text
    Btn.TextColor3 = Color3.fromRGB(200, 200, 200)
    Btn.Font = Enum.Font.GothamBold
    Btn.TextSize = 8
    Btn.Parent = TabBar
    Instance.new("UICorner", Btn).CornerRadius = UDim.new(0, 4)
    createBorder(Btn, 1)
    Btn.MouseEnter:Connect(function()
        TweenService:Create(Btn, TweenInfo.new(0.15), {BackgroundColor3 = Color3.fromRGB(60, 60, 60)}):Play()
    end)
    Btn.MouseLeave:Connect(function()
        TweenService:Create(Btn, TweenInfo.new(0.15), {BackgroundColor3 = Color3.fromRGB(30, 30, 30)}):Play()
    end)
    return Btn
end

local TabGlitchBtn = createTabButton("GLITCH", 0)
local TabEspBtn = createTabButton("ESP", 0.125)
local TabHitboxBtn = createTabButton("HITBOX", 0.25)
local TabFlyBtn = createTabButton("FLY", 0.375)
local TabAimbotBtn = createTabButton("AIMBOT", 0.5)
local TabUtilsBtn = createTabButton("UTILS", 0.625)
local TabOptBtn = createTabButton("OTIMIZ", 0.75)
local TabVisualBtn = createTabButton("VISUAL", 0.875)

local Pages = Instance.new("Frame")
Pages.Size = UDim2.new(1, 0, 0, 400)
Pages.Position = UDim2.new(0, 0, 0, 100)
Pages.BackgroundTransparency = 1
Pages.Parent = MainFrame

local pageNames = {"GlitchPage", "EspPage", "FlyPage", "HitboxPage", "AimbotPage", "UtilsPage", "OptimizationPage", "VisualPage"}
local pages = {}
for _, name in ipairs(pageNames) do
    local page = Instance.new("Frame")
    page.Size = UDim2.new(1, 0, 1, 0)
    page.BackgroundTransparency = 1
    page.Parent = Pages
    pages[name] = page
end

local function showPage(page)
    for _, p in pairs(pages) do p.Visible = (p == page) end
    local active = Color3.fromRGB(80, 80, 80)
    local inactive = Color3.fromRGB(30, 30, 30)
    TabGlitchBtn.BackgroundColor3 = (page == pages.GlitchPage) and active or inactive
    TabEspBtn.BackgroundColor3 = (page == pages.EspPage) and active or inactive
    TabHitboxBtn.BackgroundColor3 = (page == pages.HitboxPage) and active or inactive
    TabFlyBtn.BackgroundColor3 = (page == pages.FlyPage) and active or inactive
    TabAimbotBtn.BackgroundColor3 = (page == pages.AimbotPage) and active or inactive
    TabUtilsBtn.BackgroundColor3 = (page == pages.UtilsPage) and active or inactive
    TabOptBtn.BackgroundColor3 = (page == pages.OptimizationPage) and active or inactive
    TabVisualBtn.BackgroundColor3 = (page == pages.VisualPage) and active or inactive
end

showPage(pages.AimbotPage)

TabGlitchBtn.MouseButton1Click:Connect(function() showPage(pages.GlitchPage) end)
TabEspBtn.MouseButton1Click:Connect(function() showPage(pages.EspPage) end)
TabFlyBtn.MouseButton1Click:Connect(function() showPage(pages.FlyPage) end)
TabHitboxBtn.MouseButton1Click:Connect(function() showPage(pages.HitboxPage) end)
TabAimbotBtn.MouseButton1Click:Connect(function() showPage(pages.AimbotPage) end)
TabUtilsBtn.MouseButton1Click:Connect(function() showPage(pages.UtilsPage) end)
TabOptBtn.MouseButton1Click:Connect(function() showPage(pages.OptimizationPage) end)
TabVisualBtn.MouseButton1Click:Connect(function() showPage(pages.VisualPage) end)

-- ==========================================
-- FUNÇÕES AUXILIARES PARA GUI
-- ==========================================

local function createInputField(labelText, defaultVal, yPos, parentPage)
    local Label = Instance.new("TextLabel")
    Label.Size = UDim2.new(0.5, 0, 0, 25)
    Label.Position = UDim2.new(0, 10, 0, yPos)
    Label.BackgroundTransparency = 1
    Label.Text = labelText
    Label.TextColor3 = Color3.fromRGB(200, 200, 200)
    Label.TextXAlignment = Enum.TextXAlignment.Left
    Label.Font = Enum.Font.GothamMedium
    Label.TextSize = 12
    Label.Parent = parentPage

    local InputBg = Instance.new("Frame")
    InputBg.Size = UDim2.new(0.42, 0, 0, 25)
    InputBg.Position = UDim2.new(0.53, -5, 0, yPos)
    InputBg.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    InputBg.BorderSizePixel = 0
    InputBg.Parent = parentPage
    Instance.new("UICorner", InputBg).CornerRadius = UDim.new(0, 4)
    createBorder(InputBg, 1)

    local Input = Instance.new("TextBox")
    Input.Size = UDim2.new(1, 0, 1, 0)
    Input.BackgroundTransparency = 1
    Input.TextColor3 = Color3.fromRGB(255, 255, 255)
    Input.Text = tostring(defaultVal)
    Input.Font = Enum.Font.GothamMedium
    Input.TextSize = 13
    Input.Parent = InputBg
    return Input
end

local function createToggleButton(text, yPos, parentPage)
    local Btn = Instance.new("TextButton")
    Btn.Size = UDim2.new(0.9, 0, 0, 28)
    Btn.Position = UDim2.new(0.05, 0, 0, yPos)
    Btn.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    Btn.Text = text
    Btn.TextColor3 = Color3.fromRGB(200, 200, 200)
    Btn.Font = Enum.Font.GothamBold
    Btn.TextSize = 11
    Btn.Parent = parentPage
    Instance.new("UICorner", Btn).CornerRadius = UDim.new(0, 4)
    createBorder(Btn, 1.5)
    Btn.MouseEnter:Connect(function()
        TweenService:Create(Btn, TweenInfo.new(0.15), {BackgroundColor3 = Color3.fromRGB(60, 60, 60)}):Play()
    end)
    Btn.MouseLeave:Connect(function()
        TweenService:Create(Btn, TweenInfo.new(0.15), {BackgroundColor3 = Color3.fromRGB(30, 30, 30)}):Play()
    end)
    return Btn
end

-- ==========================================
-- PÁGINA ESP
-- ==========================================

local EspToggleBtn = createToggleButton("🛡️ ESP (B): ON", 5, pages.EspPage)
local function updateEspVisual()
    EspToggleBtn.Text = espEnabled and "🛡️ ESP (B): ON" or "🛡️ ESP (B): OFF"
    EspToggleBtn.BackgroundColor3 = espEnabled and Color3.fromRGB(80, 80, 80) or Color3.fromRGB(30, 30, 30)
end
EspToggleBtn.MouseButton1Click:Connect(function()
    espEnabled = not espEnabled
    updateEspVisual()
end)

-- ==========================================
-- PÁGINA GLITCH
-- ==========================================

local GlitchToggleBtn = createToggleButton("⚡ GLITCH: OFF", 5, pages.GlitchPage)
local GlitchSpeedInput = createInputField("Velocidade:", getgenv().GlideSpeed, 45, pages.GlitchPage)
local GlitchDurInput = createInputField("Duração:", getgenv().GlideTime, 75, pages.GlitchPage)
local JumpPowerInput = createInputField("G-Jump Power:", JumpForce, 105, pages.GlitchPage)

local JumpBtn = Instance.new("TextButton")
JumpBtn.Size = UDim2.new(0.4, 0, 0, 35)
JumpBtn.Position = UDim2.new(0.3, 0, 0, 145)
JumpBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
JumpBtn.Text = "🚀 JUMP"
JumpBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
JumpBtn.Font = Enum.Font.GothamBold
JumpBtn.TextSize = 14
JumpBtn.Parent = pages.GlitchPage
Instance.new("UICorner", JumpBtn).CornerRadius = UDim.new(0, 6)
createBorder(JumpBtn, 2)
JumpBtn.MouseEnter:Connect(function()
    TweenService:Create(JumpBtn, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(80, 80, 80)}):Play()
end)
JumpBtn.MouseLeave:Connect(function()
    TweenService:Create(JumpBtn, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(40, 40, 40)}):Play()
end)
JumpBtn.MouseButton1Click:Connect(function()
    if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
        local hrp = player.Character.HumanoidRootPart
        hrp.Velocity = Vector3.new(hrp.Velocity.X, 0, hrp.Velocity.Z)
        hrp.Velocity = hrp.Velocity + Vector3.new(0, JumpForce, 0)
    end
end)

local function updateGlitchVisual()
    GlitchToggleBtn.Text = getgenv().GlideEnabled and "⚡ GLITCH: ON" or "⚡ GLITCH: OFF"
    GlitchToggleBtn.BackgroundColor3 = getgenv().GlideEnabled and Color3.fromRGB(80, 80, 80) or Color3.fromRGB(30, 30, 30)
end
GlitchToggleBtn.MouseButton1Click:Connect(function()
    getgenv().GlideEnabled = not getgenv().GlideEnabled
    updateGlitchVisual()
end)

-- ==========================================
-- PÁGINA FLY (GUI FLY V3 SEM BOTÕES UP/DOWN)
-- ==========================================

local FlyPage = pages.FlyPage

local flyFrame = Instance.new("Frame")
flyFrame.Size = UDim2.new(0, 190, 0, 57)
flyFrame.Position = UDim2.new(0.5, -95, 0.1, 0)
flyFrame.BackgroundColor3 = Color3.fromRGB(163, 255, 137)
flyFrame.BorderColor3 = Color3.fromRGB(103, 221, 213)
flyFrame.Parent = FlyPage
flyFrame.Active = true
flyFrame.Draggable = true
createBorder(flyFrame, 2)
Instance.new("UICorner", flyFrame).CornerRadius = UDim.new(0, 6)

local flyLabel = Instance.new("TextLabel")
flyLabel.Parent = flyFrame
flyLabel.BackgroundColor3 = Color3.fromRGB(242, 60, 255)
flyLabel.Size = UDim2.new(0, 100, 0, 28)
flyLabel.Position = UDim2.new(0.469327301, 0, 0, 0)
flyLabel.Font = Enum.Font.SourceSans
flyLabel.Text = "FLY GUI V3"
flyLabel.TextColor3 = Color3.fromRGB(0, 0, 0)
flyLabel.TextScaled = true
flyLabel.TextSize = 14
flyLabel.TextWrapped = true

local flyToggleBtn = Instance.new("TextButton")
flyToggleBtn.Name = "onof"
flyToggleBtn.Parent = flyFrame
flyToggleBtn.BackgroundColor3 = Color3.fromRGB(255, 249, 74)
flyToggleBtn.Size = UDim2.new(0, 56, 0, 28)
flyToggleBtn.Font = Enum.Font.SourceSans
flyToggleBtn.Text = "FLY"
flyToggleBtn.TextColor3 = Color3.fromRGB(0, 0, 0)
flyToggleBtn.TextSize = 14
flyToggleBtn.Position = UDim2.new(0.702823281, 0, 0.491228074, 0)

local plusBtn = Instance.new("TextButton")
plusBtn.Name = "plus"
plusBtn.Parent = flyFrame
plusBtn.BackgroundColor3 = Color3.fromRGB(133, 145, 255)
plusBtn.Size = UDim2.new(0, 45, 0, 28)
plusBtn.Font = Enum.Font.SourceSans
plusBtn.Text = "+"
plusBtn.TextColor3 = Color3.fromRGB(0, 0, 0)
plusBtn.TextScaled = true
plusBtn.TextSize = 14
plusBtn.TextWrapped = true
plusBtn.Position = UDim2.new(0.231578946, 0, 0, 0)

local speedLabel = Instance.new("TextLabel")
speedLabel.Name = "speed"
speedLabel.Parent = flyFrame
speedLabel.BackgroundColor3 = Color3.fromRGB(255, 85, 0)
speedLabel.Size = UDim2.new(0, 44, 0, 28)
speedLabel.Position = UDim2.new(0.468421042, 0, 0.491228074, 0)
speedLabel.Font = Enum.Font.SourceSans
speedLabel.Text = "1"
speedLabel.TextColor3 = Color3.fromRGB(0, 0, 0)
speedLabel.TextScaled = true
speedLabel.TextSize = 14
speedLabel.TextWrapped = true

local mineBtn = Instance.new("TextButton")
mineBtn.Name = "mine"
mineBtn.Parent = flyFrame
mineBtn.BackgroundColor3 = Color3.fromRGB(123, 255, 247)
mineBtn.Size = UDim2.new(0, 45, 0, 28)
mineBtn.Font = Enum.Font.SourceSans
mineBtn.Text = "-"
mineBtn.TextColor3 = Color3.fromRGB(0, 0, 0)
mineBtn.TextScaled = true
mineBtn.TextSize = 14
mineBtn.TextWrapped = true
mineBtn.Position = UDim2.new(0.231578946, 0, 0.491228074, 0)

local closeFlyBtn = Instance.new("TextButton")
closeFlyBtn.Name = "Close"
closeFlyBtn.Parent = flyFrame
closeFlyBtn.BackgroundColor3 = Color3.fromRGB(225, 25, 0)
closeFlyBtn.Font = Enum.Font.SourceSans
closeFlyBtn.Size = UDim2.new(0, 45, 0, 28)
closeFlyBtn.Text = "X"
closeFlyBtn.TextSize = 30
closeFlyBtn.Position = UDim2.new(0, 0, -1, 27)
closeFlyBtn.TextColor3 = Color3.fromRGB(255, 255, 255)

local miniBtn = Instance.new("TextButton")
miniBtn.Name = "minimize"
miniBtn.Parent = flyFrame
miniBtn.BackgroundColor3 = Color3.fromRGB(192, 150, 230)
miniBtn.Font = Enum.Font.SourceSans
miniBtn.Size = UDim2.new(0, 45, 0, 28)
miniBtn.Text = "-"
miniBtn.TextSize = 40
miniBtn.Position = UDim2.new(0, 44, -1, 27)
miniBtn.TextColor3 = Color3.fromRGB(0, 0, 0)

local mini2Btn = Instance.new("TextButton")
mini2Btn.Name = "minimize2"
mini2Btn.Parent = flyFrame
mini2Btn.BackgroundColor3 = Color3.fromRGB(192, 150, 230)
mini2Btn.Font = Enum.Font.SourceSans
mini2Btn.Size = UDim2.new(0, 45, 0, 28)
mini2Btn.Text = "+"
mini2Btn.TextSize = 40
mini2Btn.Position = UDim2.new(0, 44, -1, 57)
mini2Btn.Visible = false
mini2Btn.TextColor3 = Color3.fromRGB(0, 0, 0)

-- ==========================================
-- LÓGICA DO FLY (BASEADA NO FLY GUI V3)
-- ==========================================

local function stopFly()
    nowe = false
    if player.Character and player.Character:FindFirstChild("Humanoid") then
        local hum = player.Character.Humanoid
        hum:SetStateEnabled(Enum.HumanoidStateType.Climbing, true)
        hum:SetStateEnabled(Enum.HumanoidStateType.FallingDown, true)
        hum:SetStateEnabled(Enum.HumanoidStateType.Flying, true)
        hum:SetStateEnabled(Enum.HumanoidStateType.Freefall, true)
        hum:SetStateEnabled(Enum.HumanoidStateType.GettingUp, true)
        hum:SetStateEnabled(Enum.HumanoidStateType.Jumping, true)
        hum:SetStateEnabled(Enum.HumanoidStateType.Landed, true)
        hum:SetStateEnabled(Enum.HumanoidStateType.Physics, true)
        hum:SetStateEnabled(Enum.HumanoidStateType.PlatformStanding, true)
        hum:SetStateEnabled(Enum.HumanoidStateType.Ragdoll, true)
        hum:SetStateEnabled(Enum.HumanoidStateType.Running, true)
        hum:SetStateEnabled(Enum.HumanoidStateType.RunningNoPhysics, true)
        hum:SetStateEnabled(Enum.HumanoidStateType.Seated, true)
        hum:SetStateEnabled(Enum.HumanoidStateType.StrafingNoPhysics, true)
        hum:SetStateEnabled(Enum.HumanoidStateType.Swimming, true)
        hum:ChangeState(Enum.HumanoidStateType.RunningNoPhysics)
        hum.PlatformStand = false
    end
    if player.Character and player.Character:FindFirstChild("Animate") then
        player.Character.Animate.Disabled = false
    end
    tpwalking = false
    if FlyBodyGyro then FlyBodyGyro:Destroy() FlyBodyGyro = nil end
    if FlyBodyVelocity then FlyBodyVelocity:Destroy() FlyBodyVelocity = nil end
    flyToggleBtn.Text = "FLY"
    flyToggleBtn.BackgroundColor3 = Color3.fromRGB(255, 249, 74)
    speedLabel.Text = tostring(speeds)
end

local function startFly()
    nowe = true
    if player.Character and player.Character:FindFirstChild("Humanoid") then
        local hum = player.Character.Humanoid
        hum:SetStateEnabled(Enum.HumanoidStateType.Climbing, false)
        hum:SetStateEnabled(Enum.HumanoidStateType.FallingDown, false)
        hum:SetStateEnabled(Enum.HumanoidStateType.Flying, false)
        hum:SetStateEnabled(Enum.HumanoidStateType.Freefall, false)
        hum:SetStateEnabled(Enum.HumanoidStateType.GettingUp, false)
        hum:SetStateEnabled(Enum.HumanoidStateType.Jumping, false)
        hum:SetStateEnabled(Enum.HumanoidStateType.Landed, false)
        hum:SetStateEnabled(Enum.HumanoidStateType.Physics, false)
        hum:SetStateEnabled(Enum.HumanoidStateType.PlatformStanding, false)
        hum:SetStateEnabled(Enum.HumanoidStateType.Ragdoll, false)
        hum:SetStateEnabled(Enum.HumanoidStateType.Running, false)
        hum:SetStateEnabled(Enum.HumanoidStateType.RunningNoPhysics, false)
        hum:SetStateEnabled(Enum.HumanoidStateType.Seated, false)
        hum:SetStateEnabled(Enum.HumanoidStateType.StrafingNoPhysics, false)
        hum:SetStateEnabled(Enum.HumanoidStateType.Swimming, false)
        hum:ChangeState(Enum.HumanoidStateType.Swimming)
        hum.PlatformStand = true
    end
    if player.Character and player.Character:FindFirstChild("Animate") then
        player.Character.Animate.Disabled = true
    end

    -- Speed hack (TranslateBy)
    tpwalking = false
    for i = 1, speeds do
        spawn(function()
            local hb = RunService.Heartbeat
            tpwalking = true
            local chr = player.Character
            local hum = chr and chr:FindFirstChildOfClass("Humanoid")
            while tpwalking and hb:Wait() and chr and hum and hum.Parent do
                if hum.MoveDirection.Magnitude > 0 then
                    chr:TranslateBy(hum.MoveDirection)
                end
            end
        end)
    end

    local char = player.Character
    if not char then return end
    local torso = char:FindFirstChild("Torso") or char:FindFirstChild("UpperTorso")
    if not torso then return end

    if FlyBodyGyro then FlyBodyGyro:Destroy() end
    if FlyBodyVelocity then FlyBodyVelocity:Destroy() end

    FlyBodyGyro = Instance.new("BodyGyro", torso)
    FlyBodyGyro.P = 9e4
    FlyBodyGyro.maxTorque = Vector3.new(9e9, 9e9, 9e9)
    FlyBodyGyro.cframe = torso.CFrame

    FlyBodyVelocity = Instance.new("BodyVelocity", torso)
    FlyBodyVelocity.velocity = Vector3.new(0, 0.1, 0)
    FlyBodyVelocity.maxForce = Vector3.new(9e9, 9e9, 9e9)

    flyToggleBtn.Text = "FLY ON"
    flyToggleBtn.BackgroundColor3 = Color3.fromRGB(100, 255, 100)

    -- Loop do fly (controlado por MoveDirection)
    spawn(function()
        local ctrl = {f = 0, b = 0, l = 0, r = 0}
        local lastctrl = {f = 0, b = 0, l = 0, r = 0}
        local currentSpeed = 0
        local maxspeed = flyMaxSpeed * speeds

        while nowe do
            RunService.RenderStepped:Wait()
            local char = player.Character
            if not char then break end
            local hum = char:FindFirstChildOfClass("Humanoid")
            if not hum then break end

            local moveDir = hum.MoveDirection
            -- Mapeamento: X = lateral, Z = vertical (Z positivo = sobe)
            ctrl.r = moveDir.X > 0.1 and 1 or 0
            ctrl.l = moveDir.X < -0.1 and 1 or 0
            ctrl.f = moveDir.Z > 0.1 and 1 or 0   -- sobe
            ctrl.b = moveDir.Z < -0.1 and 1 or 0  -- desce

            -- Aceleração
            if ctrl.l + ctrl.r ~= 0 or ctrl.f + ctrl.b ~= 0 then
                currentSpeed = currentSpeed + 0.5 + (currentSpeed / maxspeed)
                if currentSpeed > maxspeed then currentSpeed = maxspeed end
            elseif currentSpeed ~= 0 then
                currentSpeed = currentSpeed - 1
                if currentSpeed < 0 then currentSpeed = 0 end
            end

            -- Calcula velocidade
            if (ctrl.l + ctrl.r) ~= 0 or (ctrl.f + ctrl.b) ~= 0 then
                local right = camera.CFrame.RightVector
                local up = Vector3.new(0, 1, 0)
                local vel = (right * (ctrl.r - ctrl.l)) + (up * (ctrl.f - ctrl.b))
                FlyBodyVelocity.velocity = vel * currentSpeed
                lastctrl = {f = ctrl.f, b = ctrl.b, l = ctrl.l, r = ctrl.r}
            elseif currentSpeed ~= 0 then
                local right = camera.CFrame.RightVector
                local up = Vector3.new(0, 1, 0)
                local vel = (right * (lastctrl.r - lastctrl.l)) + (up * (lastctrl.f - lastctrl.b))
                FlyBodyVelocity.velocity = vel * currentSpeed
            else
                FlyBodyVelocity.velocity = Vector3.new(0, 0, 0)
            end

            FlyBodyGyro.cframe = camera.CFrame
        end
    end)
end

-- Eventos dos botões do fly
flyToggleBtn.MouseButton1Down:Connect(function()
    if nowe then stopFly() else startFly() end
end)

plusBtn.MouseButton1Down:Connect(function()
    speeds = speeds + 1
    speedLabel.Text = tostring(speeds)
    if nowe then
        tpwalking = false
        for i = 1, speeds do
            spawn(function()
                local hb = RunService.Heartbeat
                tpwalking = true
                local chr = player.Character
                local hum = chr and chr:FindFirstChildOfClass("Humanoid")
                while tpwalking and hb:Wait() and chr and hum and hum.Parent do
                    if hum.MoveDirection.Magnitude > 0 then
                        chr:TranslateBy(hum.MoveDirection)
                    end
                end
            end)
        end
    end
end)

mineBtn.MouseButton1Down:Connect(function()
    if speeds == 1 then
        speedLabel.Text = "cannot be less than 1"
        task.wait(1)
        speedLabel.Text = tostring(speeds)
    else
        speeds = speeds - 1
        speedLabel.Text = tostring(speeds)
        if nowe then
            tpwalking = false
            for i = 1, speeds do
                spawn(function()
                    local hb = RunService.Heartbeat
                    tpwalking = true
                    local chr = player.Character
                    local hum = chr and chr:FindFirstChildOfClass("Humanoid")
                    while tpwalking and hb:Wait() and chr and hum and hum.Parent do
                        if hum.MoveDirection.Magnitude > 0 then
                            chr:TranslateBy(hum.MoveDirection)
                        end
                    end
                end)
            end
        end
    end
end)

closeFlyBtn.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

miniBtn.MouseButton1Click:Connect(function()
    flyToggleBtn.Visible = false
    plusBtn.Visible = false
    speedLabel.Visible = false
    mineBtn.Visible = false
    miniBtn.Visible = false
    mini2Btn.Visible = true
    flyFrame.BackgroundTransparency = 1
    closeFlyBtn.Position = UDim2.new(0, 0, -1, 57)
end)

mini2Btn.MouseButton1Click:Connect(function()
    flyToggleBtn.Visible = true
    plusBtn.Visible = true
    speedLabel.Visible = true
    mineBtn.Visible = true
    miniBtn.Visible = true
    mini2Btn.Visible = false
    flyFrame.BackgroundTransparency = 0
    closeFlyBtn.Position = UDim2.new(0, 0, -1, 27)
end)

player.CharacterAdded:Connect(function(char)
    task.wait(0.7)
    if char and char:FindFirstChild("Humanoid") then
        char.Humanoid.PlatformStand = false
        if char:FindFirstChild("Animate") then
            char.Animate.Disabled = false
        end
    end
    if nowe then
        stopFly()
        startFly()
    end
end)

-- ==========================================
-- PÁGINA HITBOX
-- ==========================================

local HitboxToggleBtn = createToggleButton("🎯 HITBOX: OFF", 5, pages.HitboxPage)
local HitboxSizeInput = createInputField("Tamanho:", getgenv().HitboxSize, 45, pages.HitboxPage)
local HitboxTransInput = createInputField("Transparência:", getgenv().HitboxTransparency, 75, pages.HitboxPage)
local TeamCheckBtn = createToggleButton("👥 TEAM CHECK: OFF", 115, pages.HitboxPage)
local function updateHitboxVisual()
    HitboxToggleBtn.Text = getgenv().HitboxStatus and "🎯 HITBOX: ON" or "🎯 HITBOX: OFF"
    HitboxToggleBtn.BackgroundColor3 = getgenv().HitboxStatus and Color3.fromRGB(80, 80, 80) or Color3.fromRGB(30, 30, 30)
end
local function updateTeamCheckVisual()
    TeamCheckBtn.Text = getgenv().TeamCheck and "👥 TEAM CHECK: ON" or "👥 TEAM CHECK: OFF"
    TeamCheckBtn.BackgroundColor3 = getgenv().TeamCheck and Color3.fromRGB(80, 80, 80) or Color3.fromRGB(30, 30, 30)
end
HitboxToggleBtn.MouseButton1Click:Connect(function()
    getgenv().HitboxStatus = not getgenv().HitboxStatus
    updateHitboxVisual()
end)
TeamCheckBtn.MouseButton1Click:Connect(function()
    getgenv().TeamCheck = not getgenv().TeamCheck
    updateTeamCheckVisual()
end)

-- ==========================================
-- PÁGINA AIMBOT (resumida para não estender demais)
-- ==========================================

local AimbotPage = pages.AimbotPage

local AimbotToggleBtn = createToggleButton("🔫 Aimbot: OFF", 5, AimbotPage)
local function updateAimbotToggleVisual()
    AimbotToggleBtn.Text = aimbotActive and "🔫 Aimbot: ON" or "🔫 Aimbot: OFF"
    AimbotToggleBtn.BackgroundColor3 = aimbotActive and Color3.fromRGB(80, 80, 80) or Color3.fromRGB(30, 30, 30)
end
AimbotToggleBtn.MouseButton1Click:Connect(function()
    aimbotActive = not aimbotActive
    updateAimbotToggleVisual()
end)

local PlayerListFrame = Instance.new("Frame")
PlayerListFrame.Size = UDim2.new(0.9, 0, 0, 200)
PlayerListFrame.Position = UDim2.new(0.05, 0, 0, 45)
PlayerListFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
PlayerListFrame.BackgroundTransparency = 0.2
PlayerListFrame.Parent = AimbotPage
Instance.new("UICorner", PlayerListFrame).CornerRadius = UDim.new(0, 6)
createBorder(PlayerListFrame, 2)

local ScrollingFrame = Instance.new("ScrollingFrame")
ScrollingFrame.Size = UDim2.new(1, 0, 1, 0)
ScrollingFrame.BackgroundTransparency = 1
ScrollingFrame.CanvasSize = UDim2.new(0, 0, 0, 0)
ScrollingFrame.ScrollBarThickness = 4
ScrollingFrame.Parent = PlayerListFrame

local PlayerListLayout = Instance.new("UIListLayout")
PlayerListLayout.Padding = UDim.new(0, 2)
PlayerListLayout.SortOrder = Enum.SortOrder.Name
PlayerListLayout.Parent = ScrollingFrame

local playerButtons = {}
local selectedPlayerButton = nil

local function updateAimbotList()
    for _, btn in ipairs(playerButtons) do btn:Destroy() end
    playerButtons = {}
    selectedPlayerButton = nil

    for _, plr in ipairs(Players:GetPlayers()) do
        if plr ~= player then
            local btn = Instance.new("TextButton")
            btn.Size = UDim2.new(1, 0, 0, 25)
            btn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
            btn.Text = plr.Name
            btn.TextColor3 = Color3.fromRGB(200, 200, 200)
            btn.Font = Enum.Font.GothamBold
            btn.TextSize = 12
            btn.Parent = ScrollingFrame
            Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 4)
            createBorder(btn, 1)
            
            btn.MouseButton1Click:Connect(function()
                aimbotTarget = plr
                if selectedPlayerButton then
                    selectedPlayerButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
                end
                btn.BackgroundColor3 = Color3.fromRGB(80, 80, 80)
                selectedPlayerButton = btn
                StarterGui:SetCore("SendNotification", {
                    Title = "🎯 Alvo selecionado",
                    Text = "Mirando em " .. plr.Name,
                    Duration = 2
                })
            end)

            table.insert(playerButtons, btn)
        end
    end
    ScrollingFrame.CanvasSize = UDim2.new(0, 0, 0, #playerButtons * 27)
end

Players.PlayerAdded:Connect(updateAimbotList)
Players.PlayerRemoving:Connect(updateAimbotList)
updateAimbotList()

local ClearTargetBtn = Instance.new("TextButton")
ClearTargetBtn.Size = UDim2.new(0.4, 0, 0, 25)
ClearTargetBtn.Position = UDim2.new(0.05, 0, 0, 255)
ClearTargetBtn.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
ClearTargetBtn.Text = "✖ Limpar Alvo"
ClearTargetBtn.TextColor3 = Color3.fromRGB(200, 200, 200)
ClearTargetBtn.Font = Enum.Font.GothamBold
ClearTargetBtn.TextSize = 12
ClearTargetBtn.Parent = AimbotPage
Instance.new("UICorner", ClearTargetBtn).CornerRadius = UDim.new(0, 5)
createBorder(ClearTargetBtn, 2)
ClearTargetBtn.MouseEnter:Connect(function()
    TweenService:Create(ClearTargetBtn, TweenInfo.new(0.15), {BackgroundColor3 = Color3.fromRGB(60, 60, 60)}):Play()
end)
ClearTargetBtn.MouseLeave:Connect(function()
    TweenService:Create(ClearTargetBtn, TweenInfo.new(0.15), {BackgroundColor3 = Color3.fromRGB(30, 30, 30)}):Play()
end)
ClearTargetBtn.MouseButton1Click:Connect(function()
    aimbotTarget = nil
    if selectedPlayerButton then
        selectedPlayerButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
        selectedPlayerButton = nil
    end
    StarterGui:SetCore("SendNotification", {
        Title = "🧹 Alvo removido",
        Text = "Nenhum alvo selecionado",
        Duration = 2
    })
end)

-- ==========================================
-- PÁGINA UTILS (resumida)
-- ==========================================

local PingLabel = Instance.new("TextLabel")
PingLabel.Size = UDim2.new(1, 0, 0, 20)
PingLabel.Position = UDim2.new(0, 0, 0, 5)
PingLabel.BackgroundTransparency = 1
PingLabel.Text = "📶 Ping: 0 ms"
PingLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
PingLabel.Font = Enum.Font.GothamBold
PingLabel.TextSize = 13
PingLabel.Parent = pages.UtilsPage

local AutoClickBtn = createToggleButton("🖱️ AUTO CLICK: OFF", 35, pages.UtilsPage)
local AutoClickIntervalInput = createInputField("Intervalo (s):", autoClickInterval, 70, pages.UtilsPage)
local FastAttackBtn = createToggleButton("⚡ FAST ATTACK: OFF", 110, pages.UtilsPage)
local FastAttackIntervalInput = createInputField("Intervalo (s):", fastAttackInterval, 145, pages.UtilsPage)

local function updateAutoClickVisual()
    AutoClickBtn.Text = autoClickActive and "🖱️ AUTO CLICK: ON" or "🖱️ AUTO CLICK: OFF"
    AutoClickBtn.BackgroundColor3 = autoClickActive and Color3.fromRGB(80, 80, 80) or Color3.fromRGB(30, 30, 30)
end
local function updateFastAttackVisual()
    FastAttackBtn.Text = fastAttackActive and "⚡ FAST ATTACK: ON" or "⚡ FAST ATTACK: OFF"
    FastAttackBtn.BackgroundColor3 = fastAttackActive and Color3.fromRGB(80, 80, 80) or Color3.fromRGB(30, 30, 30)
end
AutoClickBtn.MouseButton1Click:Connect(function()
    autoClickActive = not autoClickActive
    updateAutoClickVisual()
end)
FastAttackBtn.MouseButton1Click:Connect(function()
    fastAttackActive = not fastAttackActive
    updateFastAttackVisual()
end)
AutoClickIntervalInput.FocusLost:Connect(function()
    local val = tonumber(AutoClickIntervalInput.Text)
    if val and val > 0 then autoClickInterval = val end
end)
FastAttackIntervalInput.FocusLost:Connect(function()
    local val = tonumber(FastAttackIntervalInput.Text)
    if val and val > 0 then fastAttackInterval = val end
end)

local extraY = 185
local AntiAFKBtn = createToggleButton("🛡️ ANTI-AFK: OFF", extraY, pages.UtilsPage); extraY = extraY + 38
local AutoFarmBtn = createToggleButton("🌾 AUTO-FARM: OFF", extraY, pages.UtilsPage); extraY = extraY + 38
local GodModeBtn = createToggleButton("💀 GOD MODE: OFF", extraY, pages.UtilsPage); extraY = extraY + 38
local InfiniteJumpBtn = createToggleButton("🦘 INFINITE JUMP: OFF", extraY, pages.UtilsPage); extraY = extraY + 38
local SpeedHackBtn = createToggleButton("💨 SPEED HACK: OFF", extraY, pages.UtilsPage); extraY = extraY + 38
local SpeedHackInput = createInputField("Speed Value:", speedHackValue, extraY, pages.UtilsPage); extraY = extraY + 35

local function updateMiscVisuals()
    AntiAFKBtn.Text = antiAFK and "🛡️ ANTI-AFK: ON" or "🛡️ ANTI-AFK: OFF"
    AntiAFKBtn.BackgroundColor3 = antiAFK and Color3.fromRGB(80, 80, 80) or Color3.fromRGB(30, 30, 30)
    AutoFarmBtn.Text = autoFarm and "🌾 AUTO-FARM: ON" or "🌾 AUTO-FARM: OFF"
    AutoFarmBtn.BackgroundColor3 = autoFarm and Color3.fromRGB(80, 80, 80) or Color3.fromRGB(30, 30, 30)
    GodModeBtn.Text = godMode and "💀 GOD MODE: ON" or "💀 GOD MODE: OFF"
    GodModeBtn.BackgroundColor3 = godMode and Color3.fromRGB(80, 80, 80) or Color3.fromRGB(30, 30, 30)
    InfiniteJumpBtn.Text = infiniteJump and "🦘 INFINITE JUMP: ON" or "🦘 INFINITE JUMP: OFF"
    InfiniteJumpBtn.BackgroundColor3 = infiniteJump and Color3.fromRGB(80, 80, 80) or Color3.fromRGB(30, 30, 30)
    SpeedHackBtn.Text = speedHack and "💨 SPEED HACK: ON" or "💨 SPEED HACK: OFF"
    SpeedHackBtn.BackgroundColor3 = speedHack and Color3.fromRGB(80, 80, 80) or Color3.fromRGB(30, 30, 30)
end

AntiAFKBtn.MouseButton1Click:Connect(function()
    antiAFK = not antiAFK
    updateMiscVisuals()
end)
AutoFarmBtn.MouseButton1Click:Connect(function()
    autoFarm = not autoFarm
    updateMiscVisuals()
end)
GodModeBtn.MouseButton1Click:Connect(function()
    godMode = not godMode
    updateMiscVisuals()
end)
InfiniteJumpBtn.MouseButton1Click:Connect(function()
    infiniteJump = not infiniteJump
    updateMiscVisuals()
end)
SpeedHackBtn.MouseButton1Click:Connect(function()
    speedHack = not speedHack
    updateMiscVisuals()
end)
SpeedHackInput.FocusLost:Connect(function()
    local val = tonumber(SpeedHackInput.Text)
    if val and val > 0 then speedHackValue = val end
end)

-- ==========================================
-- PÁGINA OTIMIZAÇÃO
-- ==========================================

local optY = 5
local OptParticlesBtn = createToggleButton("❄️ Reduzir Partículas: OFF", optY, pages.OptimizationPage); optY = optY + 38
local OptFogBtn = createToggleButton("🌫️ Remover Névoa: OFF", optY, pages.OptimizationPage); optY = optY + 38
local OptFPSInput = createInputField("Max FPS:", maxFPS, optY, pages.OptimizationPage); optY = optY + 35
local OptDecorBtn = createToggleButton("🗑️ Remover Decorações: OFF", optY, pages.OptimizationPage); optY = optY + 38
local OptWaterBtn = createToggleButton("💧 Remover Efeitos Água: OFF", optY, pages.OptimizationPage); optY = optY + 38
local OptShadowsBtn = createToggleButton("🌑 Desativar Sombras: OFF", optY, pages.OptimizationPage); optY = optY + 38
local OptAnimBtn = createToggleButton("🎬 Desativar Animações: OFF", optY, pages.OptimizationPage); optY = optY + 38

local function updateOptVisuals()
    OptParticlesBtn.Text = reduceParticles and "❄️ Reduzir Partículas: ON" or "❄️ Reduzir Partículas: OFF"
    OptParticlesBtn.BackgroundColor3 = reduceParticles and Color3.fromRGB(80, 80, 80) or Color3.fromRGB(30, 30, 30)
    OptFogBtn.Text = removeFog and "🌫️ Remover Névoa: ON" or "🌫️ Remover Névoa: OFF"
    OptFogBtn.BackgroundColor3 = removeFog and Color3.fromRGB(80, 80, 80) or Color3.fromRGB(30, 30, 30)
    OptDecorBtn.Text = removeDecorations and "🗑️ Remover Decorações: ON" or "🗑️ Remover Decorações: OFF"
    OptDecorBtn.BackgroundColor3 = removeDecorations and Color3.fromRGB(80, 80, 80) or Color3.fromRGB(30, 30, 30)
    OptWaterBtn.Text = removeWaterEffects and "💧 Remover Efeitos Água: ON" or "💧 Remover Efeitos Água: OFF"
    OptWaterBtn.BackgroundColor3 = removeWaterEffects and Color3.fromRGB(80, 80, 80) or Color3.fromRGB(30, 30, 30)
    OptShadowsBtn.Text = disableShadows and "🌑 Desativar Sombras: ON" or "🌑 Desativar Sombras: OFF"
    OptShadowsBtn.BackgroundColor3 = disableShadows and Color3.fromRGB(80, 80, 80) or Color3.fromRGB(30, 30, 30)
    OptAnimBtn.Text = disableAnimations and "🎬 Desativar Animações: ON" or "🎬 Desativar Animações: OFF"
    OptAnimBtn.BackgroundColor3 = disableAnimations and Color3.fromRGB(80, 80, 80) or Color3.fromRGB(30, 30, 30)
end

OptParticlesBtn.MouseButton1Click:Connect(function()
    reduceParticles = not reduceParticles
    updateOptVisuals()
end)
OptFogBtn.MouseButton1Click:Connect(function()
    removeFog = not removeFog
    Lighting.FogEnd = removeFog and 0 or 100000
    updateOptVisuals()
end)
OptFPSInput.FocusLost:Connect(function()
    local val = tonumber(OptFPSInput.Text)
    if val and val > 0 then maxFPS = val end
end)
OptDecorBtn.MouseButton1Click:Connect(function()
    removeDecorations = not removeDecorations
    updateOptVisuals()
end)
OptWaterBtn.MouseButton1Click:Connect(function()
    removeWaterEffects = not removeWaterEffects
    updateOptVisuals()
end)
OptShadowsBtn.MouseButton1Click:Connect(function()
    disableShadows = not disableShadows
    Workspace.GlobalShadows = not disableShadows
    updateOptVisuals()
end)
OptAnimBtn.MouseButton1Click:Connect(function()
    disableAnimations = not disableAnimations
    updateOptVisuals()
end)

-- ==========================================
-- PÁGINA VISUAL
-- ==========================================

local visY = 5
local ChamsBtn = createToggleButton("🌈 CHAMS: OFF", visY, pages.VisualPage); visY = visY + 38
local WireframeBtn = createToggleButton("🔲 WIREFRAME: OFF", visY, pages.VisualPage); visY = visY + 38
local ZoomInput = createInputField("Zoom (1-10):", zoomLevel, visY, pages.VisualPage); visY = visY + 35
local FOVInput = createInputField("FOV (10-120):", fovLevel, visY, pages.VisualPage); visY = visY + 35

local function updateVisualVisuals()
    ChamsBtn.Text = chamsEnabled and "🌈 CHAMS: ON" or "🌈 CHAMS: OFF"
    ChamsBtn.BackgroundColor3 = chamsEnabled and Color3.fromRGB(80, 80, 80) or Color3.fromRGB(30, 30, 30)
    WireframeBtn.Text = wireframeEnabled and "🔲 WIREFRAME: ON" or "🔲 WIREFRAME: OFF"
    WireframeBtn.BackgroundColor3 = wireframeEnabled and Color3.fromRGB(80, 80, 80) or Color3.fromRGB(30, 30, 30)
end

ChamsBtn.MouseButton1Click:Connect(function()
    chamsEnabled = not chamsEnabled
    updateVisualVisuals()
end)
WireframeBtn.MouseButton1Click:Connect(function()
    wireframeEnabled = not wireframeEnabled
    updateVisualVisuals()
end)
ZoomInput.FocusLost:Connect(function()
    local val = tonumber(ZoomInput.Text)
    if val and val >= 1 and val <= 10 then zoomLevel = val end
end)
FOVInput.FocusLost:Connect(function()
    local val = tonumber(FOVInput.Text)
    if val and val >= 10 and val <= 120 then fovLevel = val end
end)

-- ==========================================
-- BOTÃO FLUTUANTE MOBILE
-- ==========================================

local FloatingBtn = Instance.new("ImageButton")
FloatingBtn.Size = UDim2.new(0, 45, 0, 45)
FloatingBtn.Position = UDim2.new(1, -55, 1, -60)
FloatingBtn.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
FloatingBtn.Image = "rbxassetid://1297908080"
FloatingBtn.ImageColor3 = Color3.fromRGB(255, 255, 255)
FloatingBtn.BackgroundTransparency = 0.2
FloatingBtn.Parent = ScreenGui
Instance.new("UICorner", FloatingBtn).CornerRadius = UDim.new(1, 0)
createBorder(FloatingBtn, 2)

FloatingBtn.MouseButton1Click:Connect(function()
    uiVisible = not uiVisible
    MainFrame.Visible = uiVisible
end)

-- ==========================================
-- LOGIN
-- ==========================================

LoginBtn.MouseButton1Click:Connect(function()
    if KeyInput.Text == CorrectKey then
        if Background then Background:Destroy() end
        StatusText.Text = "✅ KEY CORRETA! Carregando..."
        StatusText.TextColor3 = Color3.fromRGB(200, 255, 200)
        TweenService:Create(KeyFrame, TweenInfo.new(0.3), {BackgroundTransparency = 1}):Play()
        task.wait(0.3)
        KeyFrame.Visible = false
        animateMainIn()
        StarterGui:SetCore("SendNotification", {Title = "⚡ DARK PREMIUM", Text = "Key Correta! Bem-vindo!", Duration = 3})
    else
        StatusText.Text = "❌ KEY INCORRETA! Tente novamente."
        StatusText.TextColor3 = Color3.fromRGB(255, 100, 100)
        TweenService:Create(KeyFrame, TweenInfo.new(0.1), {Position = UDim2.new(0.5, -190, 0.5, -100)}):Play()
        task.wait(0.1)
        TweenService:Create(KeyFrame, TweenInfo.new(0.1), {Position = UDim2.new(0.5, -170, 0.5, -100)}):Play()
        task.wait(0.1)
        TweenService:Create(KeyFrame, TweenInfo.new(0.1), {Position = UDim2.new(0.5, -180, 0.5, -100)}):Play()
        KeyInput.Text = ""
        task.wait(1)
        StatusText.Text = "🔑 Aguardando chave..."
        StatusText.TextColor3 = Color3.fromRGB(200, 200, 200)
    end
end)

KeyInput:GetPropertyChangedSignal("Text"):Connect(function()
    if KeyInput.Text == CorrectKey then LoginBtn.MouseButton1Click:Fire() end
end)

-- ==========================================
-- ESP SISTEMA
-- ==========================================

local function getColor(plr)
    if plr.Team and player.Team and plr.Team == player.Team then return Color3.fromRGB(200, 200, 200) end
    return Color3.fromRGB(255, 50, 50)
end

local function createEsp(plr)
    if plr == player then return end
    local folder = Instance.new("Folder")
    folder.Name = "ESP_" .. plr.Name
    folder.Parent = guiParent
    local highlight = Instance.new("Highlight")
    highlight.Name = "Highlight"
    highlight.FillTransparency = 0.5
    highlight.OutlineTransparency = 0
    highlight.Parent = folder
    local billboard = Instance.new("BillboardGui")
    billboard.Name = "NameTag"
    billboard.Size = UDim2.new(0, 150, 0, 40)
    billboard.Adornee = nil
    billboard.AlwaysOnTop = true
    billboard.Parent = folder
    local nameLabel = Instance.new("TextLabel")
    nameLabel.Size = UDim2.new(1, 0, 1, 0)
    nameLabel.BackgroundTransparency = 1
    nameLabel.TextColor3 = Color3.new(1, 1, 1)
    nameLabel.Font = Enum.Font.GothamBold
    nameLabel.TextSize = 11
    nameLabel.TextStrokeTransparency = 0
    nameLabel.Parent = billboard
    local function update()
        if espEnabled and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") and plr.Character:FindFirstChild("Humanoid") and plr.Character.Humanoid.Health > 0 then
            highlight.Adornee = plr.Character
            highlight.FillColor = getColor(plr)
            billboard.Adornee = plr.Character:FindFirstChild("Head")
            billboard.Enabled = true
            nameLabel.Text = plr.Name .. " [" .. math.floor(plr.Character.Humanoid.Health) .. "]"
            nameLabel.TextColor3 = getColor(plr)
        else
            highlight.Adornee = nil
            billboard.Enabled = false
        end
    end
    RunService.RenderStepped:Connect(update)
end

for _, p in ipairs(Players:GetPlayers()) do createEsp(p) end
Players.PlayerAdded:Connect(createEsp)

-- ==========================================
-- Sincronização de inputs
-- ==========================================

local function ResetJump()
    if player.Character and player.Character:FindFirstChild("Humanoid") then
        player.Character.Humanoid.JumpPower = 50
        player.Character.Humanoid.UseJumpPower = true
    end
end
ResetJump()
player.CharacterAdded:Connect(ResetJump)

GlitchSpeedInput.FocusLost:Connect(function() getgenv().GlideSpeed = tonumber(GlitchSpeedInput.Text) or getgenv().GlideSpeed end)
GlitchDurInput.FocusLost:Connect(function() getgenv().GlideTime = tonumber(GlitchDurInput.Text) or getgenv().GlideTime end)
JumpPowerInput.FocusLost:Connect(function() JumpForce = tonumber(JumpPowerInput.Text) or JumpForce end)
HitboxSizeInput.FocusLost:Connect(function() getgenv().HitboxSize = tonumber(HitboxSizeInput.Text) or getgenv().HitboxSize end)
HitboxTransInput.FocusLost:Connect(function() getgenv().HitboxTransparency = tonumber(HitboxTransInput.Text) or getgenv().HitboxTransparency end)

-- ==========================================
-- GLITCH DETECTION (Blox Fruits)
-- ==========================================

local function SetupToolDetection(character)
    character.ChildAdded:Connect(function(child)
        if child:IsA("Tool") and (string.find(child.Name, "Soul") or string.find(child.Name, "Guitar")) then
            child.Activated:Connect(function()
                if not getgenv().GlideEnabled then return end
                LastShootTime = tick()
            end)
        end
    end)
end
if player.Character then SetupToolDetection(player.Character) end
player.CharacterAdded:Connect(SetupToolDetection)

-- ==========================================
-- INPUTS PRINCIPAIS (teclas)
-- ==========================================

UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    if input.KeyCode == Enum.KeyCode.K then
        uiVisible = not uiVisible
        MainFrame.Visible = uiVisible
    end
    if input.KeyCode == Enum.KeyCode.H then
        if nowe then stopFly() else startFly() end
    end
    if input.KeyCode == Enum.KeyCode.B then
        espEnabled = not espEnabled
        updateEspVisual()
    end
    if input.KeyCode == Enum.KeyCode.G then
        if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            local hrp = player.Character.HumanoidRootPart
            hrp.Velocity = Vector3.new(hrp.Velocity.X, 0, hrp.Velocity.Z)
            hrp.Velocity = hrp.Velocity + Vector3.new(0, JumpForce, 0)
        end
    end
end)

-- ==========================================
-- LOOPS PRINCIPAIS
-- ==========================================

RunService.RenderStepped:Connect(function()
    pingDisplay = player:GetNetworkPing() * 1000
    PingLabel.Text = string.format("📶 Ping: %.0f ms", pingDisplay)

    -- Aimbot
    if aimbotActive and aimbotTarget and aimbotTarget.Character and aimbotTarget.Character:FindFirstChild("HumanoidRootPart") then
        camera.CFrame = CFrame.new(camera.CFrame.Position, aimbotTarget.Character.HumanoidRootPart.Position)
    end

    -- Hitbox
    if getgenv().HitboxStatus == true then
        for i,v in next, Players:GetPlayers() do
            if v ~= player then
                local onSameTeam = (v.Team ~= nil and player.Team ~= nil and v.Team == player.Team)
                if not getgenv().TeamCheck or (getgenv().TeamCheck and not onSameTeam) then
                    pcall(function()
                        local hrp = v.Character.HumanoidRootPart
                        hrp.Size = Vector3.new(getgenv().HitboxSize, getgenv().HitboxSize, getgenv().HitboxSize)
                        hrp.Transparency = getgenv().HitboxTransparency
                        hrp.BrickColor = BrickColor.new("Really red")
                        hrp.Material = "Neon"
                        hrp.CanCollide = false
                    end)
                end
            end
        end
    else
        for i,v in next, Players:GetPlayers() do
            if v ~= player then
                pcall(function()
                    local hrp = v.Character.HumanoidRootPart
                    hrp.Size = Vector3.new(2,2,1)
                    hrp.Transparency = 1
                end)
            end
        end
    end

    if camera then
        camera.FieldOfView = fovLevel
    end

    -- Chams
    if chamsEnabled then
        for i,v in next, Players:GetPlayers() do
            if v ~= player and v.Character then
                pcall(function()
                    for _, part in ipairs(v.Character:GetDescendants()) do
                        if part:IsA("BasePart") then
                            part.Color = chamsColor
                            part.Material = "Neon"
                            part.Transparency = 0.3
                        end
                    end
                end)
            end
        end
    end

    -- Wireframe
    if wireframeEnabled then
        for i,v in next, Players:GetPlayers() do
            if v ~= player and v.Character then
                pcall(function()
                    for _, part in ipairs(v.Character:GetDescendants()) do
                        if part:IsA("BasePart") then
                            part.Material = "ForceField"
                        end
                    end
                end)
            end
        end
    end

    -- Anti-AFK
    if antiAFK then
        VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.W, false, game)
        VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.W, false, game)
    end

    -- God Mode
    if godMode and player.Character and player.Character:FindFirstChild("Humanoid") then
        player.Character.Humanoid.Health = 100
        player.Character.Humanoid.MaxHealth = 100
    end

    -- Infinite Jump
    if infiniteJump and player.Character and player.Character:FindFirstChild("Humanoid") then
        player.Character.Humanoid.UseJumpPower = true
        player.Character.Humanoid.JumpPower = 1000
    else
        if player.Character and player.Character:FindFirstChild("Humanoid") then
            player.Character.Humanoid.JumpPower = 50
        end
    end

    -- Speed Hack
    if speedHack and player.Character and player.Character:FindFirstChild("Humanoid") then
        player.Character.Humanoid.WalkSpeed = speedHackValue
    else
        if player.Character and player.Character:FindFirstChild("Humanoid") then
            player.Character.Humanoid.WalkSpeed = 16
        end
    end
end)

-- Loop Glitch
RunService.Heartbeat:Connect(function()
    if not getgenv().GlideEnabled then return end
    local char = player.Character
    if char and char:FindFirstChild("HumanoidRootPart") and char:FindFirstChild("Humanoid") then
        local hrp = char.HumanoidRootPart
        local hum = char.Humanoid
        if (tick() - LastShootTime < 1.5) and hrp.Velocity.Magnitude > 20 then
            if hrp:FindFirstChild("OmniGlide") then hrp.OmniGlide:Destroy() end
            local CurrentBV = Instance.new("BodyVelocity")
            CurrentBV.Name = "OmniGlide"
            CurrentBV.MaxForce = Vector3.new(100000, 0, 100000)
            local Dir = hum.MoveDirection
            if Dir.Magnitude == 0 then
                local cameraLook = camera.CFrame.LookVector
                Dir = Vector3.new(cameraLook.X, 0, cameraLook.Z).Unit
            else
                Dir = Dir.Unit
            end
            CurrentBV.Velocity = Dir * getgenv().GlideSpeed
            CurrentBV.Parent = hrp
            Debris:AddItem(CurrentBV, getgenv().GlideTime)
            LastShootTime = 0
        end
    end
end)

-- Auto Click e Fast Attack
task.spawn(function()
    while true do
        task.wait(0.05)
        if autoClickActive then
            VirtualInputManager:SendMouseButtonEvent(0, 0, 0, true, game, 1)
            VirtualInputManager:SendMouseButtonEvent(0, 0, 0, false, game, 1)
            task.wait(autoClickInterval)
        end
        if fastAttackActive then
            VirtualInputManager:SendMouseButtonEvent(1051, 927, 0, true, game, 1)
            VirtualInputManager:SendMouseButtonEvent(1051, 927, 0, false, game, 1)
            task.wait(fastAttackInterval)
        end
    end
end)

StarterGui:SetCore("SendNotification", {
    Title = "⚡ DARKNESSXY HUB ⚡",
    Text = "Fly GUI V3 integrado! Analógico: CIMA=SOBE, BAIXO=DESCE.",
    Duration = 4
})
