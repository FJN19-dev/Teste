
-- NovaLib - Biblioteca para criação de UI no Roblox
local NovaLib = {}

function NovaLib:CreateWindow(title)
    local Players = game:GetService("Players")
    local player = Players.LocalPlayer

    -- Criação do ScreenGui e Frame
    local ScreenGui = Instance.new("ScreenGui", game:GetService("CoreGui"))
    ScreenGui.Name = "NovaUI"

    local Frame = Instance.new("Frame", ScreenGui)
    Frame.Size = UDim2.new(0, 500, 0, 500)
    Frame.Position = UDim2.new(0.5, -250, 0.5, -250)
    Frame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
    Frame.Active = true
    Frame.Draggable = true

    -- Título da Interface
    local Title = Instance.new("TextLabel", Frame)
    Title.Text = "Slayer Hub"
    Title.Size = UDim2.new(1, 0, 0, 40)
    Title.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    Title.TextColor3 = Color3.fromRGB(255, 255, 255)
    Title.Font = Enum.Font.GothamBold
    Title.TextSize = 18

    -- Imagem do Logo
    local LogoImage = Instance.new("ImageLabel", Frame)
    LogoImage.Size = UDim2.new(0, 100, 0, 100)
    LogoImage.Position = UDim2.new(0.5, -50, 0, 50)
    LogoImage.Image = "rbxassetid://91062721750487"  -- Substitua pelo ID da sua imagem
    LogoImage.BackgroundTransparency = 1

    -- Tela de Carregamento
    local LoadingFrame = Instance.new("Frame", ScreenGui)
    LoadingFrame.Size = UDim2.new(0, 500, 0, 500)
    LoadingFrame.Position = UDim2.new(0.5, -250, 0.5, -250)
    LoadingFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
    LoadingFrame.Visible = true

    local LoadingLabel = Instance.new("TextLabel", LoadingFrame)
    LoadingLabel.Size = UDim2.new(1, 0, 0, 40)
    LoadingLabel.Position = UDim2.new(0, 0, 0, 10)
    LoadingLabel.BackgroundTransparency = 1
    LoadingLabel.Text = "Carregando..."
    LoadingLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    LoadingLabel.Font = Enum.Font.GothamBold
    LoadingLabel.TextSize = 18

    -- Barra de Carregamento
    local ProgressBarBackground = Instance.new("Frame", LoadingFrame)
    ProgressBarBackground.Size = UDim2.new(0.8, 0, 0, 20)
    ProgressBarBackground.Position = UDim2.new(0.5, -160, 0, 100)
    ProgressBarBackground.BackgroundColor3 = Color3.fromRGB(50, 50, 50)

    local ProgressBar = Instance.new("Frame", ProgressBarBackground)
    ProgressBar.Size = UDim2.new(0, 0, 1, 0)
    ProgressBar.BackgroundColor3 = Color3.fromRGB(0, 255, 0)

    -- Função de Carregamento
    local function loadProgress()
        for i = 1, 100 do
            wait(0.05)
            ProgressBar.Size = UDim2.new(i / 100, 0, 1, 0)
        end
        LoadingFrame.Visible = false  -- Esconde a tela de carregamento após o fim
        Frame.Visible = true  -- Mostra a interface após o carregamento
    end

    -- Iniciar o carregamento
    loadProgress()

    -- Container da UI
    local Container = Instance.new("ScrollingFrame", Frame)
    Container.Size = UDim2.new(1, -20, 1, -85)
    Container.Position = UDim2.new(0, 10, 0, 85)
    Container.BackgroundTransparency = 1
    Container.CanvasSize = UDim2.new(0, 0, 5, 0)
    Container.ScrollBarThickness = 5

    local layout = Instance.new("UIListLayout", Container)
    layout.Padding = UDim.new(0, 8)

    -- Função para adicionar uma imagem de fundo
    function Frame:SetBackgroundImage(imageId)
        local BackgroundImage = Instance.new("ImageLabel", Frame)
        BackgroundImage.Size = UDim2.new(1, 0, 1, 0)
        BackgroundImage.Position = UDim2.new(0, 0, 0, 0)
        BackgroundImage.Image = imageId  -- Define a imagem de fundo
        BackgroundImage.BackgroundTransparency = 1
    end

    -- Exemplo de adicionar uma imagem de fundo
    -- Defina o ID da sua imagem de fundo
    Frame:SetBackgroundImage("rbxassetid://77350042728665")

    return ScreenGui
end

return NovaLib
