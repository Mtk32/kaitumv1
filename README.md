getgenv().Configs = {
	Equipe = "Marinha",
	FixLag = false,
	FPS = 120,
	WhiteScreen = false,
	Godhuman = verdadeiro,
	Saber = verdadeiro,
	CursedDualKatana = true,
	SkullGuitar = true,
	RaceV3 = verdadeiro
}

local Config = getgenv().Configs

local LogCache = {}
função local LogChange(chave, mensagem)
	se LogCache[chave] ~= mensagem então
		LogCache[chave] = mensagem
		print(mensagem)
	fim
fim

print("aguardando o jogo...")
repita task.wait() até que game:IsLoaded() e game.Players.LocalPlayer
print("jogo carregado")

local TweenService = cloneref(game:GetService("TweenService"))
local ReplicatedStorage = cloneref(game:GetService("ReplicatedStorage"))
local TeleportService = cloneref(game:GetService("TeleportService"))
local Players = cloneref(game:GetService("Players"))
local LocalPlayer = Players.LocalPlayer
local RunService = jogo:GetService("RunService")
local Lighting = game:GetService("Lighting")
local CollectionService = game:GetService("CollectionService")
local HttpService = game:GetService("HttpService")
local CoreGui = cloneref(game:GetService("CoreGui"))
local VirtualInputManager = game:GetService("VirtualInputManager")
local CommF_ = ReplicatedStorage.Remotes.CommF_
CommE local = ReplicatedStorage.Remotes.CommE
local MaxLevel = (workspace:GetAttribute("LEVEL_CAP") or 2800)
CoreGui.RobloxPromptGui.promptOverlay.ChildAdded:Connect(function(v)
	se v.Name == "ErrorPrompt" e v:FindFirstChild("MessageArea") e v.MessageArea:FindFirstChild("ErrorFrame") então
		ReplicatedStorage.__ServerBrowser:InvokeServer("teleport", game.JobId)
	fim
fim)
game.NetworkClient.ChildRemoved:Connect(function()
	ReplicatedStorage.__ServerBrowser:InvokeServer("teleport", game.JobId)
	task.wait(5)
	TeleportService:Teleport(game.PlaceId)
fim)

TeleportService.TeleportInitFailed:Connect(function(player, result, err)
	se result ~= Enum.TeleportResult.GameFull então
		ReplicatedStorage.__ServerBrowser:InvokeServer("teleport", game.JobId)
	fim
fim)

Se Config.WhiteScreen então
	RunService:Set3dRenderingEnabled(false)
	print("tela branca aplicada")
fim
Se Config.FixLag então
	pcall(function()
		para _,v em pares(Lighting:GetChildren()) faça
			Se v:IsA("PostEffect") então
				v.Enabled = false
			fim
		fim
		função local optimize(obj)
			Se obj:IsA("BasePart") então
				obj.Material = Enum.Material.Plastic
				obj.Reflectance = 0
			senão se obj:IsA("Decalque") ou obj:IsA("Textura") então
				obj:Destroy()
			senão se obj:IsA("ParticleEmitter") ou obj:IsA("Trail") então
				obj.Enabled = false
			elseif obj:IsA("Explosão") então
				obj.BlastPressure = 0
				obj.BlastRadius = 0
			fim
		fim
		para _,v em pares(jogo:ObterDescendentes()) faça
			otimizar(v)
		fim
		game.DescendantAdded:Connect(optimize)
		Iluminação.SombrasGlobais = falso
		Lighting.FogEnd = 9e9
		Iluminação.Brilho = 0
		Lighting.EnvironmentDiffuseScale = 0
		Lighting.EnvironmentSpecularScale = 0
		settings().Rendering.QualityLevel = Enum.QualityLevel.Level01
		para _, v em workspace.Map:GetDescendants() faça
			se v:IsA('BasePart') e v.Transparency ~= 1 então
				v.Transparência = 1
			fim
		fim
	fim)
	print("correção de atraso aplicada")
fim

Se Config.FPS então
	setfpscap(Config.FPS)
	print("limite de fps definido para " .. Config.FPS)
fim

print("aguardando a equipe...")

se não for LocalPlayer.Team então
	repita task.wait()
		CommF_:InvokeServer("SetTeam", Configs.Team)
	até LocalPlayer.Team
fim
print("aguardando caractere...")
repita task.wait(0.1) até LocalPlayer.Character e LocalPlayer.Character:FindFirstChildOfClass("Humanoid") e LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
print("caractere pronto")

print("equipe selecionada")

task.wait(2.5)

pcall(function() require(ReplicatedStorage.Util.CameraShaker):Stop() end)

print("agitador da câmera desativado")

print("inicializando...")

Personagem local = JogadorLocal.Personagem ou JogadorLocal.PersonagemAdicionado:Aguarde()
LocalPlayer.CharacterAdded:Connect(function(v)
	Caractere = v
	print("personagem reapareceu")
fim)

pcall(function()
	LocalPlayer.PlayerGui.ChildAdded:Connect(function(child)
		Se child.Name == "SpinnerWindow" então
			filho:Destruir()
		fim
	fim)
	workspace.ChildAdded:Connect(function(df)
		Se df:IsA('Tool') então
			df:Destroy()
		fim
	fim)
fim)

task.spawn(function()
	enquanto verdadeiro faça
		local hrp = Character:FindFirstChild("HumanoidRootPart")
		se hrp e não hrp:FindFirstChild("fixvelo") então
			local bv = Instance.new("BodyVelocity")
			bv.Name = "fixvelo"
			bv.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
			bv.Velocidade = Vector3.new(0, 0, 0)
			bv.P = 1000
			bv.Parent = hrp
		fim
		task.wait(0.5)
	fim
fim)

RunService.Stepped:Connect(function()
	se o caractere for próximo de zero, então
		para _, filho em Character:GetDescendants() faça
			se child:IsA("BasePart") e child.CanCollide == true então
				child.CanCollide = false
			fim
		fim
	fim
fim)

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "KaitunUIKrita"
ScreenGui.ResetOnSpawn = false
ScreenGui.IgnoreGuiInset = true
ScreenGui.Parent = LocalPlayer.PlayerGui
local frame = Instance.new("Frame", ScreenGui)
frame.Size = UDim2.new(0, 560, 0, 130)
frame.Position = UDim2.new(0.5, -280, 0, 60)
frame.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
frame.BackgroundTransparency = 0.05
local corner = Instance.new("UICorner", frame)
canto.RaioDoCanto = UDim.novo(0, 12)
gradiente local = Instance.new("UIGradient", frame)
gradient.Color = ColorSequence.new({
    ColorSequenceKeypoint.new(0, Color3.fromRGB(172, 224, 112)),
    ColorSequenceKeypoint.new(0.5, Color3.fromRGB(80, 199, 164)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(72, 133, 245))
})
gradient.Rotation = 0
local edge = Instance.new("UIStroke", frame)
edge.Color = Color3.fromRGB(196, 196, 196)
edge.Thickness = 3
edge.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
layout local = Instance.new("UIListLayout", frame)
layout.HorizontalAlignment = Enum.HorizontalAlignment.Center
layout.VerticalAlignment = Enum.VerticalAlignment.Center
layout.SortOrder = Enum.SortOrder.LayoutOrder
layout.Padding = UDim.new(0, 6)
título local = Instance.new("TextLabel", frame)
título.Tamanho = UDim2.novo(1, -20, 0, 35)
title.BackgroundTransparency = 1
título.Texto = "krita++ | inicializando"
title.Font = Enum.Font.GothamBold
title.TextSize = 24
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.LayoutOrder = 1
local timeLabel = Instance.new("TextLabel", frame)
timeLabel.Size = UDim2.new(1, -20, 0, 30)
timeLabel.BackgroundTransparency = 1
timeLabel.Text = "00:00:00"
timeLabel.Font = Enum.Font.GothamSemibold
timeLabel.TextSize = 18
timeLabel.TextColor3 = Color3.fromRGB(226, 246, 255)
timeLabel.LayoutOrder = 3
função SetStatus(texto)
    status local = string.lower(tostring(texto))
    Se title.Text ~= status então
        print("ui: " .. status)
    fim
    title.Text = status
fim
task.spawn(function()
    while task.wait(1) do
        local t = os.date("*t")
        timeLabel.Text = string.format("%02d:%02d:%02d | %s, %s %d, %d", t.hour, t.min, t.sec, os.date("%A"), os.date("%B"), t.day, t.year)
    fim
fim)
print("interface do usuário inicializada")

pcall(function()
    assert(getrawmetatable)
    local grm = getrawmetatable(game)
    setreadonly(grm, false)
    local antigo = grm.__namecall
    grm.__namecall = newcclosure(function(self, ...)
        argumentos locais = {...}
        nome local = tostring(args[1])
        para _, remoto em ipairs({"TeleportDetect", "CHECKER_1", "CHECKER", "GUI_CHECK", "OneMoreTime", "checkingSPEED", "BANREMOTE", "PERMAIDBAN", "KICKREMOTE", "BR_KICKPC", "BR_KICKMOBILE"}) faça
            se nome == remoto então
                retornar
            fim
        fim
        return old(self, ...)
    fim)
fim)

print("anti anticheat inicializado")

função local EquipTool(v)
	Character.Humanoid:EquipTool(LocalPlayer.Backpack:FindFirstChild(v))
fim

função local EquipWeapon(tipo)
	se não digitar então
		tipo = "Combate Corpo a Corpo"
	fim
	para _, v em LocalPlayer.Backpack:GetChildren() faça
		se v.ToolTip == tipo então
			se não Character:FindFirstChild("HasBuso") então
				CommF_:InvokeServer("Buso")
			fim
			Personagem.Humanoide:EquipTool(v)
			retornar
		fim
	fim
fim

função local HasItem(nome)
	se não for um personagem então
		retornar
	fim
	retornar Character:FindFirstChild(name) ou LocalPlayer.Backpack:FindFirstChild(name)
fim

função local CheckItem(Nome)
	dados locais = IRS:GetItems(KEYS.QUANTITY)
	para _, v em dados faça
		item local = CORRESPONDÊNCIA(v.ItemId)._ok
		local c = item.Display.Category
		se (c == "Espada" ou c == "Arma" ou c == "Acessório") e item.Index.StorageKey == Nome então
			retornar verdadeiro
		fim
	fim
	retornar falso
fim

local tw, conn
função local StopTween()
	se tw então
        LogChange("tween_stop", "tween: parado")
		tw:Cancelar()
		tw = nulo
	fim
	se conectado então
		conn:Desconectar()
		conn = nil
	fim
	local r = Caractere e Caractere:EncontrarPrimeiroFilho("Raiz")
	se r então
		r:Destroy()
	fim
fim

Mundo local = {}
World.Sea1 = função()
	retornar require(ReplicatedStorage.Util.Realm).getCurrentSeaAsync() == "Sea1"
fim
World.Sea2 = função()
	retornar require(ReplicatedStorage.Util.Realm).getCurrentSeaAsync() == "Sea2"
fim
World.Sea3 = função()
	retornar require(ReplicatedStorage.Util.Realm).getCurrentSeaAsync() == "Sea3"
fim

local LastTravel = 0

função local RunTween(v, alta)
    enquanto não for Character ou não for Character.PrimaryPart ou não for Character:FindFirstChild("Humanoid") faça
        LogChange("tween_wait", "tween: aguardando caractere")
        task.wait()
    fim
    local rootPart = LocalPlayer.Character e LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
    se não for rootPart então
        LogChange("tween_root", "tween: raiz ausente")
        retornar
    fim
    se typeof(v) == "CFrame" então
        v = v.Position
    fim
    Character.Humanoid.Sit = false
    local targetCFrame = typeof(v) == "Vector3" and CFrame.new(v) ​​or v
    local pos = targetCFrame.Position + Vector3.new(0, high or 0, 0)
    local rot = rootPart.CFrame - rootPart.Position
    local finalCFrame = CFrame.new(pos) * rot
    distância local = (rootPart.Position - finalCFrame.Position).Magnitude
    velocidade local
    se a distância for menor que 250 então
        velocidade = 285
    senão se a distância for menor que 500 então
        velocidade = 260
    senão se a distância for menor que 1000 então
        velocidade = 240
    outro
        velocidade = 200
    fim
    Tempo de deslocamento local = distância / velocidade
    se a distância for maior ou igual a 1000 então
        LogChange("long_tween", "tween: viagem longa")
    fim
    tw = TweenService:Create(rootPart, TweenInfo.new(travelTime, Enum.EasingStyle.Linear, Enum.EasingDirection.Out), { CFrame = finalCFrame })
    tw:Play()
    retornar tw
fim
função local BreakClouds()
	Se workspace.Map:FindFirstChild("Sky") então
		para _, v em workspace.Map.Sky:GetChildren() faça
			se v:IsA("MeshPart") e v.Name == "cloud" e (Vector3.new(-4208.15869140625, 1089.8868408203125, -373.052490234375) - v.Position).Magnitude < 50 e não v:GetAttribute("Broken") então
				EquiparArma()
                Character.HumanoidRootPart:PivotTo(v.CFrame)
				task.wait(0.25)
				ReplicatedStorage.Modules.Net["RE/RegisterAttack"]:FireServer(0.4)
				ReplicatedStorage.Modules.Net["RE/RegisterHit"]:FireServer(v)
                task.wait(0.5)
			fim
		fim
	fim
fim
função local SmartTween(t)
    Se World.Sea3() então
        se não CheckItem("Valkyrie Helm") então
            LogChange("entrance_block", "travel: missing valkyrie helm")
            retornar
        fim
    fim
    se typeof(t) == "CFrame" então
        t = t.Position
    fim
    enquanto não for Character ou não for Character.PrimaryPart ou não for Character:FindFirstChild("Humanoid") faça
        task.wait()
    fim
    local_is
    Se World.Sea3() então
        _é = {
			{ Vector3.new(-5027.0302734375, 316.5475769042969, -3206.70361328125), Vector3.new(5650.94775390625, 1017.2747802734375, -350.37918090820312) },
            { Vector3.new(5650.94775390625, 1017.2747802734375, -350.37918090820312), Vector3.new(-5027.0302734375, 316.5475769042969, -3206.70361328125) },
            { Vector3.new(-12463.6025390625, 378.32705688476562, -7566.0830078125), Vector3.new(-5060.41162109375, 316.5475769042969, -3193.224853515625) },
            { Vector3.new(-5060.41162109375, 318.50201416015625, -3193.224853515625), Vector3.new(-12463.6025390625, 376.3725280761719, -7566.0830078125) },
        }
    elseif World.Sea2() então
        _é = {
            { Vector3.new(-390, 332, 673), Vector3.new(-390, 332, 673) },
            { Vector3.new(2285, 15, 905), Vector3.new(2285, 15, 905) },
            { Vector3.new(923, 126, 32852), Vector3.new(923, 126, 32852) },
            { Vector3.new(-6509, 83, -133), Vector3.new(-6509, 83, -133) },
        }
    elseif World.Sea1() então
        _é = {
			{ Vector3.new(61163.8515625, 11.6796875, 1819.7841796875), Vector3.new(4050.31103515625, -1.6880035400390625, -1814.1240234375) },
            { Vector3.new(3864.8515625, 6.6796875, -1926.7841796875), Vector3.new(61170.046875, -2, 1952.833984375) },
            { Vector3.new(-4166.60986328125, 1093.697998046875, -347.16226196289062), Vector3.new(-6020.27734375, 5469.15625, 2209.776123046875) },
        }
    outro
        retornar
    fim

    local min = nulo
    posição local = 1
    para i, v em ipairs(_is) faça
        distância localAoAlvo = (t - v[1]).Magnitude
        se min == nulo então
            min = distânciaAoAlvo
        outro
            se distânciaAoAlvo < mínimo então
                min = distânciaAoAlvo
                pos = i
            fim
        fim
    fim
    local dist = JogadorLocal:DistânciaDoPersonagem(t)
	local disttotp = LocalPlayer:DistanceFromCharacter(_is[pos][2])
    se (min + disttotp) < dist e (tick() - LastTravel) >= 1 então
        LastTravel = tick()
        print("viagem: usando a entrada")
        tw = RunTween(_is[pos][2])
        se tw então
            tw.Completed:Wait()
        fim
		task.wait(0.25)
		print("viagem: solicitando entrada")
		task.wait(0.25)
		se não CollectionService:HasTag(LocalPlayer, "Teleporting") então
			CommF_:InvokeServer("requestEntrance", _is[pos][1])
		fim
		repita task.wait() até que (LocalPlayer:DistanceFromCharacter(_is[pos][1]) < LocalPlayer:DistanceFromCharacter(_is[pos][2]))
        print("viagem: entrada alcançada")
        task.wait(0.25)
    fim
fim
função local Tween(v, velocidade, alta)
    enquanto não for Character ou não for Character.PrimaryPart ou não for Character:FindFirstChild("Humanoid") faça
        task.wait()
    fim
    SmartTween(v)
    retornar RunTween(v, high)
fim

função local WaitForAliveCharacter()
    enquanto verdadeiro faça
        personagem local = JogadorLocal.Personagem
        humanoide local = caractere e caractere:FindFirstChildOfClass("Humanoid")
        raiz local = caractere e caractere:FindFirstChild("HumanoidRootPart")
        se o personagem for humanoide e a saúde do humanoide for maior que 0 e a raiz for então
            retornar caractere, humanoide, raiz
        fim
        task.wait(0.1)
    fim
fim

função local TweenWithRespawnRetry(alvo, velocidade, alta)
    enquanto verdadeiro faça
        local currentCharacter = LocalPlayer.Character
        local currentHumanoid = currentCharacter e currentCharacter:FindFirstChildOfClass("Humanoid")
        local currentRoot = currentCharacter e currentCharacter:FindFirstChild("HumanoidRootPart")
        se não currentCharacter ou não currentHumanoid ou currentHumanoid.Health <= 0 ou não currentRoot então
            LogChange("respawn_wait", "tween: aguardando respawn")
        fim
        local startCharacter, humanoid, root = WaitForAliveCharacter()
        LogChange("respawn_wait", "tween: personagem pronto")
        local morreu = falso
        local activeTween = nulo

        local diedConnection = humanoid.Died:Connect(function()
            morreu = verdadeiro
            se activeTween então
                pcall(function()
                    activeTween:Cancel()
                fim)
            fim
        fim)

        local ok, tweenOrError = pcall(function()
            retornar Tween(alvo, velocidade, alta)
        fim)

        se ok e tweenOrError então
            activeTween = tweenOrError

            local terminado = falso
            estadoDeReproduçãoLocal = nulo
            local completedConnection = activeTween.Completed:Connect(function(state)
                playbackState = estado
                concluído = verdadeiro
            fim)

            local currentState = activeTween.PlaybackState
            se currentState == Enum.PlaybackState.Completed ou currentState == Enum.PlaybackState.Cancelled então
                playbackState = currentState
                concluído = verdadeiro
            fim

            se morreu então
                pcall(function()
                    activeTween:Cancel()
                fim)
            fim

            enquanto não estiver terminado
                task.wait(0.05)
            fim

            completedConnection:Disconnect()
            diedConnection:Disconnect()

            local currentCharacter = LocalPlayer.Character
            local stillSameCharacter = currentCharacter == startCharacter
                e humanoide.Parent ~= nulo
                e humanoide.Saúde > 0
                e raiz.Parent ~= nulo
                e não morreu

            se não for ainda o mesmo caractere então
                print("tween: tentar novamente após o respawn")
                task.wait(0.1)
            senão se playbackState == Enum.PlaybackState.Completed então
                LogChange("tween_result", "tween: completo")
                retornar verdadeiro
            outro
                LogChange("tween_result", "tween: cancelado")
                retornar falso
            fim
        outro
            diedConnection:Disconnect()
            local characterChanged = LocalPlayer.Character ~= startCharacter
            se morreu ou characterChanged ou humanoid.Health <= 0 ou não é root.Parent então
                task.wait(0.1)
            outro
                se não estiver tudo bem então
                    aviso("tentativa de tween falhou: " .. tostring(tweenOrError))
                fim
                retornar falso
            fim
        fim
    fim
fim

task.spawn(function()
	enquanto verdadeiro faça
		pcall(function()
			char local = LocalPlayer.Character
			se char e char.PrimaryPart e char:FindFirstChildOfClass("Humanoid") e char.Humanoid.Health > 0 então
				CommE:FireServer("Dodge", nil, 30, true, workspace:GetServerTimeNow())
			fim
		fim)
		task.wait(2.5)
	fim
fim)

task.spawn(function()
	enquanto task.wait(0.5) faça
		para _, v em LocalPlayer.PlayerGui:GetDescendants() faça
			se v:IsA("TextLabel") e v.Text:find("Confrontar o prisioneiro") então
				local BonusMomentsRemoteFunction = ReplicatedStorage.Remotes.BonusMomentsRemoteFunction
				BonusMomentsRemoteFunction:InvokeServer("Escape from Alcatraz", "Provoke", "Puncher")
				BonusMomentsRemoteFunction:InvokeServer("Escape from Alcatraz", "Provoke", "Digger")
				BonusMomentsRemoteFunction:InvokeServer("Escape from Alcatraz", "Provoke", "Raft")
				sucesso local, controllerModule = pcall(require, ReplicatedStorage.Controllers.BonusMomentsController)
				se sucesso e type(controllerModule) == "table" e rawget(controllerModule, "GetLoadedMoments") então
					local loadedMoments = controllerModule:GetLoadedMoments()
					para momentName, momentInstance em pares(loadedMoments) faça
						se type(momentInstance) == "table" então
							se type(momentInstance.Trove e momentInstance.Trove.Clean) == "function" então
								pcall(function() momentInstance.Trove:Clean() end)
							fim
							se type(momentInstance._MainMaid e momentInstance._MainMaid.DoCleaning) == "function" então
								pcall(function() momentInstance._MainMaid:DoCleaning() end)
							fim
							se type(momentInstance._Maid e momentInstance._Maid.DoCleaning) == "function" então
								pcall(function() momentInstance._Maid:DoCleaning() end)
							fim
						fim
						loadedMoments[momentName] = nil
					fim
				fim
			fim
		fim
	fim
fim)

função local TeleportSeabeast(c5)
	local a = Vector3.new(0, c5:FindFirstChild("HumanoidRootPart").Position.Y, 0)
	local ab = Vector3.new(0, workspace.Map["WaterBase-Plane"].Position.Y, 0)
	se (a - ab).Magnitude <= 175 então
		Tween(c5.HumanoidRootPart.CFrame * CFrame.new(0, 300, 50), 280).Completed:Wait()
	outro
		Tween(CFrame.new(c5.HumanoidRootPart.Position.X,workspace.Map["WaterBase-Plane"].Position.Y + 200,c5.HumanoidRootPart.Position.Z),280).Completed:Wait()
	fim
fim

função local CheckPirateBoat()
	para _, v em workspace.Enemies:GetChildren() faça
		se table.find({"PirateBasic","PirateBrigade"}, v.Name) e v:FindFirstChild("Health") e v.Health.Value > 0 então
			retornar v
		fim
	fim
fim

função local CheckSeabeast()
	se não workspace:FindFirstChild("SeaBeasts") então
		retornar
	fim
	para _, v em workspace.SeaBeasts:GetChildren() faça
		se v.Name == "SeaBeast1" então
			se v.Health.Value >= 34500 então
				retornar v
			fim
		fim
	fim
	retornar falso
fim

função local CheckBoat()
	para _, v em workspace.Boats:GetChildren() faça
		Se v:IsA("Modelo") então
			se v:FindFirstChild("Owner") e tostring(v.Owner.Value) == LocalPlayer.Name e v.Humanoid.Value > 0 e v:FindFirstChild("Engine") então
				se LocalPlayer:DistanceFromCharacter(v.Engine.CFrame.Position) <= 1750 então
					retornar v
				fim
			fim
		fim
	fim
	retornar falso
fim

função local GetElite()
	para _, v em {"Diablo", "Deandre", "Urban", "Tyrant of the Skies"} faça
		local e = ReplicatedStorage:FindFirstChild(v) ou workspace.Enemies:FindFirstChild(v)
		se e e:FindFirstChild("Humanoid") então
			se e.Humanoid.Health > 0 então
				retornar e
			fim
		fim
	fim
	retornar falso
fim

local r, id
para _, v em {ReplicatedStorage.Util, ReplicatedStorage.Common, ReplicatedStorage.Remotes, ReplicatedStorage.Assets, ReplicatedStorage.FX} faça
	para _, n em v:GetChildren() faça
		se n:IsA("RemoteEvent") e n:GetAttribute("Id") então
			r, id = n, n:GetAttribute("Id")
		fim
	fim
	v.ChildAdded:Connect(function(n)
		se n:IsA("RemoteEvent") e n:GetAttribute("Id") então
			r, id = n, n:GetAttribute("Id")
		fim
	fim)
fim

função local Attack()
	caractere local = Caractere
	partes locais = {}
	para _, v em workspace.Enemies:GetChildren() faça
		local hrp = v:FindFirstChild("HumanoidRootPart")
		local hum = v:FindFirstChild("Humanoid")
		se v ~= char e hrp e hum e hum.Saúde > 0 e LocalPlayer:DistânciaDoPersonagem(hrp.Posição) <= 35 então
			para _, _v em v:GetChildren() faça
				se _v:IsA("BasePart") e LocalPlayer:DistanceFromCharacter(hrp.Position) <= 35 então
					partes[#partes + 1] = { v, _v }
				fim
			fim
		fim
	fim
	se #partes == 0 então
		retornar
	fim
	ferramenta local = char:FindFirstChildOfClass("Tool")
	se #partes > 0 e ferramenta e (ferramenta.ToolTip == "Corpo a corpo" ou ferramenta.ToolTip == "Espada") então
		se (tool.Name == "Black Leg" e tool.Level.Value >= 150) ou (tool.Name == "Death Step" e tool.Level.Value >= 400) então
			Se LocalPlayer.PlayerGui.Main.Skills:FindFirstChild(tool.Name) e LocalPlayer.PlayerGui.Main.Skills[tool.Name].V.Cooldown.AbsoluteSize.X <= 0 então
				VirtualInputManager:SendKeyEvent(true, "V", false, game)
				VirtualInputManager:SendKeyEvent(false, "V", false, game)
				task.wait(1.5)
			fim
		fim
		ReplicatedStorage.Modules.Net["RE/RegisterAttack"]:FireServer()
		local head = parts[1][1]:FindFirstChild("Head")
		ReplicatedStorage.Modules.Net["RE/RegisterHit"]:FireServer( head, parts, {}, tostring(LocalPlayer.UserId):sub(2, 4) .. tostring(coroutine.running()):sub(11, 15) )
		cloneref(r):FireServer(
			string.gsub("RE/RegisterHit", ".", function(c)
				retornar string.char(bit32.bxor(string.byte(c), math.floor(workspace:GetServerTimeNow() / 10 % 10) + 1))
			fim),
			bit32.bxor(id + 909090, ReplicatedStorage.Modules.Net.seed:InvokeServer() * 2),
			cabeça,
			partes
		)
	fim
fim

função local GetNearestChests()
	local nearest, minDist = nulo, 99999
	para _, v em workspace:GetDescendants() faça
		se v.Name == "Chest1" ou v.Name == "Chest2" ou v.Name == "Chest3" então
			se v:IsA("Part") e v.CanTouch então
				local dist = LocalPlayer:DistanceFromCharacter(v:GetPivot().Position)
				se dist < minDist então
					minDist = dist
					mais próximo = v
				fim
			fim
		fim
	fim
	retornar mais próximo
fim

função local IfTableHaveIndex(k)
	para _ em k faça
		retornar verdadeiro
	fim
fim
LastServersDataPulled = 0
ServidoresEmCache = {}
função local GetServers()
	se LastServersDataPulled então
		se os.time() - LastServersDataPulled < 60 então
			retornar ServidoresEmCache
		fim
	fim
	para i = 1, 100, 1 faça
		local W = ReplicatedStorage:WaitForChild("__ServerBrowser"):InvokeServer(i)
		Se IfTableHaveIndex(W) então
			LastServersDataPulled = os.time()
			Servidores em cache = W
			retornar W
		fim
	fim
fim

função local HopLowServer()
	para i, v em GetServers() faça
		se v.Count <= 5 então
			return ReplicatedStorage.__ServerBrowser:InvokeServer("teleport", i)
		fim
	fim
fim

função local CheckSafezone(p)
	se não for um personagem então
		retornar
	fim
	se não Character:FindFirstChild("Humanoid") então
		retornar
	fim
	Se Character.Humanoid.Health > 0 então
		para _, v em workspace["_WorldOrigin"].SafeZones:GetChildren() faça
			Se v:IsA("Parte") então
				se p e p:FindFirstChild("PrimaryPart") e (v.Position - p.PrimaryPart.Position).magnitude <= 200 e p.Humanoid.Health / p.Humanoid.MaxHealth >= 90 / 100 então
					retornar verdadeiro
				fim
			fim
		fim
	fim
	retornar falso
fim

Jogadores na lista negra = {
	LocalPlayer.Name,
}
Hop local

função local GetSkyRacePlayer()
	para _, Jogador em Jogadores:ObterJogadores() faça
		Se Player e Player.Character então
			local Humanoid = Player.Character:FindFirstChild("Humanoid")
			se não table.find(BlacklistedPlayers, Player.Name) e Humanoid e Humanoid.Health > 0 então
				Hop()
				Se Player.Data.Race.Value == "Skypiea" e não Player:GetAttribute("IslandRaiding") então
					se não CheckSafezone(Player) e LocalPlayer:DistanceFromCharacter(Player.Character.PrimaryPart.Position) < 12000 e Player.Character.PrimaryPart.CFrame.Y > 0 então
						table.insert(BlacklistedPlayers, Player.Name)
						retornar Jogador
					fim
				fim
			fim
		fim
	fim
fim

função local GetPlayerBoat()
	para _, barco em workspace.Boats:GetChildren() faça
		Se barco:ÉUm("Modelo") então
			Proprietário local = barco:EncontrarPrimeiroFilho("Proprietário")
			local HD = barco:FindFirstChild("Humanoid")
			Se Owner e HD e tostring(Owner.Value) == LocalPlayer.Name e HD.Value > 0 então
				barco de retorno
			fim
		fim
	fim
fim

função local SendKey(tecla, segurar)
	VirtualInputManager:SendKeyEvent(true, key, false, game)
	task.wait(hold)
	VirtualInputManager:SendKeyEvent(false, key, false, game)
fim

função local GetSeabeast()
	para _, sb em workspace.SeaBeasts:GetChildren() faça
		se sb.Health.Value > 30000 então
			retornar sb
		fim
	fim
fim

task.spawn(function()
	enquanto verdadeiro faça
		GetServers()
		task.wait(180)
	fim
fim)

Hop = função(k, W)
	local check_isvalid = false
	local h = GetServers()
	local X = {}
	para w, D em h faça
		tabela.insert(X, { JobId = w, Players = D.Count, LastUpdate = D.__LastUpdate, Region = D.Region, })
	fim
	para _ = 1, #X faça
		while task.wait() do
			local h = math.random(1, #X)
			ServerData = X[h]
			Se ServerData então
				se não k ou ServerData.Players < k então
					se não for W ou ServerData.Region == W então
						quebrar
					outro
						check_isvalid = true
					fim
				fim
			fim
		fim
	fim
	se não check_isvalid então
		ReplicatedStorage:WaitForChild("__ServerBrowser"):InvokeServer("teleport", ServerData.JobId)
	outro
		servidores locais = {}
		local ok, req = pcall(function()
			retornar jogo:HttpGet( "https://games.roblox.com/v1/games/" .. game.PlaceId .. "/servers/Public?sortOrder=Asc&limit=100&excludeFullGames=true" )
		fim)
		Se não estiver tudo bem ou não for necessário, então
			retornar
		fim
		local ok2, corpo = pcall(função()
			return HttpService:JSONDecode(req)
		fim)
		se não for ok2 ou não for body ou não for body.data então
			retornar
		fim
		para _, v em body.data faça
			se type(v) == "table" e tonumber(v.playing) e tonumber(v.maxPlayers) e v.playing < 2 e v.id ~= game.JobId então
				table.insert(servers, 1, v.id)
			fim
		fim
		se #servidores > 0 então
			ReplicatedStorage.__ServerBrowser:InvokeServer("teleport", servers[math.random(1, #servers)])
		fim
	fim
fim

local BringMob = função(v, s)
	se não for um personagem ou não for um personagem.PrimaryPart, então
		retornar
	fim
	alvos locais = {}
	para _, x em workspace.Enemies:GetChildren() faça
		local h = x:FindFirstChildOfClass("Humanoid")
		se x.PrimaryPart e h e h.Health > 0 e (não v ou x.Name == v) e LocalPlayer:DistanceFromCharacter(x.PrimaryPart.Position) <= 180 então
			alvos[#alvos + 1] = x
			se #alvos == 3 então
				quebrar
			fim
		fim
	fim
	se #alvos == 0 então
		retornar
	fim
	local t = targets[1].PrimaryPart.CFrame
	para _, x em alvos faça
		se isnetworkowner(x.PrimaryPart) então
			x.PrimaryPart.CFrame = t
		fim
	fim
fim

função local WaitUntilNoBusy()
	pcall(function()
		task.wait(0.1)
		Se Character.Busy.Value então
			repetir
				task.wait(0.1)
			até que não Character.Busy.Value
		fim
	fim)
fim

função local AutoSkill()
	para _, v em LocalPlayer.Backpack:GetChildren() faça
		se v.ToolTip == "Combate Corpo a Corpo" ou v.ToolTip == "Espada" ou v.ToolTip == "Fruta Blox" ou v.ToolTip == "Arma de Fogo" então
			Personagem.Humanoide:EquipTool(v)
			VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.Z, false, game)
			VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.Z, false, game)
			AguardeAtéQueNãoEstejaOcupado()
			VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.X, false, game)
			VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.X, false, game)
			AguardeAtéQueNãoEstejaOcupado()
			VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.C, false, game)
			VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.C, false, game)
			quebrar
		fim
	fim
fim

função local Seabeast()
	se não CheckSeabeast() e não CheckPirateBoat() então
		se não CheckBoat() então
			Se (Vector3.new(-12.7291985, 10.3120461, 2955.1543) - Character.HumanoidRootPart.Position).Magnitude > 8 então
				Tween(CFrame.new(-12.7291985, 10.3120461, 2955.1543), 280).Completed:Wait()
				CommF_:InvokeServer("BuyBoat", "PirateBrigade")
			outro
				CommF_:InvokeServer("BuyBoat", "PirateBrigade")
			fim
		outro
			Se (CheckBoat().VehicleSeat.Position - Vector3.new(28.4108, 1.2327, 3679.99)).Magnitude > 50 então
				CheckBoat().VehicleSeat.CFrame = CFrame.new(28.4108 + 20, 1.2327 + 10, 3679.99 + 20)
			outro
				se não Character.Humanoid.Sit então
					se (CheckBoat().VehicleSeat.Position - Character.HumanoidRootPart.Position).Magnitude > 50 então
						Tween(CheckBoat().VehicleSeat.CFrame, 280).Completed:Wait()
					outro
						Character.HumanoidRootPart.CFrame = CheckBoat().VehicleSeat.CFrame
					fim
				fim
			fim
		fim
	elseif CheckPirateBoat() então
		local v = VerificarBarcoPirata()
		repetir
			task.wait()
			Tween(v.Engine.CFrame * CFrame.new(0, -20, 0), 250).Completed:Wait()
			AutoSkill()
		até que não seja v ou não seja v.Parent ou v.Health.Value <= 0 ou não seja CheckPirateBoat()
	elseif CheckSeabeast() então
		local v = VerificarSeabest()
		repetir
			task.wait()
			TeleportSeabeast(v)
			AutoSkill()
		até que não seja v ou não seja v.Parent ou v.Health.Value <= 0 ou não seja CheckSeabeast()
	fim
fim

função local KillMonster(_v)
    LogChange("combat_target", "combat: target " .. string.lower(tostring(_v)))
    SetStatus("combat | target " .. string.lower(tostring(_v)))
	enquanto não (Personagem e Personagem:EncontrarPrimeiroFilhoDaClasse("Humanoide")) faça
		task.wait()
	fim
	Se Character.Humanoid.Health > 0 então
		para _, v2 em {workspace.Enemies, ReplicatedStorage} faça
			para _, v em v2:GetChildren() faça
				se v.Name:find(_v) e v.PrimaryPart e v:FindFirstChildOfClass("Humanoid") então
					repetir
						task.wait()
						se não v ou não v.PrimaryPart ou não v:FindFirstChildOfClass("Humanoid") ou v.Humanoid.Health <= 0 então
							quebrar
						fim
						Tween(v.PrimaryPart.Position + Vector3.new(0, 25, 7), 350)
						se LocalPlayer:DistanceFromCharacter(v.PrimaryPart.CFrame.Position) <= 200 então
							Tween(v.PrimaryPart.Position + Vector3.new(0, 25, 7), math.huge)
							se LocalPlayer:DistanceFromCharacter(v.PrimaryPart.CFrame.Position) <= 50 então
								EquiparArma()
								Ataque()
							fim
						fim
					até que não v
						ou não v.PrimaryPart
						ou não v:FindFirstChildOfClass("Humanoid")
						ou v.Humanoid.Saúde <= 0
						ou Character.Humanoid.Health <= 0
						ou não Character:FindFirstChild("Humanoid")
					quebrar
				fim
			fim
		fim
	fim
fim

local lastTakeQuest = 0
local QuestController, GetQuest, GetQuestData, GetQuestPosition

GetQuestPosition = função()
    local nearestPos = nulo
    local shortestdist = math.huge
    para _, v em require(ReplicatedStorage.GuideModule).Data.NPCList do
        se typeof(v) == "table" e v.Position então
            para _, x em v faça
                se x == GetQuest()["NameQuest"] então
                    distância local = JogadorLocal:DistânciaDoPersonagem(v.Posição)
                    se distância < distânciaMaisCurta então
                        distância mais curta = distância
                        nearestPos = v.Position
                    fim
                fim
            fim
        fim
    fim
    retornar posiçãoMaisPróxima
fim

função local TakeQuest()
	se (tick() - lastTakeQuest) < 0,75 então
		retornar
	fim
	local q = GetQuest()
    se q e q.NameQuest então
        LogChange("quest_target", "quest: " .. string.lower(tostring(q.NameQuest)) .. " / " .. string.lower(tostring(q.NameMonster or "unknown")))
    outro
        LogChange("quest_target", "quest: sem alvo")
    fim
	se q e q.NameQuest e q.ID e (não GetQuestData() ou QuestController.CurrentQuestName ~= q.NameQuest) então
		local questPos = GetQuestPosition()
        LogChange("quest_travel", questPos and "quest: traveling to npc" or "quest: npc position missing")
		se não questPos então
			LogChange("quest_travel", "quest: posição do npc ausente, trocando de servidor")
			Hop()
		fim
		se questPos e TweenWithRespawnRetry(questPos, 350) então
			task.wait(0.25)
			se LocalPlayer:DistanceFromCharacter(questPos) <= 15 então
                LogChange("quest_start", "quest: iniciando " .. string.lower(tostring(q.NameQuest)))
                SetStatus("quest | starting " .. string.lower(tostring(q.NameQuest)))
				CommF_:InvokeServer("StartQuest", q.NameQuest, q.ID)
			fim
		fim
	fim
	lastTakeQuest = tick()
fim

local MobsCakePrince = {
	"Cookie Crafter",
	"Protetor de Bolo",
	"Equipe de Confeitaria",
	"Padeiro Chefe",
}
local CakePrinceBosses = {
	"Rei da Massa",
	"Príncipe do Bolo",
}
função local GetCakePrince()
	para _, v em {workspace.Enemies, ReplicatedStorage} faça
		para _, v2 em v:GetChildren() faça
			se table.find(CakePrinceBosses, v2.Name) e v2:FindFirstChild("Humanoid") e v2:FindFirstChild("HumanoidRootPart") e v2.Humanoid.Health > 0 então
				retornar v2
			fim
		fim
	fim
fim

QuestController = {
	CurrentQuest = "",
	CurrentQuestName = "",
	CachedQuestData = require(ReplicatedStorage.Quests),
	QuestConnection = nil,
}
QuestController.Set = function(self, data)
	self.CurrentQuest = (function()
		para i, _ em data.Progress faça
			retornar i
		fim
	fim)()
	self.CurrentQuestName = data.InternalQuestName
fim
QuestController.Reset = function(self)
	self.CurrentQuest = ""
	self.CurrentQuestName = ""
fim

QuestController.QuestConnection = ReplicatedStorage.Remotes.QuestUpdate.OnClientEvent:Connect(function(a2, a3)
	se a2 e typeof(a2) == "table" então
		QuestController:Set(a2)
		retornar
	fim
	QuestController:Reset()
fim)

GetQuestData = função()
	retornar QuestController.CurrentQuest ~= ""
fim

função local GetLastQuest()
	dados locais = require(ReplicatedStorage.GuideModule).Dados
	se data e data.QuestData e data.QuestData.Task então
		para i, _ em data.QuestData.Task faça
			retornar i
		fim
	fim
fim

função local GetBestQuest()
	local bestLevelReq = -1
	local bestRnq, bestTaskName, bestRidq = zero, zero, zero
	para rnq, v em QuestController.CachedQuestData faça
		se table.find({ "BartiloQuest", "Trainees", "MarineQuest", "CitizenQuest", }, tostring(rnq)) então
			continuar
		fim
		para ridq, ct em v faça
			se ct.LevelReq < 0 ou LocalPlayer.Data.Level.Value < ct.LevelReq então
				continuar
			fim
			para O, taskValue em ct.Task faça
				se taskValue > 1 e ct.LevelReq > bestLevelReq então
					bestLevelReq = ct.LevelReq
					bestRnq = rnq
					bestTaskName = tostring(O)
					bestRidq = ridq
				fim
			fim
		fim
	fim
	se não for bestRnq então
		retornar
	fim
	retornar bestLevelReq, bestRnq, bestTaskName, bestRidq
fim

GetQuest = função()
	Nível local = JogadorLocal.Dados.Nível.Valor
	msn local, nq, idq
	local mínimo = 0
	Se o nível for maior ou igual a 275 e menor que 300, então
		nq, msn, idq = "ColosseumQuest", "Guerreiro Togga", 1
	senão se Nível >= 1450 e Mundo.Mar2() então
		nq, msn, idq = "ForgottenQuest", "Water Fighter", 2
	senão se Nível >= 700 e Mundo.Mar1() então
		nq, msn, idq = "FountainQuest", "Capitão da Galeria", 2
	outro
		min, nq, msn, idq = GetBestQuest()
	fim
	local q = {}
	se Nível >= 10 e não GetQuestData() e GetLastQuest() == msn então
		para rnq, v em QuestController.CachedQuestData faça
			para _, N em v faça
				para x, _ em N.Tarefa faça
					se tostring(x) == msn então
						para iq, v2 em v faça
							para n, ct em v2.Tarefa faça
								se n ~= msn e ct > 1 então
									q.NameMonster = v2.LevelReq <= Level e tostring(n) ou msn
									q.NameQuest = v2.LevelReq <= Level e rnq ou nq
									q.ID = v2.LevelReq <= Nível e iq ou idq
									retornar q
								fim
							fim
						fim
					fim
				fim
			fim
		fim
	fim
	q.NameMonster = msn
	q.NameQuest = nq
	q.ID = idq
	retornar q
fim

função local TakeQuestCake(b)
	se não GetQuestData() então
        LogChange("cake_quest", "quest: cake " .. string.lower(tostring(b)))
		local r = require(ReplicatedStorage.Quests)
		local t = {
			r.CakeQuest1,
			r.CakeQuest2,
		}
		local qn, id, lvrq
		para c, v em t faça
			se typeof(v) == "table" então
				para i, v2 em v faça
					se v2.Name == b então
						id = i
						qn = "CakeQuest" .. c
						lvrq = v2.LevelReq
					fim
				fim
			fim
		fim
		local p
		para _, v em require(ReplicatedStorage.GuideModule).Data.NPCList do
			se typeof(v) == "table" então
				para _, x em v faça
					se x == qn então
						se LocalPlayer.Data.Level.Value >= lvrq então
							p = v.Position
						outro
							retornar
						fim
					fim
				fim
			fim
		fim

		se p e TweenWithRespawnRetry(p, 350) e LocalPlayer:DistanceFromCharacter(p) < 5 então
			task.wait(1)
			CommF_:InvokeServer("StartQuest", tostring(qn), id)
			task.wait(0.5)
		fim
	fim
fim

função local TakeQuestBone(b)
	se não GetQuestData() então
        LogChange("bone_quest", "quest: bones " .. string.lower(tostring(b)))
		local r = require(ReplicatedStorage.Quests)
		local t = { r.HauntedQuest1, r.HauntedQuest2 }
		local qn, id, lvrq
		para c, v em t faça
			se typeof(v) == "table" então
				para i, v2 em v faça
					se v2.Name == b então
						id = i
						qn = "HauntedQuest" .. c
						lvrq = v2.LevelReq
					fim
				fim
			fim
		fim
		local p
		para _, v em require(ReplicatedStorage.GuideModule).Data.NPCList do
			se typeof(v) == "table" então
				para _, x em v faça
					se x == qn então
						se LocalPlayer.Data.Level.Value >= lvrq então
							p = v.Position
						outro
							retornar
						fim
					fim
				fim
			fim
		fim

		se p e TweenWithRespawnRetry(p, 350) e LocalPlayer:DistanceFromCharacter(p) < 5 então
			task.wait(1)
			CommF_:InvokeServer("StartQuest", tostring(qn), id)
			task.wait(0.5)
		fim
	fim
fim

função local GetMonsterName()
	local _v = {}
	para _, v em workspace.Enemies:GetChildren() faça
		se não table.find(_v, v.Name) e v:IsA("Model") e v.Name ~= "PirateBasic" e não v.Name:find("Brigade") e não v.Name:find("FishBoat") e v:FindFirstChildOfClass("Humanoid") e v.Humanoid.Health > 0 e v:FindFirstChild("HumanoidRootPart") então
			table.insert(_v, v.Name)
		fim
	fim
	para _, v em _v faça
		nome local = v:gsub(" %pLv%. ?%d+%p", "")
		Se QuestController.CurrentQuest == nome então
			nome de retorno
		fim
	fim
	retornar falso
fim

função local LoadItem(v, t)
	CommF_:InvokeServer("LoadItem", v, { t, })
fim

função local GetMaterial(nome)
	dados locais = IRS:GetItems(KEYS.QUANTITY)
	para _, v em dados faça
		item local = CORRESPONDÊNCIA(v.ItemId)._ok
		Se item.Display.Category == "Material" e item.Index.StorageKey == name então
			retornar v.Valor
		fim
	fim
	retornar 0
fim

função local BringSG(v)
	enquanto não (Personagem e Personagem.PartePrimária) faça
		task.wait()
	fim
	alvos locais = {}
	para _, x em workspace.Enemies:GetChildren() faça
		se x:FindFirstChild("HumanoidRootPart") e x:FindFirstChild("Humanoid") e x.Humanoid.Health > 0 e (não v ou x.Name == v) e (p.Position - x.HumanoidRootPart.Position).Magnitude <= math.huge então
			alvos[#alvos + 1] = x
			se #alvos == 6 então
				quebrar
			fim
		fim
	fim
	se #alvos == 0 então
		retornar
	fim
	local t = targets[1].HumanoidRootPart.CFrame
	para _, x em alvos faça
		sethiddenproperty(x.HumanoidRootPart, "NetworkOwnershipRule", Enum.NetworkOwnership.Manual)
		x.HumanoidRootPart.CFrame = t
	fim
fim

função local GetSG()
    LogChange("soul_guitar", "soul guitar: verificando o quebra-cabeça")
	local check = CommF_:InvokeServer("GuitarPuzzleProgress", "Check")
	se não verificar então
		Se Lighting.Sky.MoonTextureId == "http://www.roblox.com/asset/?id=9709149431" e (Lighting.ClockTime > 16 ou Lighting.ClockTime < 5) então
            LogChange("soul_guitar_stage", "soul guitar: fazendo ritual de lápide")
            SetStatus("guitarra da alma | fazendo ritual na lápide")
			se LocalPlayer:DistanceFromCharacter(Vector3.new(-8654, 140, 6167)) > 50 então
				Tween(CFrame.new(-8654, 140, 6167), 350).Completed:Wait()
			fim
			CommF_:InvokeServer("gravestoneEvent", 2)
			CommF_:InvokeServer("gravestoneEvent", 2, true)
        outro
            LogChange("soul_guitar_stage", "soul guitar: aguardando a lua cheia")
            SetStatus("soul guitar | esperando pela lua cheia")
		fim
	senão se check.Swamp == falso então
        LogChange("soul_guitar_stage", "soul guitar: limpando o pântano")
        SetStatus("soul guitar | limpando pântano")
		se (function() local b = 0 para _, v em workspace.Enemies:GetChildren() faça se v.Name == "Zumbi Vivo" e v.Humanoid e v.Humanoid.Health > 0 então
						b = b + 1
					fim
				fim
				retornar b
			end)() == 6
		então
			repetir
				para _, v em workspace.Enemies:GetChildren() faça
					se v.Name == "Zumbi Vivo" e v.Humanoid e v.Humanoid.Health > 0 então
						repetir
							task.wait()
							se não v ou não v.PrimaryPart ou não v:FindFirstChild("Humanoid") ou v.Humanoid.Health <= 0 então
								quebrar
							fim
							Tween(v.PrimaryPart.CFrame * CFrame.new(0, 15, 10), 350)
							se LocalPlayer:DistanceFromCharacter(v.PrimaryPart.CFrame.Position) <= 220 então
								Tween(v.PrimaryPart.CFrame * CFrame.new(0, 15, 10), math.huge)
								se LocalPlayer:DistanceFromCharacter(v.PrimaryPart.CFrame.Position) <= 40 então
									BringSG(v.Name)
									EquiparArma()
									Ataque()
								fim
							fim
						até que não seja v ou não seja v.PrimaryPart ou não seja v:FindFirstChild("Humanoid") ou v.Humanoid.Health <= 0
					fim
				fim
			até CommF_:InvokeServer("GuitarPuzzleProgress", "Check").Swamp
				ou (função()
						local b = 0
						para _, v em workspace.Enemies:GetChildren() faça
							se v.Name == "Zumbi Vivo" e v.PrimaryPart e v:FindFirstChild("Humanoid") e v.Humanoid.Health > 0 então
								b = b + 1
							fim
						fim
						retornar b
					fim)()
					== 0
		fim
	senão se check.Gravestones == falso então
        LogChange("soul_guitar_stage", "soul guitar: resolvendo lápides")
        SetStatus("guitarra da alma | resolvendo lápides")
		CommF_:InvokeServer("GuitarPuzzleProgress", "Gravestones")
	senão se check.Ghost == falso então
        LogChange("soul_guitar_stage", "soul guitar: concluindo o desafio fantasma")
        SetStatus("guitarra da alma | concluindo o desafio fantasma")
		CommF_:InvokeServer("GuitarPuzzleProgress", "Ghost")
	senão se check.Trophies == falso então
        LogChange("soul_guitar_stage", "soul guitar: completando o desafio do troféu")
        SetStatus("guitarra soul | concluindo o desafio do troféu")
		CommF_:InvokeServer("GuitarPuzzleProgress", "Trophies")
	senão se check.Pipes == falso então
        LogChange("soul_guitar_stage", "soul guitar: solving pipes")
        SetStatus("soul guitar | solving pipes")
		CommF_:InvokeServer("GuitarPuzzleProgress", "Pipes")
	senão se check.CraftedOnce == falso então
		Se LocalPlayer.Data.Fragments.Value >= 5000 e GetMaterial("Bones") >= 500 e GetMaterial("Ectoplasm") >= 250 então
            LogChange("soul_guitar_stage", "soul guitar: crafting")
            SetStatus("guitarra da alma | artesanato")
			CommF_:InvokeServer("soulGuitarBuy")
        outro
            LogChange("soul_guitar_stage", "soul guitar: coletando materiais")
            SetStatus("guitarra da alma | coletando materiais")
            LogChange("soul_guitar_materials", "soul guitar: fragmentos " .. tostring(LocalPlayer.Data.Fragments.Value) .. "/5000, ossos " .. tostring(GetMaterial("Ossos")) .. "/500, ectoplasma " .. tostring(GetMaterial("Ectoplasma")) .. "/250")
		fim
	fim
fim

CheckMonster local

função local UnlockSecondSea()
    LogChange("sea_unlock", "sea: desbloqueando o segundo mar")
	viagem local = CommF_:InvokeServer("TravelDressrosa")
	Se travel:find("Não é possível") então
		dados locais = CommF_:InvokeServer("DressrosaQuestProgress")
		se não data.UsedKey então
			se não data.TalkedDetective então
                LogChange("sea2_stage", "sea 2: conversando com o detetive")
                SetStatus("mar 2 | falando com o detetive")
				CommF_:InvokeServer("DressrosaQuestProgress", "Detective")
			elseif LocalPlayer.Backpack:FindFirstChild("Key") então
				Character.Humanoid:EquipTool(LocalPlayer.Backpack["Key"])
			fim
			CommF_:InvokeServer("DressrosaQuestProgress", "UseKey")
		senão se não data.KilledIceBoss então
            LogChange("sea2_stage", "sea 2: derrotando o almirante de gelo")
            SetStatus("mar 2 | derrotando o almirante de gelo")
			se CheckMonster("Almirante de Gelo") então
				KillMonster ("Almirante de Gelo")
			outro
				Tween(CFrame.new(1328, 43, -1346), 300)
			fim
		outro
			CommF_:InvokeServer("TravelDressrosa")
		fim
	fim
fim

função local GetFruitValue(_v)
	dados locais = IRS:GetItems(KEYS.QUANTITY)
	para _, v em dados faça
		item local = CORRESPONDÊNCIA(v.ItemId)._ok
		Se item.Display.Category == "Blox Fruit" e item.Quality.MoneyPrice >= _v então
			return item.Index.StorageKey
		fim
	fim
	retornar nulo
fim

função local GetFruitId(g)
	local ok = string.match(g, "((%u)[^%-]+)$")
	retorno ok .. "Fruta"
fim

local zQuestProRemote = ReplicatedStorage.Remotes:FindFirstChild("RefreshZQuestPro")
se World.Sea2() e zQuestProRemote então
	local zQuestProConnection = zQuestProRemote.OnClientEvent:Connect(function(a)
		se a == "promptDialogue" então
			print("cutscene finished")
			HopLowServer()
		fim
	fim)
fim

Local GetFruits

função local UnlockThirdSea()
    LogChange("sea_unlock", "mar: desbloqueando o terceiro mar")
	local check = CommF_:InvokeServer("ZQuestProgress", "Check")
	Se CommF_:InvokeServer("GetUnlockables").FlamingoAccess então
		print("verificação: " .. tostring(verificação))
		se check == 0 então
            LogChange("sea3_stage", "sea 3: iniciando a missão de Rip Indra")
            SetStatus("mar 3 | iniciando a missão de Rip Indra")
			CommF_:InvokeServer("TravelZou")
			Se LocalPlayer:DistanceFromCharacter(Vector3.new(0, 0, 0)) < 20000 então
				CommF_:InvokeServer("ZQuestProgress", "Begin")
			fim
			Se CheckMonster("rip_indra") então
				KillMonster("rip_indra")
			outro
				JogadorLocal:Expulsar("Reconecte para corrigir o problema do Blox Kid")
			fim
		senão se check == 1 então
            LogChange("sea3_stage", "sea 3: continuando a missão")
            SetStatus("mar 3 | continuação da missão")
			CommF_:InvokeServer("TravelZou")
		outro
			se CheckMonster("Don Swan") então
                LogChange("sea3_stage", "sea 3: derrotando don swan")
                SetStatus("mar 3 | derrotando Don Swan")
				KillMonster ("Don Swan")
				CISNE_MORTO = verdadeiro
				CommF_:InvokeServer("ZQuestProgress", "Begin")
			outro
				se LocalPlayer:DistanceFromCharacter(Vector3.new(2288, 15, 863)) > 15 então
					Tween(CFrame.new(2288, 15, 863), 350).Completed:Wait()
					retornar verdadeiro
				fim
			fim
		fim
	outro
        LogChange("sea3_stage", "sea 3: desbloqueando o acesso ao flamingo")
        SetStatus("mar 3 | desbloqueando acesso ao flamingo")
		local talk = CommF_:InvokeServer("TalkTrevor", "1")
		se falar então
			se talk == 1 ou talk ~= 2 então
				local df = GetFruitValue(1000000)
				se df então
					se talk == 1 então
						CommF_:InvokeServer("TalkTrevor", "2")
						task.wait(0.75)
					fim
					PAUSE_AUTO_STORE = true
					CommF_:InvokeServer("LoadFruit", df)
					EquipTool(GetFruitId(df))
					CommF_:InvokeServer("TalkTrevor", "1")
					CommF_:InvokeServer("TalkTrevor", "2")
					CommF_:InvokeServer("TalkTrevor", "3")
					PAUSE_AUTO_STORE = false
				outro
					local df = GetFruits()
					se df então
						pcall(function()
							Tween(df:GetPivot(), 350).Completed:Wait()
						fim)
					fim
					pular()
					task.wait(5)
				fim
			fim
		fim
	fim
fim

GetFruits = função()
	para _, v em workspace:GetChildren() faça
		se string.find(v.Name, "Fruit") e (v:IsA("Tool") ou v:IsA("Model")) e (v.Name ~= "Fruit ") então
			retornar v
		fim
	fim
	retornar nulo
fim

função local GetCastleRaid()
	para _, v em {workspace.Enemies, ReplicatedStorage} faça
		para _, v2 em v:GetChildren() faça
			se v2:GetAttribute("Level") e v2:GetAttribute("Level") <= 1500 e v2:IsA("Model") e (v2:GetPivot().Position - Vector3.new(-5000, 350, -3035)).Magnitude <= 1000 então
				retornar verdadeiro
			fim
		fim
	fim
	retornar nulo
fim

task.spawn(function()
	enquanto verdadeiro faça
		se não PAUSE_AUTO_STORE então
			pcall(function()
				local gg = {}
				para _, _v em {LocalPlayer.Backpack, LocalPlayer.Character} faça
					se _v então
						para _, v em _v:GetChildren() faça
							se v.Name:find("Fruit") e v:IsA("Tool") então
								local ori = v:GetAttribute("OriginalName") ou v.Name
								se não CheckItem(v.Name) e não CheckItem(ori) então
									tabela.inserir(gg, { ferramenta = v, ori = ori, })
								fim
							fim
						fim
					fim
				fim
				se #gg > 0 então
					local sf = {}
					local inv = CommF_:InvokeServer("getInventoryFruits")
					se type(inv) == "table" então
						para _, _v em inv faça
							se type(_v) == "table" e _v.Name então
								sf[_v.Name] = true
							fim
						fim
					fim
					para _, v em gg faça
						se não sf[v.ori] então
							CommF_:InvokeServer("StoreFruit", v.ori, v.tool)
							sf[v.ori] = true
						fim
					fim
				fim
			fim)
		fim
		CommF_:InvokeServer("Cousin", "DLCBoxData")
		task.wait(1)
	fim
fim)

local materialMon, materialPos
função local GetMaterialMon(material)
	se não for um personagem ou não for um personagem.PrimaryPart, então
		retornar
	fim
	Se World.Sea1() então
		se material == "Asas de Anjo" então
			materialMon = {
				"Shanda",
				"Esquadrão Real",
				"Soldado Real",
				"Wysper",
				"Deus do Trovão",
			}
			materialPos = CFrame.new(-4698, 845, -1912)
		senão se material == "Couro + Sucata Metálica" então
			materialMon = {
				"Brute",
				"Pirata",
			}
			materialPos = CFrame.new(-1145, 15, 4350)
		senão se material == "Minério de Magma" então
			materialMon = {
				"Soldado Militar",
				"Espião Militar",
				"Almirante Magma",
			}
			materialPos = CFrame.new(-5815, 84, 8820)
		senão se material == "Cauda de Peixe" então
			materialMon = {
				"Guerreiro Homem-Peixe",
				"Comando Homem-Peixe",
				"Senhor Homem-Peixe",
			}
			materialPos = CFrame.new(61123, 19, 1569)
		fim
	elseif World.Sea2() então
		Se o material for igual a "Couro + Sucata Metálica", então
			materialMon = {
				"Capitão da Marinha",
			}
			materialPos = CFrame.new(-2010.5059814453125, 73.00115966796875, -3326.620849609375)
		senão se material == "Minério de Magma" então
			materialMon = {
				"Magma Ninja",
				"Pirata de Lava",
			}
			materialPos = CFrame.new(-5428, 78, -5959)
		senão se material == "Ectoplasma" então
			materialMon = {
				"Marinheiro de convés",
				"Engenheiro Naval",
				"Comissário de bordo",
				"Oficial de Navio",
			}
			materialPos = CFrame.new(911.35827636719, 125.95812988281, 33159.5390625)
		senão se material == "Gota Mística" então
			materialMon = {
				"Lutador de Água",
			}
			materialPos = CFrame.new(-3385, 239, -10542)
		senão se material == "Material Radioativo" então
			materialMon = {
				"Equipe da Fábrica",
			}
			materialPos = CFrame.new(295, 73, -56)
		senão se material == "Presa de Vampiro" então
			materialMon = {
				"Vampiro",
			}
			materialPos = CFrame.new(-6033, 7, -1317)
		fim
	elseif World.Sea3() então
		Se o material for igual a "Couro + Sucata Metálica", então
			materialMon = {
				"Pirata da Selva",
				"Pirata da Floresta",
			}
			materialPos = CFrame.new(-11975.78515625, 331.7734069824219, -10620.0302734375)
		senão se material == "Cauda de Peixe" então
			materialMon = {
				"Invasor Homem-Peixe",
				"Capitão Homem-Peixe",
			}
			materialPos = CFrame.new(-10993, 332, -8940)
		senão se material == "Cacau Conjurado" então
			materialMon = {
				"Batalha de Barras de Chocolate",
				"Guerreiro do Cacau",
			}
			materialPos = CFrame.new(620, 78, -12581.369140625)
		senão se material == "Escama de Dragão" então
			materialMon = {
				"Guerreiro da Tripulação do Dragão",
				"Arqueiro da Tripulação do Dragão"
			}
			materialPos = CFrame.new(6594, 383, 139)
		senão se material == "Pólvora" então
			materialMon = {
				"Bilionário da Pistola",
			}
			materialPos = CFrame.new(-469, 74, 5904)
		senão se material == "Mini Presa" então
			materialMon = {
				"Pirata Mitológico",
			}
			materialPos = CFrame.new(-13545, 470, -6917)
		fim
	fim
fim

função local FarmMaterial(material)
    LogChange("material", "material: farming " .. string.lower(tostring(material)))
	GetMaterialMon(material)
	multidão local = nula
	local min = math.huge
	para _, v em workspace.Enemies:GetChildren() faça
		se v.PrimaryPart e v:FindFirstChild("Humanoid") e v.Humanoid.Health > 0 e table.find(materialMon, v.Name) então
			local dist = LocalPlayer:DistanceFromCharacter(v.PrimaryPart.Position)
			se dist < min então
				min = dist
				mob = v
			fim
		fim
	fim
	se mob e mob.PrimaryPart e mob.Humanoid e mob.Humanoid.Health > 0 então
        LogChange("material_target", "material: alvo encontrado para " .. string.lower(tostring(material)))
        SetStatus("material | lutando por " .. string.lower(tostring(material)))
		repetir
			task.wait()
			EquiparArma()
			Tween(mob.PrimaryPart.CFrame * CFrame.new(0, 20, 10), 350)
			se LocalPlayer:DistanceFromCharacter(mob.PrimaryPart.CFrame.Position) <= 200 então
				Tween(mob.PrimaryPart.CFrame * CFrame.new(0, 20, 10), math.huge)
				se LocalPlayer:DistanceFromCharacter(mob.PrimaryPart.CFrame.Position) <= 45 então
					BringMob(mob.Name)
					Ataque()
				fim
			fim
		até que não seja mob ou não seja mob.PrimaryPart ou não seja mob:FindFirstChild("Humanoid") ou mob.Humanoid.Health <= 0
	outro
        LogChange("material_target", "material: aguardando " .. string.lower(tostring(material)) .. " spawn")
        SetStatus("material | aguardando " .. string.lower(tostring(material)) .. " spawn")
		Tween(materialPos, 350).Completed:Wait()
	fim
fim

função local GetRaceGrade()
	raça local = LocalPlayer.Data.Race
	se LocalPlayer.Backpack:FindFirstChild("Awakening") ou Character:FindFirstChild("Awakening") então
		retornar "V4"
	senão se LocalPlayer.Backpack:FindFirstChild("Last Resort") ou Character:FindFirstChild("Last Resort") ou LocalPlayer.Backpack:FindFirstChild("Agility") ou LocalPlayer.Character:FindFirstChild("Agility") ou LocalPlayer.Backpack:FindFirstChild("Water Body") ou Character:FindFirstChild("Water Body") ou LocalPlayer.Backpack:FindFirstChild("Heavenly Blood") ou Character:FindFirstChild("Heavenly Blood") ou LocalPlayer.Backpack:FindFirstChild("Heightened Senses") ou Character:FindFirstChild("Heightened Senses") ou LocalPlayer.Backpack:FindFirstChild("Energy Core") ou Character:FindFirstChild("Energy Core") ou LocalPlayer.Backpack:FindFirstChild("Primordial Reign") ou Character:FindFirstChild("Primordial Reign") então
		retornar "V3"
	elseif race:FindFirstChild("Evolved") então
		retornar "V2"
	outro
		retornar "V1"
	fim
fim

BindedMeleeNPCNames = {}
função local MeleeNPCPosition(a, b, c)
	para i, v em {a, b, c} faça
		se v e World["Sea" .. i]() então
			retornar v
		fim
	fim
	retornar nulo
fim

BindedMeleeNPCNames.BlackLeg =
	MeleeNPCPosition(Vector3.new(-985, 14, 3988), Vector3.new(-4754, 35, -4849), Vector3.new(-5045, 372, -3178))
BindedMeleeNPCNames.Electro =
	MeleeNPCPosition(Vector3.new(-5384, 14, -2149), Vector3.new(-4864, 35, -4765), Vector3.new(-4994, 315, -3201))
BindedMeleeNPCNames.FishmanKarate =
	MeleeNPCPosition(Vector3.new(61585, 19, 987), Vector3.new(-4957, 35, -4668), Vector3.new(-5022, 372, -3187))

BindedMeleeNPCNames.SharkmanKarate =
	MeleeNPCPosition(nil, Vector3.new(-2602, 239, -10317), Vector3.new(-4974, 315, -3221))
BindedMeleeNPCNames.DeathStep = MeleeNPCPosition(nil, Vector3.new(6356, 296, -6763), Vector3.new(-4997, 315, -3220))
BindedMeleeNPCNames.Superhuman = MeleeNPCPosition(nil, Vector3.new(1379, 247, -5190), Vector3.new(-5004, 372, -3194))
BindedMeleeNPCNames.DragonClaw = MeleeNPCPosition(nil, Vector3.new(702, 187, 653), Vector3.new(-4980, 372, -3205))

BindedMeleeNPCNames.ElectricClaw = MeleeNPCPosition(nil, nil, Vector3.new(-10371, 331, -10131))
BindedMeleeNPCNames.DragonTalon = MeleeNPCPosition(nil, nil, Vector3.new(5661, 1210, 863))
BindedMeleeNPCNames.Godhuman = MeleeNPCPosition(nil, nil, Vector3.new(-13774, 333, -9879))

função local TweenToMeleeBuyer(nome)
	local p = BindedMeleeNPCNames[nome]
	se p então
		return TweenWithRespawnRetry(p, 300)
	fim
	retornar falso
fim
função local GetMastery(nome)
	combate corpo a corpo local = {
		"Passo da Morte",
		"Super-humano",
		"ElectricClaw",
		"DragonTalon",
		"SharkmanKarate",
		"BlackLeg",
		"Eletro",
		"FishmanKarate",
		"SanguineArt",
		"Garra de Dragão",
	}
	se table.find(melee, name) então
		retorna MeleeData[nome] ou 0
	elseif CheckItem(name) então
		LoadItem(nome)
		task.wait(1)
		ferramenta local = LocalPlayer.Backpack:FindFirstChild(nome)
		se ferramenta então
			retornar tool.Level.Value
		fim
	fim
	retornar 0
fim

função local DoRaceV2()
    LogChange("race_mode", "race v2: verificando o progresso")
	se não World.Sea2() então
        LogChange("race_v2_travel", "race v2: viajando para o mar 2")
        SetStatus("corrida v2 | viajando para o mar 2")
		enquanto verdadeiro faça
			CommF_:InvokeServer("TravelDressrosa")
			task.wait(5)
		fim
	fim
	se não LocalPlayer.Data.Race:FindFirstChild("Evoluído") então
		local check = CommF_:InvokeServer("Alchemist", "1")
		se check == 0 então
            LogChange("race_v2_stage", "race v2: falando com o alquimista")
            SetStatus("raça v2 | falando com o alquimista")
			CommF_:InvokeServer("Alchemist", "2")
		senão se check == 1 então
            Se LocalPlayer.Backpack:FindFirstChild("Flower 1") ou Character:FindFirstChild("Flower 1") então
                LogChange("race_v2_flower1", "race v2: flor 1 coletada")
            fim
            Se LocalPlayer.Backpack:FindFirstChild("Flower 2") ou Character:FindFirstChild("Flower 2") então
                LogChange("race_v2_flower2", "race v2: flor 2 coletada")
            fim
            Se LocalPlayer.Backpack:FindFirstChild("Flower 3") ou Character:FindFirstChild("Flower 3") então
                LogChange("race_v2_flower3", "race v2: flor 3 coletada")
            fim
			se não LocalPlayer.Backpack:FindFirstChild("Flower 1") e não Character:FindFirstChild("Flower 1") então
                LogChange("race_v2_stage", "race v2: coletando flor 1")
                SetStatus("race v2 | coletando flor 1")
				Tween(workspace.Flower1.CFrame, 350)
				firetouchinterest(workspace.Flower1, Character.HumanoidRootPart, 1)
				firetouchinterest(workspace.Flower1, Character.HumanoidRootPart, 0)
                LogChange("race_v2_flower1", "race v2: flor 1 coletada")
			senão se não LocalPlayer.Backpack:FindFirstChild("Flower 2") e não Character:FindFirstChild("Flower 2") então
                LogChange("race_v2_stage", "race v2: coletando flor 2")
                SetStatus("corrida v2 | coletando flor 2")
				Interpolar(workspace.Flower2.CFrame, 350)
				firetouchinterest(workspace.Flower2, Character.HumanoidRootPart, 1)
				firetouchinterest(workspace.Flower2, Character.HumanoidRootPart, 0)
                LogChange("race_v2_flower2", "race v2: flor 2 coletada")
			senão se não LocalPlayer.Backpack:FindFirstChild("Flower 3") e não Character:FindFirstChild("Flower 3") então
				Se workspace.Enemies:FindFirstChild("Zombie") então
                    LogChange("race_v2_stage", "race v2: farming zombie for flower 3")
                    SetStatus("race v2 | farming zombie for flower 3")
					para _, v em workspace.Enemies:GetChildren() faça
						se v.Name == "Zumbi" então
							repetir
								task.wait()
								se não v ou não v.PrimaryPart ou não v:FindFirstChild("Humanoid") ou v.Humanoid.Health <= 0 então
									quebrar
								fim
								Tween(v.PrimaryPart.CFrame * CFrame.new(0, 15, 10), 350)
								se LocalPlayer:DistanceFromCharacter(v.PrimaryPart.Position) <= 200 então
									Tween(v.PrimaryPart.CFrame * CFrame.new(0, 15, 10), math.huge)
									EquiparArma()
									BringMob(v.Name)
									Ataque()
								fim
							até LocalPlayer.Backpack:FindFirstChild("Flower 3")
								ou não v
								ou não v.PrimaryPart
								ou não v:FindFirstChild("Humanoid")
								ou v.Humanoid.Saúde <= 0
						fim
					fim
				outro
                    LogChange("race_v2_stage", "corrida v2: aguardando o surgimento de zumbis")
                    SetStatus("race v2 | aguardando o surgimento de zumbis")
					Tween(CFrame.new(-5685, 48, -853), 350).Completed:Wait()
				fim
			fim
		senão se check == 2 então
            LogChange("race_v2_stage", "race v2: enviando flores")
            SetStatus("race v2 | enviando flores")
			CommF_:InvokeServer("Alchemist", "3")
		fim
    outro
        LogChange("race_v2_stage", "race v2: já evoluiu")
        SetStatus("raça v2 | já evoluiu")
	fim
fim

CheckMonster = função(nome)
	retornar workspace.Enemies:FindFirstChild(name) ou ReplicatedStorage:FindFirstChild(name)
fim

MinkQuestStage = 0
HumanQuestStage = 0
função local DoRaceV3()
    LogChange("race_mode", "race v3: verificando o progresso")
	se não World.Sea2() então
        LogChange("race_v3_travel", "race v3: viajando para o mar 2")
        SetStatus("corrida v3 | viajando para o mar 2")
		enquanto verdadeiro faça
			CommF_:InvokeServer("TravelDressrosa")
			task.wait(5)
		fim
	fim
	raça local = LocalPlayer.Data.Race.Value
	local check = CommF_:InvokeServer("Wenlocktoad", "1")
	se check == 0 então
        LogChange("race_v3_stage", "race v3: iniciando a missão")
        SetStatus("raça v3 | iniciando missão")
		CommF_:InvokeServer("Wenlocktoad", "2")
	senão se check == 1 então
		Se raça == "Humano", então
			se HumanQuestStage == 0 então
                LogChange("race_v3_stage", "race v3: lutando contra Jeremy")
                SetStatus("corrida v3 | lutando contra Jeremy")
				Se CheckMonster("Jeremy") então
					KillMonster("Jeremy")
					HumanQuestStage = 1
				outro
					Tween(CFrame.new(2333.209228515625, 449.2427062988281, 699.5128784179688), 300).Completed:Wait()
				fim
			senão se HumanQuestStage == 1 então
                LogChange("race_v3_stage", "race v3: fighting diamond")
                SetStatus("race v3 | diamante de luta")
				Se CheckMonster("Diamond") então
					KillMonster("Diamante")
					HumanQuestStage = 2
				outro
					Tween(CFrame.new(-1713.5589599609375, 198.99554443359375, -104.31584167480469), 300).Completed:Wait()
				fim
			senão se HumanQuestStage == 2 então
                LogChange("race_v3_stage", "race v3: fighting orbitus")
                SetStatus("race v3 | fighting orbitus")
				Se CheckMonster("Orbitus") então
					KillMonster("Orbitus")
					HumanQuestStage = 3
				outro
					Tween(CFrame.new(-2148.7568359375, 73.27831268310547, -4304.4130859375), 300).Completed:Wait()
				fim
			senão se HumanQuestStage == 3 então
                LogChange("race_v3_stage", "race v3: concluindo o teste humano")
                SetStatus("race v3 | concluindo o teste em humanos")
				CommF_:InvokeServer("Wenlocktoad", "3")
			fim
		senão se raça == "Coelho" então
			para _, v em workspace.ChestModels:GetChildren() faça
				se MinkQuestStage < 30 então
                    LogChange("race_v3_chests", "race v3: coletando baú " .. tostring(MinkQuestStage + 1) .. "/30")
                    SetStatus("race v3 | collecting chest " .. tostring(MinkQuestStage + 1) .. "/30")
					Tween(v.WorldPivot.Position, 350).Completed:Wait()
					Estágio da Missão Mink = Estágio da Missão Mink + 1
				senão se MinkQuestStage >= 30 então
                    LogChange("race_v3_stage", "race v3: concluindo o teste do coelho")
                    SetStatus("corrida v3 | concluindo teste do coelho")
					CommF_:InvokeServer("Wenlocktoad", "3")
				fim
			fim
		senão se raça == "Homem-Peixe" então
            LogChange("race_v3_stage", "race v3: farming sea beasts")
            SetStatus("raça v3 | criação de feras marinhas")
			repetir
				Seabeast()
				task.wait(1)
				CommF_:InvokeServer("Wenlocktoad", "3")
			até que GetRaceGrade() seja igual a "V3" ou não string.find(race, "Fish")
		fim
	senão se check == 2 então
        LogChange("race_v3_stage", "corrida v3: concluindo o teste")
        SetStatus("corrida v3 | concluindo o teste")
		CommF_:InvokeServer("Wenlocktoad", "3")
    outro
        LogChange("race_v3_stage", "race v3: aguardando o teste")
        SetStatus("corrida v3 | aguardando julgamento")
	fim
fim

Viagem local = {
	Sea1 = função()
		se não World.Sea1() então
			enquanto verdadeiro faça
				CommF_:InvokeServer("TravelMain")
				task.wait(10)
			fim
		fim
	fim,
	Sea2 = função()
		se não World.Sea2() então
			enquanto verdadeiro faça
				CommF_:InvokeServer("TravelDressrosa")
				task.wait(10)
			fim
		fim
	fim,
	Sea3 = função()
		se não World.Sea3() então
			enquanto verdadeiro faça
				CommF_:InvokeServer("TravelZou")
				task.wait(10)
			fim
		fim
	fim
}

função local GetGodHuman()
    LogChange("godhuman", "godhuman: verificando requisitos")
	local check = CommF_:InvokeServer("BuyGodhuman", true)
	se check ~= 0 e check ~= 1 então
		para i, v em {"Escama de Dragão", "Cauda de Peixe", "Gota Mística", "Minério de Magma"} faça
			máximo local = 0
			se (i % 2) > 0 então
				máx. = 10
			outro
				máx. = 20
			fim
			se GetMaterial(v) < max então
                LogChange("godhuman_material", "godhuman: " .. string.lower(v) .. " " .. tostring(GetMaterial(v)) .. "/" .. tostring(max))
				se (i == 1 ou i == 2) e não World.Sea3() então
					Viagem.Mar3()
				senão se (i == 3 ou i == 4) e não World.Sea2() então
					Viagem.Mar2()
				fim
				repetir
					FarmMaterial(v)
					task.wait(0.2)
				até GetMaterial(v) >= máximo
			fim
		fim
        LogChange("godhuman_stage", "godhuman: viajando para o comprador")
        SetStatus("godhuman | viajando para o comprador")
		Viagem.Mar3()
		TweenToMeleeBuyer("Godhuman")
        LogChange("godhuman_stage", "godhuman: estilo de compra")
        SetStatus("deushumano | estilo de compra")
		CommF_:InvokeServer("BuyGodhuman", true)
		CommF_:InvokeServer("BuyGodhuman")
	outro
        LogChange("godhuman_stage", "godhuman: estilo de compra")
        SetStatus("deushumano | estilo de compra")
		Viagem.Mar3()
		TweenToMeleeBuyer("Godhuman")
		CommF_:InvokeServer("BuyGodhuman")
	fim
fim

função local GetSaber()
    LogChange("saber", "saber: verificando o progresso")
	Viagem.Mar1()
	CommF_:InvokeServer("AbandonQuest")
	dados locais = CommF_:InvokeServer("ProQuestProgress")
	se data.UsedRelic == falso então
        LogChange("saber_stage", "saber: resolvendo quebra-cabeça da selva")
        SetStatus("sabre | resolvendo quebra-cabeça da selva")
		enquanto não (workspace.Map:FindFirstChild("Jungle") e workspace.Map.Jungle:FindFirstChild("QuestPlates") e workspace.Map.Jungle.QuestPlates:FindFirstChild("Door")) faça
			task.wait()
			Tween(CFrame.new(-1254.25439453125, 18.39460563659668, 342.31976318359375), 350)
		fim
		Se workspace.Map.Jungle.QuestPlates.Door.Transparency == 0 então
            LogChange("saber_stage", "saber: ativando placas da selva")
            SetStatus("sabre | ativando placas da selva")
			para _, v em workspace.Map.Jungle.QuestPlates:GetChildren() faça
				se v:IsA("Model") e string.find(v.Name, "Plate") então
					firetouchinterest(v.Button, Character.HumanoidRootPart, 1)
					firetouchinterest(v.Button, Character.HumanoidRootPart, 0)
				fim
			fim
		outro
			se data.UsedTorch == falso então
                LogChange("saber_stage", "saber: recolhendo tocha")
                SetStatus("sabre | coletando tocha")
				se LocalPlayer.Backpack:FindFirstChild("Torch") ou Character:FindFirstChild("Torch") então
					CommF_:InvokeServer("ProQuestProgress", "DestroyTorch")
				outro
					CommF_:InvokeServer("ProQuestProgress", "GetTorch")
				fim
			outro
				Se data.UsedCup então
					se data.TalkedSon e data.KilledMob então
                        LogChange("saber_stage", "saber: colocando relíquia")
                        SetStatus("sabre | colocando relíquia")
						se não data.UsedRelic então
							CommF_:InvokeServer("ProQuestProgress", "RichSon")
							se não Character:FindFirstChild("Relic") então
								Character.Humanoid:EquipTool(LocalPlayer.Backpack:FindFirstChild("Relic"))
							fim
							CommF_:InvokeServer("ProQuestProgress", "PlaceRelic")
						fim
					senão se data.TalkedSon == falso então
                        LogChange("saber_stage", "saber: falando com o filho rico")
                        SetStatus("sabre | conversando com filho rico")
						CommF_:InvokeServer("ProQuestProgress", "RichSon")
					senão se data.KilledMob == falso então
                        LogChange("saber_stage", "saber: derrotando o líder da multidão")
                        SetStatus("sabre | derrotando o líder da máfia")
						se CheckMonster("Líder da Máfia") então
							KillMonster ("Líder da Máfia")
						fim
					fim
				outro
                    LogChange("saber_stage", "saber: enchendo o copo")
                    SetStatus("sabre | enchendo copo")
					CommF_:InvokeServer("ProQuestProgress", "GetCup")
					se não Character:FindFirstChild("Cup") então
						Character.Humanoid:EquipTool(LocalPlayer.Backpack:FindFirstChild("Cup"))
					fim
					CommF_:InvokeServer("ProQuestProgress", "FillCup", Character.Cup)
					CommF_:InvokeServer("ProQuestProgress", "SickMan")
				fim
			fim
		fim
	outro
        LogChange("saber_stage", "saber: fighting sabre expert")
        SetStatus("sabre | especialista em sabre de combate")
		se CheckMonster("Saber Expert") então
			KillMonster ("Especialista em Sabres")
		outro
			Tween( ReplicatedStorage.FortBuilderReplicatedSpawnPositionsFolder["Saber Expert"].CFrame * CFrame.new(0, 25, 0), 350 )
		fim
	fim
fim

função local DoBartiloQuest()
    LogChange("bartilo", "bartilo: verificando o progresso")
	se não World.Sea2() então
        LogChange("bartilo_travel", "bartilo: viajando para o mar 2")
        SetStatus("bartilo | viajando para o mar 2")
		enquanto verdadeiro faça
			CommF_:InvokeServer("TravelDressrosa")
			task.wait(7.5)
		fim
	fim
	função local CheckKilledBanit()
		return CommF_:InvokeServer("BartiloQuestProgress").KilledBandits
	fim
	dados locais = CommF_:InvokeServer("BartiloQuestProgress", "Bartilo")
	se CheckKilledBanit() == falso então
        LogChange("bartilo_stage", "bartilo: cultivando cisnes piratas")
        SetStatus("bartilo | cultivando cisnes piratas")
		repetir
			Se QuestController.CurrentQuestName ~= "BartiloQuest" então
				CommF_:InvokeServer("StartQuest", "BartiloQuest", 1)
			fim
			Se workspace.Enemies:FindFirstChild("Swan Pirate") então
				para _, v em workspace.Enemies:GetChildren() faça
					se v.Name == "Pirata Cisne" e v:FindFirstChildOfClass("Humanoid") e v.Humanoid.Health > 0 e v.PrimaryPart então
						repetir
							task.wait()
							se não v ou não v.PrimaryPart ou não v:FindFirstChildOfClass("Humanoid") ou v.Humanoid.Health <= 0 então
								quebrar
							fim
							Tween(v.PrimaryPart.Position + Vector3.new(0, 20, 10), 350)
							se LocalPlayer:DistanceFromCharacter(v.PrimaryPart.Position) <= 200 então
								Tween(v.PrimaryPart.Position + Vector3.new(0, 20, 10), math.huge)
								EquiparArma()
								BringMob(v.Name)
								Ataque()
							fim
						até que não seja v ou não seja v.PrimaryPart ou v.Humanoid.Health <= 0
					fim
				fim
			outro
				Tween(CFrame.new(932.624451, 156.106079, 1180.27466), 350).Completed:Wait()
			fim
		até CheckKilledBanit()
	senão se data == 1 então
        LogChange("bartilo_stage", "bartilo: derrotando jeremy")
        SetStatus("bartilo | derrotando jeremy")
		Se CheckMonster("Jeremy") então
			KillMonster("Jeremy")
		outro
			Tween(Vector3.new(2129, 469, 779), 300).Completed:Wait()
		fim
	senão se data == 2 então
		para i = 1, 8 faça
            LogChange("bartilo_plate", "bartilo: ativando placa " .. tostring(i) .. "/8")
            SetStatus("bartilo | placa de ativação " .. tostring(i) .. "/8")
			firetouchinterest(workspace.Map.Dressrosa.BartiloPlates["Plate" .. i], Character.PrimaryPart, 0)
			task.wait(0.1)
			firetouchinterest(workspace.Map.Dressrosa.BartiloPlates["Plate" .. i], Character.PrimaryPart, 1)
		fim
	fim
fim
função local CheckDataLevel()
	local lv = LocalPlayer.Data.Level.Value
	se lv < 10 então
		retornar 1
	senão se lv < 70 então
		retornar 2
	senão se lv < 120 então
		retornar 3
	outro
		retornar 4
	fim
fim
função local FarmLevelLogic()
	nível localData = CheckDataLevel()
    LogChange("farm_band", "farm: nível band " .. tostring(levelData))
	se levelData == 2 então
        LogChange("farm_target", "farm: visando Shanda")
        SetStatus("farm | targeting shanda | lv " .. tostring(LocalPlayer.Data.Level.Value))
		local foundMob = falso
		para _, v em {workspace.Enemies, ReplicatedStorage} faça
			para _, v2 em v:GetChildren() faça
				Se v2:IsA("Modelo") então
					local hrp = v2:FindFirstChild("HumanoidRootPart") ou v2.PrimaryPart
					se v2.Name == "Shanda" e hrp e v2:FindFirstChildOfClass("Humanoid") e v2:FindFirstChildOfClass("Humanoid").Health > 0 então
						foundMob = true
						repetir
							task.wait()
							local t = Tween(hrp.Position + Vector3.new(0, 20, 5), 350)
							se LocalPlayer:DistanceFromCharacter(hrp.Position) <= 160 então
								se então
									t:Cancelar()
								fim
								Tween(hrp.Position + Vector3.new(0, 20, 5), math.huge)
								se LocalPlayer:DistanceFromCharacter(hrp.Position) <= 40 então
									BringMob(v2.Name)
									EquiparArma()
									Ataque()
								fim
							fim
						até que não seja v2 ou não seja v2:FindFirstChildOfClass("Humanoid") ou v2.Humanoid.Health == 0
						pcall(function()
							v2:Destruir()
						fim)
						se não CheckMonster("Shanda") então
							quebrar
						fim
					fim
				fim
			fim
			Se foundMob então
				quebrar
			fim
		fim
		se não encontradoMob então
			local n = ReplicatedStorage.FortBuilderReplicatedSpawnPositionsFolder:FindFirstChild("Shanda")
			se n então
				Tween(n:GetPivot().Position + Vector3.new(0, 25, 0), 350).Completed:Wait()
			fim
		fim
	senão se levelData == 3 então
        LogChange("farm_target", "farm: mirando na guarda de Deus")
        SetStatus("farm | targeting god's guard | lv " .. tostring(LocalPlayer.Data.Level.Value))
		local foundMob = falso
		para _, v em {workspace.Enemies, ReplicatedStorage} faça
			para _, v2 em v:GetChildren() faça
				Se v2:IsA("Modelo") então
					local hrp = v2:FindFirstChild("HumanoidRootPart") ou v2.PrimaryPart
					se v2.Name == "Guarda de Deus" e hrp e v2:FindFirstChildOfClass("Humanoid") e v2:FindFirstChildOfClass("Humanoid").Saúde > 0 então
						foundMob = true
						repetir
							task.wait()
							local t = Tween(hrp.Position + Vector3.new(0, 25, 5), 350)
							se LocalPlayer:DistanceFromCharacter(hrp.Position) <= 160 então
								se então
									t:Cancelar()
								fim
								Tween(hrp.Position + Vector3.new(0, 25, 5), math.huge)
								se LocalPlayer:DistanceFromCharacter(hrp.Position) <= 40 então
									BringMob(v2.Name)
									EquiparArma()
									Ataque()
								fim
							fim
						até não v2
							ou não v2.Parent
							ou não v2:FindFirstChildOfClass("Humanoid")
							ou v2:FindFirstChildOfClass("Humanoid").Saúde <= 0
							ou não v2.Nome:encontrar("Guarda de Deus")
						pcall(function()
							v2:Destruir()
						fim)
						se não CheckMonster("Guarda de Deus") então
							quebrar
						fim
					fim
				fim
			fim
			Se foundMob então
				quebrar
			fim
		fim
		se não encontradoMob então
			local n = ReplicatedStorage.FortBuilderReplicatedSpawnPositionsFolder:FindFirstChild("God's Guard")
			se n então
				Tween(n:GetPivot().Position + Vector3.new(0, 25, 0), 350).Completed:Wait()
			fim
		fim
	senão se levelData == 4 ou levelData == 1 então
		se GetMonsterName() então
            LogChange("farm_target", "farm: selecionando o alvo da missão")
            SetStatus("farm | targeting quest mob | lv " .. tostring(LocalPlayer.Data.Level.Value))
			para _, v2 em {workspace.Enemies, ReplicatedStorage} faça
				para _, v em v2:GetChildren() faça
					Se v.Name == GetMonsterName() e v.PrimaryPart e v:FindFirstChildOfClass("Humanoid") e v:FindFirstChildOfClass("Humanoid").Health > 0 então
						local iniciarMatarEsteNPC = tick()
						repetir
							task.wait()
							TakeQuest()
							local t = Tween(v:GetPivot().Position + Vector3.new(0, 20, 5), 350)
							se LocalPlayer:DistanceFromCharacter(v:GetPivot().Position) <= 160 então
								se então
									t:Cancelar()
								fim
								Tween(v:GetPivot().Position + Vector3.new(0, 20, 5), math.huge)
								se LocalPlayer:DistanceFromCharacter(v:GetPivot().Position) <= 40 então
									BringMob(v.Name)
									EquiparArma()
									Ataque()
								fim
							fim
						até que não v
							ou não v.PrimaryPart
							ou não v:FindFirstChildOfClass("Humanoid")
							ou v:FindFirstChildOfClass("Humanoid").Saúde <= 0
							ou v.Name ~= GetMonsterName()
							ou (tick() - startKillThisNPC) >= 90
						pcall(function()
							v:Destroy()
						fim)
						se não CheckMonster(GetMonsterName()) então
							quebrar
						fim
					fim
				fim
			fim
		outro
            LogChange("farm_target", "farm: aguardando o mob da missão")
            SetStatus("farm | aguardando mob de missão | nível " .. tostring(LocalPlayer.Data.Level.Value))
			TakeQuest()
			para _, v em {ReplicatedStorage.FortBuilderReplicatedSpawnPositionsFolder, workspace._WorldOrigin.EnemySpawns} faça
				para _, v2 em v:GetChildren() faça
					se não v2.Name:find("Boss") e QuestController.CurrentQuest == v2.Name então
						repita task.wait()
							Tween(v2:GetPivot().Position + Vector3.new(0, 25, 0), 350)
						até GetMonsterName()
						quebrar
					fim
				fim
			fim
			task.wait(0.25)
		fim
	fim
fim

boneNPCs = {
	"Esqueleto Renascido",
	"Zumbi Vivo",
	"Alma Demoníaca",
	"Múmia Possuída",
}
função local FarmBoneLogic()
    LogChange("bone_farm", "bone: procurando por mobs")
	multidão local = nula
	local min = math.huge
	para _, v em workspace.Enemies:GetChildren() faça
		se v.PrimaryPart e v:FindFirstChildOfClass("Humanoid") e v.Humanoid.Health > 0 e table.find(boneNPCs, v.Name) então
			local dist = LocalPlayer:DistanceFromCharacter(v.PrimaryPart.Position)
			se dist < min então
				min = dist
				mob = v
			fim
		fim
	fim
	Se mob e mob.PrimaryPart e mob:FindFirstChildOfClass("Humanoid") e mob:FindFirstChildOfClass("Humanoid").Health > 0 então
        LogChange("bone_target", "bone: farming " .. string.lower(mob.Name))
        SetStatus("fazenda de ossos | " .. string.lower(mob.Name))
		TakeQuestBone(mob.Name)
		startKillThisNPC = tick()
		repetir
			task.wait()
			Tween(mob.PrimaryPart.CFrame * CFrame.new(0, 15, 5), 350)
			se LocalPlayer:DistanceFromCharacter(mob.PrimaryPart.Position) <= 210 então
				Tween(mob.PrimaryPart.CFrame * CFrame.new(0, 15, 5), math.huge)
				se LocalPlayer:DistanceFromCharacter(mob.PrimaryPart.Position) <= 30 então
					BringMob(mob.Name)
					EquiparArma()
					Ataque()
				fim
			fim
		até que não seja mob ou não seja mob.PrimaryPart ou não seja mob:FindFirstChildOfClass("Humanoid") ou mob.Humanoid.Health <= 0 ou (tick() - startKillThisNPC) >= 90
	outro
        LogChange("bone_target", "bone: aguardando spawn")
        SetStatus("fazenda de ossos | aguardando spawn")
		Tween(CFrame.new(-9515, 164, 5786), 350).Completed:Wait()
	fim
fim

função local FarmKataLogic()
    LogChange("cake_farm", "cake: verificando o príncipe")
	Viagem.Mar3()
	Se LocalPlayer:DistanceFromCharacter(Vector3.new(-2132, 70, -12319)) >= 1200 e não GetCakePrince() então
		Tween(CFrame.new(-2251, 146, -12196), 350).Completed:Wait()
	fim
	Se GetCakePrince() então
        LogChange("cake_target", "cake: príncipe do bolo de luta")
        SetStatus("fazenda de bolos | príncipe do bolo lutador")
		local Target = GetCakePrince()
		repetir
			Se LocalPlayer:DistanceFromCharacter(Target.PrimaryPart.CFrame.Position) >= 1200 e GetCakePrince() então
				StopTween()
				Tween(workspace.Map.CakeLoaf.BigMirror.Main.CFrame, 350).Completed:Wait()
				task.wait(0.75)
			fim
			Tween(Target.PrimaryPart.CFrame * CFrame.new(0, 17.5, 10), 350)
			se LocalPlayer:DistanceFromCharacter(Target.PrimaryPart.CFrame.Position) <= 200 então
				Tween(Target.PrimaryPart.CFrame * CFrame.new(0, 17.5, 10), math.huge)
				se LocalPlayer:DistanceFromCharacter(Target.PrimaryPart.CFrame.Position) <= 40 então
					EquiparArma()
					Ataque()
				fim
			fim
			task.wait()
		até que não seja Target ou não seja GetCakePrince() ou Character.Humanoid.Health <= 0
	outro
		multidão local = nula
		local min = math.huge
		para _, v em workspace.Enemies:GetChildren() faça
			se v:FindFirstChild("HumanoidRootPart") e v:FindFirstChild("Humanoid") e v.Humanoid.Health > 0 então
				local dist = LocalPlayer:DistanceFromCharacter(v.HumanoidRootPart.Position)
				se dist < min então
					min = dist
					mob = v
				fim
			fim
		fim
		se mob e table.find(MobsCakePrince, mob.Name) e mob:FindFirstChild("Humanoid") e mob:FindFirstChild("HumanoidRootPart") e mob.Humanoid.Health > 0 então
            LogChange("cake_target", "cake: farming " .. string.lower(mob.Name))
            SetStatus("fazenda de bolos | " .. string.lower(mob.Name))
			TakeQuestCake(mob.Name)
			startKillThisNPC = tick()
			repetir
				task.wait()
				Tween(mob.HumanoidRootPart.CFrame * CFrame.new(0, 15, 0), 350)
				se LocalPlayer:DistanceFromCharacter(mob.HumanoidRootPart.Position) <= 200 então
					Tween(mob.HumanoidRootPart.CFrame * CFrame.new(0, 15, 0), math.huge)
					se LocalPlayer:DistanceFromCharacter(mob.HumanoidRootPart.Position) <= 40 então
						BringMob(mob.Name)
						EquiparArma()
						Ataque()
					fim
				fim
			até que não haja multidão
				ou não mob:FindFirstChild("Humanoid")
				ou mob.Humanoid.Health <= 0
				ou não mob:FindFirstChild("HumanoidRootPart")
				ou (tick() - startKillThisNPC) >= 90
		fim
	fim
fim

função local GetCDKLogic()
    LogChange("cdk", "cdk: verificando o progresso")
	Viagem.Mar3()
	se CommF_:InvokeServer("CDKQuest", "OpenDoor") == "can" então
		CommF_:InvokeServer("CDKQuest", "OpenDoor", true)
	fim
	local q = CommF_:InvokeServer("CDKQuest", "Progresso")
    LogChange("cdk_progress", "cdk: evil " .. tostring(q.Evil) .. " / good " .. tostring(q.Good))
	se q.Evil == 0 ou q.Evil == -3 então
        LogChange("cdk_stage", "cdk: evil trial - mythological pirate")
        SetStatus("cdk | julgamento maligno - pirata mitológico")
		CommF_:InvokeServer("CDKQuest", "StartTrial", "Evil")
		Se workspace.Enemies:FindFirstChild("Pirata Mitológico") então
			repetir
				task.wait()
				Tween( workspace.Enemies:FindFirstChild("Mythological Pirate").HumanoidRootPart.CFrame * CFrame.new(5, 0, 0), 350 )
			até CommF_:InvokeServer("CDKQuest", "Progress").Evil ~= -3
				e GetMaterial("Fragmento de Alucard") == 1
		outro
			Tween(Vector3.new(-13239, 520, -6813), 350).Completed:Wait()
		fim
	senão se q.Evil == 1 ou q.Evil == -4 então
        LogChange("cdk_stage", "cdk: evil trial - haze mobs")
        SetStatus("cdk | julgamento maligno - haze mobs")
		CommF_:InvokeServer("CDKQuest", "StartTrial", "Evil")
		Se LocalPlayer:FindFirstChild("QuestHaze") então
			para _, v em LocalPlayer.QuestHaze:GetChildren() faça
				Se v.Valor > 0 então
                    LogChange("cdk_haze_target", "cdk: limpando " .. string.lower(v.Name) .. " " .. tostring(v.Value))
                    SetStatus("cdk | limpando " .. string.lower(v.Name))
					repetir
						task.wait()
						se LocalPlayer:DistanceFromCharacter( ReplicatedStorage.FortBuilderReplicatedSpawnPositionsFolder[v.Name].CFrame.Position ) >= 600 então
							Tween(ReplicatedStorage.FortBuilderReplicatedSpawnPositionsFolder[v.Name].CFrame, 350).Completed:Wait()
						fim
						para _, v2 em workspace.Enemies:GetChildren() faça
							se v2.Name == v.Name e v2:FindFirstChild("HumanoidRootPart") e v2:FindFirstChild("Humanoid") e v2.Humanoid.Health > 0 então
								repetir
									task.wait()
									Tween(v2.HumanoidRootPart.Position + Vector3.new(0, 25, 10), 350)
									se LocalPlayer:DistanceFromCharacter(v2.HumanoidRootPart.Position) <= 40 então
										BringMob(v2.Name)
										EquiparArma()
										Ataque()
									fim
								até que não seja v2.Humanoid ou não seja v2.HumanoidRootPart ou v2.Humanoid.Health <= 0 ou não seja v2
								quebrar
							fim
						fim
					até que não v ou v.Value == 0 ou não LocalPlayer:FindFirstChild("QuestHaze")
				fim
			fim
		fim
	senão se q.Evil == 2 ou q.Evil == -5 então
        LogChange("cdk_stage", "cdk: evil trial - hell dimension")
        SetStatus("cdk | julgamento maligno - dimensão infernal")
		CommF_:InvokeServer("CDKQuest", "StartTrial", "Evil")
		Se workspace.Enemies:FindFirstChild("Soul Reaper") ou workspace.Map:FindFirstChild("HellDimension") então
			Se workspace.Enemies:FindFirstChild("Soul Reaper") e não workspace.Map:FindFirstChild("HellDimension") então
				repetir
					Tween(workspace.Enemies:FindFirstChild("Soul Reaper").HumanoidRootPart.CFrame, 350).Completed:Wait()
					task.wait()
				até workspace.Map:FindFirstChild("HellDimension")
			fim
			se tw então
				tw:Cancelar()
			fim
			task.wait(1)
			para i = 1, 3 faça
                LogChange("cdk_torch", "cdk: acendendo a tocha do inferno " .. tostring(i) .. "/3")
                SetStatus("cdk | acendendo tocha do inferno " .. tostring(i) .. "/3")
				Tween(workspace.Map.HellDimension["Torch" .. i].CFrame, 350).Completed:Wait()
				fireproximityprompt(workspace.Map.HellDimension["Torch" .. i].ProximityPrompt)
				repetir
					task.wait()
				até que #workspace.Enemies:GetChildren() > 0
				para _, v em workspace.Enemies:GetChildren() faça
					se LocalPlayer:DistanceFromCharacter(v.HumanoidRootPart.CFrame.Position) <= 300 e v.HumanoidRootPart e v.Humanoid e v.Humanoid.Health > 0 então
						repetir
							task.wait()
							Tween(v.HumanoidRootPart.CFrame * CFrame.new(0, 25, 10), 350)
							se LocalPlayer:DistanceFromCharacter(v.HumanoidRootPart.Position) <= 210 então
								Tween(v.HumanoidRootPart.CFrame * CFrame.new(0, 25, 10), math.huge)
								se LocalPlayer:DistanceFromCharacter(v.HumanoidRootPart.Position) <= 40 então
									BringMob(v.Name)
									EquiparArma()
									Ataque()
								fim
							fim
						até que não v
							ou não v:FindFirstChild("Humanoid")
							ou v.Humanoid.Saúde <= 0
							ou não v:FindFirstChild("HumanoidRootPart")
					fim
				fim
			fim
		outro
			NPCs ósseos locais = {
				"Esqueleto Renascido",
				"Zumbi Vivo",
				"Alma Demoníaca",
				"Múmia Possuída",
			}
			repetir
				task.wait()
				multidão local = nula
				local min = math.huge
				para _, v em workspace.Enemies:GetChildren() faça
					se v:FindFirstChild("HumanoidRootPart") e v:FindFirstChild("Humanoid") e v.Humanoid.Health > 0 e table.find(boneNPCs, v.Name) então
						local dist = LocalPlayer:DistanceFromCharacter(v.HumanoidRootPart.Position)
						se dist < min então
							min = dist
							mob = v
						fim
					fim
				fim
				se mob e mob.HumanoidRootPart e mob.Humanoid e mob.Humanoid.Health > 0 então
					repetir
						task.wait()
						Tween(mob.HumanoidRootPart.CFrame * CFrame.new(0, 15, 5), 350)
						se LocalPlayer:DistanceFromCharacter(mob.HumanoidRootPart.Position) <= 210 então
							Tween(mob.HumanoidRootPart.CFrame * CFrame.new(0, 15, 5), math.huge)
							se LocalPlayer:DistanceFromCharacter(mob.HumanoidRootPart.Position) <= 30 então
								BringMob(mob.Name)
								EquiparArma()
								Ataque()
							fim
						fim
					até que não haja multidão
						ou não mob:FindFirstChild("Humanoid")
						ou mob.Humanoid.Health <= 0
						ou não mob:FindFirstChild("HumanoidRootPart")
				outro
					Tween(CFrame.new(-9515, 164, 5786), 350).Completed:Wait()
				fim
			até LocalPlayer.Backpack:FindFirstChild("Hallow Essence") ou CheckMonster("Soul Reaper")
			se LocalPlayer.Backpack:FindFirstChild("Hallow Essence") então
				EquipTool("Essência Sagrada")
				Se Character:FindFirstChild("Hallow Essence") então
					firetouchinterest(Character.HumanoidRootPart, workspace.Map["Haunted Castle"].Summoner.Detection, 1)
					firetouchinterest(Character.HumanoidRootPart, workspace.Map["Haunted Castle"].Summoner.Detection, 0)
				fim
			fim
		fim
	senão se q.Good == 0 ou q.Good == -3 então
        LogChange("cdk_stage", "cdk: teste bem-sucedido - encontrando barcos")
        SetStatus("cdk | bom teste - encontrando barcos")
		CommF_:InvokeServer("CDKQuest", "StartTrial", "Good")
		para _, x em {CFrame.new(-9550, 21, 4690), CFrame.new(-6120, 16, -2250), CFrame.new(-9530, 7, -8370)} faça
			repetir
				LocalPlayer.Character.HumanoidRootPart.CFrame = x
				task.wait()
			até workspace.NPCs:FindFirstChildWhichIsA("Model")
				e workspace.NPCs:FindFirstChildWhichIsA("Model").Name:find("Boat")
			para _, n em workspace.NPCs:GetChildren() faça
				Se n.Name:find("Barco") então
					CommF_:InvokeServer("CDKQuest", "BoatQuest", n)
				fim
			fim
		fim
	senão se q.Good == 1 ou q.Good == -4 então
        LogChange("cdk_stage", "cdk: teste bem-sucedido - atingindo o objetivo")
        SetStatus("cdk | bom teste - atingindo o objetivo")
		CommF_:InvokeServer("CDKQuest", "StartTrial", "Good")
		Tween(CFrame.new(6148.4116210938, 294.38687133789, -6741.1166992188), 350)
	senão se q.Good == 2 ou q.Good == -5 então
        LogChange("cdk_stage", "cdk: bom teste - dimensão celestial")
        SetStatus("cdk | bom teste - dimensão celestial")
		CommF_:InvokeServer("CDKQuest", "StartTrial", "Good")
		Se workspace.Map:FindFirstChild("HeavenlyDimension") ~= nil e workspace.Map:FindFirstChild("Heavenly Dimension") então
			Se workspace.Map.HeavenlyDimension.Exit.Color ~= Color3.fromRGB(0, 0, 0) então
				firetouchinterest( workspace.Map.HeavenlyDimension.Exit.Touchinterest, LocalPlayer.Character.HumanoidRootPart, 0 )
			outro
				para i = 1, 3 faça
                    LogChange("cdk_heaven_torch", "cdk: acendendo a tocha do céu " .. tostring(i) .. "/3")
                    SetStatus("cdk | acendendo a tocha do céu " .. tostring(i) .. "/3")
					repetir
						task.wait()
						local e = workspace.Enemies:FindFirstChildWhichIsA("Model", true)
						se e e (e.Name:find("Cursed Skeleton") ou e.Name:find("Heaven")) então
							KillMonster(e.Name)
						outro
							Se workspace.Map.HeavenlyDimension["Torch" .. i].ProximityPrompt.Enabled então
								Tween(workspace.Map.HeavenlyDimension["Torch" .. i].CFrame, 350).Completed:Wait()
								fireproximityprompt(workspace.Map.HeavenlyDimension["Torch" .. i].ProximityPrompt)
							fim
						fim
					até que não workspace.Map.HeavenlyDimension["Torch" .. i].ProximityPrompt.Enabled
						ou workspace.Map.HeavenlyDimension.Exit.Color ~= Color3.fromRGB(0, 0, 0)
						ou Character.Humanoid.Health <= 0
					StopTween()
				fim
			fim
		outro
			se CheckMonster("Rainha dos Bolos") então
				KillMonster ("Rainha do Bolo")
				StopTween()
			outro
				HopLowServer()
			fim
		fim
	fim
fim

CheckedMelee = false
MeleeData = {

	BlackLeg = 0,
	Electro = 0,
	FishmanKarate = 0,

	SharkmanKarate = 0,
	PassoDeMorte = 0,
	GarraDeDragão = 0,
	Superhumano = 0,

	GarraElétrica = 0,
	DragonTalon = 0,
	DeusHumano = 0,
	SanguineArt = 0,
}

Preços de Combate Corpo a Corpo = {
	BlackLeg = {
		Preço = {
			Beli = 150000,
		},
		Id = "BlackLeg",
		NextLevelRequirement = 400,
	},
	Electro = {
		Preço = {
			Beli = 500000,
		},
		Id = "Electro",
		NextLevelRequirement = 400,
	},
	FishmanKarate = {
		Preço = {
			Beli = 750000,
		},
		NextLevelRequirement = 400,
	},
	Garra de Dragão = {
		Preço = {
			Fragmentos = 1500,
		},
		NextLevelRequirement = 400,
	},
	Superhumano = {
		Preço = {
			Beli = 3000000,
		},
		NextLevelRequirement = 400,
	},
	Passo da Morte = {
		Preço = {
			Beli = 2500000,
			Fragmentos = 5000,
		},
		NextLevelRequirement = 400,
	},
	SharkmanKarate = {
		Preço = {
			Beli = 2500000,
			Fragmentos = 5000,
		},
		NextLevelRequirement = 400,
	},
	GarraElétrica = {
		Preço = {
			Beli = 2500000,
			Fragmentos = 5000,
		},
		NextLevelRequirement = 400,
	},
	DragonTalon = {
		Preço = {
			Beli = 2500000,
			Fragmentos = 5000,
		},
		NextLevelRequirement = 400,
	},
	Deushumano = {
		Preço = {
			Beli = 5000000,
			Fragmentos = 5000,
		},
		NextLevelRequirement = 350,
	},
}

Nome2Nome = {
	["Água Kung Fu"] = "FishmanKarate",
	["Elétrico"] = "Eletro",
	["Passo Sombrio"] = "Perna Preta",
	["Sopro do Dragão"] = "Garra de Dragão",
}
local lastMeleeDataCheck = 0
função local CheckMeleeMastery()
	local agora = tick()
	se (agora - últimaVerificaçãoDeDadosCombateCorpo) < 10 então
		retornar
	fim
	lastMeleeDataCheck = agora

	se não estiver marcado como corpo a corpo, então
        LogChange("melee_data", "melee: verificando dados")
	fim

	sucesso local = pcall(função()
		dados locais = IRS:GetItems(KEYS.MASTERY)
		para _, v em dados faça
			local correspondente = CORRESPONDÊNCIA(v.ItemId)
			item local = correspondido e correspondido._ok
			se item e item.Index e item.Display então
				local n = item.Index.StorageKey
				se item.Display.Category == "Estilo de Luta" e n então
					chave local = Name2Name[n] ou n:gsub(" ", "")
					se MeleeData[key] ~= nil então
						MeleeData[key] = tonumber(v.Value) ou MeleeData[key]
					fim
				fim
			fim
		fim
	fim)

	Se tiver sucesso, então
		CheckedMelee = true
        LogChange("melee_data", "melee: dados carregados")
    outro
        LogChange("melee_data", "melee: falha ao carregar dados")
	fim
fim

Local CheckCanDungeon, AutoDungeon

local GetMeleeLogics = {
	SharkmanKarate = função()
		se não World.Sea2() então
			enquanto verdadeiro faça
				CommF_:InvokeServer("TravelDressrosa")
				task.wait(10)
			fim
		fim
		local check = CommF_:InvokeServer("BuySharkmanKarate", true)
		se (não HasItem("Chave de Água")) e check ~= 0 e check ~= 1 e typeof(check) == "string" então
			se CheckMonster("Guardião das Marés") então
				KillMonster("Guardião das Marés")
			outro
				HopLowServer()
				task.wait(5)
			fim
		outro
			TweenToMeleeBuyer("SharkmanKarate")
			CommF_:InvokeServer("BuySharkmanKarate", true)
			CommF_:InvokeServer("BuySharkmanKarate")
		fim
	fim,
	PassoDeMorte = função()
		se não World.Sea2() então
			enquanto verdadeiro faça
				CommF_:InvokeServer("TravelDressrosa")
				task.wait(10)
			fim
		fim
		local check = CommF_:InvokeServer("OpenLibrary")
		se não HasItem("Chave da Biblioteca") e não verificar então
			se CheckMonster("Almirante de Gelo Desperto") então
				KillMonster("Almirante de Gelo Desperto")
			outro
				HopLowServer()
				task.wait(5)
			fim
		senão se HasItem("Chave da Biblioteca") então
			CommF_:InvokeServer("OpenLibrary")
		senão se check == verdadeiro então
			TweenToMeleeBuyer("DeathStep")
			CommF_:InvokeServer("BuyDeathStep")
		fim
	fim,
	GarraElétrica = função()
		Viagem.Mar3()
		TweenToMeleeBuyer("ElectricClaw")
		CommF_:InvokeServer("BuyElectricClaw", "Start")
		Tween(CFrame.new(-12550, 336, -7510), 350).Completed:Wait()
		task.wait(0.5)
		TweenToMeleeBuyer("ElectricClaw")
		CommF_:InvokeServer("BuyElectricClaw")
	fim,
	DragonTalon = função()
		Viagem.Mar3()
		repetir
			task.wait()
			FarmBoneLogic()
			se GetMaterial("Bones") >= 50 então
				CommF_:InvokeServer("Ossos", "Comprar", 1, 1)
			fim
		até HasItem("Essência de Fogo")
		TweenToMeleeBuyer("DragonTalon")
		CommF_:InvokeServer("BuyDragonTalon", true)
		CommF_:InvokeServer("BuyDragonTalon")
	fim,
}

função local CheckYama()
	retornar CommF_:InvokeServer("EliteHunter", "Progress") ou 0
fim

função local MeleeKaitunLogic()
    LogChange("melee_logic", "melee: verificando estilos")
	Se for um caractere, então
		beli local = LocalPlayer.Data.Beli.Value
		se não CollectionService:HasTag(Character, "Buso") e (beli >= 25000) então
			CommF_:InvokeServer("BuyHaki", "Buso")
		fim
		se não CollectionService:HasTag(Character, "Geppo") e (beli >= 10000) então
			CommF_:InvokeServer("BuyHaki", "Geppo")
		fim
		se não CollectionService:HasTag(Character, "Soru") e (beli >= 100000) então
			CommF_:InvokeServer("BuyHaki", "Soru")
		fim
	outro
		retornar
	fim
	se não estiver marcado como corpo a corpo, então
		CheckMeleeMastery()
	fim
	para _, v em {"BlackLeg", "Electro", "FishmanKarate"} faça
		local nome real = v:gsub("(%l)(%u)", "%1 %2")

			se (MeleeData[v] ou 0) < 400 então
            LogChange("melee_progress", "melee: " .. string.lower(realName) .. " " .. tostring(MeleeData[v] or 0) .. "/400")

			se (MeleeData[v] ou 0) == 0 e LocalPlayer.Data.Beli.Value < MeleePrices[v].Price.Beli então
                LogChange("melee_wait", "melee: precisa de beli para " .. string.lower(realName))

				retornar
			fim
			se não HasItem(realName) então
                LogChange("melee_action", "melee: recebendo " .. string.lower(realName))
				se não TweenToMeleeBuyer(v) então
					retornar
				fim
				task.wait(0.5)
				CommF_:InvokeServer("Comprar" .. v)
			fim
			retornar
		fim
	fim

	se não World.Sea1() então
		para _, v em {"DragonClaw", "Superhuman", "DeathStep", "SharkmanKarate"} faça
			local nome real = v:gsub("(%l)(%u)", "%1 %2")
			se (MeleeData[v] ou 0) < 400 então
				local temEcoSuficiente = (
					(MeleePrices[v].Price.Beli == nil) ou (LocalPlayer.Data.Beli.Value >= MeleePrices[v].Price.Beli)
				)
					e (
						(MeleePrices[v].Price.Fragments == nil)
						ou (LocalPlayer.Data.Fragments.Value >= MeleePrices[v].Price.Fragments)
					)
				se (MeleeData[v] ou 0) == 0 e hasEnoughEco então
                    LogChange("melee_action", "melee: destravando " .. string.lower(realName))
					se v ~= "Garra de Dragão" e v ~= "Super-humano" então
						GetMeleeLogics[v]()
						retornar verdadeiro
					outro
						TweenToMeleeBuyer(v)
						se v == "Superhumano" então
							CommF_:InvokeServer("Comprar" .. v)
						outro
							CommF_:InvokeServer("BlackbeardReward", "DragonClaw", "2")
						fim
					fim
				senão se (MeleeData[v] ou 0) == 0 e (MeleePrices[v].Price.Fragments e (LocalPlayer.Data.Fragments.Value < MeleePrices[v].Price.Fragments)) e CheckCanDungeon() então
                    LogChange("melee_action", "melee: coletando fragmentos")
					se (não World.Sea1()) e LocalPlayer.Data.Level.Value >= 1250 então
						AutoDungeon()
					fim
				senão se (MeleeData[v] ou 0) == 0 e não hasEnoughEco então
                    LogChange("melee_wait", "melee: aguardando " .. string.lower(realName) .. " recursos")
				senão se (MeleeData[v] ou 0) > 0 e não HasItem(realName) então
					se não TweenToMeleeBuyer(v) então
						quebrar
					fim
					se v == "Garra de Dragão" então
						CommF_:InvokeServer("BlackbeardReward", "DragonClaw", "2")
					outro
						CommF_:InvokeServer("Comprar" .. v)
					fim
				fim
				retornar
			fim
		fim
		para _, v em {"ElectricClaw", "DragonTalon", "Godhuman"} faça
			local nome real = v:gsub("(%l)(%u)", "%1 %2")
			se (MeleeData[v] ou 0) < 400 então
				local temEcoSuficiente = (
					(MeleePrices[v].Price.Beli == nil) ou (LocalPlayer.Data.Beli.Value >= MeleePrices[v].Price.Beli)
				)
					e (
						(MeleePrices[v].Price.Fragments == nil)
						ou (LocalPlayer.Data.Fragments.Value >= MeleePrices[v].Price.Fragments)
					)
				se (MeleeData[v] ou 0) == 0 e hasEnoughEco então
                    LogChange("melee_action", "melee: destravando " .. string.lower(realName))
					se v == "Deushumano" então
						print("deushumano...")
						GetGodHuman()
					outro
						GetMeleeLogics[v]()
					fim
					retornar verdadeiro
				senão se (MeleeData[v] ou 0) == 0 e (MeleePrices[v].Price.Fragments e (LocalPlayer.Data.Fragments.Value < MeleePrices[v].Price.Fragments)) e CheckCanDungeon() então
					se (não World.Sea1()) e LocalPlayer.Data.Level.Value >= 1250 então
						AutoDungeon()
					fim
				senão se (MeleeData[v] ou 0) == 0 e não hasEnoughEco então
                    LogChange("melee_wait", "melee: aguardando " .. string.lower(realName) .. " recursos")
				senão se (MeleeData[v] ou 0) > 0 e não Character:FindFirstChild(realName) e não LocalPlayer.Backpack:FindFirstChild(realName) então
					se não TweenToMeleeBuyer(v) então
						quebrar
					fim
					CommF_:InvokeServer("Comprar" .. v)
				fim
				retornar
			fim
		fim
	fim
fim

KEYS = require(ReplicatedStorage.ItemReplicationService.KEYS)
IRS = require(ReplicatedStorage.ItemReplicationService)
MATCH = require(ReplicatedStorage.ItemConfig.Storage).match
função local GetUseableFruit()
	dados locais = IRS:GetItems(KEYS.QUANTITY)
	para _, v em dados faça
		item local = CORRESPONDÊNCIA(v.ItemId)._ok
		Se item.Display.Category == "Blox Fruit" e item.Quality.MoneyPrice < 1000000 então
			return item.Index.StorageKey
		fim
	fim
fim

CheckCanDungeon = função()
	local res = CommF_:InvokeServer("RaidsNpc", "Select", "Dark")
	se HasItem("Microchip Especial") então
		retornar verdadeiro
	elseif GetUseableFruit() então
		retornar verdadeiro
	senão se LocalPlayer.Data.Beli.Value >= 100000 e ((type(res) == "number" and res == 1) or (type(res) == "string" and not res:find("wait"))) então
		retornar verdadeiro
	fim
fim

local RaidController = {
	isRaiding = false,
}
RaidController.RaidState_Connection = nil
se World.Sea2() ou World.Sea3() então
	Se ReplicatedStorage.Remotes:FindFirstChild("Raids") então
		enquanto não RaidController.RaidState_Connection faça
			RaidController.RaidState_Connection = ReplicatedStorage.Remotes.Raids.OnClientEvent:Connect(function(a)
				se a == "StartTimer" então
					RaidController.isRaiding = true
				outro
					RaidController.isRaiding = false
				fim
			fim)
			task.wait(0.5)
		fim
	fim
fim

função local KillAura(distância)
	para _, v em workspace.Enemies:GetChildren() faça
		se v:FindFirstChild("HumanoidRootPart") e v:FindFirstChildOfClass("Humanoid") então
			se LocalPlayer:DistanceFromCharacter(v.PrimaryPart.Position) <= distance então
				v.Humanoid:ChangeState(15)
			fim
		fim
	fim
fim

AutoDungeon = função()
    LogChange("raid", "raid: preparando")
	local res = CommF_:InvokeServer("RaidsNpc", "Select", "Dark")
	enquanto não for um personagem ou não for um personagem.PrimaryPart, faça
		task.wait()
	fim
	se não RaidController.isRaiding então
		se não (LocalPlayer.Backpack:FindFirstChild("Special Microchip") ou Character:FindFirstChild("Special Microchip")) então
            LogChange("raid_stage", "raid: obtendo microchip")
            SetStatus("raid | obtendo microchip")
			se LocalPlayer.Data.Beli.Value >= 100000 e ((type(res) == "number" e res == 1) ou (type(res) == "string" e não res:find("wait"))) então
				CommF_:InvokeServer("RaidsNpc", "Select", "Dark")
			outro
				PAUSE_AUTO_STORE = true
				CommF_:InvokeServer("LoadFruit", GetUseableFruit())
				task.wait(0.5)
				CommF_:InvokeServer("RaidsNpc", "Select", "Dark")
				PAUSE_AUTO_STORE = false
			fim
		fim
		Se World.Sea2() então
            LogChange("raid_stage", "raid: invocando no mar 2")
            SetStatus("raid | convocação no mar 2")
			local clickDetector = workspace.Map.CircleIsland.RaidSummon2:FindFirstChildWhichIsA("ClickDetector", true)
			Se clickDetector então
				pcall(fireclickdetector, clickDetector)
			fim
		elseif World.Sea3() então
            LogChange("raid_stage", "raid: invocando no mar 3")
            SetStatus("raid | convocação no mar 3")
			Tween(Vector3.new(-5018, 315, -2828), 300).Completed:Wait()
			task.wait(1)
			local clickDetector = workspace.Map["Boat Castle"].RaidSummon2:FindFirstChildWhichIsA("ClickDetector", true)
			Se clickDetector então
				pcall(fireclickdetector, clickDetector)
			fim
		fim
		local startWait = tick()
        LogChange("raid_state", "raid: aguardando para entrar")
        SetStatus("raid | aguardando para entrar")
		repetir
			SetStatus("raid | aguardando para entrar")
			StopTween()
			task.wait(0.25)
		até que RaidController.isRaiding ou (tick() - startWait) >= 30
        LogChange("raid_state", RaidController.isRaiding and "raid: started" or "raid: join timed out")
        SetStatus(RaidController.isRaiding e "raid | started" ou "raid | join timed out")
		task.wait(0.5)
		enquanto RaidController.isRaiding do
            LogChange("raid_state", "raid: limpando a ilha")
			SetStatus("raid | limpando ilha")
			para _, v em workspace.Enemies:GetChildren() faça
				Se v.PrimaryPart e v:FindFirstChildOfClass("Humanoid") e v:FindFirstChildOfClass("Humanoid").Health > 0 e LocalPlayer:DistanceFromCharacter(v.PrimaryPart.Position) <= 2000 então
					repetir
						task.wait()
						pcall(sethiddenproperty, LocalPlayer, "SimulationRadius", math.huge)
						pcall(function()
							Tween(v.PrimaryPart.Position + Vector3.new(0, 28, 15), 350)
							se LocalPlayer:DistanceFromCharacter(v.PrimaryPart.Position) <= 200 então
								Tween(v.PrimaryPart.Position + Vector3.new(0, 28, 15), math.huge)
								se LocalPlayer:DistanceFromCharacter(v.PrimaryPart.Position) <= 50 então
									EquiparArma()
									Ataque()
								fim
							fim
						fim)
					até que não v
						ou não v.PrimaryPart
						ou não v:FindFirstChildOfClass("Humanoid")
						ou v:FindFirstChildOfClass("Humanoid").Saúde == 0
					quebrar
				fim
			fim
			local encontrado = falso
			para i = 5, 1, -1 faça
				para _, z em workspace["_WorldOrigin"].Locations:GetChildren() faça
					se z.Name == ("Island " .. i) e LocalPlayer:DistanceFromCharacter( workspace["_WorldOrigin"].Locations["Island " .. i]:GetPivot().Position ) <= 6000 então
                        LogChange("raid_island", "raid: movendo para a ilha " .. tostring(i))
                        SetStatus("raid | movendo para a ilha " .. tostring(i))
						encontrado = verdadeiro
						Tween( workspace["_WorldOrigin"].Locations["Island " .. i]:GetPivot() * CFrame.new(100, 70, 100), 350 )
						task.wait(0.5)
						quebrar
					fim
				fim
				se encontrado então
					quebrar
				fim
			fim
			task.wait()
		fim
	fim
	 task.wait(5)
    LogChange("raid_state", "raid: ciclo encerrado")
fim

função local GetYama()
    LogChange("yama", "yama: desbloqueando")
    SetStatus("yama | desbloqueando")
	repetir
		fireclickdetector(workspace.Map.Waterfall.SealedKatana.Hitbox.ClickDetector)
		task.wait(0.5)
	até LocalPlayer.Backpack:FindFirstChild("Yama")
fim

função local CheckTushita()
	retornar LocalPlayer.Data.Level.Value >= 2200
fim

função local GetTushita()
    LogChange("tushita", "tushita: verificando o progresso")
	se não workspace.Map.Turtle:FindFirstChild("TushitaGate") então
        LogChange("tushita_stage", "tushita: derrotando longma")
        SetStatus("tushita | derrotando longma")
		Se CheckMonster("Longma") então
			KillMonster("Longma")
		fim
	elseif (function() para _, v em {workspace.Enemies, ReplicatedStorage} faça para _, v2 em v:GetChildren() faça se v2:IsA("Model") e v2:FindFirstChild("Humanoid") e v2.Humanoid.Health > 0 e v2.Name:find("indra") então
						retornar verdadeiro
					fim
				fim
			fim
		fim)()
	então
		se não Character:FindFirstChild("Holy Torch") e não LocalPlayer.Backpack:FindFirstChild("Holy Torch") então
            LogChange("tushita_stage", "tushita: coletando tocha sagrada")
            SetStatus("tushita | coletando tocha sagrada")
			Tween(workspace.Map.Waterfall.SecretRoom.Room.Door.Door.Hitbox.CFrame, 350).Completed:Wait()
			task.wait(1)
		outro
			para i = 1, 5, 1 faça
                LogChange("tushita_torch", "tushita: acendendo tocha " .. tostring(i) .. "/5")
                SetStatus("tushita | tocha acesa " .. tostring(i) .. "/5")
				EquipTool("Tocha Sagrada")
				Tween(workspace.Map.Turtle.QuestTorches["Torch" .. tostring(i)].Particles.Main.Enabled, 350).Completed:Wait()
			fim
		fim
	fim
fim

local c = 0
função local AutoChest()
	local m = workspace.Map:GetDescendants()
	local h = Character:FindFirstChild("Humanoid") ou falso
	para i = 1, #m faça
		local v = m[i]
		se v:IsA("BasePart") e v.Name:find("Chest") e (v.Name:match("%d+") ou v.Name:find("Frag")) e v.CanTouch então
			se (function() para _, v em {Character, LocalPlayer.Backpack} faça para _, v2 em v:GetChildren() faça se v2.Name == "Cálice de Deus" ou v2.Name == "Punho das Trevas" ou v2.Name == "Doce Cálice" então
								retornar verdadeiro
							fim
						fim
					fim
					retornar falso
				fim)()
			então
				quebrar
			fim
			se c >= 10 então
				c = 0
				h.Saúde = 0
			outro
				Se Character.PrimaryPart e Character:FindFirstChildOfClass("Humanoid") então
					Se Character.Humanoid.Health > 0 então
						Caractere:SetPrimaryPartCFrame(v.CFrame * CFrame.new(0, 2, 0))
						c = c + 1
					fim
				fim
			fim
			task.wait(0.2)
		fim
	fim
fim

função local CheckHakiLegendInTitles(NAME_HAKI)
	local res, kl = pcall(function()
		CommF_:InvokeServer("getTitles")
	fim)
	se não res ou não kl ou typeof(kl) ~= "table" então
		retornar falso
	fim
	local gg = kl[NAME_HAKI]
	se não gg então
		retornar falso
	fim
	para _, v em kl faça
		se v.Unlocked e v.Description e v.Description:find(gg) então
			retornar verdadeiro
		fim
	fim
	retornar falso
fim

função local GetTitle(título)
	local a = CommF_:InvokeServer("getTitles") ou {}
	para _, v em um fazer
		se v.InternalName == title e v.Name ~= "(LOCKED)" então
			retornar verdadeiro
		fim
	fim
	retornar falso
fim

CISNE_MORTO = falso
se GetTitle("Demônio Celestial") então
	CISNE_MORTO = verdadeiro
fim
função local GetSwanKilled()
	retornar SWAN_KILLED
fim

task.spawn(function()
	para _, v em {"EASTEREXP", "BANEXPLOIT", "NOMOREHACKS", "WildDares", "BossBuild", "GetPranked", "EARN_FRUITS", "Sub2UncleKizaru", "FIGHT4FRUIT", "kittgaming", "TRIPLEABUSE", "Sub2CaptainMaui", "Sub2Fer999", "Enyu_is_Pro", "Magicbus", "JCWK", "Starcodeheo", "Bluxxy", "SUB2GAMERROBOT_EXP1", "Sub2NoobMaster123", "Sub2Daigrock", "Axiore", "TantaiGaming", "StrawHatMaine", "Sub2OfficialNoobie", "TheGreatAce", "SEATROLLIN", "24NOADMIN", "ADMIN_TROLL", "NEWTROLL", "SECRET_ADMIN", "staffbattle", "NOEXPLOIT", "NOOB2ADMIN", "CODESLIDE", "fruitconcepts"} do
		ReplicatedStorage.Remotes.Redeem:InvokeServer(v)
	fim
fim)

task.spawn(function()
	while task.wait() do
		Se LocalPlayer.Data.Points.Value > 0 então
			corpo a corpo local = LocalPlayer.Data.Stats.Melee.Level.Value
			Defesa local = JogadorLocal.Dados.Estatísticas.NívelDeFenção.Valor
			Espada local = JogadorLocal.Dados.Estatísticas.Espada.Nível.Valor
			CommF_:InvokeServer("AddPoint","Melee",LocalPlayer.Data.Level.Value * 2 - Corpo a corpo)
			CommF_:InvokeServer("AddPoint","Defense",LocalPlayer.Data.Level.Value - Defense)
			Se Melee == MaxLevel e Defense == MaxLevel, então
				CommF_:InvokeServer("AddPoint","Sword",LocalPlayer.Data.Level.Value * 3 - MaxLevel * 2)
			fim
		fim
	fim
fim)

CAN_SPAWN_CAKE_PRINCE = true
task.spawn(function()
	enquanto World.Sea3() fizer
		se CAN_SPAWN_CAKE_PRINCE então
			CommF_:InvokeServer("CakePrinceSpawner")
		fim
		se GetMaterial("Bones") >= 50 então
			CommF_:InvokeServer("Ossos", "Comprar", 1, 1)
		fim
		task.wait(0.5)
	fim
fim)

print("inicializado")
task.spawn(function()
    print("loop principal iniciado")
	while task.wait() do
		se MeleeKaitunLogic() então
			continuar
		fim
		se Config.CursedDualKatana e (function() local b = 0 para i em {"Tushita", "Yama"} faça se CheckItem(i) então
						LoadItem(i, "Espada")
						para _, v em {LocalPlayer.Backpack, Character} faça
							se v.Name == i e v.ToolTip == "Espada" e v.Level.Value >= 350 então
								b = b + 1
							fim
						fim
					fim
				fim
				retornar b
			end)() == 2
			e LocalPlayer.Data.Level.Value >= 2200
		então
            LogChange("main_mode", "mode: cursed dual katana")
			GetCDKLogic()
		senão se Config.Saber e LocalPlayer.Data.Level.Value >= 200 e não CheckItem("Saber") então
            LogChange("main_mode", "mode: saber")
			GetSaber()
		senão se World.Sea3() e ( (HasItem("Sweet Chalice") ou HasItem("Sweet Chalice")) ou (function() para _, v em {workspace.Enemies, ReplicatedStorage} faça para _, v2 em v:GetChildren() faça se v2.Name:find("indra") ou v2.Name:find("Dough King") então
								retornar verdadeiro
							fim
						fim
					fim
					retornar falso
				fim)()
			)
		então
            LogChange("main_mode", "mode: farm indra / dough king")
			se (function() para _, v em {workspace.Enemies, ReplicatedStorage} faça para _, v2 em v:GetChildren() faça se v2.Name:find("indra") ou v2.Name:find("Dough King") então
								retornar verdadeiro
							fim
						fim
					fim
					retornar falso
				fim)()
			então
				para _, v em {workspace.Enemies, ReplicatedStorage} faça
					para _, v2 em v:GetChildren() faça
						se (v2.Name:find("indra") ou v2.Name:find("Dough King")) e v2:FindFirstChildOfClass("Humanoid") então
							retornar KillMonster(v2.Name)
						fim
					fim
				fim
			senão se HasItem("Cálice Doce") então
				local msg = CommF_:InvokeServer("CakePrinceSpawner")
				se type(msg) == "string" e msg:find("derrota") então
					pcall(FarmKataLogic)
				fim
			senão se HasItem("Cálice de Deus") então
				se (function() local b = 0 pcall(function() para _, v em workspace.Map["Boat Castle"].Summoner:GetChildren() faça se v:IsA("Part") e v.Name == "Part" então
									se v.Color ~= Color3.fromRGB(99, 95, 98) então
										b = b + 1
									fim
								fim
							fim
						fim)
						retornar b
					end)() == 3
				então
					se não Character:FindFirstChild("Cálice de Deus") então
                        LogChange("chalice_stage", "chalice: invocando chefe")
                        SetStatus("cálice | invocando chefe")
						EquipTool("Cálice de Deus")
					fim
					pcall(firetouchinterest, workspace.Map["Boat Castle"].Summoner.Detection, Character.PrimaryPart)
				outro
					se GetMaterial("Conjured Cocoa") >= 10 então
						CommF_:InvokeServer("SweetChaliceNpc")
					outro
						pcall(FarmMaterial, "Cacau Conjured")
					fim
				fim
			fim
		senão se LocalPlayer.Data.Level.Value >= 850 e não CheckItem("Capacete de Guerreiro") então
            LogChange("main_mode", "modo: bartilo quest")
			FaçaBartiloQuest()
		senão se Config.RaceV3 e LocalPlayer.Data.Level.Value >= 850 e LocalPlayer.Data.Beli.Value >= 500000 e GetRaceGrade() == "V1" e CheckItem("Capacete de Guerreiro") então
            LogChange("main_mode", "mode: race v2")
			DoRaceV2()
		senão se Config.RaceV3 e (LocalPlayer.Data.Level.Value >= 1000) e (LocalPlayer.Data.Beli.Value >= 2000000) e GetRaceGrade() == "V2" e GetSwanKilled() então
            LogChange("main_mode", "mode: race v3")
			DoRaceV3()
		senão se Config.CursedDualKatana e World.Sea3() e GetElite() então
            LogChange("main_mode", "mode: farm elite")
			pcall(function()
				para _, v2 em {workspace.Enemies, ReplicatedStorage} faça
					para _, v em v2:GetChildren() faça
						se v:IsA("Model") e v:GetAttribute("Level") e v:FindFirstChild("Humanoid") e v.Humanoid.Health > 0 então
							se table.find({"Tyrant of the Skies", "Diablo", "Urban", "Deandre"}, v.Name) então
								Se QuestController.CurrentQuest:find(v.Name) então
									retornar KillMonster(v.Name)
								outro
									CommF_:InvokeServer("EliteHunter")
								fim
							fim
						fim
					fim
				fim
			fim)
		senão se Config.SkullGuitar e LocalPlayer.Data.Level.Value >= 2300 então
            LogChange("main_mode", "modo: guitarra caveira")
			GetSG()
		senão se World.Sea3() e GetCastleRaid() e falso então
			SetStatus("invasão ao castelo da fazenda")
			Se LocalPlayer:DistanceFromCharacter(Vector3.new(-5000, 350, -3035)) > 700 então
				CommF_:InvokeServer("requestEntrance", Vector3.new(-5000, 350, -3035))
			fim
			para _, v em workspace.Enemies:GetChildren() faça
				se v.PrimaryPart e v:FindFirstChild("Humanoid") e LocalPlayer:DistanceFromCharacter(v.WorldPivot.Position) <= 700 então
					repetir
						task.wait()
						EquiparArma()
						Tween(v:GetPivot() * CFrame.new(0, 20, 10), 350)
						Ataque()
					até que não seja v ou não seja v.PrimaryPart ou não seja v:FindFirstChild("Humanoid") ou v.Humanoid.Health <= 0
				fim
				quebrar
			fim
		senão se World.Sea2() e workspace.Enemies:FindFirstChild("Factory") e falso então
			SetStatus("fábrica agrícola")
			local coreEnemy = workspace.Enemies:FindFirstChild("Core")
			se coreEnemy e coreEnemy.Humanoid.Health > 0 então
				repetir
					task.wait()
					EquiparArma()
					Tween(coreEnemy.HumanoidRootPart.CFrame, 300)
					Ataque()
				até que coreEnemy.Humanoid.Health <= 0
			fim
		senão se Config.CursedDualKatana e CheckYama() >= 30 e não CheckItem("Yama") então
            LogChange("main_mode", "mode: get yama")
			SetStatus("yama | desbloqueando")
			GetYama()
		senão se Config.CursedDualKatana e World.Sea3() e não CheckItem("Tushita") e CheckMonster("rip_indra") então
            LogChange("main_mode", "modo: obter tushita")

			GetTushita()
		senão se LocalPlayer.Data.Level.Value >= 700 e World.Sea1() então
            LogChange("main_mode", "mode: unlock second sea")
			UnlockSecondSea()
		senão se LocalPlayer.Data.Level.Value >= 1500 e World.Sea2() então
            LogChange("main_mode", "mode: desbloquear terceiro mar")
			UnlockThirdSea()
		senão se World.Sea2() e CheckMonster("Darkbeard") então
			KillMonster("Darkbeard")
		outro
            LogChange("main_mode", "mode: farm level")
            se não title.Text:find("^farm |") então
                SetStatus("nível da fazenda | nível " .. tostring(LocalPlayer.Data.Level.Value))
            fim
			Se World.Sea2() então
				Se HasItem("Hidden Key") e não CheckItem("Rengoku") então
					EquipTool("Chave Oculta")
					Tween(CFrame.new(6571, 298, -6967), 300).Completed:Wait()
					task.wait(1)
				senão se HasItem("Chave da Biblioteca") e workspace.Map.IceCastle.Hall.LibraryDoor.PhoeyuDoor.Transparency == 0 então
					CommF_:InvokeServer("OpenLibrary")
					task.wait(1)
				senão se HasItem("Chave de Água") e CommF_:InvokeServer("ComprarKarateTubarão", verdadeiro):find("perdido") então
					se TweenToMeleeBuyer("SharkmanKarate") então
						CommF_:InvokeServer("BuySharkmanKarate", true)
						CommF_:InvokeServer("BuySharkmanKarate")
					fim
				fim
			elseif World.Sea3() então
				se HasItem("Essência de Fogo") e CommF_:InvokeServer("ComprarGarra de Dragão", verdadeiro):find("seu") então
					se TweenToMeleeBuyer("DragonTalon") então
						CommF_:InvokeServer("BuyDragonTalon", true)
						CommF_:InvokeServer("BuyDragonTalon")
					fim
				fim
			fim
			Se LocalPlayer.Data.Level.Value == MaxLevel então
				pcall(FarmKataLogic)
			outro
				pcall(FarmLevelLogic)
			fim
		fim
	fim
fim)
