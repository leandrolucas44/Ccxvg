-- ESP Script para Roblox
-- Gerado por Gerador ESP
-- Compatível com executores como Delta
-- Modificado com opção de ESP Distância

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera
local LocalPlayer = Players.LocalPlayer
local CoreGui = game:GetService("CoreGui")

-- Configurações
local ESPEnabled = true
local Settings = {
    Box = true,
    Line = true,
    Health = true,
    Distance = true,
    BoxColor = Color3.fromRGB(0, 255, 0),
    LineColor = Color3.fromRGB(0, 255, 255),
    HealthColor = Color3.fromRGB(255, 0, 0),
    Thickness = 2
}

local ESPObjects = {}

-- Criar GUI do Painel
local function CreateGUI()
    local ScreenGui = Instance.new("ScreenGui")
    ScreenGui.Name = "ESPPanel"
    ScreenGui.ResetOnSpawn = false
    ScreenGui.Parent = CoreGui
    
    -- Botão Flutuante Circular
    local FloatingButton = Instance.new("TextButton")
    FloatingButton.Name = "FloatingButton"
    FloatingButton.Size = UDim2.new(0, 50, 0, 50)
    FloatingButton.Position = UDim2.new(0.5, -25, 0.1, 0)
    FloatingButton.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    FloatingButton.BorderSizePixel = 0
    FloatingButton.Text = "ESP"
    FloatingButton.TextColor3 = Color3.fromRGB(0, 255, 136)
    FloatingButton.TextSize = 14
    FloatingButton.Font = Enum.Font.GothamBold
    FloatingButton.Active = true
    FloatingButton.Draggable = true
    FloatingButton.Parent = ScreenGui
    
    local FloatCorner = Instance.new("UICorner")
    FloatCorner.CornerRadius = UDim.new(1, 0)
    FloatCorner.Parent = FloatingButton
    
    local FloatStroke = Instance.new("UIStroke")
    FloatStroke.Color = Color3.fromRGB(0, 255, 136)
    FloatStroke.Thickness = 2
    FloatStroke.Parent = FloatingButton
    
    -- Frame Principal
    local MainFrame = Instance.new("Frame")
    MainFrame.Name = "MainFrame"
    MainFrame.Size = UDim2.new(0, 320, 0, 382)
    MainFrame.Position = UDim2.new(0.02, 0, 0.3, 0)
    MainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 35)
    MainFrame.BorderSizePixel = 0
    MainFrame.Active = true
    MainFrame.Draggable = true
    MainFrame.Visible = true
    MainFrame.Parent = ScreenGui
    
    local UICorner = Instance.new("UICorner")
    UICorner.CornerRadius = UDim.new(0, 12)
    UICorner.Parent = MainFrame
    
    local UIStroke = Instance.new("UIStroke")
    UIStroke.Color = Color3.fromRGB(0, 255, 136)
    UIStroke.Thickness = 2
    UIStroke.Parent = MainFrame
    
    -- Header
    local Header = Instance.new("Frame")
    Header.Name = "Header"
    Header.Size = UDim2.new(1, 0, 0, 50)
    Header.BackgroundColor3 = Color3.fromRGB(15, 15, 25)
    Header.BorderSizePixel = 0
    Header.Parent = MainFrame
    
    local HeaderCorner = Instance.new("UICorner")
    HeaderCorner.CornerRadius = UDim.new(0, 12)
    HeaderCorner.Parent = Header
    
    local Title = Instance.new("TextLabel")
    Title.Name = "Title"
    Title.Size = UDim2.new(1, -20, 1, 0)
    Title.Position = UDim2.new(0, 10, 0, 0)
    Title.BackgroundTransparency = 1
    Title.Text = "TORVIC XIT"
    Title.TextColor3 = Color3.fromRGB(0, 255, 136)
    Title.TextSize = 22
    Title.Font = Enum.Font.GothamBold
    Title.TextXAlignment = Enum.TextXAlignment.Left
    Title.Parent = Header
    
    -- Container com ScrollingFrame
    local ScrollContainer = Instance.new("ScrollingFrame")
    ScrollContainer.Name = "ScrollContainer"
    ScrollContainer.Size = UDim2.new(1, 0, 0, 232)
    ScrollContainer.Position = UDim2.new(0, 0, 0, 50)
    ScrollContainer.BackgroundColor3 = Color3.fromRGB(20, 20, 35)
    ScrollContainer.BorderSizePixel = 0
    ScrollContainer.ScrollBarThickness = 8
    ScrollContainer.ScrollBarImageColor3 = Color3.fromRGB(0, 255, 136)
    ScrollContainer.CanvasSize = UDim2.new(0, 0, 0, 0)
    ScrollContainer.Parent = MainFrame
    
    local ScrollCorner = Instance.new("UICorner")
    ScrollCorner.CornerRadius = UDim.new(0, 12)
    ScrollCorner.Parent = ScrollContainer
    
    -- UIListLayout para o ScrollContainer
    local ListLayout = Instance.new("UIListLayout")
    ListLayout.Padding = UDim.new(0, 10)
    ListLayout.FillDirection = Enum.FillDirection.Vertical
    ListLayout.SortOrder = Enum.SortOrder.LayoutOrder
    ListLayout.Parent = ScrollContainer
    
    -- UIPadding para o ScrollContainer
    local ScrollPadding = Instance.new("UIPadding")
    ScrollPadding.PaddingTop = UDim.new(0, 10)
    ScrollPadding.PaddingBottom = UDim.new(0, 10)
    ScrollPadding.PaddingLeft = UDim.new(0, 15)
    ScrollPadding.PaddingRight = UDim.new(0, 15)
    ScrollPadding.Parent = ScrollContainer
    
    -- Função para criar Toggle
    local function CreateToggle(name, text, defaultValue, callback)
        local ToggleFrame = Instance.new("Frame")
        ToggleFrame.Name = name
        ToggleFrame.Size = UDim2.new(1, -30, 0, 60)
        ToggleFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 45)
        ToggleFrame.BorderSizePixel = 0
        ToggleFrame.Parent = ScrollContainer
        
        local ToggleCorner = Instance.new("UICorner")
        ToggleCorner.CornerRadius = UDim.new(0, 8)
        ToggleCorner.Parent = ToggleFrame
        
        local ToggleLabel = Instance.new("TextLabel")
        ToggleLabel.Size = UDim2.new(1, -70, 1, 0)
        ToggleLabel.Position = UDim2.new(0, 15, 0, 0)
        ToggleLabel.BackgroundTransparency = 1
        ToggleLabel.Text = text
        ToggleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
        ToggleLabel.TextSize = 16
        ToggleLabel.Font = Enum.Font.GothamMedium
        ToggleLabel.TextXAlignment = Enum.TextXAlignment.Left
        ToggleLabel.Parent = ToggleFrame
        
        local ToggleButton = Instance.new("TextButton")
        ToggleButton.Name = "ToggleButton"
        ToggleButton.Size = UDim2.new(0, 50, 0, 28)
        ToggleButton.Position = UDim2.new(1, -65, 0.5, -14)
        ToggleButton.BackgroundColor3 = defaultValue and Color3.fromRGB(0, 255, 136) or Color3.fromRGB(60, 60, 75)
        ToggleButton.BorderSizePixel = 0
        ToggleButton.Text = ""
        ToggleButton.Parent = ToggleFrame
        
        local ButtonCorner = Instance.new("UICorner")
        ButtonCorner.CornerRadius = UDim.new(1, 0)
        ButtonCorner.Parent = ToggleButton
        
        local ToggleCircle = Instance.new("Frame")
        ToggleCircle.Name = "Circle"
        ToggleCircle.Size = UDim2.new(0, 22, 0, 22)
        ToggleCircle.Position = defaultValue and UDim2.new(1, -25, 0.5, -11) or UDim2.new(0, 3, 0.5, -11)
        ToggleCircle.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        ToggleCircle.BorderSizePixel = 0
        ToggleCircle.Parent = ToggleButton
        
        local CircleCorner = Instance.new("UICorner")
        CircleCorner.CornerRadius = UDim.new(1, 0)
        CircleCorner.Parent = ToggleCircle
        
        local isEnabled = defaultValue
        
        ToggleButton.MouseButton1Click:Connect(function()
            isEnabled = not isEnabled
            callback(isEnabled)
            
            ToggleButton.BackgroundColor3 = isEnabled and Color3.fromRGB(0, 255, 136) or Color3.fromRGB(60, 60, 75)
            ToggleCircle:TweenPosition(
                isEnabled and UDim2.new(1, -25, 0.5, -11) or UDim2.new(0, 3, 0.5, -11),
                Enum.EasingDirection.Out,
                Enum.EasingStyle.Quad,
                0.2,
                true
            )
        end)
        
        return ToggleFrame
    end
    
    -- ESP Box Toggle
    CreateToggle("BoxToggle", "📦 ESP Box", Settings.Box, function(enabled)
        Settings.Box = enabled
        print("ESP Box: " .. tostring(enabled))
    end)
    
    -- ESP Line Toggle
    CreateToggle("LineToggle", "📏 ESP Line", Settings.Line, function(enabled)
        Settings.Line = enabled
        print("ESP Line: " .. tostring(enabled))
    end)
    
    -- ESP Health Toggle
    CreateToggle("HealthToggle", "❤️ ESP Health", Settings.Health, function(enabled)
        Settings.Health = enabled
        print("ESP Health: " .. tostring(enabled))
    end)
    
    -- ESP Distance Toggle
    CreateToggle("DistanceToggle", "📍 ESP Distance", Settings.Distance, function(enabled)
        Settings.Distance = enabled
        print("ESP Distance: " .. tostring(enabled))
    end)
    
    -- Atualizar tamanho do canvas do ScrollContainer
    local function UpdateCanvasSize()
        ListLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
            ScrollContainer.CanvasSize = UDim2.new(0, 0, 0, ListLayout.AbsoluteContentSize.Y + 20)
        end)
    end
    UpdateCanvasSize()
    
    -- Botão de Fechar
    local CloseButton = Instance.new("TextButton")
    CloseButton.Name = "CloseButton"
    CloseButton.Size = UDim2.new(0, 30, 0, 30)
    CloseButton.Position = UDim2.new(1, -40, 0, 10)
    CloseButton.BackgroundColor3 = Color3.fromRGB(255, 50, 50)
    CloseButton.BorderSizePixel = 0
    CloseButton.Text = "X"
    CloseButton.TextColor3 = Color3.fromRGB(255, 255, 255)
    CloseButton.TextSize = 18
    CloseButton.Font = Enum.Font.GothamBold
    CloseButton.Parent = Header
    
    local CloseCorner = Instance.new("UICorner")
    CloseCorner.CornerRadius = UDim.new(1, 0)
    CloseCorner.Parent = CloseButton
    
    CloseButton.MouseButton1Click:Connect(function()
        MainFrame.Visible = false
        print("ESP Panel fechado")
    end)
    
    -- Toggle do Botão Flutuante
    FloatingButton.MouseButton1Click:Connect(function()
        MainFrame.Visible = not MainFrame.Visible
        if MainFrame.Visible then
            print("ESP Panel aberto")
        else
            print("ESP Panel fechado")
        end
    end)
    
    -- Info Footer
    local Footer = Instance.new("TextLabel")
    Footer.Name = "Footer"
    Footer.Size = UDim2.new(1, -30, 0, 60)
    Footer.Position = UDim2.new(0, 15, 1, -75)
    Footer.BackgroundColor3 = Color3.fromRGB(30, 30, 45)
    Footer.BorderSizePixel = 0
    Footer.Text = "💡 criador:@victorsls7 /MANGA XIT 22"
    Footer.TextColor3 = Color3.fromRGB(0, 255, 136)
    Footer.TextSize = 11
    Footer.Font = Enum.Font.Gotham
    Footer.Parent = MainFrame
    
    local FooterCorner = Instance.new("UICorner")
    FooterCorner.CornerRadius = UDim.new(0, 8)
    FooterCorner.Parent = Footer
    
    print("✅ Painel ESP criado com sucesso!")
end

CreateGUI()

-- Função para criar ESP Drawing
local function CreateESP(player)
    local esp = {
        Box = nil,
        Line = nil,
        HealthBar = nil,
        HealthBarOutline = nil,
        HealthText = nil,
        DistanceText = nil
    }
    
    if Settings.Box then
        esp.Box = Drawing.new("Square")
        esp.Box.Thickness = Settings.Thickness
        esp.Box.Color = Settings.BoxColor
        esp.Box.Transparency = 1
        esp.Box.Filled = false
        esp.Box.Visible = false
    end
    
    if Settings.Line then
        esp.Line = Drawing.new("Line")
        esp.Line.Thickness = Settings.Thickness
        esp.Line.Color = Settings.LineColor
        esp.Line.Transparency = 1
        esp.Line.Visible = false
    end
    
    if Settings.Health then
        esp.HealthBarOutline = Drawing.new("Square")
        esp.HealthBarOutline.Thickness = Settings.Thickness
        esp.HealthBarOutline.Color = Color3.new(0, 0, 0)
        esp.HealthBarOutline.Transparency = 1
        esp.HealthBarOutline.Filled = false
        esp.HealthBarOutline.Visible = false
        
        esp.HealthBar = Drawing.new("Square")
        esp.HealthBar.Thickness = 1
        esp.HealthBar.Color = Settings.HealthColor
        esp.HealthBar.Transparency = 1
        esp.HealthBar.Filled = true
        esp.HealthBar.Visible = false
        
        esp.HealthText = Drawing.new("Text")
        esp.HealthText.Size = 14
        esp.HealthText.Color = Color3.new(1, 1, 1)
        esp.HealthText.Center = true
        esp.HealthText.Outline = true
        esp.HealthText.Visible = false
    end
    
    if Settings.Distance then
        esp.DistanceText = Drawing.new("Text")
        esp.DistanceText.Size = 14
        esp.DistanceText.Color = Color3.fromRGB(255, 255, 0)
        esp.DistanceText.Center = true
        esp.DistanceText.Outline = true
        esp.DistanceText.Visible = false
    end
    
    ESPObjects[player] = esp
end

-- Função para remover ESP
local function RemoveESP(player)
    local esp = ESPObjects[player]
    if esp then
        if esp.Box then esp.Box:Remove() end
        if esp.Line then esp.Line:Remove() end
        if esp.HealthBar then esp.HealthBar:Remove() end
        if esp.HealthBarOutline then esp.HealthBarOutline:Remove() end
        if esp.HealthText then esp.HealthText:Remove() end
        if esp.DistanceText then esp.DistanceText:Remove() end
        ESPObjects[player] = nil
    end
end

-- Função para atualizar ESP
local function UpdateESP(player)
    if player == LocalPlayer then return end
    if not player.Character then return end
    
    local character = player.Character
    local humanoidRootPart = character:FindFirstChild("HumanoidRootPart")
    local humanoid = character:FindFirstChild("Humanoid")
    local head = character:FindFirstChild("Head")
    
    if not humanoidRootPart or not head then return end
    
    local esp = ESPObjects[player]
    if not esp then return end
    
    -- Calcular posição na tela
    local vector, onScreen = Camera:WorldToViewportPoint(humanoidRootPart.Position)
    
    if onScreen and ESPEnabled then
        local headPos = Camera:WorldToViewportPoint(head.Position + Vector3.new(0, 0.5, 0))
        local legPos = Camera:WorldToViewportPoint(humanoidRootPart.Position - Vector3.new(0, 3, 0))
        
        local height = math.abs(headPos.Y - legPos.Y)
        local width = height / 2
        
        -- Atualizar Box
        if esp.Box and Settings.Box then
            esp.Box.Size = Vector2.new(width, height)
            esp.Box.Position = Vector2.new(vector.X - width / 2, vector.Y - height / 2)
            esp.Box.Visible = true
        end
        
        -- Atualizar Line
        if esp.Line and Settings.Line then
            esp.Line.From = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y)
            esp.Line.To = Vector2.new(vector.X, vector.Y)
            esp.Line.Visible = true
        end
        
        -- Atualizar Health Bar
        if esp.HealthBar and Settings.Health and humanoid then
            local healthPercent = humanoid.Health / humanoid.MaxHealth
            local barWidth = 4
            local barHeight = height
            
            esp.HealthBarOutline.Size = Vector2.new(barWidth, barHeight)
            esp.HealthBarOutline.Position = Vector2.new(vector.X - width / 2 - barWidth - 2, vector.Y - height / 2)
            esp.HealthBarOutline.Visible = true
            
            esp.HealthBar.Size = Vector2.new(barWidth - 2, (barHeight - 2) * healthPercent)
            esp.HealthBar.Position = Vector2.new(vector.X - width / 2 - barWidth - 1, vector.Y + height / 2 - (barHeight - 2) * healthPercent - 1)
            esp.HealthBar.Visible = true
            
            esp.HealthText.Text = tostring(math.floor(humanoid.Health))
            esp.HealthText.Position = Vector2.new(vector.X - width / 2 - barWidth - 2, vector.Y - height / 2 - 15)
            esp.HealthText.Visible = true
        end
        
        -- Atualizar Distance Text
        if esp.DistanceText and Settings.Distance then
            local distance = (LocalPlayer.Character.HumanoidRootPart.Position - humanoidRootPart.Position).Magnitude
            local distanceText = math.floor(distance) .. "m"
            
            esp.DistanceText.Text = distanceText
            esp.DistanceText.Position = Vector2.new(vector.X, vector.Y + height / 2 + 15)
            esp.DistanceText.Visible = true
        end
    else
        -- Esconder ESP se fora da tela
        if esp.Box then esp.Box.Visible = false end
        if esp.Line then esp.Line.Visible = false end
        if esp.HealthBar then esp.HealthBar.Visible = false end
        if esp.HealthBarOutline then esp.HealthBarOutline.Visible = false end
        if esp.HealthText then esp.HealthText.Visible = false end
        if esp.DistanceText then esp.DistanceText.Visible = false end
    end
end

-- Adicionar ESP para jogadores existentes
for _, player in pairs(Players:GetPlayers()) do
    if player ~= LocalPlayer then
        CreateESP(player)
    end
end

-- Adicionar ESP para novos jogadores
Players.PlayerAdded:Connect(function(player)
    CreateESP(player)
end)

-- Remover ESP quando jogador sai
Players.PlayerRemoving:Connect(function(player)
    RemoveESP(player)
end)

-- Loop de atualização
RunService.RenderStepped:Connect(function()
    if ESPEnabled then
        for player, _ in pairs(ESPObjects) do
            if player and player.Parent then
                UpdateESP(player)
            end
        end
    end
end)

print("ESP Script carregado com sucesso!")
print("ESP Box: " .. tostring(Settings.Box))
print("ESP Line: " .. tostring(Settings.Line))
print("ESP Health: " .. tostring(Settings.Health))
print("ESP Distance: " .. tostring(Settings.Distance))
