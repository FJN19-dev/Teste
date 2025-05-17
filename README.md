-- KeyLibSimple.lua
local KeyLibSimple = {}

function KeyLibSimple:Start(config)
    assert(type(config) == "table", "Configuração obrigatória")
    assert(type(config.Keys) == "table", "Lista de Keys obrigatória")
    assert(type(config.Script) == "string", "URL do script obrigatório")

    -- Criar ScreenGui
    local screenGui = Instance.new("ScreenGui")
    screenGui.Name = "KeyLibSimpleGui"
    screenGui.ResetOnSpawn = false
    screenGui.Parent = game:GetService("CoreGui")

    -- Fundo full screen usando ImageLabel para mostrar imagem do Roblox pelo Id
    local background = Instance.new("ImageLabel")
    background.Size = UDim2.new(1, 0, 1, 0)
    background.Position = UDim2.new(0, 0, 0, 0)
    background.Image = "rbxassetid://77350042728665" .. tostring(config.BackgroundImageId or 0)
    background.BackgroundTransparency = 0
    background.ScaleType = Enum.ScaleType.Crop
    background.Parent = screenGui

    -- Texto central no topo
    local titleHub = Instance.new("TextLabel")
    titleHub.Size = UDim2.new(1, 0, 0, 60)
    titleHub.Position = UDim2.new(0, 0, 0, 20)
    titleHub.BackgroundTransparency = 1
    titleHub.TextColor3 = config.TitleColor or Color3.fromRGB(255, 255, 255)
    titleHub.Font = Enum.Font.GothamBold
    titleHub.TextSize = 40
    titleHub.Text = config.HubName or "Slayer Hub"
    titleHub.TextStrokeTransparency = 0.75
    titleHub.Parent = screenGui

    -- Frame central para input
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(0, 300, 0, 150)
    frame.Position = UDim2.new(0.5, -150, 0.5, -75)
    frame.BackgroundColor3 = config.FrameColor or Color3.fromRGB(30, 30, 30)
    frame.BorderSizePixel = 0
    frame.Parent = screenGui

    -- Título do frame
    local title = Instance.new("TextLabel")
    title.Size = UDim2.new(1, 0, 0, 40)
    title.BackgroundTransparency = 1
    title.TextColor3 = config.TitleColor or Color3.fromRGB(255, 255, 255)
    title.Font = Enum.Font.GothamBold
    title.TextSize = 24
    title.Text = config.Title or "Sistema de Key"
    title.Parent = frame

    -- TextBox para digitar a key
    local keyBox = Instance.new("TextBox")
    keyBox.Size = UDim2.new(0.9, 0, 0, 40)
    keyBox.Position = UDim2.new(0.05, 0, 0, 50)
    keyBox.PlaceholderText = config.Placeholder or "Digite a chave aqui"
    keyBox.ClearTextOnFocus = false
    keyBox.Text = ""
    keyBox.TextColor3 = Color3.fromRGB(255, 255, 255)
    keyBox.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    keyBox.Parent = frame

    -- Botão confirmar
    local button = Instance.new("TextButton")
    button.Size = UDim2.new(0.5, 0, 0, 40)
    button.Position = UDim2.new(0.25, 0, 0, 100)
    button.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
    button.TextColor3 = Color3.fromRGB(255, 255, 255)
    button.Text = config.ButtonText or "Confirmar"
    button.Parent = frame

    -- Texto de mensagem (erro/sucesso)
    local msgLabel = Instance.new("TextLabel")
    msgLabel.Size = UDim2.new(1, 0, 0, 30)
    msgLabel.Position = UDim2.new(0, 0, 0, 140)
    msgLabel.BackgroundTransparency = 1
    msgLabel.TextColor3 = Color3.fromRGB(255, 100, 100)
    msgLabel.Font = Enum.Font.Gotham
    msgLabel.TextSize = 18
    msgLabel.Text = ""
    msgLabel.Parent = frame

    -- Função para validar key
    local function ValidateKey(input)
        for _, key in ipairs(config.Keys) do
            if input == key then
                return true
            end
        end
        return false
    end

    -- Clique no botão
    button.MouseButton1Click:Connect(function()
        local inputKey = keyBox.Text
        if ValidateKey(inputKey) then
            msgLabel.TextColor3 = Color3.fromRGB(100, 255, 100)
            msgLabel.Text = "Chave válida! Carregando..."
            task.spawn(function()
                task.wait(1)
                screenGui:Destroy()
                loadstring(game:HttpGet(config.Script))()
            end)
        else
            msgLabel.TextColor3 = Color3.fromRGB(255, 100, 100)
            msgLabel.Text = "Chave inválida. Tente novamente."
        end
    end)
end

return KeyLibSimple
