local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Lighting = game:GetService("Lighting")

local player = Players.LocalPlayer
local PlayerGui = player:WaitForChild("PlayerGui")

-- Criar ou encontrar o RemoteEvent
local ChangeSkyEvent = ReplicatedStorage:FindFirstChild("ChangeSkyEvent")

if not ChangeSkyEvent then
    ChangeSkyEvent = Instance.new("RemoteEvent")
    ChangeSkyEvent.Name = "ChangeSkyEvent"
    ChangeSkyEvent.Parent = ReplicatedStorage
end

-- Criar a ScreenGui no PlayerGui do jogador
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Parent = PlayerGui

-- Criar a Frame e o Botão
local Frame = Instance.new("Frame")
local TextButton = Instance.new("TextButton")

Frame.Name = "SkyChangerFrame"
Frame.Parent = ScreenGui
Frame.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
Frame.BorderColor3 = Color3.fromRGB(0, 0, 0)
Frame.BorderSizePixel = 0
Frame.Position = UDim2.new(0.5, -75, 0.8, -25)
Frame.Size = UDim2.new(0, 150, 0, 50)

TextButton.Parent = Frame
TextButton.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
TextButton.BorderColor3 = Color3.fromRGB(0, 0, 0)
TextButton.BorderSizePixel = 0
TextButton.Size = UDim2.new(1, 0, 1, 0)
TextButton.Font = Enum.Font.SourceSans
TextButton.Text = "ChangeSky"
TextButton.TextColor3 = Color3.fromRGB(0, 0, 0)
TextButton.TextSize = 14

-- Quando o botão for clicado, avisar o servidor para mudar o céu para todos
TextButton.MouseButton1Click:Connect(function()
    ChangeSkyEvent:FireServer()
end)

-- Código do servidor embutido
if player == Players.LocalPlayer then
    ChangeSkyEvent.OnServerEvent:Connect(function()
        -- Remover qualquer Sky existente
        for _, obj in pairs(Lighting:GetChildren()) do
            if obj:IsA("Sky") then
                obj:Destroy()
            end
        end

        -- Criar um novo Sky
        local newSky = Instance.new("Sky")
        newSky.Name = "Teste"
        newSky.Parent = Lighting

        -- Definir os IDs
        local assetId = "rbxassetid://117040568125131"
        newSky.SkyboxBk = assetId
        newSky.SkyboxDn = assetId
        newSky.SkyboxFt = assetId
        newSky.SkyboxLf = assetId
        newSky.SkyboxRt = assetId
        newSky.SkyboxUp = assetId
        newSky.SunTextureId = "0"
    end)
end
