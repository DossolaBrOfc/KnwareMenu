local player = game.Players.LocalPlayer

local function criarMenuTP()
    local screenGui = Instance.new("ScreenGui")
    screenGui.Name = "TPMenu"
    screenGui.ResetOnSpawn = false
    screenGui.Parent = player:WaitForChild("PlayerGui")

    -- Botão para abrir o menu
    local abrirBotao = Instance.new("TextButton")
    abrirBotao.Size = UDim2.new(0, 120, 0, 40)
    abrirBotao.Position = UDim2.new(0, 10, 0, 10)
    abrirBotao.Text = "Abrir Menu"
    abrirBotao.BackgroundColor3 = Color3.fromRGB(0, 120, 255)
    abrirBotao.TextColor3 = Color3.new(1, 1, 1)
    abrirBotao.Font = Enum.Font.GothamBold
    abrirBotao.TextSize = 20
    abrirBotao.Visible = false
    abrirBotao.Parent = screenGui

    local abrirCorner = Instance.new("UICorner")
    abrirCorner.CornerRadius = UDim.new(0, 8)
    abrirCorner.Parent = abrirBotao

    abrirBotao.MouseEnter:Connect(function()
        abrirBotao.BackgroundColor3 = Color3.fromRGB(0, 150, 255)
    end)

    abrirBotao.MouseLeave:Connect(function()
        abrirBotao.BackgroundColor3 = Color3.fromRGB(0, 120, 255)
    end)

    -- Menu principal
    local menuFrame = Instance.new("Frame")
    menuFrame.Size = UDim2.new(0, 520, 0, 400)
    menuFrame.Position = UDim2.new(0.5, -250, 0.5, -200)
    menuFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
    menuFrame.BorderSizePixel = 0
    menuFrame.Active = true
    menuFrame.Draggable = true
    menuFrame.Parent = screenGui

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 12)
    corner.Parent = menuFrame

    local abasContainer = Instance.new("Frame")
    abasContainer.Size = UDim2.new(1, 0, 0, 30)
    abasContainer.Position = UDim2.new(0, 0, 0, 60) -- abaixo do título
    abasContainer.BackgroundTransparency = 1
    abasContainer.Parent = menuFrame

    -- Container para centralizar as abas
    local abasContainer = Instance.new("Frame")
    abasContainer.Size = UDim2.new(1, 0, 0, 30)
    abasContainer.Position = UDim2.new(0, 0, 0, 60) -- abaixo do título
    abasContainer.BackgroundTransparency = 1
    abasContainer.Parent = menuFrame

local abasLayout = Instance.new("UIListLayout")
abasLayout.FillDirection = Enum.FillDirection.Horizontal
abasLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
abasLayout.Padding = UDim.new(0, 10)
abasLayout.SortOrder = Enum.SortOrder.LayoutOrder
abasLayout.Parent = abasContainer

    -- Título
    local titulo = Instance.new("TextLabel")
    titulo.Size = UDim2.new(1, 0, 0, 50)
    titulo.Position = UDim2.new(0, 0, 0, 0)
    titulo.Text = "Knware Hub"
    titulo.BackgroundTransparency = 1
    titulo.TextColor3 = Color3.new(1, 1, 1)
    titulo.Font = Enum.Font.GothamBold
    titulo.TextSize = 24
    titulo.TextXAlignment = Enum.TextXAlignment.Center
    titulo.Parent = menuFrame

    -- Efeito RGB no título
    local TweenService = game:GetService("TweenService")

    -- Função para aplicar efeito RGB em botões
local function aplicarRGB(botao)
    local cores = {
        Color3.fromRGB(255, 0, 0),
        Color3.fromRGB(0, 255, 0),
        Color3.fromRGB(0, 0, 255),
        Color3.fromRGB(255, 0, 255),
        Color3.fromRGB(0, 255, 255),
        Color3.fromRGB(255, 255, 0)
    }

    task.spawn(function()
        while true do
            for _, cor in ipairs(cores) do
                local tween = TweenService:Create(botao, TweenInfo.new(1.5, Enum.EasingStyle.Linear), {BackgroundColor3 = cor})
                tween:Play()
                task.wait(1.5)
            end
        end
    end)
end

    local function tweenTitulo(corFinal)
        local info = TweenInfo.new(1.5, Enum.EasingStyle.Linear, Enum.EasingDirection.InOut)
        return TweenService:Create(titulo, info, {TextColor3 = corFinal})
    end

    task.spawn(function()
        while true do
            tweenTitulo(Color3.fromRGB(255, 0, 0)):Play()
            task.wait(1.5)
            tweenTitulo(Color3.fromRGB(0, 255, 0)):Play()
            task.wait(1.5)
            tweenTitulo(Color3.fromRGB(0, 0, 255)):Play()
            task.wait(1.5)
            tweenTitulo(Color3.fromRGB(255, 0, 255)):Play()
            task.wait(1.5)
            tweenTitulo(Color3.fromRGB(0, 255, 255)):Play()
            task.wait(1.5)
            tweenTitulo(Color3.fromRGB(255, 255, 0)):Play()
            task.wait(1.5)
        end
    end)

    -- Estilização de abas
    local function estilizarAba(botao)
        botao.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
        botao.TextColor3 = Color3.new(1, 1, 1)
        botao.Font = Enum.Font.GothamBold
        botao.TextSize = 18

        local corner = Instance.new("UICorner")
        corner.CornerRadius = UDim.new(0, 8)
        corner.Parent = botao

        local stroke = Instance.new("UIStroke")
        stroke.Color = Color3.fromRGB(60, 60, 60)
        stroke.Thickness = 1
        stroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
        stroke.Parent = botao

        botao.MouseEnter:Connect(function()
            botao.BackgroundColor3 = Color3.fromRGB(0, 120, 255)
        end)

        botao.MouseLeave:Connect(function()
            botao.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
        end)
    end

    -- Abas
    local abaTeleporte = Instance.new("TextButton")
    abaTeleporte.Size = UDim2.new(0, 120, 0, 30)
    abaTeleporte.Text = "🍆 Teleporte"
    abaTeleporte.Parent = abasContainer
    estilizarAba(abaTeleporte)
    aplicarRGB(abaTeleporte)


    local abaAcoes = Instance.new("TextButton")
    abaAcoes.Size = UDim2.new(0, 120, 0, 30)
    abaAcoes.Text = "🍆 Ações"
    abaAcoes.Parent = abasContainer
    estilizarAba(abaAcoes)
    aplicarRGB(abaAcoes)

    local abaLoader = Instance.new("TextButton")
    abaLoader.Size = UDim2.new(0, 120, 0, 30)
    abaLoader.Text = "🍆 Loader"
    abaLoader.Parent = abasContainer
    estilizarAba(abaLoader)
    aplicarRGB(abaLoader)


    -- Frames das abas
    local frameTeleporte = Instance.new("Frame")
    frameTeleporte.Position = UDim2.new(0, 10, 0, 120)
    frameTeleporte.Size = UDim2.new(1, -20, 1, -120)
    frameTeleporte.BackgroundTransparency = 1
    frameTeleporte.Parent = menuFrame

    local frameAcoes = Instance.new("Frame")
    frameAcoes.Size = UDim2.new(1, -20, 1, -100)
    frameAcoes.Position = UDim2.new(0, 10, 0, 100)
    frameAcoes.BackgroundTransparency = 1
    frameAcoes.Visible = false
    frameAcoes.Parent = menuFrame

    local frameLoader = Instance.new("Frame")
    frameLoader.Size = UDim2.new(1, -20, 1, -120)
    frameLoader.Position = UDim2.new(0, 10, 0, 120)
    frameLoader.BackgroundTransparency = 1
    frameLoader.Visible = false
    frameLoader.Parent = menuFrame

    local gridLoader = Instance.new("UIGridLayout")
    gridLoader.CellSize = UDim2.new(0, 230, 0, 40)
    gridLoader.CellPadding = UDim2.new(0, 10, 0, 10)
    gridLoader.FillDirection = Enum.FillDirection.Vertical
    gridLoader.StartCorner = Enum.StartCorner.TopLeft
    gridLoader.SortOrder = Enum.SortOrder.LayoutOrder
    gridLoader.Parent = frameLoader

    -- Alternância de abas
    local function ativarAba(botaoAtivo)
        for _, b in pairs({abaTeleporte, abaAcoes, abaLoader}) do
            b.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
        end
        botaoAtivo.BackgroundColor3 = Color3.fromRGB(0, 120, 255)
    end

    abaTeleporte.MouseButton1Click:Connect(function()
        frameTeleporte.Visible = true
        frameAcoes.Visible = false
        ativarAba(abaTeleporte)
    end)

    abaAcoes.MouseButton1Click:Connect(function()
        frameTeleporte.Visible = false
        frameAcoes.Visible = true
        ativarAba(abaAcoes)
    end)

    abaLoader.MouseButton1Click:Connect(function()
    frameTeleporte.Visible = false
    frameAcoes.Visible = false
    frameLoader.Visible = true
    ativarAba(abaLoader)
    end)

    -- Botão de fechar
    local fecharBotao = Instance.new("TextButton")
    fecharBotao.Size = UDim2.new(0, 30, 0, 30)
    fecharBotao.Position = UDim2.new(1, -35, 0, 10)
    fecharBotao.Text = "X"
    fecharBotao.BackgroundColor3 = Color3.fromRGB(170, 0, 0)
    fecharBotao.TextColor3 = Color3.new(1, 1, 1)
    fecharBotao.Font = Enum.Font.GothamBold
    fecharBotao.TextSize = 18
    fecharBotao.Parent = menuFrame

    local fecharCorner = Instance.new("UICorner")
    fecharCorner.CornerRadius = UDim.new(0, 8)
    fecharCorner.Parent = fecharBotao

    fecharBotao.MouseEnter:Connect(function()
        fecharBotao.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
    end)

    fecharBotao.MouseLeave:Connect(function()
        fecharBotao.BackgroundColor3 = Color3.fromRGB(170, 0, 0)
    end)

    fecharBotao.MouseButton1Click:Connect(function()
        menuFrame.Visible = false
        abrirBotao.Visible = true
    end)

    -- ScrollingFrame para teleporte
    local listaTP = Instance.new("ScrollingFrame")
    listaTP.ScrollingDirection = Enum.ScrollingDirection.XY
    listaTP.Size = UDim2.new(1, -20, 1, -20)
    listaTP.Position = UDim2.new(0, 10, 0, 10)
    listaTP.CanvasSize = UDim2.new(0, 0, 0, 0)
    listaTP.ScrollBarThickness = 0
    listaTP.BackgroundTransparency = 1
    listaTP.BorderSizePixel = 0
    listaTP.Parent = frameTeleporte

    local grid = Instance.new("UIGridLayout")
    grid.CellSize = UDim2.new(0, 230, 0, 40)
    grid.CellPadding = UDim2.new(0, 20, 0, 10)
    grid.SortOrder = Enum.SortOrder.LayoutOrder
    grid.FillDirection = Enum.FillDirection.Vertical -- empilha verticalmente
    grid.StartCorner = Enum.StartCorner.TopLeft -- começa no canto superior esquerdo
    grid.Parent = listaTP

    grid:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
    listaTP.CanvasSize = UDim2.new(0, grid.AbsoluteContentSize.X, 0, grid.AbsoluteContentSize.Y)
end)

    -- Função para criar botões de teleporte
    local function criarBotao(nome, posicao)
        local botao = Instance.new("TextButton")
        botao.Size = UDim2.new(0, 260, 0, 40)
        botao.Text = nome
        botao.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
        botao.TextColor3 = Color3.new(1, 1, 1)
        botao.Font = Enum.Font.Gotham
        botao.TextSize = 22
        botao.TextXAlignment = Enum.TextXAlignment.Center
        botao.Parent = listaTP

        local corner = Instance.new("UICorner")
        corner.CornerRadius = UDim.new(0, 8)
        corner.Parent = botao

        botao.MouseEnter:Connect(function()
            botao.BackgroundColor3 = Color3.fromRGB(0, 120, 255)
        end)

        botao.MouseLeave:Connect(function()
            botao.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
        end)

        botao.MouseButton1Click:Connect(function()
            local character = player.Character or player.CharacterAdded:Wait()
            local rootPart = character:WaitForChild("HumanoidRootPart")
            rootPart.CFrame = CFrame.new(posicao)
        end)
    end

    -- Função para criar botões na aba Loader que executam scripts externos
local function criarBotaoLoader(nome, scriptURL)
    local botao = Instance.new("TextButton")
    botao.Size = UDim2.new(0, 230, 0, 40)
    botao.Text = nome
    botao.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    botao.TextColor3 = Color3.new(1, 1, 1)
    botao.Font = Enum.Font.Gotham
    botao.TextSize = 22
    botao.Parent = frameLoader

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 8)
    corner.Parent = botao

    botao.MouseEnter:Connect(function()
        botao.BackgroundColor3 = Color3.fromRGB(0, 120, 255)
    end)

    botao.MouseLeave:Connect(function()
        botao.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    end)

    botao.MouseButton1Click:Connect(function()
        loadstring(game:HttpGet(scriptURL))()
    end)
end

    -- Botões de teleporte
    criarBotao("Fábrica", Vector3.new(-2185, 4, 6034))
    criarBotao("Prefeitura", Vector3.new(-1287, 6, -433))
    criarBotao("Lavar Dinheiro", Vector3.new(-959, 16, -179))
    criarBotao("Joalheria", Vector3.new(-1872, 4, -348))
    criarBotao("Armária", Vector3.new(-1807, 4, -791))
    criarBotao("Praça", Vector3.new(-1294, 4, -158))
    criarBotao("Oficina", Vector3.new(-2007, 3, -1332))
    criarBotao("Fazenda", Vector3.new(-2963, 3, 4242))

    -- Botões da aba Loader
criarBotaoLoader("Infinite Yield", "https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source")
criarBotaoLoader("Dex Explorer", "https://raw.githubusercontent.com/peyton2465/Dex/master/out.lua")
criarBotaoLoader("Remote Spy", "https://raw.githubusercontent.com/exxtremestuffs/SimpleSpySource/master/SimpleSpy.lua")

    -- Abrir menu
    abrirBotao.MouseButton1Click:Connect(function()
        menuFrame.Visible = true
        abrirBotao.Visible = false
    end)

    -- Fechar menu
    fecharBotao.MouseButton1Click:Connect(function()
        menuFrame.Visible = false
        abrirBotao.Visible = true
    end)
end

criarMenuTP()
