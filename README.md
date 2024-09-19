loadstring([[
Player and GUI setup
local player = game.Players.LocalPlayer
 Create ScreenGui
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "HubGui"
screenGui.Parent = player:WaitForChild("PlayerGui")
Create Frame for GUI elements
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0.3, 0, 0.5, 0)
frame.Position = UDim2.new(0.35, 0, 0.25, 0)
frame.BackgroundColor3 = Color3.new(0, 0, 0)
frame.BackgroundTransparency = 0.5
frame.Parent = screenGui
Create Button 1: Teleport to Spawn
local button1 = Instance.new("TextButton")
button1.Size = UDim2.new(0.8, 0, 0.25, 0)
button1.Position = UDim2.new(0.1, 0, 0.05, 0)
button1.Text = "Teleport to Spawn"
button1.Parent = frame
Create Button 2: Change Character Color
local button2 = Instance.new("TextButton")
button2.Size = UDim2.new(0.8, 0, 0.25, 0)
button2.Position = UDim2.new(0.1, 0, 0.35, 0)
button2.Text = "Change Character Color"
button2.Parent = frame
Create Button 3: Fly
local button3 = Instance.new("TextButton")
button3.Size = UDim2.new(0.8, 0, 0.25, 0)
button3.Position = UDim2.new(0.1, 0, 0.65, 0)
button3.Text = "Fly"
button3.Parent = frame
Variables for flying
local flying = false
local speed = 50 -- Flying speed
local bodyGyro, bodyVelocity
local UIS = game:GetService("UserInputService")
Function to teleport the player to the spawn location
local function teleportToSpawn()
    local character = player.Character or player.CharacterAdded:Wait()
    if character and workspace:FindFirstChild("SpawnLocation") then
        character:SetPrimaryPartCFrame(workspace.SpawnLocation.CFrame)
    end
end
 Function to change the character's head color
local function changeCharacterColor()
    local character = player.Character or player.CharacterAdded:Wait()
    local head = character:FindFirstChild("Head")
    if head then
        local color = Color3.new(math.random(), math.random(), math.random())
        head.BrickColor = BrickColor.new(color)
    else
        warn("Character does not have a Head.")
    end
end
Function to toggle flying mode
local function toggleFly()
    local character = player.Character or player.CharacterAdded:Wait()
    if not character then return end
   
    if flying then
        flying = false
        if bodyGyro then bodyGyro:Destroy() end
        if bodyVelocity then bodyVelocity:Destroy() end
        button3.Text = "Fly"  Change button text back
    else
        flying = true
        bodyGyro = Instance.new("BodyGyro")
        bodyGyro.Parent = character:FindFirstChild("HumanoidRootPart")
        bodyGyro.MaxTorque = Vector3.new(4000, 4000, 4000)
        bodyGyro.P = 3000
        bodyGyro.CFrame = character:FindFirstChild("HumanoidRootPart").CFrame
        bodyVelocity = Instance.new("BodyVelocity")
        bodyVelocity.Parent = character:FindFirstChild("HumanoidRootPart")
        bodyVelocity.Velocity = Vector3.new(0, 0, 0)
        bodyVelocity.MaxForce = Vector3.new(4000, 4000, 4000)
 Change button text to indicate flying mode
        button3.Text = "Stop Flying"
     Start the flying loop
        while flying do
            wait()
            local moveDirection = Vector3.new(0, 0, 0)
            if UIS:IsKeyDown(Enum.KeyCode.W) then
                moveDirection = moveDirection + workspace.CurrentCamera.CFrame.LookVector
            end
            if UIS:IsKeyDown(Enum.KeyCode.S) then
                moveDirection = moveDirection - workspace.CurrentCamera.CFrame.LookVector
            end
            if UIS:IsKeyDown(Enum.KeyCode.A) then
                moveDirection = moveDirection - workspace.CurrentCamera.CFrame.RightVector
            end
            if UIS:IsKeyDown(Enum.KeyCode.D) then
                moveDirection = moveDirection + workspace.CurrentCamera.CFrame.RightVector
            end
            bodyVelocity.Velocity = moveDirection.Unit * speed + Vector3.new(0, 5, 0) -- Add upward force for flying
            bodyGyro.CFrame = workspace.CurrentCamera.CFrame -- Keep the player facing the camera's direction
        end
    end
end
Connect button clicks to functions
button1.MouseButton1Click:Connect(teleportToSpawn)
button2.MouseButton1Click:Connect(changeCharacterColor)
button3.MouseButton1Click:Connect(toggleFly)
 Enable the GUI when the script is run
screenGui.Enabled = true
]])()
