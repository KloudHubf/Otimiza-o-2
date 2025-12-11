local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
   Name = "Otimizador ULTRA | +FPS & Utilitários",
   LoadingTitle = "Carregando Script...",
   LoadingSubtitle = "Versão 2.0 - Central Hub",
   ConfigurationSaving = {
      Enabled = true,
      FolderName = "OtimizadorConfigv2",
      FileName = "Configv2"
   },
   Discord = {
      Enabled = false,
      Invite = "noinvitelink", 
      RememberJoins = true 
   },
   KeySystem = false, 
})

-- NOTIFICAÇÃO
Rayfield:Notify({
   Title = "Script Atualizado",
   Content = "Nova aba JOGADOR adicionada!",
   Duration = 5,
   Image = 4483362458,
})

-- ABAS
local TabRender = Window:CreateTab("Renderização", 4483362458)
local TabLight = Window:CreateTab("Iluminação", 4483362458)
local TabPlayer = Window:CreateTab("Jogador", 4483362458) -- Nova Aba
local TabMisc = Window:CreateTab("Outros", 4483362458)

----------------------------------------------------------------
-- ABA JOGADOR (NOVAS FUNÇÕES: WalkSpeed, Jump, NoClip, etc.)
----------------------------------------------------------------

-- 12. WalkSpeed (Velocidade)
TabPlayer:CreateSlider({
   Name = "Velocidade (WalkSpeed)",
   Range = {16, 500},
   Increment = 1,
   Suffix = "Speed",
   CurrentValue = 16,
   Flag = "WalkSpeedSlider", 
   Callback = function(Value)
       pcall(function()
           game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = Value
       end)
   end,
})

-- 13. JumpPower (Pulo)
TabPlayer:CreateSlider({
   Name = "Força do Pulo (JumpPower)",
   Range = {50, 500},
   Increment = 1,
   Suffix = "Power",
   CurrentValue = 50,
   Flag = "JumpPowerSlider", 
   Callback = function(Value)
       pcall(function()
           game.Players.LocalPlayer.Character.Humanoid.JumpPower = Value
       end)
   end,
})

-- 14. Infinite Jump (Pulo Infinito)
local InfiniteJumpEnabled = false
game:GetService("UserInputService").JumpRequest:Connect(function()
	if InfiniteJumpEnabled then
		game:GetService"Players".LocalPlayer.Character:FindFirstChildOfClass'Humanoid':ChangeState("Jumping")
	end
end)

TabPlayer:CreateToggle({
   Name = "Pulo Infinito",
   CurrentValue = false,
   Flag = "InfJump",
   Callback = function(Value)
       InfiniteJumpEnabled = Value
   end,
})

-- 15. NoClip (Atravessar Paredes)
local Noclip = nil
local Clip = nil

function noclip()
	Clip = false
	local function Nocl()
		if Clip == false and game.Players.LocalPlayer.Character ~= nil then
			for _,v in pairs(game.Players.LocalPlayer.Character:GetDescendants()) do
				if v:IsA('BasePart') and v.CanCollide and v.Name ~= floatName then
					v.CanCollide = false
				end
			end
		end
		wait(0.21)
	end
	Noclip = game:GetService('RunService').Stepped:Connect(Nocl)
end

function clip()
	if Noclip then Noclip:Disconnect() end
	Clip = true
end

TabPlayer:CreateToggle({
   Name = "NoClip (Atravessar Paredes)",
   CurrentValue = false,
   Flag = "NoClipToggle",
   Callback = function(Value)
       if Value then
           noclip()
       else
           clip()
       end
   end,
})

-- 16. FOV Slider (Campo de Visão)
TabPlayer:CreateSlider({
   Name = "Campo de Visão (FOV)",
   Range = {70, 120},
   Increment = 1,
   Suffix = "º",
   CurrentValue = 70,
   Flag = "FOVSlider", 
   Callback = function(Value)
       game.Workspace.CurrentCamera.FieldOfView = Value
   end,
})

----------------------------------------------------------------
-- ABA RENDERIZAÇÃO (Melhorias e Novas Opções)
----------------------------------------------------------------

-- 1. Remover Sombras
TabRender:CreateButton({
   Name = "1. Remover Sombras (Global)",
   Callback = function()
       game.Lighting.GlobalShadows = false
       Rayfield:Notify({Title = "Sucesso", Content = "Sombras OFF", Duration = 2})
   end,
})

-- 2. Modo Plástico
TabRender:CreateButton({
   Name = "2. Modo Plástico (Super Leve)",
   Callback = function()
       for _, v in pairs(game:GetService("Workspace"):GetDescendants()) do
           if v:IsA("BasePart") and not v:IsA("MeshPart") then
               v.Material = Enum.Material.Plastic
               v.Reflectance = 0
           elseif v:IsA("Decal") or v:IsA("Texture") then
               v:Destroy()
           end
       end
   end,
})

-- 3. Remover Texturas
TabRender:CreateButton({
   Name = "3. Deletar Texturas",
   Callback = function()
       for _, v in pairs(game:GetService("Workspace"):GetDescendants()) do
           if v:IsA("Decal") or v:IsA("Texture") then
               v:Destroy()
           end
       end
   end,
})

-- 17. Remover Terreno e Água (NOVA)
TabRender:CreateButton({
   Name = "17. Otimizar Terreno (Sem Água/Grama)",
   Callback = function()
       local Terrain = game.Workspace.Terrain
       Terrain.WaterWaveSize = 0
       Terrain.WaterReflectance = 0
       Terrain.WaterTransparency = 0
       Terrain.Decoration = false
       Rayfield:Notify({Title = "Terreno", Content = "Água e Grama otimizados.", Duration = 2})
   end,
})

-- 18. Invisible Mode (Local) (NOVA)
TabRender:CreateButton({
   Name = "18. Ficar Invisível (Apenas Visual)",
   Callback = function()
       local Char = game.Players.LocalPlayer.Character
       for _, v in pairs(Char:GetDescendants()) do
           if v:IsA("BasePart") or v:IsA("Decal") then
               v.Transparency = 1
           end
       end
   end,
})

----------------------------------------------------------------
-- ABA ILUMINAÇÃO
----------------------------------------------------------------

-- 6. Full Bright
TabLight:CreateButton({
   Name = "6. Full Bright (Luz Infinita)",
   Callback = function()
       game.Lighting.Brightness = 2
       game.Lighting.ClockTime = 14
       game.Lighting.FogEnd = 100000
       game.Lighting.GlobalShadows = false
   end,
})

-- 19. Remover Neblina (Fog) (NOVA)
TabLight:CreateButton({
   Name = "19. Remover Neblina (No Fog)",
   Callback = function()
       game.Lighting.FogEnd = 9e9
       game.Lighting.FogStart = 9e9
   end,
})

-- 7. Remover Blur/Bloom
TabLight:CreateButton({
   Name = "7. Remover Efeitos de Luz",
   Callback = function()
       for _, v in pairs(game:GetService("Lighting"):GetChildren()) do
           if v:IsA("PostEffect") then
               v.Enabled = false
           end
       end
   end,
})

----------------------------------------------------------------
-- ABA OUTROS (Utilitários)
----------------------------------------------------------------

-- 20. Interação Instantânea (NOVA)
TabMisc:CreateButton({
   Name = "20. Interação Instantânea (E)",
   Callback = function()
       for _, v in pairs(game:GetService("Workspace"):GetDescendants()) do
           if v:IsA("ProximityPrompt") then
               v.HoldDuration = 0
           end
       end
       Rayfield:Notify({Title = "Prompt", Content = "Segure 'E' instantaneamente.", Duration = 2})
   end,
})

-- 21. Anti-AFK (NOVA)
TabMisc:CreateToggle({
   Name = "21. Anti-AFK (Não cair por inatividade)",
   CurrentValue = false,
   Flag = "AntiAFK",
   Callback = function(Value)
       if Value then
           local vu = game:GetService("VirtualUser")
           game:GetService("Players").LocalPlayer.Idled:Connect(function()
               vu:Button2Down(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
               wait(1)
               vu:Button2Up(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
           end)
           Rayfield:Notify({Title = "Anti-AFK", Content = "Ativado!", Duration = 2})
       end
   end,
})

-- 22. Mutar Sons do Jogo (NOVA)
TabMisc:CreateButton({
   Name = "22. Deletar Sons (Mute Geral)",
   Callback = function()
       for _, v in pairs(game:GetService("Workspace"):GetDescendants()) do
           if v:IsA("Sound") then
               v:Stop()
               v.Volume = 0
           end
       end
   end,
})

-- 23. Rejoin Server (NOVA)
TabMisc:CreateButton({
   Name = "23. Reentrar no Servidor (Rejoin)",
   Callback = function()
       game:GetService("TeleportService"):Teleport(game.PlaceId, game.Players.LocalPlayer)
   end,
})

-- 9. Limpar Memória
TabMisc:CreateButton({
   Name = "9. Limpar RAM (Garbage Collect)",
   Callback = function()
       local GC = getgc or function() end
       GC()
   end,
})

-- 10. FPS Cap
TabMisc:CreateSlider({
   Name = "10. Limite de FPS",
   Range = {30, 360},
   Increment = 10,
   Suffix = "FPS",
   CurrentValue = 60,
   Flag = "FPSCap", 
   Callback = function(Value)
       if setfpscap then setfpscap(Value) end
   end,
})
