local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
   Name = "Script Piterkx",
   Icon = 0, -- Icon in Topbar. Can use Lucide Icons (string) or Roblox Image (number). 0 to use no icon (default).
   LoadingTitle = "Script Piterkx Abrindo...",
   LoadingSubtitle = "by Piterkx",
   ShowText = "ScriptPiterkx", -- for mobile users to unhide rayfield, change if you'd like
   Theme = "Dark Blue", -- Check https://docs.sirius.menu/rayfield/configuration/themes

   ToggleUIKeybind = "T", -- The keybind to toggle the UI visibility (string like "K" or Enum.KeyCode)

   DisableRayfieldPrompts = false,
   DisableBuildWarnings = false, -- Prevents Rayfield from warning when the script has a version mismatch with the interface

   ConfigurationSaving = {
      Enabled = true,
      FolderName = nil, -- Create a custom folder for your hub/game
      FileName = "Big Hub"
   },

   Discord = {
      Enabled = false, -- Prompt the user to join your Discord server if their executor supports it
      Invite = "noinvitelink", -- The Discord invite code, do not include discord.gg/. E.g. discord.gg/ ABCD would be ABCD
      RememberJoins = true -- Set this to false to make them join the discord every time they load it up
   },

   KeySystem = false, -- Set this to true to use our key system
   KeySettings = {
      Title = "Untitled",
      Subtitle = "Key System",
      Note = "No method of obtaining the key is provided", -- Use this to tell the user how to get a key
      FileName = "Key", -- It is recommended to use something unique as other scripts using Rayfield may overwrite your key file
      SaveKey = true, -- The user's key will be saved, but if you change the key, they will be unable to use your script
      GrabKeyFromSite = false, -- If this is true, set Key below to the RAW site you would like Rayfield to get the key from
      Key = {"Hello"} -- List of keys that will be accepted by the system, can be RAW file links (pastebin, github etc) or simple strings ("hello","key22")
   }
})

local Maintab = Window:CreateTab("Maintab", 4483362458) -- Title, Image

Rayfield:Notify({
   Title = "Script Piterzx Funcionando",
   Content = "Criando por Piterzx",
   Duration = 6.5,
   Image = 4483362458,
})

local flying = false
local bodyGyro, bodyVelocity

local Fly = Maintab:CreateButton({
    Name = "Fly",
    Callback = function()
        local player = game.Players.LocalPlayer
        local character = player.Character or player.CharacterAdded:Wait()
        local humanoidRootPart = character:WaitForChild("HumanoidRootPart")
        local camera = workspace.CurrentCamera

        if flying == false then
            -- Ativar voo
            flying = true
            warn("🚀 Voo ativado!")

            -- Criar objetos de física
            bodyGyro = Instance.new("BodyGyro")
            bodyVelocity = Instance.new("BodyVelocity")

            bodyGyro.P = 9e4
            bodyGyro.MaxTorque = Vector3.new(9e9, 9e9, 9e9)
            bodyGyro.CFrame = humanoidRootPart.CFrame
            bodyGyro.Parent = humanoidRootPart

            bodyVelocity.MaxForce = Vector3.new(9e9, 9e9, 9e9)
            bodyVelocity.Velocity = Vector3.new(0, 0, 0)
            bodyVelocity.Parent = humanoidRootPart

            local speed = 70 -- velocidade de voo
            local upForce = 20 -- força vertical (para manter no ar)

            task.spawn(function()
                while flying and humanoidRootPart do
                    task.wait()
                    -- Manter orientação da câmera
                    bodyGyro.CFrame = camera.CFrame

                    -- Fazer o personagem voar para frente e levemente para cima
                    local direction = camera.CFrame.LookVector * speed + Vector3.new(0, upForce, 0)
                    bodyVelocity.Velocity = direction
                end
            end)

        else
            -- Desativar voo
            flying = false
            warn("🛑 Voo desativado!")

            if bodyGyro then
                bodyGyro:Destroy()
                bodyGyro = nil
            end
            if bodyVelocity then
                bodyVelocity:Destroy()
                bodyVelocity = nil
            end
        end
    end,
})

-- Variável que guarda a posição salva
local savedPosition = nil
local originalPosition = nil

-- Botão para salvar posição atual
local SavePosition = Maintab:CreateButton({
    Name = "💾 Salvar Posição Atual",
    Callback = function()
        local player = game.Players.LocalPlayer
        local character = player.Character or player.CharacterAdded:Wait()
        local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

        savedPosition = humanoidRootPart.Position
        warn("📍 Posição salva em: " .. tostring(savedPosition))
    end,
})

-- Toggle para teleporte
local TeleportToggle = Maintab:CreateToggle({
    Name = "⚡ Teleportar / Voltar",
    CurrentValue = false,
    Flag = "TeleportToggle",
    Callback = function(Value)
        local player = game.Players.LocalPlayer
        local character = player.Character or player.CharacterAdded:Wait()
        local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

        if Value == true then
            -- Ativar: teleportar para a posição salva
            if savedPosition then
                originalPosition = humanoidRootPart.Position -- salva onde estava antes
                humanoidRootPart.CFrame = CFrame.new(savedPosition)
                warn("🚀 Teleportado para a posição salva!")
            else
                warn("⚠️ Nenhuma posição salva! Clique em 'Salvar Posição Atual' primeiro.")
            end

        else
            -- Desativar: voltar para a posição original
            if originalPosition then
                humanoidRootPart.CFrame = CFrame.new(originalPosition)
                warn("⬅️ Voltou para a posição original!")
            else
                warn("⚠️ Nenhuma posição original salva!")
            end
        end
    end,
})

local WalksSpeed = Maintab:CreateSlider({
   Name = "WalksSpeed",
   Range = {0, 100},
   Increment = 10,
   Suffix = "WalksSpeed",
   CurrentValue = 10,
   Flag = "Speed1", -- A flag is the identifier for the configuration file, make sure every element has a different flag if you're using configuration saving to ensure no overlaps
   Callback = function(Value)
   -- The function that takes place when the slider changes
   -- The variable (Value) is a number which correlates to the value the slider is currently at
   end,
})
