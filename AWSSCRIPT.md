-- Get necessary services
local Players = game:GetService("Players")

-- Create a ScreenGui to hold the menu
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "MenuGui"
screenGui.Parent = Players.LocalPlayer.PlayerGui

-- Create a frame for the menu
local menuFrame = Instance.new("Frame")
menuFrame.Name = "MenuFrame"
menuFrame.Size = UDim2.new(0, 200, 0, 300)
menuFrame.Position = UDim2.new(0.5, -1000, 0.5, -150) -- Start off-screen
menuFrame.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
menuFrame.Parent = screenGui

-- Create a button to toggle the menu
local toggleButton = Instance.new("TextButton")
toggleButton.Name = "ToggleButton"
toggleButton.Size = UDim2.new(0, 80, 0, 30)
toggleButton.Position = UDim2.new(0, 10, 0, 10)
toggleButton.Text = "Toggle Menu"
toggleButton.Parent = screenGui

-- Create buttons for each action
local actionButtons = {}
local actionButtonText = {
    "Auto Heated Egg: Off",
    "Auto W9 Boss: Off",
    "AUTO P/H: Off",
    "Auto Craft Snacks: Off",
    "Auto Buy Apple Seeds: Off",
    "AUTO CATCH FISH: Off"
}
local yOffset = 50
for i = 1, 6 do
    local actionButton = Instance.new("TextButton")
    actionButton.Name = "ActionButton" .. i
    actionButton.Size = UDim2.new(0, 150, 0, 30)
    actionButton.Position = UDim2.new(0, 10, 0, yOffset)
    actionButton.Text = actionButtonText[i]
    actionButton.Parent = menuFrame
    table.insert(actionButtons, actionButton)
    yOffset = yOffset + 40
end

-- Create a button to remove the menu
local removeButton = Instance.new("TextButton")
removeButton.Name = "RemoveButton"
removeButton.Size = UDim2.new(0, 150, 0, 30)
removeButton.Position = UDim2.new(0, 10, 0, yOffset)
removeButton.Text = "Remove Menu"
removeButton.Parent = menuFrame

-- Function to toggle the menu visibility
local function ToggleMenuVisibility()
    local isMenuVisible = menuFrame.Position.X.Offset == 0
    local targetPosition = isMenuVisible and -1000 or 0
    menuFrame:TweenPosition(UDim2.new(0.5, targetPosition, 0.5, -150), "Out", "Quad", 0.3, true)
end

-- Function to toggle an action and update the button text
local function ToggleAction(index)
    local isToggled = false
    local actionCoroutine = nil
    
    return function()
        isToggled = not isToggled
        if isToggled then
            actionButtons[index].Text = actionButtonText[index]:gsub(": Off", ": On")
            actionButtons[index].TextColor3 = Color3.fromRGB(0, 255, 0) -- Green color for "Turn On" button
            
            -- Start the action
            actionCoroutine = coroutine.create(function()
                while isToggled do
                    if index == 1 then
                        -- Auto Heated Egg action code
                        spawn(function()
                            while isToggled do
                                local args = {
                                    [1] = "Heated",
                                    [2] = {
                                        ["Demonling"] = true,
                                        ["LavaBeetle"] = true,
                                        ["Wraith"] = true
                                    },
                                    [3] = true,
                                    [4] = true,
                                    [5] = true
                                }
                                game:GetService("ReplicatedStorage").Packages.Knit.Services.EggService.RF.purchaseEgg:InvokeServer(unpack(args))
                                wait(1)
                            end
                        end)
                    elseif index == 2 then
                        -- Auto W9 Boss action code
                        spawn(function()
                            while isToggled do
                                local args = {
                                    [1] = "MoltenBlaze",
                                    [2] = workspace.GameObjects.ArmWrestling:FindFirstChild("9").NPC.MoltenBlaze.Table,
                                    [3] = "9"
                                }
                                game:GetService("ReplicatedStorage").Packages.Knit.Services.ArmWrestleService.RE.onEnterNPCTable:FireServer(unpack(args))
                                wait(4)
                            end
                        end)
                    elseif index == 3 then
                        -- AUTO P/H action code
                        spawn(function()
                            local isPlanting = true
                            while isToggled do
                                for i = 1, 6 do
                                    local args
                                    if isPlanting then
                                        args = {
                                            [1] = "Apple Seeds",
                                            [2] = "1",
                                            [3] = tostring(i)
                                        }
                                        wait(0.5)
                                        game:GetService("ReplicatedStorage").Packages.Knit.Services.ItemPlantingService.RF.Plant:InvokeServer(unpack(args))
                                    else
                                        args = {
                                            [1] = tostring(i)
                                        }
                                        wait(0.1)
                                        game:GetService("ReplicatedStorage").Packages.Knit.Services.ItemPlantingService.RF.Harvest:InvokeServer(unpack(args))
                                    end
                                end
                                isPlanting = not isPlanting
                            end
                        end)
                    elseif index == 4 then
                        -- Auto Craft Snacks action code
                        spawn(function()
                            while isToggled do
                                for i = 1, 3 do
                                    local args_tier1 = {
                                        [1] = {
                                            ["Item"] = "Apple",
                                            ["Tier"] = 1
                                        }
                                    }
                                    
                                    game:GetService("ReplicatedStorage").Packages.Knit.Services.ItemCraftingService.RF.UpgradeSnack:InvokeServer(unpack(args_tier1))
                                    
                                    local args_tier2 = {
                                        [1] = {
                                            ["Item"] = "Apple",
                                            ["Tier"] = 2
                                        }
                                    }
                                    
                                    game:GetService("ReplicatedStorage").Packages.Knit.Services.ItemCraftingService.RF.UpgradeSnack:InvokeServer(unpack(args_tier2))
                                    
                                    wait(1) -- Wait for 1 second before next iteration
                                end
                            end
                        end)
                    elseif index == 5 then
                        -- Auto Buy Apple Seeds action code
                        spawn(function()
                            while isToggled do
                                local args = {
                                    [1] = "Farmer",
                                    [2] = 2
                                }
                                game:GetService("ReplicatedStorage").Packages.Knit.Services.MerchantService.RF.BuyItem:InvokeServer(unpack(args))
                                wait(1)
                            end
                        end)
                    elseif index == 6 then
                        -- AUTO CATCH FISH action code
                        spawn(function()
                            while isToggled do
                                local function fishLoop()
                                    local args = {
                                        [1] = true
                                    }
                      
                                    game:GetService("ReplicatedStorage").Packages.Knit.Services.NetService.RF.StartCatching:InvokeServer()
                                
                                    local args2 = {
                                        [1] = 1,
                                        [2] = 1
                                    }
                                
                                    game:GetService("ReplicatedStorage").Packages.Knit.Services.NetService.RF.VerifyCatch:InvokeServer(unpack(args2))
                                end
                                
                                while isToggled do
                                    fishLoop()
                                    wait(9999999)  -- Wait for 1 second before looping again
                                end
                            end
                        end)
                    end
                    wait(2) -- Adjust the delay as needed
                end
            end)
            coroutine.resume(actionCoroutine)
        else
            actionButtons[index].Text = actionButtonText[index]:gsub(": On", ": Off")
            actionButtons[index].TextColor3 = Color3.fromRGB(255, 0, 0) -- Red color for "Turn Off" button
            
            -- Stop the action
            if actionCoroutine then
                coroutine.yield(actionCoroutine)
            end
        end
    end
end

-- Function to remove the menu
local function RemoveMenu()
    screenGui:Destroy()
end

-- Connect the functions to the buttons' Click events
toggleButton.MouseButton1Click:Connect(ToggleMenuVisibility)
for i = 1, 6 do
    actionButtons[i].MouseButton1Click:Connect(ToggleAction(i))
end
removeButton.MouseButton1Click:Connect(RemoveMenu)
