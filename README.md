local player = game.Players.LocalPlayer

local function criarMenuTP()

    ----------------------------------------------------------------
    -- Remover o antigo caso exista
    ----------------------------------------------------------------
    
    local antigoMenu = player:FindFirstChild("PlayerGui"):FindFirstChild("TPMenu")
    if antigoMenu then
        antigoMenu:Destroy()
    end


    local screenGui = Instance.new("ScreenGui")
    screenGui.Name = "TPMenu"
    screenGui.ResetOnSpawn = false
    screenGui.Parent = player:WaitForChild("PlayerGui")

    ----------------------------------------------------------------
    -- Botão para abrir o menu
    ----------------------------------------------------------------

    local abrirBotao = Instance.new("TextButton")
    abrirBotao.Size = UDim2.new(0, 120, 0, 40)
    abrirBotao.Position = UDim2.new(0, 30, 0.3, 30)
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

local UserInputService = game:GetService("UserInputService")

local dragging = false
local dragInput, mouseStartPos, buttonStartPos
local foiArrastado = false
local tempoInicio

abrirBotao.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        foiArrastado = false
        tempoInicio = tick()
        mouseStartPos = input.Position
        buttonStartPos = abrirBotao.Position
    end
end)

abrirBotao.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement then
        dragInput = input
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if input == dragInput and dragging then
        local delta = input.Position - mouseStartPos
        if delta.Magnitude > 5 then
            foiArrastado = true
        end
        abrirBotao.Position = UDim2.new(
            buttonStartPos.X.Scale,
            buttonStartPos.X.Offset + delta.X,
            buttonStartPos.Y.Scale,
            buttonStartPos.Y.Offset + delta.Y
        )
    end
end)

abrirBotao.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = false
        local tempoTotal = tick() - tempoInicio

        if not foiArrastado and tempoTotal < 0.3 then
            -- Clique curto e sem arrasto: abrir menu
            menuFrame.Visible = true
            abrirBotao.Visible = false
        end
    end
end)

    ----------------------------------------------------------------
    -- Menu Principal
    ----------------------------------------------------------------

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

    ----------------------------------------------------------------
    -- Título
    ----------------------------------------------------------------

    local titulo = Instance.new("TextLabel")
    titulo.Size = UDim2.new(1, 0, 0, 50)
    titulo.Text = "Knware Hub"
    titulo.BackgroundTransparency = 1
    titulo.TextColor3 = Color3.new(1, 1, 1)
    titulo.Font = Enum.Font.GothamBold
    titulo.TextSize = 24
    titulo.TextXAlignment = Enum.TextXAlignment.Center
    titulo.Parent = menuFrame

    local TweenService = game:GetService("TweenService")

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
                    TweenService:Create(botao, TweenInfo.new(1.5, Enum.EasingStyle.Linear), {BackgroundColor3 = cor}):Play()
                    task.wait(1.5)
                end
            end
        end)
    end

    local function tweenTitulo(corFinal)
        return TweenService:Create(titulo, TweenInfo.new(1.5, Enum.EasingStyle.Linear, Enum.EasingDirection.InOut), {TextColor3 = corFinal})
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

    ----------------------------------------------------------------
    -- Indicador de roubo do Banco (estilizado e reposicionado)
    ----------------------------------------------------------------
    local bancoStatusFrame = Instance.new("Frame")
    bancoStatusFrame.Size = UDim2.new(0, 200, 0, 30)
    bancoStatusFrame.Position = UDim2.new(0, 10, 1, -40) -- canto inferior esquerdo
    bancoStatusFrame.BackgroundTransparency = 1
    bancoStatusFrame.Parent = menuFrame

    local bancoLabel = Instance.new("TextLabel")
    bancoLabel.Size = UDim2.new(0, 70, 1, 0)
    bancoLabel.Position = UDim2.new(0.2, -25, 0.3, 0)
    bancoLabel.Text = "🏦 -"
    bancoLabel.TextColor3 = Color3.new(1, 1, 1)
    bancoLabel.BackgroundTransparency = 1
    bancoLabel.Font = Enum.Font.GothamBold
    bancoLabel.TextSize = 20
    bancoLabel.TextXAlignment = Enum.TextXAlignment.Left
    bancoLabel.Parent = bancoStatusFrame

    local bancoResposta = Instance.new("TextLabel")
    bancoResposta.Size = UDim2.new(0, 120, 1, 0)
    bancoResposta.Position = UDim2.new(0.2, 10, 0.3, 0)
    bancoResposta.Text = "Fechado"
    bancoResposta.TextColor3 = Color3.fromRGB(255, 0, 0)
    bancoResposta.BackgroundTransparency = 1
    bancoResposta.Font = Enum.Font.GothamBold
    bancoResposta.TextSize = 20
    bancoResposta.TextXAlignment = Enum.TextXAlignment.Left
    bancoResposta.Parent = bancoStatusFrame

local function atualizarStatusBanco()
    local xWorkspace = workspace:FindFirstChild("xWorkspace")
    if not xWorkspace then return end

    local sistemas = xWorkspace:FindFirstChild("Sistemas")
    if not sistemas then return end

    local roubos = sistemas:FindFirstChild("Roubos")
    if not roubos then return end

    local banco = roubos:FindFirstChild("Banco")
    if not banco then return end

    local computadores = banco:FindFirstChild("Computadores")
    if not computadores then return end

    local prompts = {}
    for _, obj in ipairs(computadores:GetDescendants()) do
        if obj:IsA("ProximityPrompt") then
            table.insert(prompts, obj)
        end
    end

    local ativos = 0
    for _, prompt in ipairs(prompts) do
        if prompt.Enabled then
            ativos += 1
        end
    end

    print("Prompts ativos:", ativos)

    if ativos == #prompts and ativos > 0 then
        bancoResposta.Text = "Aberto"
        bancoResposta.TextColor3 = Color3.fromRGB(0, 255, 0)
    else
        bancoResposta.Text = "Fechado"
        bancoResposta.TextColor3 = Color3.fromRGB(255, 0, 0)
    end
end

        task.spawn(function()
        local sucesso, erro = pcall(function()
            local xWorkspace = workspace:WaitForChild("xWorkspace", 10)
            local sistemas = xWorkspace:WaitForChild("Sistemas", 10)
            local roubos = sistemas:WaitForChild("Roubos", 10)
            local banco = roubos:WaitForChild("Banco", 10)
            local computadores = banco:WaitForChild("Computadores", 10)

for _, obj in ipairs(computadores:GetDescendants()) do
    if obj:IsA("ProximityPrompt") then
        obj:GetPropertyChangedSignal("Enabled"):Connect(atualizarStatusBanco)
    end
end

            atualizarStatusBanco()
        end)

        if not sucesso then
            warn("Erro ao monitorar prompts do Banco:", erro)
        end
    end)

----------------------------------------------------------------
-- Indicador de roubo do Porto (ao lado do Banco)
----------------------------------------------------------------

local portoStatusFrame = Instance.new("Frame")
portoStatusFrame.Size = UDim2.new(0, 200, 0, 30)
portoStatusFrame.Position = UDim2.new(0, 220, 1, -40) -- ao lado do Banco
portoStatusFrame.BackgroundTransparency = 1
portoStatusFrame.Parent = menuFrame

local portoLabel = Instance.new("TextLabel")
portoLabel.Size = UDim2.new(0, 70, 1, 0)
portoLabel.Position = UDim2.new(-0.3, 0, 0.3, 0)
portoLabel.Text = "🏭 -"
portoLabel.TextColor3 = Color3.new(1, 1, 1)
portoLabel.BackgroundTransparency = 1
portoLabel.Font = Enum.Font.GothamBold
portoLabel.TextSize = 20
portoLabel.TextXAlignment = Enum.TextXAlignment.Left
portoLabel.Parent = portoStatusFrame

local portoResposta = Instance.new("TextLabel")
portoResposta.Size = UDim2.new(0, 120, 1, 0)
portoResposta.Position = UDim2.new(-0.3, 35, 0.3, 0)
portoResposta.Text = "Fechado"
portoResposta.TextColor3 = Color3.fromRGB(255, 0, 0)
portoResposta.BackgroundTransparency = 1
portoResposta.Font = Enum.Font.GothamBold
portoResposta.TextSize = 20
portoResposta.TextXAlignment = Enum.TextXAlignment.Left
portoResposta.Parent = portoStatusFrame

-- Função para atualizar o status do Porto
local function atualizarStatusPorto()
    local xWorkspace = workspace:FindFirstChild("xWorkspace")
    if not xWorkspace then return end

    local sistemas = xWorkspace:FindFirstChild("Sistemas")
    if not sistemas then return end

    local roubos = sistemas:FindFirstChild("Roubos")
    if not roubos then return end

    local porto = roubos:FindFirstChild("Porto")
    if not porto then return end

    local puzzle = porto:FindFirstChild("Puzzle")
    if not puzzle then return end

    local prompts = {}
    for _, obj in ipairs(puzzle:GetDescendants()) do
        if obj:IsA("ProximityPrompt") then
            table.insert(prompts, obj)
        end
    end

    local ativos = 0
    for _, prompt in ipairs(prompts) do
        if prompt.Enabled then
            ativos += 1
        end
    end

    if ativos == #prompts and ativos > 0 then
        portoResposta.Text = "Aberto"
        portoResposta.TextColor3 = Color3.fromRGB(0, 255, 0)
    else
        portoResposta.Text = "Fechado"
        portoResposta.TextColor3 = Color3.fromRGB(255, 0, 0)
    end
end

-- Monitoramento dos prompts do Porto
task.spawn(function()
    local sucesso, erro = pcall(function()
        local xWorkspace = workspace:WaitForChild("xWorkspace", 10)
        local sistemas = xWorkspace:WaitForChild("Sistemas", 10)
        local roubos = sistemas:WaitForChild("Roubos", 10)
        local porto = roubos:WaitForChild("Porto", 10)
        local puzzle = porto:WaitForChild("Puzzle", 10)

        for _, obj in ipairs(puzzle:GetDescendants()) do
            if obj:IsA("ProximityPrompt") then
                obj:GetPropertyChangedSignal("Enabled"):Connect(atualizarStatusPorto)
            end
        end

        atualizarStatusPorto()
    end)

    if not sucesso then
        warn("Erro ao monitorar prompts do Porto:", erro)
    end
end)

    ----------------------------------------------------------------
    -- Aabas
    ----------------------------------------------------------------

    local abasContainer = Instance.new("Frame")
    abasContainer.Size = UDim2.new(1, 0, 0, 30)
    abasContainer.Position = UDim2.new(0, 0, 0, 60)
    abasContainer.BackgroundTransparency = 1
    abasContainer.Parent = menuFrame

    local abasLayout = Instance.new("UIListLayout")
    abasLayout.FillDirection = Enum.FillDirection.Horizontal
    abasLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
    abasLayout.Padding = UDim.new(0, 10)
    abasLayout.Parent = abasContainer

    local function estilizarAba(botao)
        botao.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
        botao.TextColor3 = Color3.new(1, 1, 1)
        botao.Font = Enum.Font.GothamBold
        botao.TextSize = 18
        Instance.new("UICorner", botao).CornerRadius = UDim.new(0, 8)
        local stroke = Instance.new("UIStroke", botao)
        stroke.Color = Color3.fromRGB(60, 60, 60)
        stroke.Thickness = 1
        botao.MouseEnter:Connect(function() botao.BackgroundColor3 = Color3.fromRGB(0, 120, 255) end)
        botao.MouseLeave:Connect(function() botao.BackgroundColor3 = Color3.fromRGB(30, 30, 30) end)
    end

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

    ----------------------------------------------------------------
    -- frames das abas
    ----------------------------------------------------------------

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

    Instance.new("UIGridLayout", frameLoader).CellSize = UDim2.new(0, 230, 0, 40)

    ----------------------------------------------------------------
    -- Alternância de abas
    ----------------------------------------------------------------
    
    local function ativarAba(botaoAtivo)
        for _, b in pairs({abaTeleporte, abaAcoes, abaLoader}) do
            b.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
        end
        botaoAtivo.BackgroundColor3 = Color3.fromRGB(0, 120, 255)
        frameTeleporte.Visible = botaoAtivo == abaTeleporte
                frameAcoes.Visible = botaoAtivo == abaAcoes
        frameLoader.Visible = botaoAtivo == abaLoader
    end

    abaTeleporte.MouseButton1Click:Connect(function()
        ativarAba(abaTeleporte)
    end)
    abaAcoes.MouseButton1Click:Connect(function()
        ativarAba(abaAcoes)
    end)
    abaLoader.MouseButton1Click:Connect(function()
        ativarAba(abaLoader)
    end)

    ----------------------------------------------------------------
    -- Botão de fechar
    ----------------------------------------------------------------

    local fecharBotao = Instance.new("TextButton")
    fecharBotao.Size = UDim2.new(0, 30, 0, 30)
    fecharBotao.Position = UDim2.new(1, -35, 0, 10)
    fecharBotao.Text = "X"
    fecharBotao.BackgroundColor3 = Color3.fromRGB(170, 0, 0)
    fecharBotao.TextColor3 = Color3.new(1, 1, 1)
    fecharBotao.Font = Enum.Font.GothamBold
    fecharBotao.TextSize = 18
    fecharBotao.Parent = menuFrame
    Instance.new("UICorner", fecharBotao).CornerRadius = UDim.new(0, 8)

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

    ----------------------------------------------------------------
    -- Função para clicar no ProximityPrompt mais próximo
    ----------------------------------------------------------------

    local function clicarPromptMaisProximo()
        local char = player.Character or player.CharacterAdded:Wait()
        local hrp = char:WaitForChild("HumanoidRootPart")
        task.wait(0.3)

        local promptMaisProximo = nil
        local menorDistancia = math.huge

        for _, obj in ipairs(workspace:GetDescendants()) do
            if obj:IsA("ProximityPrompt") and obj.Enabled then
                local part = obj.Parent:IsA("BasePart") and obj.Parent or obj.Parent:FindFirstChildWhichIsA("BasePart")
                if part then
                    local dist = (part.Position - hrp.Position).Magnitude
                    if dist < menorDistancia and dist <= obj.MaxActivationDistance then
                        menorDistancia = dist
                        promptMaisProximo = obj
                    end
                end
            end
        end

        if promptMaisProximo then
            pcall(function()
                promptMaisProximo:InputHoldBegin()
                task.wait(0.05) -- clique rápido
                promptMaisProximo:InputHoldEnd()
            end)
        end
    end

    ----------------------------------------------------------------
    -- Botão AutoFarm Lixeiro
    ----------------------------------------------------------------

    local botaoAutoFarm = Instance.new("TextButton")
    botaoAutoFarm.Size = UDim2.new(0, 230, 0, 40)
    botaoAutoFarm.Text = "Lixeiro Farm [OFF]"
    botaoAutoFarm.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    botaoAutoFarm.TextColor3 = Color3.new(1, 1, 1)
    botaoAutoFarm.Font = Enum.Font.GothamBold
    botaoAutoFarm.TextSize = 18
    botaoAutoFarm.Parent = frameAcoes
    Instance.new("UICorner", botaoAutoFarm).CornerRadius = UDim.new(0, 8)

    local autoFarmAtivo = false

    local function iniciarAutoFarmFazendeiro()
        task.spawn(function()
            while autoFarmAtivo do
                local char = player.Character or player.CharacterAdded:Wait()
                local hrp = char:WaitForChild("HumanoidRootPart")

                -- Primeiro ponto
                hrp.CFrame = CFrame.new(Vector3.new(-1642, 4, -548))
                task.wait(0)
                clicarPromptMaisProximo()

                -- Espera antes do segundo ponto
                task.wait(4)

                -- Segundo ponto
                hrp.CFrame = CFrame.new(Vector3.new(-1626, 4, -425))
                task.wait(0.5)
                clicarPromptMaisProximo()

                -- Espera antes de repetir
                task.wait(0)
            end
        end)
    end

    botaoAutoFarm.MouseButton1Click:Connect(function()
        autoFarmAtivo = not autoFarmAtivo
        botaoAutoFarm.Text = autoFarmAtivo and "Lixeiro Farm [ON]" or "Lixeiro Farm [OFF]"
        botaoAutoFarm.BackgroundColor3 = autoFarmAtivo and Color3.fromRGB(0, 170, 0) or Color3.fromRGB(40, 40, 40)
        if autoFarmAtivo then
            iniciarAutoFarmFazendeiro()
        end
    end)

    ----------------------------------------------------------------
    -- ScrollingFrame para teleporte
    ----------------------------------------------------------------

    local listaTP = Instance.new("ScrollingFrame")
    listaTP.ScrollingDirection = Enum.ScrollingDirection.XY
    listaTP.Size = UDim2.new(1, -20, 1, -20)
    listaTP.Position = UDim2.new(0, 10, 0, 10)
    listaTP.CanvasSize = UDim2.new(0, 0, 0, 0)
    listaTP.ScrollBarThickness = 0
    listaTP.BackgroundTransparency = 1
    listaTP.Parent = frameTeleporte

    local grid = Instance.new("UIGridLayout")
    grid.CellSize = UDim2.new(0, 230, 0, 40)
    grid.CellPadding = UDim2.new(0, 20, 0, 10)
    grid.SortOrder = Enum.SortOrder.LayoutOrder
    grid.FillDirection = Enum.FillDirection.Vertical
    grid.Parent = listaTP

    grid:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
        listaTP.CanvasSize = UDim2.new(0, grid.AbsoluteContentSize.X, 0, grid.AbsoluteContentSize.Y)
    end)

    ----------------------------------------------------------------
    -- Função para criar botões de teleporte
    ----------------------------------------------------------------

    local function criarBotao(nome, posicao)
        local botao = Instance.new("TextButton")
        botao.Size = UDim2.new(0, 260, 0, 40)
        botao.Text = nome
        botao.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
        botao.TextColor3 = Color3.new(1, 1, 1)
        botao.Font = Enum.Font.Gotham
        botao.TextSize = 22
        botao.Parent = listaTP
        Instance.new("UICorner", botao).CornerRadius = UDim.new(0, 8)

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

    ----------------------------------------------------------------
    -- Função para criar botões na aba Loader
    ----------------------------------------------------------------

    local function criarBotaoLoader(nome, scriptURL)
        local botao = Instance.new("TextButton")
        botao.Size = UDim2.new(0, 230, 0, 40)
        botao.Text = nome
        botao.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
        botao.TextColor3 = Color3.new(1, 1, 1)
        botao.Font = Enum.Font.Gotham
        botao.TextSize = 22
        botao.Parent = frameLoader
        Instance.new("UICorner", botao).CornerRadius = UDim.new(0, 8)

        botao.MouseEnter:Connect(function()
            botao.BackgroundColor3 = Color3.fromRGB(0, 120, 255)
        end)
        botao.MouseLeave:Connect(function()
            botao.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
        end)
        botao.MouseButton1Click:Connect(function()
            local success, err = pcall(function()
                local scriptCode = game:HttpGet(scriptURL, true)
                local executeScript = loadstring(scriptCode)
                if executeScript then
                    executeScript()
                else
                    warn("Falha ao carregar o script.")
                end
            end)
            if not success then
                warn("Erro ao executar script:", err)
            end
        end)
    end

    ----------------------------------------------------------------
    -- Botões de teleporte
    ----------------------------------------------------------------

    criarBotao("Armazem", Vector3.new(-2185, 4, 6034))
    criarBotao("Prefeitura", Vector3.new(-1287, 6, -433))
    criarBotao("Lavar Dinheiro", Vector3.new(-959, 16, -179))
    criarBotao("Joalheria", Vector3.new(-1872, 4, -348))
    criarBotao("Armária", Vector3.new(-1807, 4, -791))
    criarBotao("Praça", Vector3.new(-1294, 4, -158))
    criarBotao("Oficina", Vector3.new(-2007, 3, -1332))
    criarBotao("Fazenda", Vector3.new(-2963, 3, 4242))
    criarBotao("Equipamentos", Vector3.new(-1991, 4, -941))
    criarBotao("Banco", Vector3.new(-1166, 4, -1043))
    criarBotao("Posto praça", Vector3.new(-992, 4, 267))    
    criarBotao("Posto atrás prefeitura", Vector3.new(-1424, 4, -544))
    criarBotao("Bradesco", Vector3.new(-1952, 4, -816))
    criarBotao("Concessionária", Vector3.new(-992, 4, -32))

    ----------------------------------------------------------------
    -- Botões da aba Loader
    ----------------------------------------------------------------

    criarBotaoLoader("Infinit Yield", "https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source")

    ----------------------------------------------------------------
    -- Abrir menu
    ----------------------------------------------------------------

    abrirBotao.MouseButton1Click:Connect(function()
        menuFrame.Visible = true
        abrirBotao.Visible = false
    end)
end

-- Executa o menu
criarMenuTP()
