-- Inicializando o Orion Library
local OrionLib = loadstring(game:HttpGet(('https://raw.githubusercontent.com/shlexware/Orion/main/source')))()

-- Criando a Janela Principal com Configuração Estética
local Window = OrionLib:MakeWindow({
    Name = "🌟 Eclipse Hub 🌟", -- Nome com emojis para destacar
    HidePremium = false,
    SaveConfig = true,
    ConfigFolder = "EclipseHubConfigs",
    IntroEnabled = true,
    IntroText = "✨ Bem-vindo ao Eclipse Hub! 🚀\nPrepare-se para dominar o jogo! 😎",
    Icon = "rbxassetid://4483345998", -- Ícone personalizado
    Theme = {
        Background = Color3.fromRGB(25, 25, 25), -- Cor de fundo escuro
        TextColor = Color3.fromRGB(255, 255, 255), -- Cor do texto branco
        AccentColor = Color3.fromRGB(85, 170, 255), -- Azul claro para detalhes
        OutlineColor = Color3.fromRGB(0, 0, 0), -- Contorno preto
        Font = Enum.Font.GothamBold -- Fonte personalizada
    },
    IntroIcon = "rbxassetid://4483345998", -- Ícone da introdução
    IntroEffect = "Zoom", -- Animação de introdução
    IntroDuration = 3 -- Duração da introdução em segundos
})


-- Função para criar uma Tab de forma simplificada e estilizada
local function CreateTab(name, icon, premiumOnly)
    return Window:MakeTab({
        Name = name,
        Icon = icon or "rbxassetid://4483345998", -- Ícone padrão caso não seja especificado
        PremiumOnly = premiumOnly or false -- False por padrão
    })
end

-- Criando as Tabs com ícones e nomes destacados
local HacksTab = CreateTab("⚡ Hacks Gerais", "rbxassetid://4483345998")
local OpsTab = CreateTab("💥 Hacks OP", "rbxassetid://4483345998")
local PlayerTab = CreateTab("🧍 Player", "rbxassetid://4483345998")
local TrollTab = CreateTab("😂 Troll", "rbxassetid://4483345998")
local CommandsTab = CreateTab("📜 Comandos", "rbxassetid://4483345998")
local InfoTab = CreateTab("ℹ️ Info", "rbxassetid://4483345998")
local ToolsTab = CreateTab("🛠️ Tools", "rbxassetid://4483345998")
local SoonTab = CreateTab("⏳ Em Breve", "rbxassetid://4483345998")

-- Serviços e Variáveis
local Players = game:GetService("Players")
local Debris = game:GetService("Debris")
local LocalPlayer = Players.LocalPlayer
local Mouse = LocalPlayer:GetMouse()
local LimboPower = 1000 -- Ajuste a força do lançamento (maior valor joga mais longe)

-- Função para Jogar o Player no Limbo
local function SendToLimbo(TargetPlayer)
    -- Verifica se o jogador alvo é válido
    if not TargetPlayer or not TargetPlayer.Character then
        OrionLib:MakeNotification({
            Name = "Erro",
            Content = "Jogador inválido ou não encontrado!",
            Image = "rbxassetid://4483345998",
            Time = 5
        })
        return
    end

    -- Obtém o HumanoidRootPart do jogador alvo
    local HumanoidRootPart = TargetPlayer.Character:FindFirstChild("HumanoidRootPart")
    if not HumanoidRootPart then
        OrionLib:MakeNotification({
            Name = "Erro",
            Content = "Não foi possível lançar o jogador. Parte principal não encontrada!",
            Image = "rbxassetid://4483345998",
            Time = 5
        })
        return
    end

    -- Aplica a força para lançar o jogador
    local BodyVelocity = Instance.new("BodyVelocity")
    BodyVelocity.Velocity = Vector3.new(0, LimboPower, 0) -- Força para cima
    BodyVelocity.MaxForce = Vector3.new(1e6, 1e6, 1e6) -- Permite força máxima
    BodyVelocity.Parent = HumanoidRootPart

    -- Remove o BodyVelocity após 2 segundos para evitar problemas
    Debris:AddItem(BodyVelocity, 2)

    -- Notificação de sucesso
    OrionLib:MakeNotification({
        Name = "Player Lançado!",
        Content = TargetPlayer.Name .. " foi enviado para o limbo!",
        Image = "rbxassetid://4483345998",
        Time = 5
    })
end

-- Conexão para Lançar Jogador ao Clicar
Mouse.Button1Down:Connect(function()
    -- Obtém o objeto no qual o mouse está clicando
    local Target = Mouse.Target
    if not Target then
        return -- Sai da função se não houver um alvo
    end

    -- Identifica o jogador correspondente ao objeto clicado
    local TargetPlayer = Players:GetPlayerFromCharacter(Target.Parent)
    if TargetPlayer and TargetPlayer ~= LocalPlayer then
        SendToLimbo(TargetPlayer)
    else
        OrionLib:MakeNotification({
            Name = "Erro",
            Content = "Clique em um jogador válido que não seja você!",
            Image = "rbxassetid://4483345998",
            Time = 5
        })
    end
end)

-- Adiciona a função ao menu de hacks
HacksTab:AddButton({
    Name = "Lançar Jogador no Limbo",
    Callback = function()
        OrionLib:MakeNotification({
            Name = "Modo Limbo Ativado",
            Content = "Clique em um jogador para lançá-lo no limbo!",
            Image = "rbxassetid://4483345998",
            Time = 5
        })
    end
})





-- Função para Teleportar para um Jogador Específico
local function TeleportToPlayer(TargetPlayer)
    local LocalPlayer = Players.LocalPlayer
    if TargetPlayer and TargetPlayer.Character and TargetPlayer.Character:FindFirstChild("HumanoidRootPart") then
        LocalPlayer.Character.HumanoidRootPart.CFrame = TargetPlayer.Character.HumanoidRootPart.CFrame
        OrionLib:MakeNotification({
            Name = "Teleporte",
            Content = "Você foi teleportado para " .. TargetPlayer.Name .. "!",
            Image = "rbxassetid://4483345998",
            Time = 5
        })
    end
end


-- Função para Super Pulo
local JumpEnabled = false
local function ToggleSuperJump(Value)
    local LocalPlayer = Players.LocalPlayer
    JumpEnabled = Value
    while JumpEnabled do
        if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
            LocalPlayer.Character.Humanoid.JumpPower = 200 -- Potência do salto aumentada
        end
        wait(0.1)
    end
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
        LocalPlayer.Character.Humanoid.JumpPower = 50 -- Potência padrão do salto
    end
end

-- Adicionando Toggle para Super Pulo
OpsTab:AddToggle({
    Name = "Ativar Super Pulo",
    Default = false,
    Callback = function(Value)
        ToggleSuperJump(Value)
    end
})

-- Função para Ativar ESP com BitBox, Linha e Cor Verde
local function ActivateESP()
    for _, Player in pairs(game:GetService("Players"):GetPlayers()) do
        if Player ~= LocalPlayer and Player.Character and Player.Character:FindFirstChild("HumanoidRootPart") then
            -- Configurar a cor verde
            local TeamColor = Color3.fromRGB(0, 255, 0) -- Verde para todos os jogadores

            -- Criar a Highlight (BitBox)
            local Highlight = Instance.new("Highlight")
            Highlight.Parent = Player.Character
            Highlight.Adornee = Player.Character
            Highlight.FillColor = TeamColor
            Highlight.OutlineColor = Color3.fromRGB(255, 255, 255) -- Branco para o contorno
            Highlight.FillTransparency = 0.5
            Highlight.OutlineTransparency = 0

            -- Criar Linha até o jogador local
            local Line = Instance.new("Beam")
            local Attachment0 = Instance.new("Attachment", LocalPlayer.Character.HumanoidRootPart)
            local Attachment1 = Instance.new("Attachment", Player.Character.HumanoidRootPart)
            Line.Attachment0 = Attachment0
            Line.Attachment1 = Attachment1
            Line.Parent = LocalPlayer.Character
            Line.Color = ColorSequence.new(TeamColor)
            Line.Width0 = 0.1
            Line.Width1 = 0.1

            -- Criar BillboardGui para exibir o nome
            local BillboardGui = Instance.new("BillboardGui")
            BillboardGui.Parent = Player.Character:FindFirstChild("Head")
            BillboardGui.Adornee = Player.Character:FindFirstChild("Head")
            BillboardGui.Size = UDim2.new(4, 0, 1, 0) -- Tamanho do Billboard
            BillboardGui.StudsOffset = Vector3.new(0, 3, 0) -- Distância acima da cabeça
            BillboardGui.AlwaysOnTop = true

            -- Configurar o texto do nome
            local TextLabel = Instance.new("TextLabel")
            TextLabel.Parent = BillboardGui
            TextLabel.Text = Player.Name
            TextLabel.BackgroundTransparency = 1
            TextLabel.TextSize = 14
            TextLabel.Font = Enum.Font.SourceSansBold
            TextLabel.TextColor3 = Color3.new(1, 1, 1) -- Branco
            TextLabel.Size = UDim2.new(1, 0, 1, 0)
        end
    end
end

-- Toggle para Ativar o ESP com a nova configuração
OpsTab:AddToggle({
    Name = "Ativar ESP com BitBox e Linha Verde",
    Default = false,
    Callback = function(Value)
        if Value then
            ActivateESP()
        end
    end
})

-- Função para aumentar a velocidade do carro
local function IncreaseCarSpeed()
    local Vehicle = LocalPlayer.Character:FindFirstChild("HumanoidRootPart") and LocalPlayer.Character:FindFirstChild("HumanoidRootPart"):FindFirstAncestorWhichIsA("Model")
    if Vehicle and Vehicle:FindFirstChild("VehicleSeat") then
        local VehicleSeat = Vehicle:FindFirstChild("VehicleSeat")
        if VehicleSeat:FindFirstChild("BodyVelocity") then
            -- Ajusta a velocidade se já existe um BodyVelocity
            VehicleSeat.BodyVelocity.Velocity = VehicleSeat.BodyVelocity.Velocity * 2 -- Dobra a velocidade atual
        else
            -- Cria um BodyVelocity para manipular a velocidade
            local BodyVelocity = Instance.new("BodyVelocity")
            BodyVelocity.MaxForce = Vector3.new(1e6, 1e6, 1e6) -- Permite força máxima
            BodyVelocity.Velocity = VehicleSeat.CFrame.LookVector * 100 -- Velocidade inicial
            BodyVelocity.Parent = VehicleSeat
        end

        -- Notificação de sucesso
        OrionLib:MakeNotification({
            Name = "Velocidade Aumentada",
            Content = "Velocidade do carro aumentada!",
            Image = "rbxassetid://4483345998",
            Time = 3
        })
    else
        -- Caso o jogador não esteja em um veículo
        OrionLib:MakeNotification({
            Name = "Erro",
            Content = "Você não está em um carro!",
            Image = "rbxassetid://4483345998",
            Time = 3
        })
    end
end

-- Botão para aumentar a velocidade do carro
HacksTab:AddButton({
    Name = "Aumentar Velocidade do Carro",
    Callback = function()
        IncreaseCarSpeed()
    end
})

-- Variável para ativar/desativar o Fly Car
local FlyCarEnabled = false

-- Função para ativar/desativar Fly Car
local function ToggleFlyCar()
    FlyCarEnabled = not FlyCarEnabled
    local Vehicle = LocalPlayer.Character:FindFirstChild("HumanoidRootPart") and LocalPlayer.Character:FindFirstChild("HumanoidRootPart"):FindFirstAncestorWhichIsA("Model")
    if Vehicle and Vehicle:FindFirstChild("VehicleSeat") then
        local VehicleSeat = Vehicle:FindFirstChild("VehicleSeat")
        if FlyCarEnabled then
            -- Adiciona BodyVelocity para voar
            local BodyVelocity = Instance.new("BodyVelocity")
            BodyVelocity.MaxForce = Vector3.new(1e6, 1e6, 1e6)
            BodyVelocity.Velocity = Vector3.new(0, 0, 0)
            BodyVelocity.Parent = VehicleSeat

            -- Controle de voo (WASD e altura com E/Q)
            game:GetService("RunService").RenderStepped:Connect(function()
                if FlyCarEnabled and BodyVelocity.Parent then
                    local MoveDirection = Vector3.new(0, 0, 0)

                    if UserInputService:IsKeyDown(Enum.KeyCode.W) then
                        MoveDirection = MoveDirection + workspace.CurrentCamera.CFrame.LookVector
                    end
                    if UserInputService:IsKeyDown(Enum.KeyCode.S) then
                        MoveDirection = MoveDirection - workspace.CurrentCamera.CFrame.LookVector
                    end
                    if UserInputService:IsKeyDown(Enum.KeyCode.A) then
                        MoveDirection = MoveDirection - workspace.CurrentCamera.CFrame.RightVector
                    end
                    if UserInputService:IsKeyDown(Enum.KeyCode.D) then
                        MoveDirection = MoveDirection + workspace.CurrentCamera.CFrame.RightVector
                    end
                    if UserInputService:IsKeyDown(Enum.KeyCode.E) then
                        MoveDirection = MoveDirection + Vector3.new(0, 1, 0)
                    end
                    if UserInputService:IsKeyDown(Enum.KeyCode.Q) then
                        MoveDirection = MoveDirection - Vector3.new(0, 1, 0)
                    end

                    -- Aplica a direção e velocidade
                    BodyVelocity.Velocity = MoveDirection.Unit * 100
                end
            end)

            -- Notificação
            OrionLib:MakeNotification({
                Name = "Fly Car Ativado",
                Content = "Use WASD para se mover e E/Q para ajustar altura.",
                Image = "rbxassetid://4483345998",
                Time = 5
            })
        else
            -- Desativa o Fly Car
            if VehicleSeat:FindFirstChild("BodyVelocity") then
                VehicleSeat.BodyVelocity:Destroy()
            end
            OrionLib:MakeNotification({
                Name = "Fly Car Desativado",
                Content = "Fly Car foi desativado!",
                Image = "rbxassetid://4483345998",
                Time = 3
            })
        end
    else
        -- Caso o jogador não esteja em um carro
        OrionLib:MakeNotification({
            Name = "Erro",
            Content = "Você não está em um carro!",
            Image = "rbxassetid://4483345998",
            Time = 3
        })
    end
end

-- Botão para ativar/desativar o Fly Car
HacksTab:AddToggle({
    Name = "Ativar Fly Car",
    Default = false,
    Callback = function(Value)
        ToggleFlyCar()
    end
})


-- Função para Speed Hack
local SpeedEnabled = false
local function ToggleSpeed(Value)
    local LocalPlayer = Players.LocalPlayer
    SpeedEnabled = Value
    while SpeedEnabled do
        if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
            LocalPlayer.Character.Humanoid.WalkSpeed = 100 -- Velocidade aumentada
        end
        wait(0.1)
    end
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
        LocalPlayer.Character.Humanoid.WalkSpeed = 16 -- Velocidade padrão
    end
end

-- Adicionando Toggle para Speed Hack
HacksTab:AddToggle({
    Name = "Ativar Speed Hack",
    Default = false,
    Save = true,
    Flag = "SpeedHackFlag",
    Callback = function(Value)
        ToggleSpeed(Value)
    end
})

-- Variáveis de Controle
local FlyEnabled = false
local FlySpeed = 50 -- Velocidade inicial
local SpeedIncrement = 10 -- Valor para aumentar/diminuir a velocidade
local MaxSpeed = 300 -- Velocidade máxima permitida
local MinSpeed = 10 -- Velocidade mínima
local UserInputService = game:GetService("UserInputService")
local Players = game:GetService("Players")
local Player = Players.LocalPlayer
local Character = Player.Character or Player.CharacterAdded:Wait()
local HumanoidRootPart = Character:WaitForChild("HumanoidRootPart")

-- Criação do BodyVelocity para aplicar movimento
local BodyVelocity = Instance.new("BodyVelocity")
BodyVelocity.MaxForce = Vector3.new(50000, 50000, 50000) -- Permite movimento em qualquer direção
BodyVelocity.Velocity = Vector3.new(0, 0, 0)

local FlyEnabled = false
local FlySpeed = 50 -- Velocidade padrão

local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
local HumanoidRootPart = Character:WaitForChild("HumanoidRootPart")

-- Variável para o BodyVelocity
local BodyVelocity = Instance.new("BodyVelocity")
BodyVelocity.MaxForce = Vector3.new(1e6, 1e6, 1e6) -- Força máxima permitida

-- Função para ativar/desativar o Fly
local function ToggleFly()
    FlyEnabled = not FlyEnabled
    if FlyEnabled then
        BodyVelocity.Parent = HumanoidRootPart
        OrionLib:MakeNotification({
            Name = "Fly Ativado",
            Content = "Use WASD para se mover e E/Q para ajustar a altura.",
            Image = "rbxassetid://4483345998",
            Time = 5
        })
    else
        BodyVelocity.Parent = nil
        HumanoidRootPart.Velocity = Vector3.new(0, 0, 0) -- Para o movimento
        OrionLib:MakeNotification({
            Name = "Fly Desativado",
            Content = "Você parou de voar.",
            Image = "rbxassetid://4483345998",
            Time = 5
        })
    end
end

-- Controle de Movimento
RunService.RenderStepped:Connect(function()
    if FlyEnabled then
        local MoveDirection = Vector3.new(0, 0, 0)
        local Camera = workspace.CurrentCamera

        -- Movimento com teclas WASD
        if UserInputService:IsKeyDown(Enum.KeyCode.W) then
            MoveDirection = MoveDirection + Camera.CFrame.LookVector
        end
        if UserInputService:IsKeyDown(Enum.KeyCode.S) then
            MoveDirection = MoveDirection - Camera.CFrame.LookVector
        end
        if UserInputService:IsKeyDown(Enum.KeyCode.A) then
            MoveDirection = MoveDirection - Camera.CFrame.RightVector
        end
        if UserInputService:IsKeyDown(Enum.KeyCode.D) then
            MoveDirection = MoveDirection + Camera.CFrame.RightVector
        end
        -- Controle de altura (E sobe, Q desce)
        if UserInputService:IsKeyDown(Enum.KeyCode.E) then
            MoveDirection = MoveDirection + Vector3.new(0, 1, 0)
        end
        if UserInputService:IsKeyDown(Enum.KeyCode.Q) then
            MoveDirection = MoveDirection - Vector3.new(0, 1, 0)
        end

        -- Aplica o movimento com a velocidade
        BodyVelocity.Velocity = MoveDirection.Unit * FlySpeed
    end
end)

-- Toggle no menu Hacks
HacksTab:AddToggle({
    Name = "Ativar Fly Melhorado",
    Default = false,
    Callback = function(Value)
        ToggleFly()
    end
})



local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera
local Mouse = LocalPlayer:GetMouse()

local AimEnabled = false -- Ativa/Desativa o Aimbot
local AimKey = Enum.KeyCode.E -- Tecla para ativar o Aimbot

-- Função para verificar se o jogador é inimigo
local function IsEnemy(TargetPlayer)
    -- Verifique se o jogo tem equipes
    if TargetPlayer.Team and LocalPlayer.Team then
        return TargetPlayer.Team ~= LocalPlayer.Team -- Retorna true apenas se estiverem em equipes diferentes
    end
    -- Se não houver equipes, considera todos como inimigos
    return true
end

-- Função para encontrar o inimigo mais próximo do mouse
local function GetClosestEnemy()
    local ClosestPlayer = nil
    local ShortestDistance = math.huge

    for _, Player in pairs(Players:GetPlayers()) do
        if Player ~= LocalPlayer and Player.Character and Player.Character:FindFirstChild("HumanoidRootPart") and IsEnemy(Player) then
            local RootPart = Player.Character.HumanoidRootPart
            local ScreenPos, OnScreen = Camera:WorldToScreenPoint(RootPart.Position)

            if OnScreen then
                local Distance = (Vector2.new(Mouse.X, Mouse.Y) - Vector2.new(ScreenPos.X, ScreenPos.Y)).Magnitude
                if Distance < ShortestDistance then
                    ShortestDistance = Distance
                    ClosestPlayer = Player
                end
            end
        end
    end

    return ClosestPlayer
end

-- Função Principal: Mira no Jogador Alvo
local function AimAt(TargetPlayer)
    if TargetPlayer and TargetPlayer.Character then
        local RootPart = TargetPlayer.Character:FindFirstChild("HumanoidRootPart")
        if RootPart then
            -- Ajusta a câmera para mirar no jogador
            Camera.CFrame = CFrame.new(Camera.CFrame.Position, RootPart.Position)
        end
    end
end

-- Ativar/Desativar o Aimbot
game:GetService("UserInputService").InputBegan:Connect(function(Input, Processed)
    if Input.KeyCode == AimKey then
        AimEnabled = not AimEnabled
        OrionLib:MakeNotification({
            Name = "Aimbot",
            Content = AimEnabled and "Aimbot Ativado" or "Aimbot Desativado",
            Image = "rbxassetid://4483345998",
            Time = 3
        })
    end
end)

-- Loop Contínuo para Mirar
game:GetService("RunService").RenderStepped:Connect(function()
    if AimEnabled then
        local TargetPlayer = GetClosestEnemy() -- Busca apenas inimigos
        if TargetPlayer then
            AimAt(TargetPlayer)
        end
    end
end)

-- Adiciona a Opção no Menu
HacksTab:AddButton({
    Name = "Aimbot (Inimigos Apenas - Tecla E)",
    Callback = function()
        OrionLib:MakeNotification({
            Name = "Aimbot",
            Content = "Pressione 'E' para ativar/desativar. Mira apenas em inimigos.",
            Image = "rbxassetid://4483345998",
            Time = 5
        })
    end
})


-- Função para Wallhack
local function ActivateWallhack()
    for _, Player in pairs(game:GetService("Players"):GetPlayers()) do
        if Player.Character and Player.Character:FindFirstChild("Head") then
            local Highlight = Instance.new("Highlight")
            Highlight.Parent = Player.Character
            Highlight.Adornee = Player.Character
            Highlight.FillColor = Color3.fromRGB(255, 0, 0)
        end
    end
end

-- Adicionando Toggle para Wallhack
HacksTab:AddToggle({
    Name = "Ativar Wallhack",
    Default = false,
    Callback = function(Value)
        print("Wallhack: ", Value)
        if Value then
            ActivateWallhack()
        end
    end
})

-- Função para No Recoil
local function RemoveRecoil()
    local ReplicatedStorage = game:GetService("ReplicatedStorage")
    local Weapons = ReplicatedStorage:FindFirstChild("Weapons")

    if Weapons then
        for _, Weapon in pairs(Weapons:GetChildren()) do
            if Weapon:FindFirstChild("Recoil") then
                Weapon.Recoil.Value = 0
            end
        end
    end
end

-- Adicionando Toggle para No Recoil
OpsTab:AddToggle({
    Name = "Sem Recuo",
    Default = false,
    Callback = function(Value)
        print("No Recoil: ", Value)
        if Value then
            RemoveRecoil()
        end
    end
})

-- Função para Spinbot
local function ActivateSpinbot()
    local Player = game:GetService("Players").LocalPlayer
    game:GetService("RunService").RenderStepped:Connect(function()
        if Player.Character and Player.Character:FindFirstChild("HumanoidRootPart") then
            Player.Character.HumanoidRootPart.CFrame = Player.Character.HumanoidRootPart.CFrame * CFrame.Angles(0, math.rad(45), 0)
        end
    end)
end

-- Variável para controlar o NoClip
local NoClipEnabled = false

-- Função para ativar/desativar o NoClip
local function ToggleNoClip(Value)
    NoClipEnabled = Value
    game:GetService("RunService").Stepped:Connect(function()
        if NoClipEnabled then
            local LocalPlayer = game:GetService("Players").LocalPlayer
            if LocalPlayer.Character then
                for _, Part in pairs(LocalPlayer.Character:GetDescendants()) do
                    if Part:IsA("BasePart") and Part.CanCollide then
                        Part.CanCollide = false -- Desativa a colisão
                    end
                end
            end
        end
    end)
    if not NoClipEnabled then
        local LocalPlayer = game:GetService("Players").LocalPlayer
        if LocalPlayer.Character then
            for _, Part in pairs(LocalPlayer.Character:GetDescendants()) do
                if Part:IsA("BasePart") then
                    Part.CanCollide = true -- Restaura a colisão ao desativar
                end
            end
        end
    end
end

-- Adicionando Toggle para NoClip
HacksTab:AddToggle({
    Name = "Ativar NoClip",
    Default = false,
    Save = true,
    Flag = "NoClipFlag",
    Callback = function(Value)
        ToggleNoClip(Value)
        if Value then
            OrionLib:MakeNotification({
                Name = "NoClip",
                Content = "NoClip ativado!",
                Image = "rbxassetid://4483345998",
                Time = 5
            })
        else
            OrionLib:MakeNotification({
                Name = "NoClip",
                Content = "NoClip desativado!",
                Image = "rbxassetid://4483345998",
                Time = 5
            })
        end
    end
})


-- Adicionando Toggle para Spinbot
OpsTab:AddToggle({
    Name = "Ativar Spinbot",
    Default = false,
    Callback = function(Value)
        print("Spinbot: ", Value)
        if Value then
            ActivateSpinbot()
        end
    end
})

-- Função para Teleporte
local function TeleportToEnemy()
    local Players = game:GetService("Players")
    local LocalPlayer = Players.LocalPlayer

    for _, Player in pairs(Players:GetPlayers()) do
        if Player ~= LocalPlayer and Player.Character and Player.Character:FindFirstChild("HumanoidRootPart") then
            LocalPlayer.Character.HumanoidRootPart.CFrame = Player.Character.HumanoidRootPart.CFrame
            break
        end
    end
end

-- Serviços e Variáveis
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Mouse = LocalPlayer:GetMouse()

-- Função para Atirar Através das Paredes
local function ShootThroughWalls(TargetPlayer)
    -- Verifica se o alvo é válido
    if TargetPlayer and TargetPlayer.Character then
        local Humanoid = TargetPlayer.Character:FindFirstChild("Humanoid")
        local RootPart = TargetPlayer.Character:FindFirstChild("HumanoidRootPart")

        if Humanoid and RootPart then
            -- Teleporta temporariamente o jogador para a posição de tiro
            local SavedPosition = RootPart.CFrame -- Salva a posição original
            RootPart.CFrame = CFrame.new(RootPart.Position + Vector3.new(0, 0, -3)) -- Move ligeiramente para frente

            -- Aplica o dano
            Humanoid:TakeDamage(50) -- Ajuste o dano aqui

            -- Restaura a posição original
            wait(0.1)
            RootPart.CFrame = SavedPosition

            -- Notificação de sucesso
            OrionLib:MakeNotification({
                Name = "Atingido!",
                Content = TargetPlayer.Name .. " foi acertado através da parede!",
                Image = "rbxassetid://4483345998",
                Time = 3
            })
        end
    end
end

-- Seleciona e Atira com Clique
Mouse.Button1Down:Connect(function()
    local Target = Mouse.Target
    if Target then
        local TargetPlayer = Players:GetPlayerFromCharacter(Target.Parent)
        if TargetPlayer and TargetPlayer ~= LocalPlayer then
            ShootThroughWalls(TargetPlayer)
        end
    end
end)

-- Adiciona a opção no menu
HacksTab:AddButton({
    Name = "Atirar Através das Paredes",
    Callback = function()
        OrionLib:MakeNotification({
            Name = "Modo Atirar",
            Content = "Clique em um jogador para atirar mesmo atrás das paredes!",
            Image = "rbxassetid://4483345998",
            Time = 5
        })
    end
})



-- Adicionando botão de Teleporte
HacksTab:AddButton({
    Name = "Teleportar para Inimigo",
    Callback = function()
        print("Teleporte ativado!")
        TeleportToEnemy()
    end
})

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Admins = {} -- Tabela para armazenar jogadores com permissões de administrador

-- Lista de comandos disponíveis
local CommandsList = {
    "!speed [valor] - Ajusta a velocidade do jogador.",
    "!noclip - Permite atravessar paredes.",
    "!teleport [nome] - Teleporta para o jogador especificado.",
    "!fly - Ativa o modo voo temporário.",
    "!explode - Causa uma explosão no local do jogador.",
    "!kill - Mata o jogador instantaneamente.",
    "!heal - Cura completamente o jogador.",
    "!god - Ativa o modo invencível.",
    "!freeze - Congela o jogador.",
    "!unfreeze - Descongela o jogador.",
    "!cmd ou !cmds - Exibe a lista de comandos."
}

-- Função para exibir os comandos
local function ShowCommands(Player)
    local CommandText = table.concat(CommandsList, "\n")
    OrionLib:MakeNotification({
        Name = "Lista de Comandos",
        Content = CommandText,
        Image = "rbxassetid://4483345998",
        Time = 10
    })
end

-- Função para Conceder Admin
local function GiveAdmin(TargetPlayer)
    if TargetPlayer and not Admins[TargetPlayer.Name] then
        Admins[TargetPlayer.Name] = true -- Marca o jogador como Admin

        -- Notificação de sucesso
        OrionLib:MakeNotification({
            Name = "Admin Concedido",
            Content = TargetPlayer.Name .. " agora é administrador!",
            Image = "rbxassetid://4483345998",
            Time = 5
        })

        -- Comandos do Administrador
        TargetPlayer.Chatted:Connect(function(Message)
            if Admins[TargetPlayer.Name] then
                local args = string.split(Message, " ")
                local command = args[1]:lower()

                -- Comando: !speed [valor]
                if command == "!speed" then
                    local SpeedValue = tonumber(args[2]) or 50
                    if TargetPlayer.Character and TargetPlayer.Character:FindFirstChild("Humanoid") then
                        TargetPlayer.Character.Humanoid.WalkSpeed = SpeedValue
                        OrionLib:MakeNotification({
                            Name = "Velocidade Ajustada",
                            Content = "Velocidade ajustada para " .. SpeedValue,
                            Image = "rbxassetid://4483345998",
                            Time = 5
                        })
                    end
                end

                -- Comando: !noclip
                if command == "!noclip" then
                    for _, Part in pairs(TargetPlayer.Character:GetDescendants()) do
                        if Part:IsA("BasePart") then
                            Part.CanCollide = false -- Desativa colisão
                        end
                    end
                    OrionLib:MakeNotification({
                        Name = "NoClip Ativado",
                        Content = "Agora pode atravessar paredes!",
                        Image = "rbxassetid://4483345998",
                        Time = 5
                    })
                end

                -- Comando: !teleport [nome]
                if command == "!teleport" then
                    local TargetName = args[2]
                    local TargetDestination = Players:FindFirstChild(TargetName)
                    if TargetDestination and TargetDestination.Character and TargetDestination.Character:FindFirstChild("HumanoidRootPart") then
                        TargetPlayer.Character.HumanoidRootPart.CFrame = TargetDestination.Character.HumanoidRootPart.CFrame
                        OrionLib:MakeNotification({
                            Name = "Teleportado",
                            Content = "Teleportado para " .. TargetName,
                            Image = "rbxassetid://4483345998",
                            Time = 5
                        })
                    end
                end

                -- Comando: !fly
                if command == "!fly" then
                    local Character = TargetPlayer.Character
                    if Character and Character:FindFirstChild("HumanoidRootPart") then
                        local BodyVelocity = Instance.new("BodyVelocity")
                        BodyVelocity.MaxForce = Vector3.new(1e6, 1e6, 1e6)
                        BodyVelocity.Velocity = Vector3.new(0, 50, 0)
                        BodyVelocity.Parent = Character.HumanoidRootPart
                        OrionLib:MakeNotification({
                            Name = "Fly Ativado",
                            Content = "Voando ativado!",
                            Image = "rbxassetid://4483345998",
                            Time = 5
                        })
                    end
                end

                -- Comando: !explode
                if command == "!explode" then
                    local Explosion = Instance.new("Explosion")
                    Explosion.Position = TargetPlayer.Character.HumanoidRootPart.Position
                    Explosion.BlastRadius = 10
                    Explosion.BlastPressure = 10000
                    Explosion.Parent = workspace
                end

                -- Comando: !kill
                if command == "!kill" then
                    if TargetPlayer.Character and TargetPlayer.Character:FindFirstChild("Humanoid") then
                        TargetPlayer.Character.Humanoid.Health = 0
                    end
                end

                -- Comando: !heal
                if command == "!heal" then
                    if TargetPlayer.Character and TargetPlayer.Character:FindFirstChild("Humanoid") then
                        TargetPlayer.Character.Humanoid.Health = TargetPlayer.Character.Humanoid.MaxHealth
                    end
                end

                -- Comando: !god
                if command == "!god" then
                    if TargetPlayer.Character and TargetPlayer.Character:FindFirstChild("Humanoid") then
                        TargetPlayer.Character.Humanoid.MaxHealth = math.huge
                        TargetPlayer.Character.Humanoid.Health = math.huge
                    end
                end

                -- Comando: !freeze
                if command == "!freeze" then
                    for _, Part in pairs(TargetPlayer.Character:GetDescendants()) do
                        if Part:IsA("BasePart") then
                            Part.Anchored = true
                        end
                    end
                end

                -- Comando: !unfreeze
                if command == "!unfreeze" then
                    for _, Part in pairs(TargetPlayer.Character:GetDescendants()) do
                        if Part:IsA("BasePart") then
                            Part.Anchored = false
                        end
                    end
                end

                -- Comando: !cmd ou !cmds
                if command == "!cmd" or command == "!cmds" then
                    ShowCommands(TargetPlayer)
                end
            end
        end)
    else
        OrionLib:MakeNotification({
            Name = "Erro",
            Content = "Jogador já é administrador ou não encontrado.",
            Image = "rbxassetid://4483345998",
            Time = 5
        })
    end
end


-- Função para Remover Admin
local function RemoveAdmin(TargetPlayer)
    if TargetPlayer and Admins[TargetPlayer.Name] then
        Admins[TargetPlayer.Name] = nil -- Remove da lista de administradores

        -- Notificação de sucesso
        OrionLib:MakeNotification({
            Name = "Admin Removido",
            Content = "Permissões de administrador removidas de " .. TargetPlayer.Name,
            Image = "rbxassetid://4483345998",
            Time = 5
        })
    else
        OrionLib:MakeNotification({
            Name = "Erro",
            Content = "Jogador não é administrador ou não encontrado.",
            Image = "rbxassetid://4483345998",
            Time = 5
        })
    end
end


-- Função para Exibir Lista de Administradores
local function ShowAdminList()
    local AdminNames = ""
    for Name, _ in pairs(Admins) do
        AdminNames = AdminNames .. Name .. "\n"
    end
    OrionLib:MakeNotification({
        Name = "Lista de Administradores",
        Content = "Administradores Atuais:\n" .. (AdminNames ~= "" and AdminNames or "Nenhum administrador no momento."),
        Image = "rbxassetid://4483345998",
        Time = 10
    })
end

-- Botão para Exibir Lista de Administradores
HacksTab:AddButton({
    Name = "Exibir Lista de Administradores",
    Callback = function()
        ShowAdminList()
    end
})

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local VirtualUser = game:GetService("VirtualUser")

local AntiAFKEnabled = false
local lastActivity = 0

-- Prevenir desconexão por AFK
LocalPlayer.Idled:Connect(function()
    if AntiAFKEnabled then
        VirtualUser:CaptureController()
        VirtualUser:ClickButton2(Vector2.new(0, 0))
        lastActivity = tick() -- Atualiza o tempo de última atividade
    end
end)

-- Função para Simular Movimentos Anti-AFK
local function StartAntiAFK()
    while AntiAFKEnabled do
        if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
            -- Movimenta e rotaciona levemente o jogador
            local RootPart = LocalPlayer.Character.HumanoidRootPart
            RootPart.CFrame = RootPart.CFrame * CFrame.Angles(0, math.rad(math.random(-10, 10)), 0) -- Gira um pouco
            RootPart.CFrame = RootPart.CFrame * CFrame.new(0, 0, math.random(-1, 1) / 10) -- Move para frente/trás

            -- Simula entrada do teclado
            VirtualUser:CaptureController()
            VirtualUser:SetKeyDown("w")
            wait(math.random(0.5, 1))
            VirtualUser:SetKeyUp("w")
        end

        -- Aguardar intervalo variável para parecer mais humano
        local nextInterval = math.random(900, 1200) -- Entre 15 e 20 minutos
        lastActivity = tick()
        wait(nextInterval)
    end
end

-- Função para Ativar/Desativar o Anti-AFK
local function ToggleAntiAFK(Value)
    AntiAFKEnabled = Value
    if AntiAFKEnabled then
        OrionLib:MakeNotification({
            Name = "Anti-AFK Ativado",
            Content = "Você está protegido contra desconexões por inatividade.",
            Image = "rbxassetid://4483345998",
            Time = 5
        })
        StartAntiAFK()
    else
        OrionLib:MakeNotification({
            Name = "Anti-AFK Desativado",
            Content = "Você pode ser desconectado por inatividade.",
            Image = "rbxassetid://4483345998",
            Time = 5
        })
    end
end

-- Adicionando Toggle ao Menu
HacksTab:AddToggle({
    Name = "Ativar Anti-AFK Melhorado",
    Default = false,
    Save = true,
    Flag = "AntiAFKFlag",
    Callback = function(Value)
        ToggleAntiAFK(Value)
    end
})

-- Lista de comandos disponíveis
local CommandsList = {
    "!speed [valor] - Ajusta a velocidade do jogador.",
    "!noclip - Permite atravessar paredes.",
    "!teleport [nome] - Teleporta para o jogador especificado.",
    "!fly - Ativa o modo voo temporário.",
    "!explode - Causa uma explosão no local do jogador.",
}

-- Função para exibir os comandos como uma notificação
local function ShowCommands()
    local CommandText = "Comandos Disponíveis:\n" .. table.concat(CommandsList, "\n")
    OrionLib:MakeNotification({
        Name = "Lista de Comandos",
        Content = CommandText,
        Image = "rbxassetid://4483345998",
        Time = 10 -- Tempo que a notificação ficará visível
    })
end

-- Adiciona botão na aba "Comandos"
CommandsTab:AddButton({
    Name = "Exibir Lista de Comandos",
    Callback = function()
        ShowCommands()
    end
})

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

-- Tabela centralizada para as Dropdowns
local PlayerDropdowns = {
    Teleport = nil,
    GiveAdmin = nil,
    RemoveAdmin = nil
}

local PlayerList = {} -- Lista compartilhada de jogadores

-- Função para atualizar a lista de jogadores em todas as Dropdowns
local function UpdatePlayerList()
    PlayerList = {} -- Limpa a lista atual
    for _, player in ipairs(Players:GetPlayers()) do
        if player ~= LocalPlayer then -- Exclui o jogador local
            table.insert(PlayerList, player.Name)
        end
    end

    -- Atualiza todas as Dropdowns
    for _, dropdown in pairs(PlayerDropdowns) do
        if dropdown then
            dropdown:Refresh(PlayerList, true)
        end
    end
end

-- Criar a Dropdown para Teleporte
PlayerDropdowns.Teleport = HacksTab:AddDropdown({
    Name = "Teleporte para Jogador",
    Default = "",
    Options = PlayerList,
    Callback = function(Value)
        local TargetPlayer = Players:FindFirstChild(Value)
        if TargetPlayer and TargetPlayer.Character and TargetPlayer.Character:FindFirstChild("HumanoidRootPart") then
            LocalPlayer.Character.HumanoidRootPart.CFrame = TargetPlayer.Character.HumanoidRootPart.CFrame
            OrionLib:MakeNotification({
                Name = "Teleporte",
                Content = "Você foi teleportado para " .. TargetPlayer.Name .. "!",
                Image = "rbxassetid://4483345998",
                Time = 3
            })
        end
    end
})

-- Criar a Dropdown para Conceder Admin
PlayerDropdowns.GiveAdmin = HacksTab:AddDropdown({
    Name = "Dar Admin para Jogador",
    Default = "",
    Options = PlayerList,
    Callback = function(Value)
        local TargetPlayer = Players:FindFirstChild(Value)
        if TargetPlayer then
            GiveAdmin(TargetPlayer) -- Função existente
        end
    end
})

-- Criar a Dropdown para Remover Admin
PlayerDropdowns.RemoveAdmin = HacksTab:AddDropdown({
    Name = "Remover Admin de Jogador",
    Default = "",
    Options = PlayerList,
    Callback = function(Value)
        local TargetPlayer = Players:FindFirstChild(Value)
        if TargetPlayer then
            RemoveAdmin(TargetPlayer) -- Função existente
        end
    end
})

-- Atualiza a lista sempre que jogadores entram ou saem
Players.PlayerAdded:Connect(UpdatePlayerList)
Players.PlayerRemoving:Connect(UpdatePlayerList)

-- Inicializa a lista de jogadores ao carregar o script
UpdatePlayerList()

-- Variáveis principais
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

-- Função: Fazer o jogador girar sem parar
local function SpinPlayer(TargetPlayer)
    if TargetPlayer and TargetPlayer.Character and TargetPlayer.Character:FindFirstChild("HumanoidRootPart") then
        local RootPart = TargetPlayer.Character.HumanoidRootPart
        game:GetService("RunService").RenderStepped:Connect(function()
            RootPart.CFrame = RootPart.CFrame * CFrame.Angles(0, math.rad(10), 0) -- Gira o jogador
        end)
    end
end

-- Função: Lançar o jogador para cima repetidamente
local function LaunchPlayer(TargetPlayer)
    if TargetPlayer and TargetPlayer.Character and TargetPlayer.Character:FindFirstChild("HumanoidRootPart") then
        local RootPart = TargetPlayer.Character.HumanoidRootPart
        while true do
            RootPart.Velocity = Vector3.new(0, 100, 0) -- Joga o jogador para cima
            wait(0.5) -- Intervalo entre os lançamentos
        end
    end
end

-- Função: Congelar o jogador em uma posição estranha
local function FreezePlayer(TargetPlayer)
    if TargetPlayer and TargetPlayer.Character then
        for _, Part in pairs(TargetPlayer.Character:GetDescendants()) do
            if Part:IsA("BasePart") then
                Part.Anchored = true -- Congela o jogador
            end
        end
        TargetPlayer.Character.HumanoidRootPart.CFrame = TargetPlayer.Character.HumanoidRootPart.CFrame * CFrame.Angles(math.rad(90), 0, math.rad(45))
    end
end

-- Menu Troll com Dropdown
local TrollTab = Window:MakeTab({ Name = "😂 Troll", Icon = "rbxassetid://4483345998" })
local TrollDropdown = {}
local PlayerList = {}

-- Função para atualizar a lista de jogadores
local function UpdatePlayerList()
    PlayerList = {}
    for _, player in ipairs(Players:GetPlayers()) do
        if player ~= LocalPlayer then
            table.insert(PlayerList, player.Name)
        end
    end
    TrollDropdown:Refresh(PlayerList, true)
end

Players.PlayerAdded:Connect(UpdatePlayerList)
Players.PlayerRemoving:Connect(UpdatePlayerList)

-- Dropdown para selecionar jogador e aplicar troll
TrollDropdown = TrollTab:AddDropdown({
    Name = "Selecionar Jogador para Troll",
    Default = "",
    Options = PlayerList,
    Callback = function(Value)
        local TargetPlayer = Players:FindFirstChild(Value)
        if TargetPlayer then
            -- Escolha do troll
            TrollTab:AddButton({
                Name = "Fazer Girar",
                Callback = function()
                    SpinPlayer(TargetPlayer)
                    OrionLib:MakeNotification({
                        Name = "Troll Ativado",
                        Content = TargetPlayer.Name .. " está girando sem parar!",
                        Image = "rbxassetid://4483345998",
                        Time = 5
                    })
                end
            })

            TrollTab:AddButton({
                Name = "Lançar Para Cima",
                Callback = function()
                    LaunchPlayer(TargetPlayer)
                    OrionLib:MakeNotification({
                        Name = "Troll Ativado",
                        Content = TargetPlayer.Name .. " está sendo lançado!",
                        Image = "rbxassetid://4483345998",
                        Time = 5
                    })
                end
            })

            TrollTab:AddButton({
                Name = "Congelar em Posição Estranha",
                Callback = function()
                    FreezePlayer(TargetPlayer)
                    OrionLib:MakeNotification({
                        Name = "Troll Ativado",
                        Content = TargetPlayer.Name .. " foi congelado!",
                        Image = "rbxassetid://4483345998",
                        Time = 5
                    })
                end
            })
        end
    end
})

-- Atualiza a lista de jogadores ao iniciar o script
UpdatePlayerList()

-- Função para exibir informações detalhadas
local function ShowInfo()
    local InfoText = [[
🌟 **Eclipse Hub** 🌟
🚀 **Versão:** 1.0  
🧑‍💻 **Criador:** SeuNomeAqui  
🛠️ **Script:** Feito para dominar o jogo com hacks exclusivos!

📜 **Instruções:**
1. Use as opções no menu para ativar hacks e trolls.
2. Acesse a aba "Comandos" para ver todos os comandos disponíveis.
3. Divirta-se, mas use com responsabilidade! 😉

🔧 **Principais Funcionalidades:**
- ⚡ Hacks Gerais (Speed, Fly, NoClip)
- 💥 Troll (Girar, Lançar, Congelar Jogadores)
- 🧍 Player (Teleporte, Super Pulo, etc.)
- 📜 Comandos (Lista com `!cmd` ou `!cmds`)

✨ **Agradecimento:** Obrigado por usar o Eclipse Hub!
]]
    OrionLib:MakeNotification({
        Name = "Informações do Script",
        Content = "Veja as informações no console.",
        Image = "rbxassetid://4483345998",
        Time = 5
    })

    -- Imprime no console (opcional)
    print(InfoText)
end

-- Aba Info
local InfoTab = Window:MakeTab({
    Name = "ℹ️ Info",
    Icon = "rbxassetid://4483345998"
})

-- Adiciona um botão na aba Info
InfoTab:AddButton({
    Name = "Mostrar Informações",
    Callback = function()
        ShowInfo()
    end
})

-- Adiciona créditos simples
InfoTab:AddLabel("🌟 Eclipse Hub v1.0")
InfoTab:AddLabel("🧑‍💻 Criador: SeuNomeAqui")
InfoTab:AddLabel("📅 Última Atualização: 2024")

-- Aba Player
local PlayerTab = Window:MakeTab({
    Name = "🧍 Player",
    Icon = "rbxassetid://4483345998"
})

local LocalPlayer = game:GetService("Players").LocalPlayer
local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()

-- Função para Ajustar Velocidade do Jogador
local function SetWalkSpeed(Speed)
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
        LocalPlayer.Character.Humanoid.WalkSpeed = Speed
    end
end

-- Função para Ajustar Poder do Salto
local function SetJumpPower(Power)
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
        LocalPlayer.Character.Humanoid.JumpPower = Power
    end
end

-- Função para Ativar God Mode
local function GodMode()
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
        LocalPlayer.Character.Humanoid.MaxHealth = math.huge
        LocalPlayer.Character.Humanoid.Health = math.huge
        OrionLib:MakeNotification({
            Name = "God Mode Ativado",
            Content = "Você agora é invencível!",
            Image = "rbxassetid://4483345998",
            Time = 5
        })
    end
end

-- Função para Resetar o Personagem
local function ResetCharacter()
    LocalPlayer:LoadCharacter()
end

-- Toggle para Super Velocidade
PlayerTab:AddSlider({
    Name = "Ajustar Velocidade",
    Min = 16, -- Velocidade padrão
    Max = 500, -- Máxima velocidade
    Default = 16,
    Color = Color3.fromRGB(85, 170, 255),
    Increment = 5,
    ValueName = "Velocidade",
    Callback = function(Value)
        SetWalkSpeed(Value)
    end
})

-- Toggle para Super Pulo
PlayerTab:AddSlider({
    Name = "Ajustar Super Pulo",
    Min = 50, -- Pulo padrão
    Max = 500, -- Pulo máximo
    Default = 50,
    Color = Color3.fromRGB(85, 170, 255),
    Increment = 10,
    ValueName = "Pulo",
    Callback = function(Value)
        SetJumpPower(Value)
    end
})

-- Botão para Ativar God Mode
PlayerTab:AddButton({
    Name = "Ativar God Mode",
    Callback = function()
        GodMode()
    end
})

-- Botão para Resetar o Personagem
PlayerTab:AddButton({
    Name = "Resetar Personagem",
    Callback = function()
        ResetCharacter()
        OrionLib:MakeNotification({
            Name = "Resetar Personagem",
            Content = "Seu personagem foi resetado!",
            Image = "rbxassetid://4483345998",
            Time = 5
        })
    end
})

-- Toggle para Invisibilidade
local Invisible = false
PlayerTab:AddToggle({
    Name = "Ativar Invisibilidade",
    Default = false,
    Callback = function(Value)
        Invisible = Value
        if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
            LocalPlayer.Character.HumanoidRootPart.Transparency = Value and 1 or 0
            for _, Part in pairs(LocalPlayer.Character:GetDescendants()) do
                if Part:IsA("BasePart") or Part:IsA("Decal") then
                    Part.Transparency = Value and 1 or 0
                end
            end
            OrionLib:MakeNotification({
                Name = "Invisibilidade",
                Content = Value and "Você está invisível!" or "Você está visível novamente!",
                Image = "rbxassetid://4483345998",
                Time = 5
            })
        end
    end
})


-- Finalizando a Interface
OrionLib:Init()
