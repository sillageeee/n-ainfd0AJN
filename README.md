local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
    Name = "Xenz Hub",
    Icon = 0, -- Icon in Topbar. Can use Lucide Icons (string) or Roblox Image (number). 0 to use no icon (default).
    LoadingTitle = "Xenz Cheats",
    LoadingSubtitle = "by Xenz Cheats",
    Theme = "Default", -- Check https://docs.sirius.menu/rayfield/configuration/themes
 
    DisableRayfieldPrompts = false,
    DisableBuildWarnings = false, -- Prevents Rayfield from warning when the script has a version mismatch with the interface
 
    ConfigurationSaving = {
       Enabled = true,
       FolderName = nil, -- Create a custom folder for your hub/game
       FileName = "XENZ CHEATS"
    },
 
    Discord = {
       Enabled = false, -- Prompt the user to join your Discord server if their executor supports it
       Invite = "noinvitelink", -- The Discord invite code, do not include discord.gg/. E.g. discord.gg/ ABCD would be ABCD
       RememberJoins = false -- Set this to false to make them join the discord every time they load it up
    },
 
local ESPEnabled = false
local ESPObjects = {}

local function CreateESP(player)
    local box = Drawing.new("Square")
    box.Thickness = 1
    box.Transparency = 1
    box.Color = Color3.fromRGB(255, 0, 0)
    box.Filled = false

    local nameText = Drawing.new("Text")
    nameText.Size = 14
    nameText.Center = true
    nameText.Outline = true
    nameText.Color = Color3.fromRGB(255, 255, 255)

    local infoText = Drawing.new("Text")
    infoText.Size = 13
    infoText.Center = true
    infoText.Outline = true
    infoText.Color = Color3.fromRGB(0, 255, 0)

    ESPObjects[player] = {Box = box, Name = nameText, Info = infoText}
end

local function RemoveESP(player)
    if ESPObjects[player] then
        for _, obj in pairs(ESPObjects[player]) do
            obj:Remove()
        end
        ESPObjects[player] = nil
    end
end

RunService.RenderStepped:Connect(function()
    if not ESPEnabled then
        for _, esp in pairs(ESPObjects) do
            for _, obj in pairs(esp) do
                obj.Visible = false
            end
        end
        return
    end

    for _, player in pairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            if not ESPObjects[player] then
                CreateESP(player)
            end

            local esp = ESPObjects[player]
            local root = player.Character:FindFirstChild("HumanoidRootPart")
            local head = player.Character:FindFirstChild("Head")
            local humanoid = player.Character:FindFirstChild("Humanoid")
            local pos, onScreen = Camera:WorldToViewportPoint(root.Position)

            if onScreen then
                local distance = math.floor((root.Position - Camera.CFrame.Position).Magnitude)
                local size = Vector2.new(50, 100) / (distance / 50)
                local topLeft = Vector2.new(pos.X - size.X / 2, pos.Y - size.Y / 2)

                -- Caixa
                esp.Box.Size = size
                esp.Box.Position = topLeft
                esp.Box.Visible = true

                -- Nome
                esp.Name.Position = Vector2.new(pos.X, topLeft.Y - 16)
                esp.Name.Text = player.Name
                esp.Name.Visible = true

                -- Vida + distância
                local health = math.floor(humanoid.Health)
                esp.Info.Position = Vector2.new(pos.X, pos.Y + size.Y / 2 + 5)
                esp.Info.Text = "HP: " .. health .. " | " .. distance .. "m"
                esp.Info.Visible = true
            else
                for _, obj in pairs(esp) do
                    obj.Visible = false
                end
            end
        else
            RemoveESP(player)
        end
    end
end)

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

local Aimbot = Window:CreateTab("Aimbot", 4483362458) -- Title, Image

local Toggle = Aimbot:CreateToggle({
   Name = "Aimbot",
   CurrentValue = false,
   Flag = "Toggle1", -- A flag is the identifier for the configuration file, make sure every element has a different flag if you're using configuration saving to ensure no overlaps
   Callback = function(Value)
   -- The function that takes place when the toggle is pressed
   -- The variable (Value) is a boolean on whether the toggle is true or false
   end,
})

local Visual = Window:CreateTab("Visual", 4483362458) -- Title, Image

local Toggle = Visual:CreateToggle({
   Name = "ESP",
   CurrentValue = false,
   Flag = "Toggle1", -- A flag is the identifier for the configuration file, make sure every element has a different flag if you're using configuration saving to ensure no overlaps
   Callback = function(Value)
        ESPEnabled = value
        if not value then
            for _, esp in pairs(ESPObjects) do
                for _, obj in pairs(esp) do
                    obj.Visible = false
                end
            end
        end
    end
})
