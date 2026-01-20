-- Roblox Aimbot & ESP Script para Mobile
-- Funciona automaticamente, sem necessidade de segurar botões

-- Serviços
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local CoreGui = game:GetService("CoreGui")
local GuiService = game:GetService("GuiService")

-- Variáveis
local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera

-- Configurações
local Settings = {
    Aimbot = {
        Enabled = false,
        FOV = 100,
        Smoothness = 0.3,
        TeamCheck = true,
        VisibleCheck = true,
        AutoFire = false,  -- Atirar automaticamente
        AimPart = "Head"   -- Parte do corpo para mirar
    },
    ESP = {
        Enabled = false,
        Boxes = true,
        Tracers = false,
        Names = true,
        Distance = true
    }
}

-- Criar a Interface
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "AimbotESPMobile"
ScreenGui.Parent = CoreGui
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.ResetOnSpawn = false

-- Frame Principal
local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, 380, 0, 500)
MainFrame.Position = UDim2.new(0.5, -190, 0.5, -250)
MainFrame.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Parent = ScreenGui

-- Borda Neon Animada
local BorderFrame = Instance.new("Frame")
BorderFrame.Name = "BorderFrame"
BorderFrame.Size = UDim2.new(1, 6, 1, 6)
BorderFrame.Position = UDim2.new(0, -3, 0, -3)
BorderFrame.BackgroundColor3 = Color3.fromRGB(0, 255, 255)
BorderFrame.BorderSizePixel = 0
BorderFrame.Parent = MainFrame

local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(0, 12)
UICorner.Parent = MainFrame

local UICorner2 = Instance.new("UICorner")
UICorner2.CornerRadius = UDim.new(0, 15)
UICorner2.Parent = BorderFrame

-- Animar borda neon
local borderHue = 0
spawn(function()
    while true do
        borderHue = (borderHue + 1) % 360
        local color = Color3.fromHSV(borderHue/360, 0.8, 1)
        BorderFrame.BackgroundColor3 = color
        wait(0.03)
    end
end)

-- Barra de Título
local TitleBar = Instance.new("Frame")
TitleBar.Name = "TitleBar"
TitleBar.Size = UDim2.new(1, 0, 0, 45)
TitleBar.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
TitleBar.BorderSizePixel = 0
TitleBar.Parent = MainFrame

local UICorner3 = Instance.new("UICorner")
UICorner3.CornerRadius = UDim.new(0, 12)
UICorner3.Parent = TitleBar

local TitleLabel = Instance.new("TextLabel")
TitleLabel.Name = "TitleLabel"
TitleLabel.Size = UDim2.new(0.7, 0, 1, 0)
TitleLabel.Position = UDim2.new(0, 15, 0, 0)
TitleLabel.BackgroundTransparency = 1
TitleLabel.Text = "MOBILE AIMBOT PRO"
TitleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
TitleLabel.TextSize = 20
TitleLabel.Font = Enum.Font.GothamBold
TitleLabel.TextXAlignment = Enum.TextXAlignment.Left
TitleLabel.Parent = TitleBar

-- Botão Minimizar
local MinimizeButton = Instance.new("TextButton")
MinimizeButton.Name = "MinimizeButton"
MinimizeButton.Size = UDim2.new(0, 35, 0, 35)
MinimizeButton.Position = UDim2.new(1, -45, 0.5, -17.5)
MinimizeButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
MinimizeButton.Text = "-"
MinimizeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
MinimizeButton.TextSize = 24
MinimizeButton.Font = Enum.Font.GothamBold
MinimizeButton.Parent = TitleBar

local UICorner4 = Instance.new("UICorner")
UICorner4.CornerRadius = UDim.new(0, 8)
UICorner4.Parent = MinimizeButton

-- Ícone Minimizado
local MinimizedIcon = Instance.new("TextButton")
MinimizedIcon.Name = "MinimizedIcon"
MinimizedIcon.Size = UDim2.new(0, 55, 0, 55)
MinimizedIcon.Position = UDim2.new(1, -65, 0, 25)
MinimizedIcon.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
MinimizedIcon.BackgroundTransparency = 0.2
MinimizedIcon.Text = "🎯"
MinimizedIcon.TextColor3 = Color3.fromRGB(255, 255, 255)
MinimizedIcon.TextSize = 28
MinimizedIcon.Visible = false
MinimizedIcon.Active = true
MinimizedIcon.Draggable = true
MinimizedIcon.Parent = ScreenGui

local UICorner5 = Instance.new("UICorner")
UICorner5.CornerRadius = UDim.new(0, 27)
UICorner5.Parent = MinimizedIcon

-- Conteúdo Principal
local ContentFrame = Instance.new("Frame")
ContentFrame.Name = "ContentFrame"
ContentFrame.Size = UDim2.new(1, -20, 1, -75)
ContentFrame.Position = UDim2.new(0, 10, 0, 65)
ContentFrame.BackgroundTransparency = 1
ContentFrame.Parent = MainFrame

-- Seção Aimbot
local AimbotSection = Instance.new("Frame")
AimbotSection.Name = "AimbotSection"
AimbotSection.Size = UDim2.new(1, 0, 0, 220)
AimbotSection.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
AimbotSection.BorderSizePixel = 0
AimbotSection.Parent = ContentFrame

local UICorner6 = Instance.new("UICorner")
UICorner6.CornerRadius = UDim.new(0, 10)
UICorner6.Parent = AimbotSection

local AimbotLabel = Instance.new("TextLabel")
AimbotLabel.Name = "AimbotLabel"
AimbotLabel.Size = UDim2.new(1, -20, 0, 35)
AimbotLabel.Position = UDim2.new(0, 10, 0, 5)
AimbotLabel.BackgroundTransparency = 1
AimbotLabel.Text = "🎯 AIMBOT AUTOMÁTICO"
AimbotLabel.TextColor3 = Color3.fromRGB(255, 100, 100)
AimbotLabel.TextSize = 18
AimbotLabel.Font = Enum.Font.GothamBold
AimbotLabel.TextXAlignment = Enum.TextXAlignment.Left
AimbotLabel.Parent = AimbotSection

-- Botão Ligar Aimbot (GRANDE para mobile)
local AimbotToggle = Instance.new("TextButton")
AimbotToggle.Name = "AimbotToggle"
AimbotToggle.Size = UDim2.new(1, -20, 0, 50)
AimbotToggle.Position = UDim2.new(0, 10, 0, 50)
AimbotToggle.BackgroundColor3 = Color3.fromRGB(255, 60, 60)
AimbotToggle.Text = "🚫 AIMBOT DESLIGADO"
AimbotToggle.TextColor3 = Color3.fromRGB(255, 255, 255)
AimbotToggle.TextSize = 18
AimbotToggle.Font = Enum.Font.GothamBold
AimbotToggle.Parent = AimbotSection

local UICorner7 = Instance.new("UICorner")
UICorner7.CornerRadius = UDim.new(0, 8)
UICorner7.Parent = AimbotToggle

-- Controles Aimbot
local controlsY = 110

-- Controle FOV
local FOVControl = Instance.new("Frame")
FOVControl.Name = "FOVControl"
FOVControl.Size = UDim2.new(1, -20, 0, 50)
FOVControl.Position = UDim2.new(0, 10, 0, controlsY)
FOVControl.BackgroundTransparency = 1
FOVControl.Parent = AimbotSection

local FOVLabel = Instance.new("TextLabel")
FOVLabel.Name = "FOVLabel"
FOVLabel.Size = UDim2.new(0.6, 0, 0.5, 0)
FOVLabel.BackgroundTransparency = 1
FOVLabel.Text = "TAMANHO DA ÁREA:"
FOVLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
FOVLabel.TextSize = 14
FOVLabel.Font = Enum.Font.Gotham
FOVLabel.TextXAlignment = Enum.TextXAlignment.Left
FOVLabel.Parent = FOVControl

local FOVValue = Instance.new("TextLabel")
FOVValue.Name = "FOVValue"
FOVValue.Size = UDim2.new(0.4, 0, 0.5, 0)
FOVValue.Position = UDim2.new(0.6, 0, 0, 0)
FOVValue.BackgroundTransparency = 1
FOVValue.Text = tostring(Settings.Aimbot.FOV)
FOVValue.TextColor3 = Color3.fromRGB(100, 200, 255)
FOVValue.TextSize = 16
FOVValue.Font = Enum.Font.GothamBold
FOVValue.TextXAlignment = Enum.TextXAlignment.Right
FOVValue.Parent = FOVControl

local FOVTrack = Instance.new("Frame")
FOVTrack.Name = "FOVTrack"
FOVTrack.Size = UDim2.new(1, 0, 0, 8)
FOVTrack.Position = UDim2.new(0, 0, 1, -15)
FOVTrack.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
FOVTrack.BorderSizePixel = 0
FOVTrack.Parent = FOVControl

local UICorner8 = Instance.new("UICorner")
UICorner8.CornerRadius = UDim.new(0, 4)
UICorner8.Parent = FOVTrack

local FOVFill = Instance.new("Frame")
FOVFill.Name = "FOVFill"
FOVFill.Size = UDim2.new(Settings.Aimbot.FOV / 300, 0, 1, 0)
FOVFill.BackgroundColor3 = Color3.fromRGB(100, 200, 255)
FOVFill.BorderSizePixel = 0
FOVFill.Parent = FOVTrack

local UICorner9 = Instance.new("UICorner")
UICorner9.CornerRadius = UDim.new(0, 4)
UICorner9.Parent = FOVFill

-- Controle Suavidade
local SmoothControl = Instance.new("Frame")
SmoothControl.Name = "SmoothControl"
SmoothControl.Size = UDim2.new(1, -20, 0, 50)
SmoothControl.Position = UDim2.new(0, 10, 0, controlsY + 60)
SmoothControl.BackgroundTransparency = 1
SmoothControl.Parent = AimbotSection

local SmoothLabel = Instance.new("TextLabel")
SmoothLabel.Name = "SmoothLabel"
SmoothLabel.Size = UDim2.new(0.6, 0, 0.5, 0)
SmoothLabel.BackgroundTransparency = 1
SmoothLabel.Text = "SUAVIDADE:"
SmoothLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
SmoothLabel.TextSize = 14
SmoothLabel.Font = Enum.Font.Gotham
SmoothLabel.TextXAlignment = Enum.TextXAlignment.Left
SmoothLabel.Parent = SmoothControl

local SmoothValue = Instance.new("TextLabel")
SmoothValue.Name = "SmoothValue"
SmoothValue.Size = UDim2.new(0.4, 0, 0.5, 0)
SmoothValue.Position = UDim2.new(0.6, 0, 0, 0)
SmoothValue.BackgroundTransparency = 1
SmoothValue.Text = string.format("%.1f", Settings.Aimbot.Smoothness)
SmoothValue.TextColor3 = Color3.fromRGB(255, 200, 100)
SmoothValue.TextSize = 16
SmoothValue.Font = Enum.Font.GothamBold
SmoothValue.TextXAlignment = Enum.TextXAlignment.Right
SmoothValue.Parent = SmoothControl

local SmoothTrack = Instance.new("Frame")
SmoothTrack.Name = "SmoothTrack"
SmoothTrack.Size = UDim2.new(1, 0, 0, 8)
SmoothTrack.Position = UDim2.new(0, 0, 1, -15)
SmoothTrack.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
SmoothTrack.BorderSizePixel = 0
SmoothTrack.Parent = SmoothControl

local UICorner10 = Instance.new("UICorner")
UICorner10.CornerRadius = UDim.new(0, 4)
UICorner10.Parent = SmoothTrack

local SmoothFill = Instance.new("Frame")
SmoothFill.Name = "SmoothFill"
SmoothFill.Size = UDim2.new(Settings.Aimbot.Smoothness, 0, 1, 0)
SmoothFill.BackgroundColor3 = Color3.fromRGB(255, 200, 100)
SmoothFill.BorderSizePixel = 0
SmoothFill.Parent = SmoothTrack

local UICorner11 = Instance.new("UICorner")
UICorner11.CornerRadius = UDim.new(0, 4)
UICorner11.Parent = SmoothFill

-- Seção ESP
local ESPSection = Instance.new("Frame")
ESPSection.Name = "ESPSection"
ESPSection.Size = UDim2.new(1, 0, 0, 180)
ESPSection.Position = UDim2.new(0, 0, 0, 230)
ESPSection.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
ESPSection.BorderSizePixel = 0
ESPSection.Parent = ContentFrame

local UICorner12 = Instance.new("UICorner")
UICorner12.CornerRadius = UDim.new(0, 10)
UICorner12.Parent = ESPSection

local ESPLabel = Instance.new("TextLabel")
ESPLabel.Name = "ESPLabel"
ESPLabel.Size = UDim2.new(1, -20, 0, 35)
ESPLabel.Position = UDim2.new(0, 10, 0, 5)
ESPLabel.BackgroundTransparency = 1
ESPLabel.Text = "👁️ VISÃO ESPECIAL"
ESPLabel.TextColor3 = Color3.fromRGB(100, 200, 255)
ESPLabel.TextSize = 18
ESPLabel.Font = Enum.Font.GothamBold
ESPLabel.TextXAlignment = Enum.TextXAlignment.Left
ESPLabel.Parent = ESPSection

-- Botão Ligar ESP (GRANDE para mobile)
local ESPToggle = Instance.new("TextButton")
ESPToggle.Name = "ESPToggle"
ESPToggle.Size = UDim2.new(1, -20, 0, 50)
ESPToggle.Position = UDim2.new(0, 10, 0, 50)
ESPToggle.BackgroundColor3 = Color3.fromRGB(60, 60, 255)
ESPToggle.Text = "🚫 ESP DESLIGADO"
ESPToggle.TextColor3 = Color3.fromRGB(255, 255, 255)
ESPToggle.TextSize = 18
ESPToggle.Font = Enum.Font.GothamBold
ESPToggle.Parent = ESPSection

local UICorner13 = Instance.new("UICorner")
UICorner13.CornerRadius = UDim.new(0, 8)
UICorner13.Parent = ESPToggle

-- Botão Configurações
local ConfigButton = Instance.new("TextButton")
ConfigButton.Name = "ConfigButton"
ConfigButton.Size = UDim2.new(1, -20, 0, 40)
ConfigButton.Position = UDim2.new(0, 10, 0, 110)
ConfigButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
ConfigButton.Text = "⚙️ CONFIGURAÇÕES"
ConfigButton.TextColor3 = Color3.fromRGB(200, 200, 200)
ConfigButton.TextSize = 16
ConfigButton.Font = Enum.Font.GothamBold
ConfigButton.Parent = ESPSection

local UICorner14 = Instance.new("UICorner")
UICorner14.CornerRadius = UDim.new(0, 6)
UICorner14.Parent = ConfigButton

-- Botão Fechar
local CloseButton = Instance.new("TextButton")
CloseButton.Name = "CloseButton"
CloseButton.Size = UDim2.new(1, -20, 0, 40)
CloseButton.Position = UDim2.new(0, 10, 1, -50)
CloseButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
CloseButton.Text = "✖️ FECHAR INTERFACE"
CloseButton.TextColor3 = Color3.fromRGB(255, 100, 100)
CloseButton.TextSize = 16
CloseButton.Font = Enum.Font.GothamBold
CloseButton.Parent = ContentFrame

local UICorner15 = Instance.new("UICorner")
UICorner15.CornerRadius = UDim.new(0, 8)
UICorner15.Parent = CloseButton

-- Círculo FOV (Desenho)
local FOVDrawing = nil

local function createFOVCircle()
    if FOVDrawing then
        FOVDrawing:Remove()
    end
    
    FOVDrawing = Drawing.new("Circle")
    FOVDrawing.Visible = Settings.Aimbot.Enabled
    FOVDrawing.Color = Color3.fromRGB(255, 50, 50)
    FOVDrawing.Thickness = 2
    FOVDrawing.Radius = Settings.Aimbot.FOV
    FOVDrawing.Position = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)
    FOVDrawing.Transparency = 0.5
    FOVDrawing.Filled = false
end

local function removeFOVCircle()
    if FOVDrawing then
        FOVDrawing:Remove()
        FOVDrawing = nil
    end
end

-- Sistema Aimbot para Mobile (Automático)
local AimbotRunning = false
local AimbotConnection = nil
local CurrentTarget = nil

-- Função para obter o melhor alvo
local function getBestTarget()
    if not LocalPlayer.Character then return nil end
    
    local bestTarget = nil
    local closestDistance = Settings.Aimbot.FOV + 50  -- Margem extra
    local screenCenter = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)
    
    for _, player in pairs(Players:GetPlayers()) do
        if player == LocalPlayer then continue end
        if not player.Character then continue end
        
        local humanoid = player.Character:FindFirstChild("Humanoid")
        local head = player.Character:FindFirstChild("Head")
        
        if not humanoid or not head or humanoid.Health <= 0 then continue end
        
        -- Verificar time
        if Settings.Aimbot.TeamCheck then
            if player.Team and LocalPlayer.Team and player.Team == LocalPlayer.Team then
                continue
            end
        end
        
        -- Converter posição para tela
        local screenPos, onScreen = Camera:WorldToViewportPoint(head.Position)
        if not onScreen then continue end
        
        -- Verificar visibilidade
        if Settings.Aimbot.VisibleCheck then
            local origin = Camera.CFrame.Position
            local direction = (head.Position - origin).Unit * 500
            local raycastParams = RaycastParams.new()
            raycastParams.FilterDescendantsInstances = {LocalPlayer.Character, Camera}
            raycastParams.FilterType = Enum.RaycastFilterType.Exclude
            
            local result = workspace:Raycast(origin, direction, raycastParams)
            if result then
                if result.Instance and not result.Instance:IsDescendantOf(player.Character) then
                    continue
                end
            end
        end
        
        -- Calcular distância do centro da tela
        local playerScreenPos = Vector2.new(screenPos.X, screenPos.Y)
        local distance = (screenCenter - playerScreenPos).Magnitude
        
        if distance < closestDistance then
            closestDistance = distance
            bestTarget = head
        end
    end
    
    return bestTarget
end

-- Função para mirar automaticamente
local function autoAim()
    local targetHead = getBestTarget()
    
    if targetHead then
        CurrentTarget = targetHead.Parent  -- Salvar o character alvo
        
        -- Calcular direção para a cabeça
        local cameraPos = Camera.CFrame.Position
        local headPos = targetHead.Position
        local direction = (headPos - cameraPos).Unit
        
        -- Criar novo CFrame mirando na cabeça
        local targetCF = CFrame.new(cameraPos, cameraPos + direction)
        
        -- Aplicar suavidade
        local currentCF = Camera.CFrame
        local smoothFactor = Settings.Aimbot.Smoothness * 0.5
        
        -- Interpolar suavemente usando Lerp
        local newLookVector = currentCF.LookVector:Lerp(direction, smoothFactor)
        local newCF = CFrame.new(cameraPos, cameraPos + newLookVector)
        
        -- Aplicar à câmera
        Camera.CFrame = newCF
        
        -- Tentar atirar automaticamente se configurado
        if Settings.Aimbot.AutoFire then
            -- Simular clique para atirar
            if LocalPlayer.Character then
                local tool = LocalPlayer.Character:FindFirstChildOfClass("Tool")
                if tool then
                    -- Esta parte requer ajustes específicos para cada jogo
                    -- Pode ser necessário adaptar conforme o jogo
                end
            end
        end
        
        return true
    else
        CurrentTarget = nil
        return false
    end
end

-- Iniciar sistema de aimbot
local function startAimbot()
    if AimbotRunning then return end
    
    print("[MOBILE AIMBOT] Sistema iniciado - Modo automático ativado")
    AimbotRunning = true
    
    createFOVCircle()
    
    AimbotConnection = RunService.RenderStepped:Connect(function(deltaTime)
        if not Settings.Aimbot.Enabled then
            if FOVDrawing then
                FOVDrawing.Visible = false
            end
            CurrentTarget = nil
            return
        end
        
        -- Atualizar círculo FOV
        if FOVDrawing then
            FOVDrawing.Visible = true
            FOVDrawing.Radius = Settings.Aimbot.FOV
            FOVDrawing.Position = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)
        end
        
        -- Aimbot automático SEM necessidade de botão
        autoAim()
    end)
end

local function stopAimbot()
    print("[MOBILE AIMBOT] Sistema desligado")
    AimbotRunning = false
    if AimbotConnection then
        AimbotConnection:Disconnect()
        AimbotConnection = nil
    end
    CurrentTarget = nil
    removeFOVCircle()
end

-- Sistema ESP
local ESPRunning = false
local ESPConnections = {}
local ESPDrawings = {}

local function createESP(player)
    if not player.Character then return end
    
    local esp = {
        Box = nil,
        Name = nil,
        Distance = nil
    }
    
    -- Caixa
    if Settings.ESP.Boxes then
        esp.Box = Drawing.new("Square")
        esp.Box.Visible = false
        esp.Box.Color = (player.Team and LocalPlayer.Team and player.Team == LocalPlayer.Team) and 
                        Color3.fromRGB(0, 255, 0) or Color3.fromRGB(255, 0, 0)
        esp.Box.Thickness = 2
        esp.Box.Filled = false
    end
    
    -- Nome
    if Settings.ESP.Names then
        esp.Name = Drawing.new("Text")
        esp.Name.Visible = false
        esp.Name.Color = Color3.fromRGB(255, 255, 255)
        esp.Name.Size = 14
        esp.Name.Text = player.Name
        esp.Name.Center = true
        esp.Name.Outline = true
    end
    
    -- Distância
    if Settings.ESP.Distance then
        esp.Distance = Drawing.new("Text")
        esp.Distance.Visible = false
        esp.Distance.Color = Color3.fromRGB(200, 200, 200)
        esp.Distance.Size = 12
        esp.Distance.Center = true
        esp.Distance.Outline = true
    end
    
    ESPDrawings[player] = esp
    
    -- Atualizar ESP
    local connection = RunService.RenderStepped:Connect(function()
        if not Settings.ESP.Enabled or not player.Character then
            for _, drawing in pairs(esp) do
                if drawing then
                    drawing.Visible = false
                end
            end
            return
        end
        
        local character = player.Character
        local humanoid = character:FindFirstChild("Humanoid")
        local head = character:FindFirstChild("Head")
        
        if humanoid and head and humanoid.Health > 0 then
            local screenPos, onScreen = Camera:WorldToViewportPoint(head.Position)
            
            if onScreen then
                local distance = (Camera.CFrame.Position - head.Position).Magnitude
                local boxSize = Vector2.new(2000 / distance, 3000 / distance)
                local boxPos = Vector2.new(screenPos.X - boxSize.X / 2, screenPos.Y - boxSize.Y / 2)
                
                -- Caixa
                if esp.Box then
                    esp.Box.Size = boxSize
                    esp.Box.Position = boxPos
                    esp.Box.Visible = Settings.ESP.Boxes
                    esp.Box.Color = (player.Team and LocalPlayer.Team and player.Team == LocalPlayer.Team) and 
                                    Color3.fromRGB(0, 255, 0) or Color3.fromRGB(255, 0, 0)
                end
                
                -- Nome
                if esp.Name then
                    esp.Name.Position = Vector2.new(screenPos.X, boxPos.Y - 20)
                    esp.Name.Visible = Settings.ESP.Names
                end
                
                -- Distância
                if esp.Distance then
                    esp.Distance.Text = math.floor(distance) .. "m"
                    esp.Distance.Position = Vector2.new(screenPos.X, boxPos.Y + boxSize.Y + 5)
                    esp.Distance.Visible = Settings.ESP.Distance
                end
            else
                for _, drawing in pairs(esp) do
                    if drawing then
                        drawing.Visible = false
                    end
                end
            end
        else
            for _, drawing in pairs(esp) do
                if drawing then
                    drawing.Visible = false
                end
            end
        end
    end)
    
    table.insert(ESPConnections, connection)
end

local function startESP()
    if ESPRunning then return end
    ESPRunning = true
    Settings.ESP.Enabled = true
    
    -- Limpar ESP antigo
    for _, esp in pairs(ESPDrawings) do
        for _, drawing in pairs(esp) do
            if drawing then
                drawing:Remove()
            end
        end
    end
    ESPDrawings = {}
    
    for _, connection in pairs(ESPConnections) do
        connection:Disconnect()
    end
    ESPConnections = {}
    
    -- Criar ESP para jogadores existentes
    for _, player in pairs(Players:GetPlayers()) do
        if player ~= LocalPlayer then
            createESP(player)
        end
    end
    
    -- Conectar para novos jogadores
    local playerAdded = Players.PlayerAdded:Connect(function(player)
        wait(1)
        if player ~= LocalPlayer then
            createESP(player)
        end
    end)
    
    table.insert(ESPConnections, playerAdded)
    
    -- Remover quando jogador sair
    local playerRemoving = Players.PlayerRemoving:Connect(function(player)
        if ESPDrawings[player] then
            for _, drawing in pairs(ESPDrawings[player]) do
                if drawing then
                    drawing:Remove()
                end
            end
            ESPDrawings[player] = nil
        end
    end)
    
    table.insert(ESPConnections, playerRemoving)
end

local function stopESP()
    ESPRunning = false
    Settings.ESP.Enabled = false
    
    -- Remover todos os desenhos
    for _, esp in pairs(ESPDrawings) do
        for _, drawing in pairs(esp) do
            if drawing then
                drawing:Remove()
            end
        end
    end
    ESPDrawings = {}
    
    -- Desconectar todas as conexões
    for _, connection in pairs(ESPConnections) do
        connection:Disconnect()
    end
    ESPConnections = {}
end

-- Funções da Interface
local function toggleAimbot()
    Settings.Aimbot.Enabled = not Settings.Aimbot.Enabled
    
    if Settings.Aimbot.Enabled then
        AimbotToggle.BackgroundColor3 = Color3.fromRGB(60, 255, 60)
        AimbotToggle.Text = "✅ AIMBOT ATIVADO"
        startAimbot()
        print("[MOBILE AIMBOT] ATIVADO - Funcionando automaticamente!")
    else
        AimbotToggle.BackgroundColor3 = Color3.fromRGB(255, 60, 60)
        AimbotToggle.Text = "🚫 AIMBOT DESLIGADO"
        stopAimbot()
    end
end

local function toggleESP()
    Settings.ESP.Enabled = not Settings.ESP.Enabled
    
    if Settings.ESP.Enabled then
        ESPToggle.BackgroundColor3 = Color3.fromRGB(60, 255, 60)
        ESPToggle.Text = "✅ ESP ATIVADO"
        startESP()
        print("[ESP] ATIVADO - Inimigos em VERMELHO")
    else
        ESPToggle.BackgroundColor3 = Color3.fromRGB(60, 60, 255)
        ESPToggle.Text = "🚫 ESP DESLIGADO"
        stopESP()
    end
end

-- Configurações avançadas
local ConfigFrame = nil

local function showConfig()
    if ConfigFrame then
        ConfigFrame:Destroy()
        ConfigFrame = nil
        return
    end
    
    -- Criar frame de configurações
    ConfigFrame = Instance.new("Frame")
    ConfigFrame.Name = "ConfigFrame"
    ConfigFrame.Size = UDim2.new(1, -40, 0, 200)
    ConfigFrame.Position = UDim2.new(0, 20, 0.5, -100)
    ConfigFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
    ConfigFrame.BorderSizePixel = 0
    ConfigFrame.ZIndex = 10
    ConfigFrame.Parent = MainFrame
    
    local UICornerConfig = Instance.new("UICorner")
    UICornerConfig.CornerRadius = UDim.new(0, 10)
    UICornerConfig.Parent = ConfigFrame
    
    local ConfigTitle = Instance.new("TextLabel")
    ConfigTitle.Name = "ConfigTitle"
    ConfigTitle.Size = UDim2.new(1, 0, 0, 40)
    ConfigTitle.BackgroundTransparency = 1
    ConfigTitle.Text = "⚙️ CONFIGURAÇÕES AVANÇADAS"
    ConfigTitle.TextColor3 = Color3.fromRGB(255, 255, 255)
    ConfigTitle.TextSize = 18
    ConfigTitle.Font = Enum.Font.GothamBold
    ConfigTitle.Parent = ConfigFrame
    
    -- Checkbox Team Check
    local TeamCheckBtn = Instance.new("TextButton")
    TeamCheckBtn.Name = "TeamCheckBtn"
    TeamCheckBtn.Size = UDim2.new(1, -20, 0, 35)
    TeamCheckBtn.Position = UDim2.new(0, 10, 0, 50)
    TeamCheckBtn.BackgroundColor3 = Settings.Aimbot.TeamCheck and Color3.fromRGB(60, 200, 60) or Color3.fromRGB(80, 80, 80)
    TeamCheckBtn.Text = "IGNORAR TIME AMIGO: " .. (Settings.Aimbot.TeamCheck and "ON" or "OFF")
    TeamCheckBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    TeamCheckBtn.TextSize = 14
    TeamCheckBtn.Font = Enum.Font.GothamBold
    TeamCheckBtn.Parent = ConfigFrame
    
    local UICornerTeam = Instance.new("UICorner")
    UICornerTeam.CornerRadius = UDim.new(0, 6)
    UICornerTeam.Parent = TeamCheckBtn
    
    -- Checkbox Visible Check
    local VisibleCheckBtn = Instance.new("TextButton")
    VisibleCheckBtn.Name = "VisibleCheckBtn"
    VisibleCheckBtn.Size = UDim2.new(1, -20, 0, 35)
    VisibleCheckBtn.Position = UDim2.new(0, 10, 0, 95)
    VisibleCheckBtn.BackgroundColor3 = Settings.Aimbot.VisibleCheck and Color3.fromRGB(60, 200, 60) or Color3.fromRGB(80, 80, 80)
    VisibleCheckBtn.Text = "VERIFICAR VISIBILIDADE: " .. (Settings.Aimbot.VisibleCheck and "ON" or "OFF")
    VisibleCheckBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    VisibleCheckBtn.TextSize = 14
    VisibleCheckBtn.Font = Enum.Font.GothamBold
    VisibleCheckBtn.Parent = ConfigFrame
    
    local UICornerVisible = Instance.new("UICorner")
    UICornerVisible.CornerRadius = UDim.new(0, 6)
    UICornerVisible.Parent = VisibleCheckBtn
    
    -- Botão Fechar Config
    local CloseConfigBtn = Instance.new("TextButton")
    CloseConfigBtn.Name = "CloseConfigBtn"
    CloseConfigBtn.Size = UDim2.new(1, -20, 0, 35)
    CloseConfigBtn.Position = UDim2.new(0, 10, 0, 150)
    CloseConfigBtn.BackgroundColor3 = Color3.fromRGB(255, 60, 60)
    CloseConfigBtn.Text = "FECHAR CONFIG"
    CloseConfigBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    CloseConfigBtn.TextSize = 14
    CloseConfigBtn.Font = Enum.Font.GothamBold
    CloseConfigBtn.Parent = ConfigFrame
    
    local UICornerClose = Instance.new("UICorner")
    UICornerClose.CornerRadius = UDim.new(0, 6)
    UICornerClose.Parent = CloseConfigBtn
    
    -- Conectar eventos
    TeamCheckBtn.MouseButton1Click:Connect(function()
        Settings.Aimbot.TeamCheck = not Settings.Aimbot.TeamCheck
        TeamCheckBtn.BackgroundColor3 = Settings.Aimbot.TeamCheck and Color3.fromRGB(60, 200, 60) or Color3.fromRGB(80, 80, 80)
        TeamCheckBtn.Text = "IGNORAR TIME AMIGO: " .. (Settings.Aimbot.TeamCheck and "ON" or "OFF")
        print("[CONFIG] Team Check: " .. (Settings.Aimbot.TeamCheck and "ON" or "OFF"))
    end)
    
    VisibleCheckBtn.MouseButton1Click:Connect(function()
        Settings.Aimbot.VisibleCheck = not Settings.Aimbot.VisibleCheck
        VisibleCheckBtn.BackgroundColor3 = Settings.Aimbot.VisibleCheck and Color3.fromRGB(60, 200, 60) or Color3.fromRGB(80, 80, 80)
        VisibleCheckBtn.Text = "VERIFICAR VISIBILIDADE: " .. (Settings.Aimbot.VisibleCheck and "ON" or "OFF")
        print("[CONFIG] Visible Check: " .. (Settings.Aimbot.VisibleCheck and "ON" or "OFF"))
    end)
    
    CloseConfigBtn.MouseButton1Click:Connect(function()
        ConfigFrame:Destroy()
        ConfigFrame = nil
    end)
end

-- Conectar Eventos
AimbotToggle.MouseButton1Click:Connect(toggleAimbot)
ESPToggle.MouseButton1Click:Connect(toggleESP)
ConfigButton.MouseButton1Click:Connect(showConfig)

-- Slider FOV (Toque para mobile)
FOVTrack.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseButton1 then
        local touchPos = input.Position
        local trackPos = FOVTrack.AbsolutePosition.X
        local trackSize = FOVTrack.AbsoluteSize.X
        
        local relativeX = (touchPos.X - trackPos) / trackSize
        relativeX = math.clamp(relativeX, 0, 1)
        
        Settings.Aimbot.FOV = math.floor(50 + relativeX * 250) -- Range: 50-300
        FOVValue.Text = tostring(Settings.Aimbot.FOV)
        FOVFill.Size = UDim2.new(Settings.Aimbot.FOV / 300, 0, 1, 0)
    end
end)

-- Slider Suavidade (Toque para mobile)
SmoothTrack.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseButton1 then
        local touchPos = input.Position
        local trackPos = SmoothTrack.AbsolutePosition.X
        local trackSize = SmoothTrack.AbsoluteSize.X
        
        local relativeX = (touchPos.X - trackPos) / trackSize
        relativeX = math.clamp(relativeX, 0, 1)
        
        Settings.Aimbot.Smoothness = math.floor((0.1 + relativeX * 0.9) * 10) / 10
        SmoothValue.Text = string.format("%.1f", Settings.Aimbot.Smoothness)
        SmoothFill.Size = UDim2.new(Settings.Aimbot.Smoothness, 0, 1, 0)
    end
end)

-- Minimizar Interface
MinimizeButton.MouseButton1Click:Connect(function()
    MainFrame.Visible = false
    MinimizedIcon.Visible = true
end)

MinimizedIcon.MouseButton1Click:Connect(function()
    MainFrame.Visible = true
    MinimizedIcon.Visible = false
end)

-- Fechar Interface
CloseButton.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
    print("[SYSTEM] Interface fechada")
end)

-- Atualizar quando a tela redimensionar
Camera:GetPropertyChangedSignal("ViewportSize"):Connect(function()
    if Settings.Aimbot.Enabled and FOVDrawing then
        FOVDrawing.Position = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)
    end
end)

-- Inicialização
print("==============================================")
print("📱 MOBILE AIMBOT PRO - CARREGADO!")
print("==============================================")
print("✅ Sistema otimizado para mobile")
print("✅ Aimbot funciona AUTOMATICAMENTE")
print("✅ Não precisa segurar botão algum")
print("✅ Interface amigável para touch")
print("==============================================")
print("1. Ligue o Aimbot - Funciona automaticamente")
print("2. Ligue o ESP - Vê inimigos através das paredes")
print("3. Toque nos sliders para ajustar")
print("4. Clique no ícone 🎯 para abrir/fechar")
print("==============================================")

-- Atalho de teclado para abrir/fechar (opcional para PC)
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if not gameProcessed then
        if input.KeyCode == Enum.KeyCode.P then
            MainFrame.Visible = not MainFrame.Visible
            MinimizedIcon.Visible = not MainFrame.Visible
        end
    end
end)
