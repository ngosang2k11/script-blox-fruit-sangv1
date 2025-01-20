-- Tạo GUI
local ScreenGui = Instance.new("ScreenGui")
local Frame = Instance.new("Frame")
local Button1 = Instance.new("TĂNG CẤP ĐỘ")
local Button2 = Instance.new("DỊCH CHUYỂN")
local Button3 = Instance.new("THIẾT LẬP LẠI KĨ NĂNG)
local Button4 = Instance.new("AUTO LÀM NHIỆM VỤ")
local Button5 = Instance.new("FARM TRÁI")
local Button6 = Instance.new("TÌM RƯƠNG")

ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")
ScreenGui.Name = "NGÔ TẤN SANG BLOX FRUIT"

Frame.Parent = ScreenGui
Frame.Size = UDim2.new(0, 300, 0, 400)
Frame.Position = UDim2.new(0.5, -150, 0.5, -200)
Frame.BackgroundColor3 = Color3.fromRGB(50, 50, 50)

-- Tạo các nút bấm
Button1.Parent = Frame
Button1.Size = UDim2.new(0, 250, 0, 50)
Button1.Position = UDim2.new(0, 25, 0, 25)
Button1.Text = "Tăng Level"
Button1.BackgroundColor3 = Color3.fromRGB(100, 200, 100)

Button2.Parent = Frame
Button2.Size = UDim2.new(0, 250, 0, 50)
Button2.Position = UDim2.new(0, 25, 0, 85)
Button2.Text = "Teleport"
Button2.BackgroundColor3 = Color3.fromRGB(100, 100, 200)

Button3.Parent = Frame
Button3.Size = UDim2.new(0, 250, 0, 50)
Button3.Position = UDim2.new(0, 25, 0, 145)
Button3.Text = "Reset Skill"
Button3.BackgroundColor3 = Color3.fromRGB(200, 100, 100)

Button4.Parent = Frame
Button4.Size = UDim2.new(0, 250, 0, 50)
Button4.Position = UDim2.new(0, 25, 0, 205)
Button4.Text = "Tự động làm nhiệm vụ"
Button4.BackgroundColor3 = Color3.fromRGB(200, 200, 100)

Button5.Parent = Frame
Button5.Size = UDim2.new(0, 250, 0, 50)
Button5.Position = UDim2.new(0, 25, 0, 265)
Button5.Text = "Tìm trái ác quỷ"
Button5.BackgroundColor3 = Color3.fromRGB(255, 165, 0)

Button6.Parent = Frame
Button6.Size = UDim2.new(0, 250, 0, 50)
Button6.Position = UDim2.new(0, 25, 0, 325)
Button6.Text = "Tự động farm chest"
Button6.BackgroundColor3 = Color3.fromRGB(0, 255, 255)

-- Tăng Level
Button1.MouseButton1Click:Connect(function()
    local player = game.Players.LocalPlayer
    player.leaderstats.Level.Value = 100
    print("Level đã được tăng lên 100")
end)

-- Teleport
Button2.MouseButton1Click:Connect(function()
    local player = game.Players.LocalPlayer
    player.Character.HumanoidRootPart.CFrame = CFrame.new(0, 100, 0)
    print("Đã teleport đến tọa độ (0, 100, 0)")
end)

-- Reset Skill
Button3.MouseButton1Click:Connect(function()
    local player = game.Players.LocalPlayer
    player.leaderstats.SkillPoints.Value = 0
    print("Skill đã được reset về 0")
end)

-- Tự động làm nhiệm vụ
function findQuestGiver()
    for _, npc in pairs(workspace.NPCs:GetChildren()) do
        if npc:FindFirstChild("Quest") then
            return npc
        end
    end
    return nil
end

function completeQuest()
    local npc = findQuestGiver()
    if npc then
        local player = game.Players.LocalPlayer
        local character = player.Character
        local humanoidRootPart = character:WaitForChild("HumanoidRootPart")
        humanoidRootPart.CFrame = npc.HumanoidRootPart.CFrame
        wait(1)
        fireclickdetector(npc:FindFirstChild("ClickDetector"))
        print("Đang hoàn thành nhiệm vụ...")
    else
        print("Không tìm thấy NPC với nhiệm vụ.")
    end
end

Button4.MouseButton1Click:Connect(function()
    completeQuest()
end)

-- Tự động tìm trái ác quỷ
function findDevilFruit()
    for _, item in pairs(workspace:GetChildren()) do
        if item:IsA("Model") and item.Name == "DevilFruit" then
            local player = game.Players.LocalPlayer
            local character = player.Character
            local humanoidRootPart = character:WaitForChild("HumanoidRootPart")
            humanoidRootPart.CFrame = item.HumanoidRootPart.CFrame
            print("Đã tìm thấy trái ác quỷ!")
            return true
        end
    end
    return false
end

function autoFindDevilFruit()
    while true do
        if findDevilFruit() then
            break
        else
            wait(2)
        end
    end
end

Button5.MouseButton1Click:Connect(function()
    autoFindDevilFruit()
end)

-- Tự động farm chest
function findAndFarmChest()
    for _, item in pairs(workspace:GetChildren()) do
        if item:IsA("Model") and item.Name == "Chest" then
            local player = game.Players.LocalPlayer
            local character = player.Character
            local humanoidRootPart = character:WaitForChild("HumanoidRootPart")
            
            -- Di chuyển đến vị trí chest
            humanoidRootPart.CFrame = item.HumanoidRootPart.CFrame
            wait(1)  -- Đợi một chút để nhân vật đến nơi

            -- Mở chest (giả sử chest có ClickDetector để mở)
            local clickDetector = item:FindFirstChild("ClickDetector")
            if clickDetector then
                fireclickdetector(clickDetector)  -- Mô phỏng click vào chest
                print("Đã mở chest!")
            end
        end
    end
end

function autoFarmChest()
    while true do
        findAndFarmChest()  -- Tìm và farm chest
        wait(3)  -- Đợi một chút trước khi kiểm tra lại
    end
end

Button6.MouseButton1Click:Connect(function()
    autoFarmChest()  -- Bắt đầu farm chest khi nhấn nút
end)
