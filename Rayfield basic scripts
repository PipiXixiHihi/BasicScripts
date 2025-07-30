loadstring(game:HttpGet("https://raw.githubusercontent.com/Quantum-Computing/Rayfield/main/source"))()(function(Rayfield)
    local Players = game:GetService("Players")
    local UserInputService = game:GetService("UserInputService")
    local RunService = game:GetService("RunService")
    local Workspace = workspace

    local player = Players.LocalPlayer
    local character = player.Character or player.CharacterAdded:Wait()
    local hrp = character:WaitForChild("HumanoidRootPart")

    -- Setup BodyVelocity and BodyGyro for Fly
    local bodyVelocity = Instance.new("BodyVelocity")
    bodyVelocity.MaxForce = Vector3.new(1e5, 1e5, 1e5)
    bodyVelocity.Velocity = Vector3.new(0, 0, 0)
    bodyVelocity.Parent = hrp
    bodyVelocity.Enabled = false

    local bodyGyro = Instance.new("BodyGyro")
    bodyGyro.MaxTorque = Vector3.new(1e5, 1e5, 1e5)
    bodyGyro.CFrame = hrp.CFrame
    bodyGyro.Parent = hrp
    bodyGyro.Enabled = false

    local flying = false
    local noclip = false
    local speed = 50
    local espEnabled = false

    local Window = Rayfield:CreateWindow({
        Name = "My Exploit UI",
        LoadingTitle = "Loading...",
        LoadingSubtitle = "by You",
        ConfigurationSaving = {
            Enabled = true,
            FolderName = nil,
            FileName = "MyExploitConfig"
        },
        Discord = {
            Enabled = false,
            Invite = "",
            RememberJoins = false
        },
        KeySystem = false
    })

    local PlayerSection = Window:CreateSection("Player")
    local VisualSection = Window:CreateSection("Visual")

    -- FLY TOGGLE
    PlayerSection:CreateToggle({
        Name = "Fly",
        CurrentValue = false,
        Flag = "FlyToggle",
        Callback = function(value)
            flying = value
            bodyVelocity.Enabled = value
            bodyGyro.Enabled = value
            if not value then
                hrp.Velocity = Vector3.new(0, 0, 0)
            end
        end
    })

    -- NOCLIP TOGGLE
    PlayerSection:CreateToggle({
        Name = "Noclip",
        CurrentValue = false,
        Flag = "NoclipToggle",
        Callback = function(value)
            noclip = value
        end
    })

    -- SPEED SLIDER
    PlayerSection:CreateSlider({
        Name = "Speed",
        Min = 16,
        Max = 250,
        Default = 50,
        Color = Color3.fromRGB(255, 0, 0),
        Increment = 1,
        ValueName = "Speed",
        Callback = function(value)
            speed = value
        end
    })

    -- ESP TOGGLE
    VisualSection:CreateToggle({
        Name = "ESP",
        CurrentValue = false,
        Flag = "ESP",
        Callback = function(value)
            espEnabled = value
            if not value then
                if Workspace:FindFirstChild("ESP_Container") then
                    Workspace.ESP_Container:Destroy()
                end
            else
                createESP()
            end
        end
    })

    -- Noclip implementation
    RunService.Stepped:Connect(function()
        if noclip then
            for _, part in pairs(character:GetChildren()) do
                if part:IsA("BasePart") then
                    part.CanCollide = false
                end
            end
        else
            for _, part in pairs(character:GetChildren()) do
                if part:IsA("BasePart") then
                    part.CanCollide = true
                end
            end
        end
    end)

    -- Fly movement update
    RunService.RenderStepped:Connect(function()
        if flying then
            local camera = workspace.CurrentCamera
            local moveDirection = Vector3.new(0, 0, 0)
            if UserInputService:IsKeyDown(Enum.KeyCode.W) then
                moveDirection = moveDirection + camera.CFrame.LookVector
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.S) then
                moveDirection = moveDirection - camera.CFrame.LookVector
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.A) then
                moveDirection = moveDirection - camera.CFrame.RightVector
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.D) then
                moveDirection = moveDirection + camera.CFrame.RightVector
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.Space) then
                moveDirection = moveDirection + Vector3.new(0, 1, 0)
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.LeftControl) then
                moveDirection = moveDirection - Vector3.new(0, 1, 0)
            end

            if moveDirection.Magnitude > 0 then
                bodyVelocity.Velocity = moveDirection.Unit * speed
            else
                bodyVelocity.Velocity = Vector3.new(0, 0, 0)
            end

            bodyGyro.CFrame = workspace.CurrentCamera.CFrame
        else
            bodyVelocity.Velocity = Vector3.new(0, 0, 0)
        end
    end)

    -- ESP function
    function createESP()
        if Workspace:FindFirstChild("ESP_Container") then
            Workspace.ESP_Container:Destroy()
        end

        local espFolder = Instance.new("Folder", Workspace)
        espFolder.Name = "ESP_Container"

        local function addESP(plr)
            if plr == player then return end

            local function createBillboard(character)
                local head = character:FindFirstChild("Head")
                if not head then return end

                local billboard = Instance.new("BillboardGui")
                billboard.Name = "ESP"
                billboard.Adornee = head
                billboard.Size = UDim2.new(0, 200, 0, 30)
                billboard.StudsOffset = Vector3.new(0, 2, 0)
                billboard.AlwaysOnTop = true
                billboard.Parent = espFolder

                local label = Instance.new("TextLabel", billboard)
                label.Size = UDim2.new(1, 0, 1, 0)
                label.BackgroundTransparency = 1
                label.Text = plr.Name
                label.TextColor3 = Color3.new(1, 0, 0)
                label.TextStrokeTransparency = 0.5
                label.TextScaled = true
                label.Font = Enum.Font.GothamBold
            end

            if plr.Character and plr.Character.Parent then
                createBillboard(plr.Character)
            end

            plr.CharacterAdded:Connect(function(char)
                wait(1)
                createBillboard(char)
            end)
        end

        for _, plr in pairs(Players:GetPlayers()) do
            addESP(plr)
        end

        Players.PlayerAdded:Connect(function(plr)
            addESP(plr)
        end)
    end
end)
