local Players, RunService, CoreGui = game:GetService("Players"), game:GetService("RunService"), game:GetService("CoreGui")
local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera

if CoreGui:FindFirstChild("DeltaMenuVip_V2") then CoreGui.DeltaMenuVip_V2:Destroy() end

getgenv().Aimbot = false; getgenv().ESPBox = false; getgenv().ESPDot = false; getgenv().ESPLine = false
getgenv().Velocidade = false; getgenv().Pulo = false

local ScreenGui = Instance.new("ScreenGui", CoreGui); ScreenGui.Name = "DeltaMenuVip_V2"
local OpenBtn = Instance.new("TextButton", ScreenGui); OpenBtn.Size = UDim2.new(0, 50, 0, 50); OpenBtn.Position = UDim2.new(0, 20, 0.5, -25); OpenBtn.Text = "UI"; OpenBtn.BackgroundColor3 = Color3.fromRGB(30,30,30); Instance.new("UICorner", OpenBtn)

local Main = Instance.new("Frame", ScreenGui); Main.Size = UDim2.new(0, 320, 0, 280); Main.Position = UDim2.new(0.5, -160, 0.5, -140); Main.BackgroundColor3 = Color3.fromRGB(20, 20, 20); Main.Visible = false; Instance.new("UICorner", Main)
local Stroke = Instance.new("UIStroke", Main); Stroke.Color = Color3.fromRGB(255, 255, 0); Stroke.Thickness = 2
OpenBtn.MouseButton1Click:Connect(function() Main.Visible = not Main.Visible end)

local Sidebar = Instance.new("Frame", Main); Sidebar.Size = UDim2.new(0.3, 0, 1, 0); Sidebar.BackgroundColor3 = Color3.fromRGB(25, 25, 25); Instance.new("UICorner", Sidebar).CornerRadius = UDim.new(0, 8)
local Content = Instance.new("Frame", Main); Content.Size = UDim2.new(0.7, 0, 1, 0); Content.Position = UDim2.new(0.3, 0, 0, 0); Content.BackgroundTransparency = 1
Instance.new("UIListLayout", Sidebar).Padding = UDim.new(0, 2)

local function criarPagina()
    local scroll = Instance.new("ScrollingFrame", Content); scroll.Size = UDim2.new(1, 0, 1, 0); scroll.BackgroundTransparency = 1; scroll.CanvasSize = UDim2.new(0, 0, 1.5, 0); scroll.ScrollBarThickness = 0; scroll.Visible = false
    Instance.new("UIListLayout", scroll).Padding = UDim.new(0, 5); Instance.new("UIPadding", scroll).PaddingTop = UDim.new(0, 10); Instance.new("UIPadding", scroll).PaddingLeft = UDim.new(0, 5)
    return scroll
end

local MenuPage = criarPagina(); MenuPage.Visible = true
local VisualPage = criarPagina()
local PlayerPage = criarPagina()
local MiscPage = criarPagina()

local function criarAba(nome, target)
    local b = Instance.new("TextButton", Sidebar); b.Size = UDim2.new(1, 0, 0, 45); b.Text = nome; b.BackgroundColor3 = Color3.fromRGB(35,35,35); b.TextColor3 = Color3.new(1,1,1); b.Font = Enum.Font.Code; b.TextSize = 12; b.BorderSizePixel = 0
    b.MouseButton1Click:Connect(function()
        for _, p in pairs(Content:GetChildren()) do if p:IsA("ScrollingFrame") then p.Visible = false end end
        target.Visible = true
    end)
end

criarAba("MENU", MenuPage); criarAba("VISUAL", VisualPage); criarAba("PLAYER", PlayerPage); criarAba("MISC", MiscPage)

local function btn(parent, nome, cfg)
    local b = Instance.new("TextButton", parent); b.Size = UDim2.new(0.9, 0, 0, 40); b.Text = nome; b.BackgroundColor3 = Color3.fromRGB(50,50,50); b.TextColor3 = Color3.new(1,1,1); b.Font = Enum.Font.Code; b.TextSize = 14; Instance.new("UICorner", b)
    b.MouseButton1Click:Connect(function() getgenv()[cfg] = not getgenv()[cfg]; b.BackgroundColor3 = getgenv()[cfg] and Color3.fromRGB(255, 255, 0) or Color3.fromRGB(50,50,50); b.TextColor3 = getgenv()[cfg] and Color3.new(0,0,0) or Color3.new(1,1,1) end)
end

btn(MenuPage, "Aimbot", "Aimbot"); btn(VisualPage, "ESP Box", "ESPBox"); btn(VisualPage, "ESP Ponto", "ESPDot"); btn(VisualPage, "ESP Linha", "ESPLine"); btn(PlayerPage, "Velocidade", "Velocidade"); btn(PlayerPage, "Pulo Alto", "Pulo")

local info = Instance.new("TextLabel", MiscPage); info.Size = UDim2.new(0.9, 0, 0, 80); info.Text = "Criado por: Ziggy\nData: 22/06/2026"; info.TextColor3 = Color3.new(1,1,1); info.BackgroundTransparency = 1; info.Font = Enum.Font.Code; info.TextSize = 12

-- Lógica Robusta
local espLines = {}
RunService.RenderStepped:Connect(function()
    local char = LocalPlayer.Character
    if char and char:FindFirstChild("Humanoid") then
        if getgenv().Velocidade then char.HumanoidRootPart.CFrame += (char.Humanoid.MoveDirection * 0.3) end
        char.Humanoid.JumpPower = getgenv().Pulo and 100 or 50
    end
    for _, p in pairs(Players:GetPlayers()) do
        if p ~= LocalPlayer and p.Character then
            if getgenv().ESPBox then
                if not p.Character:FindFirstChild("BoxESP") then local hl = Instance.new("Highlight", p.Character); hl.Name = "BoxESP"; hl.FillColor = Color3.fromRGB(255, 255, 0); hl.FillTransparency = 0.5 end
            elseif p.Character:FindFirstChild("BoxESP") then p.Character.BoxESP:Destroy() end
        end
    end
end)
