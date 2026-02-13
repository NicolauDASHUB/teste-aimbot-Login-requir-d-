# teste-aimbot-Login-requir-d-
--[[
    Aimbot Teste V7 - WALL TRANSPARENCY EDITION
    Substituído: Team Check -> Wall Transparency
    Credenciais: Nicolas aimbot / nicolas696xits
]]

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera

-- Configurações Globais
_G.AimbotSettings = {
    AimbotEnabled = false,
    SpinBotEnabled = false,
    ESPEnabled = false,
    NoclipEnabled = false,
    WallCheck = false,
    WallTransEnabled = false, -- Nova Função
    TargetPart = "Head",
    AimbotRange = 2000,
    SpinSpeed = 100,
    LoggedIn = false
}

local CORRECT_USER = "Nicolas aimbot"
local CORRECT_PASS = "nicolas696xits"

-- Armazenamento de transparência original
local originalTransparencies = {}

-- Interface
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "AimbotTeste_V7"
ScreenGui.ResetOnSpawn = false
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")

-- TELA DE LOGIN
local LoginFrame = Instance.new("Frame")
LoginFrame.Size = UDim2.new(0, 280, 0, 340)
LoginFrame.Position = UDim2.new(0.5, -140, 0.5, -170)
LoginFrame.BackgroundColor3 = Color3.fromRGB(30, 0, 0)
LoginFrame.BorderSizePixel = 2
LoginFrame.BorderColor3 = Color3.fromRGB(255, 0, 0)
LoginFrame.Active = true
LoginFrame.Draggable = true
LoginFrame.Parent = ScreenGui

local LoginTitle = Instance.new("TextLabel")
LoginTitle.Size = UDim2.new(1, 0, 0, 50)
LoginTitle.BackgroundColor3 = Color3.fromRGB(50, 0, 0)
LoginTitle.Text = "LOGIN - V7 X-RAY"
LoginTitle.TextColor3 = Color3.fromRGB(255, 255, 255)
LoginTitle.Font = Enum.Font.GothamBold
LoginTitle.TextSize = 18
LoginTitle.Parent = LoginFrame

local UserInput = Instance.new("TextBox")
UserInput.Size = UDim2.new(0.8, 0, 0, 40)
UserInput.Position = UDim2.new(0.1, 0, 0, 80)
UserInput.PlaceholderText = "Usuário"
UserInput.Text = ""
UserInput.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
UserInput.TextColor3 = Color3.fromRGB(255, 255, 255)
UserInput.Parent = LoginFrame

local PassInput = Instance.new("TextBox")
PassInput.Size = UDim2.new(0.8, 0, 0, 40)
PassInput.Position = UDim2.new(0.1, 0, 0, 130)
PassInput.PlaceholderText = "Senha"
PassInput.Text = ""
PassInput.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
PassInput.TextColor3 = Color3.fromRGB(255, 255, 255)
PassInput.Parent = LoginFrame

local LoginBtn = Instance.new("TextButton")
LoginBtn.Size = UDim2.new(0.8, 0, 0, 45)
LoginBtn.Position = UDim2.new(0.1, 0, 0, 200)
LoginBtn.BackgroundColor3 = Color3.fromRGB(0, 120, 0)
LoginBtn.Text = "ENTRAR"
LoginBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
LoginBtn.Font = Enum.Font.GothamBold
LoginBtn.Parent = LoginFrame

-- MENU PRINCIPAL
local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 240, 0, 420)
MainFrame.Position = UDim2.new(0.5, -120, 0.5, -210)
MainFrame.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
MainFrame.BorderSizePixel = 2
MainFrame.BorderColor3 = Color3.fromRGB(255, 0, 0)
MainFrame.Visible = false
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Parent = ScreenGui

local TopBar = Instance.new("Frame")
TopBar.Size = UDim2.new(1, 0, 0, 35)
TopBar.BackgroundColor3 = Color3.fromRGB(80, 0, 0)
TopBar.Parent = MainFrame

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(0.6, 0, 1, 0)
Title.BackgroundTransparency = 1
Title.Text = "AIMBOT V7"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.Font = Enum.Font.GothamBold
Title.TextSize = 14
Title.Parent = TopBar

local CloseBtn = Instance.new("TextButton")
CloseBtn.Size = UDim2.new(0, 30, 0, 25)
CloseBtn.Position = UDim2.new(1, -35, 0, 5)
CloseBtn.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
CloseBtn.Text = "X"
CloseBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseBtn.Parent = TopBar

local MinBtn = Instance.new("TextButton")
MinBtn.Size = UDim2.new(0, 30, 0, 25)
MinBtn.Position = UDim2.new(1, -70, 0, 5)
MinBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
MinBtn.Text = "-"
MinBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
MinBtn.Parent = TopBar

local Content = Instance.new("ScrollingFrame")
Content.Size = UDim2.new(1, 0, 1, -35)
Content.Position = UDim2.new(0, 0, 0, 35)
Content.BackgroundTransparency = 1
Content.CanvasSize = UDim2.new(0, 0, 1.3, 0)
Content.ScrollBarThickness = 2
Content.Parent = MainFrame

local UIList = Instance.new("UIListLayout")
UIList.Padding = UDim.new(0, 5)
UIList.HorizontalAlignment = Enum.HorizontalAlignment.Center
UIList.Parent = Content

-- Função de Toggle
local function CreateToggle(text, settingKey, callback)
    local Btn = Instance.new("TextButton")
    Btn.Size = UDim2.new(0.9, 0, 0, 35)
    Btn.BackgroundColor3 = _G.AimbotSettings[settingKey] and Color3.fromRGB(0, 150, 0) or Color3.fromRGB(120, 0, 0)
    Btn.Text = text .. (_G.AimbotSettings[settingKey] and ": ON" or ": OFF")
    Btn.TextColor3 = Color3.fromRGB(255, 255, 255)
    Btn.Font = Enum.Font.SourceSansBold
    Btn.Parent = Content

    Btn.MouseButton1Click:Connect(function()
        _G.AimbotSettings[settingKey] = not _G.AimbotSettings[settingKey]
        Btn.Text = text .. (_G.AimbotSettings[settingKey] and ": ON" or ": OFF")
        Btn.BackgroundColor3 = _G.AimbotSettings[settingKey] and Color3.fromRGB(0, 150, 0) or Color3.fromRGB(120, 0, 0)
        if callback then callback(_G.AimbotSettings[settingKey]) end
    end)
end

-- Lógica de Transparência de Paredes
local function SetWallTransparency(enabled)
    for _, obj in pairs(workspace:GetDescendants()) do
        if obj:IsA("BasePart") and not obj:IsDescendantOf(LocalPlayer.Character) and not obj.Parent:FindFirstChild("Humanoid") then
            if enabled then
                if not originalTransparencies[obj] then
                    originalTransparencies[obj] = obj.Transparency
                end
                obj.Transparency = 0.5 -- Semi-transparente
            else
                if originalTransparencies[obj] then
                    obj.Transparency = originalTransparencies[obj]
                end
            end
        end
    end
end

local PartBtn = Instance.new("TextButton")
PartBtn.Size = UDim2.new(0.9, 0, 0, 35)
PartBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
PartBtn.Text = "Alvo: Cabeça"
PartBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
PartBtn.Font = Enum.Font.SourceSansBold
PartBtn.Parent = Content

PartBtn.MouseButton1Click:Connect(function()
    if _G.AimbotSettings.TargetPart == "Head" then
        _G.AimbotSettings.TargetPart = "HumanoidRootPart"
        PartBtn.Text = "Alvo: Torso"
    else
        _G.AimbotSettings.TargetPart = "Head"
        PartBtn.Text = "Alvo: Cabeça"
    end
end)

CreateToggle("Aimbot", "AimbotEnabled")
CreateToggle("Wall Trans", "WallTransEnabled", SetWallTransparency) -- Substituiu Team Check
CreateToggle("Wall Check", "WallCheck")
CreateToggle("SpinBot", "SpinBotEnabled")
CreateToggle("ESP Lines", "ESPEnabled")
CreateToggle("Noclip", "NoclipEnabled")

-- Lógica de Minimizar/Fechar
local minimized = false
MinBtn.MouseButton1Click:Connect(function()
    minimized = not minimized
    Content.Visible = not minimized
    MainFrame.Size = minimized and UDim2.new(0, 240, 0, 35) or UDim2.new(0, 240, 0, 420)
end)

CloseBtn.MouseButton1Click:Connect(function() ScreenGui:Destroy() end)

-- Lógica de Login
LoginBtn.MouseButton1Click:Connect(function()
    local u = UserInput.Text:gsub("^%s*(.-)%s*$", "%1")
    local p = PassInput.Text:gsub("^%s*(.-)%s*$", "%1")
    if u == CORRECT_USER and p == CORRECT_PASS then
        LoginFrame.Visible = false
        MainFrame.Visible = true
        _G.AimbotSettings.LoggedIn = true
    end
end)

-- LÓGICA DE JOGO
local function IsVisible(part)
    if not _G.AimbotSettings.WallCheck then return true end
    local origin = Camera.CFrame.Position
    local direction = (part.Position - origin).Unit * (part.Position - origin).Magnitude
    local rayParams = RaycastParams.new()
    rayParams.FilterType = Enum.RaycastFilterType.Blacklist
    rayParams.FilterDescendantsInstances = {LocalPlayer.Character, part.Parent}
    local result = workspace:Raycast(origin, direction, rayParams)
    return result == nil
end

local function GetClosestTarget()
    local target = nil
    local shortestDist = _G.AimbotSettings.AimbotRange
    
    for _, p in pairs(Players:GetPlayers()) do
        if p ~= LocalPlayer and p.Character and p.Character:FindFirstChild(_G.AimbotSettings.TargetPart) and p.Character:FindFirstChild("Humanoid") and p.Character.Humanoid.Health > 0 then
            local part = p.Character[_G.AimbotSettings.TargetPart]
            local pos, onScreen = Camera:WorldToViewportPoint(part.Position)
            
            if onScreen then
                if IsVisible(part) then
                    local mousePos = UserInputService:GetMouseLocation()
                    local dist = (Vector2.new(pos.X, pos.Y) - mousePos).Magnitude
                    if dist < shortestDist then
                        target = part
                        shortestDist = dist
                    end
                end
            end
        end
    end
    return target
end

-- ESP Lines
local espLines = {}
local function ClearESP()
    for _, line in pairs(espLines) do line:Remove() end
    espLines = {}
end

-- Loops
RunService.RenderStepped:Connect(function()
    if not _G.AimbotSettings.LoggedIn then return end
    
    if _G.AimbotSettings.AimbotEnabled then
        local target = GetClosestTarget()
        if target then
            Camera.CFrame = CFrame.new(Camera.CFrame.Position, target.Position)
        end
    end
    
    if _G.AimbotSettings.SpinBotEnabled and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
        LocalPlayer.Character.HumanoidRootPart.CFrame *= CFrame.Angles(0, math.rad(_G.AimbotSettings.SpinSpeed), 0)
    end
    
    ClearESP()
    if _G.AimbotSettings.ESPEnabled then
        for _, p in pairs(Players:GetPlayers()) do
            if p ~= LocalPlayer and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                local hrp = p.Character.HumanoidRootPart
                local pos, onScreen = Camera:WorldToViewportPoint(hrp.Position)
                if onScreen then
                    local line = Drawing.new("Line")
                    line.Visible = true
                    line.From = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y)
                    line.To = Vector2.new(pos.X, pos.Y)
                    line.Color = Color3.fromRGB(255, 0, 0)
                    line.Thickness = 1
                    table.insert(espLines, line)
                end
            end
        end
    end
end)

RunService.Stepped:Connect(function()
    if _G.AimbotSettings.NoclipEnabled and LocalPlayer.Character then
        for _, part in pairs(LocalPlayer.Character:GetDescendants()) do
            if part:IsA("BasePart") then part.CanCollide = false end
        end
    end
end)

print("Aimbot Teste V7 - WALL TRANSPARENCY CARREGADO!")
