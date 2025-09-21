--==================== HUB 1 - Amendoim-Hub ====================--
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local player = Players.LocalPlayer
local Camera = workspace.CurrentCamera

-- ScreenGui 1
local ScreenGui1 = Instance.new("ScreenGui", game.CoreGui)
ScreenGui1.Name = "AmendoimHub"
ScreenGui1.ResetOnSpawn = false

-- Main Frame 1
local MainFrame1 = Instance.new("Frame", ScreenGui1)
MainFrame1.Size = UDim2.new(0, 320, 0, 420)
MainFrame1.Position = UDim2.new(0.05, 0, 0.2, 0)
MainFrame1.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
MainFrame1.BorderSizePixel = 0
MainFrame1.Active = true
MainFrame1.Draggable = true

local UICorner1 = Instance.new("UICorner", MainFrame1)
UICorner1.CornerRadius = UDim.new(0, 12)

-- Header 1
local Header1 = Instance.new("Frame", MainFrame1)
Header1.Size = UDim2.new(1, 0, 0, 45)
Header1.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
local HeaderCorner1 = Instance.new("UICorner", Header1)
HeaderCorner1.CornerRadius = UDim.new(0, 12)

local Title1 = Instance.new("TextLabel", Header1)
Title1.Size = UDim2.new(1, -50, 1, 0)
Title1.Position = UDim2.new(0, 15, 0, 0)
Title1.BackgroundTransparency = 1
Title1.Text = "⚡Amendoim-Hub⚡"
Title1.TextColor3 = Color3.fromRGB(255, 220, 0)
Title1.Font = Enum.Font.GothamBold
Title1.TextSize = 22
Title1.TextXAlignment = Enum.TextXAlignment.Left

-- Botão minimizar 1
local ToggleButton1 = Instance.new("TextButton", Header1)
ToggleButton1.Size = UDim2.new(0, 45, 1, 0)
ToggleButton1.Position = UDim2.new(1, -45, 0, 0)
ToggleButton1.Text = "−"
ToggleButton1.BackgroundTransparency = 1
ToggleButton1.TextColor3 = Color3.fromRGB(255, 220, 0)
ToggleButton1.Font = Enum.Font.GothamBold
ToggleButton1.TextSize = 24

local MinimizedFrame1 = Instance.new("Frame", ScreenGui1)
MinimizedFrame1.Size = UDim2.new(0, 160, 0, 40)
MinimizedFrame1.Position = MainFrame1.Position
MinimizedFrame1.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
MinimizedFrame1.BorderSizePixel = 0
MinimizedFrame1.Active = true
MinimizedFrame1.Draggable = true
MinimizedFrame1.Visible = false

local MiniCorner1 = Instance.new("UICorner", MinimizedFrame1)
MiniCorner1.CornerRadius = UDim.new(0, 8)

local RestoreButton1 = Instance.new("TextButton", MinimizedFrame1)
RestoreButton1.Size = UDim2.new(1,0,1,0)
RestoreButton1.Text = "📂 Abrir Hub"
RestoreButton1.BackgroundTransparency = 1
RestoreButton1.TextColor3 = Color3.fromRGB(255,220,0)
RestoreButton1.Font = Enum.Font.GothamBold
RestoreButton1.TextSize = 18

ToggleButton1.MouseButton1Click:Connect(function()
	MainFrame1.Visible = false
	MinimizedFrame1.Position = MainFrame1.Position
	MinimizedFrame1.Visible = true
end)
RestoreButton1.MouseButton1Click:Connect(function()
	MainFrame1.Position = MinimizedFrame1.Position
	MainFrame1.Visible = true
	MinimizedFrame1.Visible = false
end)

-- Botões 1
local ButtonHolder1 = Instance.new("ScrollingFrame", MainFrame1)
ButtonHolder1.Size = UDim2.new(1,-20,1,-60)
ButtonHolder1.Position = UDim2.new(0,10,0,55)
ButtonHolder1.BackgroundTransparency = 1
ButtonHolder1.BorderSizePixel = 0
ButtonHolder1.ScrollBarThickness = 6
ButtonHolder1.CanvasSize = UDim2.new(0,0,0,0)

local UIListLayout1 = Instance.new("UIListLayout", ButtonHolder1)
UIListLayout1.Padding = UDim.new(0,10)
UIListLayout1.SortOrder = Enum.SortOrder.LayoutOrder

UIListLayout1:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
	ButtonHolder1.CanvasSize = UDim2.new(0,0,0,UIListLayout1.AbsoluteContentSize.Y + 10)
end)

local function criarBotao1(nome, ativarFunc, desativarFunc)
	local btn = Instance.new("TextButton", ButtonHolder1)
	btn.Size = UDim2.new(1,0,0,45)
	btn.BackgroundColor3 = Color3.fromRGB(30,30,30)
	btn.TextColor3 = Color3.fromRGB(255,220,0)
	btn.Font = Enum.Font.GothamBold
	btn.TextSize = 18
	btn.Text = nome
	local btnCorner = Instance.new("UICorner", btn)
	btnCorner.CornerRadius = UDim.new(0,8)
	local ativo=false
	btn.MouseButton1Click:Connect(function()
		ativo = not ativo
		if ativo then
			btn.BackgroundColor3 = Color3.fromRGB(50,50,50)
			ativarFunc()
		else
			btn.BackgroundColor3 = Color3.fromRGB(30,30,30)
			if desativarFunc then desativarFunc() end
		end
	end)
end

-- Fly
criarBotao1("✈ Fly", function()
	loadstring(game:HttpGet("https://gist.githubusercontent.com/meozoneYT/bf037dff9f0a70017304ddd67fdcd370/raw/e14e74f425b060df523343cf30b787074eb3c5d2/arceus%2520x%2520fly%25202%2520obflucator",true))()
end)

-- ESP
local ESPEnabled = false
local tracers, names, connections = {}, {}, {}
local cores = {police=Color3.fromRGB(0,0,255), criminals=Color3.fromRGB(255,0,0), prisoners=Color3.fromRGB(255,140,0), heroes=Color3.fromRGB(255,255,0), villains=Color3.fromRGB(128,0,128)}
local function removerESP()
	for _,d in pairs(tracers) do if d then d:Remove() end end
	for _,d in pairs(names) do if d then d:Remove() end end
	tracers, names = {}, {}
	for _,c in pairs(connections) do c:Disconnect() end
	connections = {}
end
local function criarESP(p)
	if p==player then return end
	local line,text = Drawing.new("Line"),Drawing.new("Text")
	line.Thickness,line.Transparency,line.Visible=2,1,true
	text.Size,text.Center,text.Outline,text.Font,text.Visible=15,true,true,2,true
	tracers[p],names[p]=line,text
	table.insert(connections, RunService.RenderStepped:Connect(function()
		if not ESPEnabled then return end
		if p.Character and p.Character:FindFirstChild("HumanoidRootPart") and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
			local myPos,targetPos=player.Character.HumanoidRootPart.Position,p.Character.HumanoidRootPart.Position
			local screen1,on1=Camera:WorldToViewportPoint(myPos)
			local screen2,on2=Camera:WorldToViewportPoint(targetPos)
			if on1 and on2 then
				line.From,line.To=Vector2.new(screen1.X,screen1.Y),Vector2.new(screen2.X,screen2.Y)
				local team=p.Team and p.Team.Name:lower() or ""
				line.Color,line.Visible=cores[team] or Color3.new(1,1,1),true
				local head=p.Character:FindFirstChild("Head")
				if head then
					local hpos,hon=Camera:WorldToViewportPoint(head.Position+Vector3.new(0,1.5,0))
					if hon then text.Position,text.Text=Vector2.new(hpos.X,hpos.Y),p.Name; text.Color,text.Visible=cores[team] or Color3.new(1,1,1),true
					else text.Visible=false end
				else text.Visible=false end
			else line.Visible,text.Visible=false,false end
		end
	end))
end
criarBotao1("👁 ESP", function()
	ESPEnabled = true
	for _,p in ipairs(Players:GetPlayers()) do criarESP(p) end
	table.insert(connections, Players.PlayerAdded:Connect(function(p)
		p.CharacterAdded:Connect(function() if ESPEnabled then criarESP(p) end end)
	end))
end, function()
	ESPEnabled=false
	removerESP()
end)

-- Aimbot
local AimPart, AimFov, AimSmoothness = "Head",120,0.15
local aiming, renderConnection = false,nil
local fovCircle = Drawing.new("Circle")
fovCircle.Radius,fovCircle.Thickness,fovCircle.Transparency=AimFov,2,1
fovCircle.Color,fovCircle.Filled,fovCircle.Visible=Color3.fromRGB(255,0,0),false,false
local function getClosest()
	local closest,dist=nil,AimFov
	for _,p in pairs(Players:GetPlayers()) do
		if p~=player and p.Character and p.Character:FindFirstChild(AimPart) and p.Character:FindFirstChild("Humanoid") and p.Character.Humanoid.Health>0 then
			local part=p.Character[AimPart]
			local pos,on=Camera:WorldToViewportPoint(part.Position)
			if on then
				local sPos,center=Vector2.new(pos.X,pos.Y),Vector2.new(Camera.ViewportSize.X/2,Camera.ViewportSize.Y/2)
				local mag=(sPos-center).Magnitude
				if mag<dist then dist,closest=mag,part end
			end
		end
	end
	return closest
end
local function ativarAimbot()
	aiming,fovCircle.Visible=true,true
	renderConnection=RunService.RenderStepped:Connect(function()
		fovCircle.Position=Vector2.new(Camera.ViewportSize.X/2,Camera.ViewportSize.Y/2)
		if aiming then
			local target=getClosest()
			if target then
				local camPos,newC=Camera.CFrame.Position,CFrame.new(Camera.CFrame.Position,target.Position)
				Camera.CFrame=Camera.CFrame:Lerp(newC,AimSmoothness)
			end
		end
	end)
end
local function desativarAimbot()
	aiming,fovCircle.Visible=false,false
	if renderConnection then renderConnection:Disconnect() renderConnection=nil end
end
criarBotao1("🎯 Aimbot",ativarAimbot,desativarAimbot)

--==================== HUB 2 - Gatuno Hub ====================--
local TweenService = game:GetService("TweenService")

local ScreenGui2 = Instance.new("ScreenGui", game.CoreGui)
ScreenGui2.Name = "GatunoHub"
ScreenGui2.ResetOnSpawn = false

local MainFrame2 = Instance.new("Frame", ScreenGui2)
MainFrame2.Size=UDim2.new(0,300,0,400)
MainFrame2.Position=UDim2.new(0.45,0,0.2,0)
MainFrame2.BackgroundColor3=Color3.fromRGB(10,10,10)
MainFrame2.BorderSizePixel=0
MainFrame2.Active=true
MainFrame2.Draggable=true

local UICorner2=Instance.new("UICorner",MainFrame2)
UICorner2.CornerRadius=UDim.new(0,12)

local Header2=Instance.new("Frame",MainFrame2)
Header2.Size=UDim2.new(1,0,0,45)
Header2.BackgroundColor3=Color3.fromRGB(20,20,20)
local HeaderCorner2=Instance.new("UICorner",Header2)
HeaderCorner2.CornerRadius=UDim.new(0,12)

local Title2=Instance.new("TextLabel",Header2)
Title2.Size=UDim2.new(1,-50,1,0)
Title2.Position=UDim2.new(0,15,0,0)
Title2.BackgroundTransparency=1
Title2.Text="🌟 Gatuno Hub"
Title2.TextColor3=Color3.fromRGB(255,220,0)
Title2.Font=Enum.Font.GothamBold
Title2.TextSize=20
Title2.TextXAlignment=Enum.TextXAlignment.Left

local ToggleButton2=Instance.new("TextButton",Header2)
ToggleButton2.Size=UDim2.new(0,45,1,0)
ToggleButton2.Position=UDim2.new(1,-45,0,0)
ToggleButton2.Text="−"
ToggleButton2.BackgroundTransparency=1
ToggleButton2.TextColor3=Color3.fromRGB(255,220,0)
ToggleButton2.Font=Enum.Font.GothamBold
ToggleButton2.TextSize=22

local MinimizedFrame2=Instance.new("Frame",ScreenGui2)
MinimizedFrame2.Size=UDim2.new(0,150,0,40)
MinimizedFrame2.Position=MainFrame2.Position
MinimizedFrame2.BackgroundColor3=Color3.fromRGB(15,15,15)
MinimizedFrame2.BorderSizePixel=0
MinimizedFrame2.Active=true
MinimizedFrame2.Draggable=true
MinimizedFrame2.Visible=false

local MiniCorner2=Instance.new("UICorner",MinimizedFrame2)
MiniCorner2.CornerRadius=UDim.new(0,8)

local RestoreButton2=Instance.new("TextButton",MinimizedFrame2)
RestoreButton2.Size=UDim2.new(1,0,1,0)
RestoreButton2.Text="📂 Abrir Hub"
RestoreButton2.BackgroundTransparency=1
RestoreButton2.TextColor3=Color3.fromRGB(255,220,0)
RestoreButton2.Font=Enum.Font.GothamBold
RestoreButton2.TextSize=16

ToggleButton2.MouseButton1Click:Connect(function()
	MainFrame2.Visible=false
	MinimizedFrame2.Position=MainFrame2.Position
	MinimizedFrame2.Visible=true
end)
RestoreButton2.MouseButton1Click:Connect(function()
	MainFrame2.Position=MinimizedFrame2.Position
	MainFrame2.Visible=true
	MinimizedFrame2.Visible=false
end)

-- Botões 2
local ButtonHolder2=Instance.new("ScrollingFrame",MainFrame2)
ButtonHolder2.Size=UDim2.new(1,-20,1,-60)
ButtonHolder2.Position=UDim2.new(0,10,0,55)
ButtonHolder2.BackgroundTransparency=1
ButtonHolder2.BorderSizePixel=0
ButtonHolder2.ScrollBarThickness=6
ButtonHolder2.CanvasSize=UDim2.new(0,0,0,0)

local UIListLayout2=Instance.new("UIListLayout",ButtonHolder2)
UIListLayout2.Padding=UDim.new(0,10)
UIListLayout2.SortOrder=Enum.SortOrder.LayoutOrder

UIListLayout2:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
	ButtonHolder2.CanvasSize=UDim2.new(0,0,0,UIListLayout2.AbsoluteContentSize.Y+10)
end)

local function criarBotao2(nome,destino)
	local btn=Instance.new("TextButton",ButtonHolder2)
	btn.Size=UDim2.new(1,0,0,40)
	btn.BackgroundColor3=Color3.fromRGB(30,30,30)
	btn.TextColor3=Color3.fromRGB(255,220,0)
	btn.Font=Enum.Font.GothamBold
	btn.TextSize=16
	btn.Text=nome
	local btnCorner=Instance.new("UICorner",btn)
	btnCorner.CornerRadius=UDim.new(0,8)
	btn.MouseEnter:Connect(function()
		TweenService:Create(btn,TweenInfo.new(0.2),{BackgroundColor3=Color3.fromRGB(50,50,50)}):Play()
	end)
	btn.MouseLeave:Connect(function()
		TweenService:Create(btn,TweenInfo.new(0.2),{BackgroundColor3=Color3.fromRGB(30,30,30)}):Play()
	end)
	btn.MouseButton1Click:Connect(function()
		local character=player.Character or player.CharacterAdded:Wait()
		local rootPart=character:WaitForChild("HumanoidRootPart")
		local info=TweenInfo.new(3,Enum.EasingStyle.Linear,Enum.EasingDirection.Out)
		local objetivo={Position=destino}
		local tween=TweenService:Create(rootPart,info,objetivo)
		tween:Play()
	end)
end

-- Teleportes
criarBotao2("Balada",Vector3.new(1286.9,25.4,33.3))
criarBotao2("Cassino",Vector3.new(1696.2,37.8,825.7))
criarBotao2("Joalheria",Vector3.new(-80.4,85.8,806.9))
criarBotao2("Aeroporto 2",Vector3.new(-2165.3,29.4,-1405.7))
criarBotao2("Criminal",Vector3.new(2050.6,25.5,385.8))
criarBotao2("Lojinha",Vector3.new(-1620,42.7,685.0))
criarBotao2("Pirâmide",Vector3.new(-1047.0,17.9,-498.9))
criarBotao2("Prisao",Vector3.new(-861.7,53.6,-2809.2))
criarBotao2("Banco",Vector3.new(743.6,25.5,504.0))
