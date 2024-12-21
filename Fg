local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer
local CurrentTarget = nil -- เป้าหมาย Aimbot ปัจจุบัน
local AttractionStrength = 0.1 -- เริ่มต้นแรงดูดที่ 0.1
local maxAttractionStrength = 1
local minAttractionStrength = 0.01

-- UI Components
local function createUI()
    -- เช็คว่ามี UI อยู่แล้วหรือไม่ ถ้ามีให้ลบออก
    local existingUI = LocalPlayer:FindFirstChild("AimbotUI")
    if existingUI then
        existingUI:Destroy()
    end

    -- สร้าง ScreenGui
    local ScreenGui = Instance.new("ScreenGui", LocalPlayer:WaitForChild("PlayerGui"))
    ScreenGui.Name = "AimbotUI"

    local Frame = Instance.new("Frame", ScreenGui)
    Frame.Size = UDim2.new(0, 300, 0, 150)
    Frame.Position = UDim2.new(0.5, -150, 0.5, -75)
    Frame.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    Frame.BorderSizePixel = 2

    local Title = Instance.new("TextLabel", Frame)
    Title.Size = UDim2.new(1, 0, 0, 40)
    Title.BackgroundTransparency = 1
    Title.Text = "Aimbot Settings"
    Title.TextSize = 20
    Title.Font = Enum.Font.SourceSansBold
    Title.TextColor3 = Color3.fromRGB(255, 255, 255)

    -- แสดงข้อความสถานะ Aimbot
    local StatusLabel = Instance.new("TextLabel", Frame)
    StatusLabel.Size = UDim2.new(1, 0, 0, 30)
    StatusLabel.Position = UDim2.new(0, 0, 0, 40)
    StatusLabel.BackgroundTransparency = 1
    StatusLabel.Text = "Aimbot Target: None"
    StatusLabel.TextSize = 16
    StatusLabel.Font = Enum.Font.SourceSans
    StatusLabel.TextColor3 = Color3.fromRGB(255, 255, 255)

    -- แถบสไลด์เพื่อปรับแรงดูด
    local AttractionSlider = Instance.new("Slider", Frame)
    AttractionSlider.Size = UDim2.new(1, -10, 0, 40)
    AttractionSlider.Position = UDim2.new(0, 5, 0, 70)
    AttractionSlider.MinValue = minAttractionStrength
    AttractionSlider.MaxValue = maxAttractionStrength
    AttractionSlider.Value = AttractionStrength
    AttractionSlider.Changed:Connect(function()
        AttractionStrength = AttractionSlider.Value
    end)

    -- ฟังก์ชันสำหรับ Aimbot
    RunService.RenderStepped:Connect(function()
        if CurrentTarget and CurrentTarget.Character and CurrentTarget.Character:FindFirstChild("HumanoidRootPart") then
            local TargetPart = CurrentTarget.Character.HumanoidRootPart
            local Camera = Workspace.CurrentCamera
            Camera.CFrame = CFrame.new(Camera.CFrame.Position, TargetPart.Position)

            -- ถ้ามีแรงดูด
            if AttractionStrength > 0 then
                local playerPos = LocalPlayer.Character.HumanoidRootPart.Position
                local directionToTarget = (TargetPart.Position - playerPos).unit
                local attractionForce = directionToTarget * AttractionStrength * 10  -- ปรับแรงดูด
                LocalPlayer.Character:MoveTo(playerPos + attractionForce)
            end
        end
    end)

    -- เลือกผู้เล่นที่ใกล้ที่สุด
    local function selectClosestTarget()
        local closestDistance = math.huge
        local closestPlayer = nil

        for _, player in pairs(Players:GetPlayers()) do
            if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                local targetRoot = player.Character.HumanoidRootPart
                local distance = (LocalPlayer.Character.HumanoidRootPart.Position - targetRoot.Position).Magnitude
                if distance < closestDistance then
                    closestDistance = distance
                    closestPlayer = player
                end
            end
        end

        if closestPlayer then
            CurrentTarget = closestPlayer
            StatusLabel.Text = "Aimbot Target: " .. closestPlayer.Name
        else
            StatusLabel.Text = "Aimbot Target: None"
        end
    end

    -- ฟังก์ชันสำหรับการสบัด Aimbot
    local function clearTarget()
        CurrentTarget = nil
        StatusLabel.Text = "Aimbot Target: None"
    end

    -- การเลือก Aimbot โดยอัตโนมัติเมื่อกดปุ่ม 'E'
    game:GetService("UserInputService").InputBegan:Connect(function(input, gameProcessed)
        if gameProcessed then return end
        if input.KeyCode == Enum.KeyCode.E then
            selectClosestTarget()  -- เลือกเป้าหมายที่ใกล้ที่สุด
        elseif input.KeyCode == Enum.KeyCode.Q then
            clearTarget()  -- ยกเลิก Aimbot เมื่อกด Q
        end
    end)
end

-- สร้าง UI เมื่อเกมเริ่ม
createUI()
