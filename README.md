-- NovaLib - UI Avançada

local NovaLib = {}

function NovaLib:CreateWindow(title)
    local Players = game:GetService("Players")
    local player = Players.LocalPlayer

    local ScreenGui = Instance.new("ScreenGui", game:GetService("CoreGui"))
    ScreenGui.Name = "NovaUI"

    local Frame = Instance.new("Frame", ScreenGui)
    Frame.Size = UDim2.new(0, 500, 0, 400)
    Frame.Position = UDim2.new(0.5, -250, 0.5, -200)
    Frame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
    Frame.Active = true
    Frame.Draggable = true

    local Title = Instance.new("TextLabel", Frame)
    Title.Text = title or "NovaLib UI"
    Title.Size = UDim2.new(1, 0, 0, 40)
    Title.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    Title.TextColor3 = Color3.fromRGB(255, 255, 255)
    Title.Font = Enum.Font.GothamBold
    Title.TextSize = 18

    local MinimizeButton = Instance.new("TextButton", Frame)
    MinimizeButton.Size = UDim2.new(0, 40, 0, 40)
    MinimizeButton.Position = UDim2.new(1, -40, 0, 0)
    MinimizeButton.Text = "-"
    MinimizeButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    MinimizeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
    MinimizeButton.Font = Enum.Font.GothamBold

    local SearchBox = Instance.new("TextBox", Frame)
    SearchBox.PlaceholderText = "Pesquisar..."
    SearchBox.Size = UDim2.new(1, -20, 0, 30)
    SearchBox.Position = UDim2.new(0, 10, 0, 45)
    SearchBox.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
    SearchBox.TextColor3 = Color3.fromRGB(255, 255, 255)
    SearchBox.Font = Enum.Font.Gotham
    SearchBox.TextSize = 14

    local Container = Instance.new("ScrollingFrame", Frame)
    Container.Size = UDim2.new(1, -20, 1, -85)
    Container.Position = UDim2.new(0, 10, 0, 85)
    Container.BackgroundTransparency = 1
    Container.CanvasSize = UDim2.new(0, 0, 5, 0)
    Container.ScrollBarThickness = 5

    local layout = Instance.new("UIListLayout", Container)
    layout.Padding = UDim.new(0, 8)

    local hidden = false
    MinimizeButton.MouseButton1Click:Connect(function()
        hidden = not hidden
        Container.Visible = not hidden
    end)

    local interface = {}

    function interface:CreateToggle(text, default, callback)
        local Toggle = Instance.new("TextButton", Container)
        Toggle.Size = UDim2.new(1, 0, 0, 30)
        Toggle.Text = text .. ": " .. tostring(default)
        Toggle.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
        Toggle.TextColor3 = Color3.fromRGB(255, 255, 255)
        Toggle.Font = Enum.Font.Gotham
        Toggle.TextSize = 14

        local state = default
        Toggle.MouseButton1Click:Connect(function()
            state = not state
            Toggle.Text = text .. ": " .. tostring(state)
            if callback then callback(state) end
        end)
    end

    function interface:CreateSlider(text, min, max, default, callback)
        local Frame = Instance.new("Frame", Container)
        Frame.Size = UDim2.new(1, 0, 0, 30)
        Frame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)

        local Label = Instance.new("TextLabel", Frame)
        Label.Size = UDim2.new(0.6, 0, 1, 0)
        Label.Text = text .. ": " .. tostring(default)
        Label.BackgroundTransparency = 1
        Label.TextColor3 = Color3.fromRGB(255, 255, 255)
        Label.Font = Enum.Font.Gotham
        Label.TextSize = 14

        local Button = Instance.new("TextButton", Frame)
        Button.Size = UDim2.new(0.4, 0, 1, 0)
        Button.Position = UDim2.new(0.6, 0, 0, 0)
        Button.Text = tostring(default)
        Button.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
        Button.TextColor3 = Color3.fromRGB(255, 255, 255)
        Button.Font = Enum.Font.Gotham
        Button.TextSize = 14

        local value = default
        Button.MouseButton1Click:Connect(function()
            value = (value + 1 > max) and min or value + 1
            Button.Text = tostring(value)
            Label.Text = text .. ": " .. tostring(value)
            if callback then callback(value) end
        end)
    end

    function interface:CreateDropdown(text, options, callback)
        local Dropdown = Instance.new("TextButton", Container)
        Dropdown.Size = UDim2.new(1, 0, 0, 30)
        Dropdown.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
        Dropdown.TextColor3 = Color3.fromRGB(255, 255, 255)
        Dropdown.Font = Enum.Font.Gotham
        Dropdown.TextSize = 14
        local selected = options[1]
        Dropdown.Text = text .. ": " .. selected

        Dropdown.MouseButton1Click:Connect(function()
            local i = table.find(options, selected) + 1
            if i > #options then i = 1 end
            selected = options[i]
            Dropdown.Text = text .. ": " .. selected
            if callback then callback(selected) end
        end)
    end

    return interface
end

return NovaLib
