--// Identity (for client-sided edits)
local setidentity = setidentity or set_thread_identity or (syn and syn.set_thread_identity)
if setidentity then setidentity(8) end
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
-- Cleanup old GUIs
local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")
for _, c in ipairs(playerGui:GetChildren()) do
if c.Name == "CompactPetGUI" then c:Destroy() end
end
-- Main ScreenGui
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "CompactPetGUI"
screenGui.ResetOnSpawn = false
screenGui.Parent = playerGui
-- Container - SMALLER VERTICAL RECTANGLE
local container = Instance.new("Frame")
container.Name = "Container"
container.Parent = screenGui
container.AnchorPoint = Vector2.new(0.5,0.5)
container.Position = UDim2.new(0.5,0,0.5,0)
container.Size = UDim2.new(0,120,0,200)
container.BackgroundColor3 = Color3.fromRGB(18,18,20)
container.BorderSizePixel = 0
container.Active = true
container.Draggable = true
Instance.new("UICorner",container).CornerRadius = UDim.new(0,8)
local stroke = Instance.new("UIStroke")
stroke.Parent = container
stroke.Thickness = 2
stroke.Color = Color3.fromRGB(75, 0, 130) -- DARK PURPLE border
-- Title - SMALLER TEXT
local title = Instance.new("TextLabel")
title.Parent = container
title.Size = UDim2.new(1,0,0,20)
title.Position = UDim2.new(0,0,0.02,0)
title.BackgroundTransparency = 1
title.Font = Enum.Font.GothamBold
title.TextSize = 14
title.Text = "jenners fake proof"
title.TextColor3 = Color3.new(1,1,1)
-- Tabs - SMALLER
local tabBar = Instance.new("Frame")
tabBar.Parent = container
tabBar.Size = UDim2.new(0.9,0,0,18)
tabBar.Position = UDim2.new(0.5,0,0.12,0)
tabBar.AnchorPoint = Vector2.new(0.5,0)
tabBar.BackgroundTransparency = 1
local tabNames = {"Pets","Toys","Names"}
local tabs, currentTab = {}, "Pets"
local savedInputs = {Pets={From="",To=""},Toys={From="",To=""},Names={From="",To=""}}
for i,name in ipairs(tabNames) do
local btn = Instance.new("TextButton")
btn.Parent = tabBar
btn.Size = UDim2.new(1/3,-2,1,0)
btn.Position = UDim2.new((i-1)/3,0,0,0)
btn.BackgroundColor3 = (i==1) and Color3.fromRGB(80,80,120) or
Color3.fromRGB(60,60,100)
btn.Font = Enum.Font.GothamBold
btn.TextSize = 10
btn.TextColor3 = Color3.new(1,1,1)
btn.Text = name
Instance.new("UICorner",btn).CornerRadius = UDim.new(0,4)
table.insert(tabs,btn)
end
-- Inputs - SMALLER
local fromBox = Instance.new("TextBox")
fromBox.Parent = container
fromBox.Size = UDim2.new(0.85,0,0,18)
fromBox.Position = UDim2.new(0.5,0,0.25,0)
fromBox.AnchorPoint = Vector2.new(0.5,0)
fromBox.BackgroundColor3 = Color3.fromRGB(30,30,30)
fromBox.PlaceholderText = "From"
fromBox.Text = ""
fromBox.ClearTextOnFocus = false
fromBox.TextColor3 = Color3.new(1,1,1)
fromBox.Font = Enum.Font.Gotham
fromBox.TextSize = 10
Instance.new("UICorner",fromBox).CornerRadius = UDim.new(0,4)
local toBox = fromBox:Clone()
toBox.Parent = container
toBox.Position = UDim2.new(0.5,0,0.38,0)
toBox.PlaceholderText = "To"
-- small util
local function trim(s) return (s or ""):match("^%s*(.-)%s*$") or "" end
-- Dropdown creation function - ADJUSTED FOR SMALLER GUI
local function createListDropdown(textBox, itemsSource)
local toggleBtn = Instance.new("TextButton")
toggleBtn.Size = UDim2.new(0,16,0,16)
toggleBtn.Position = UDim2.new(1,-18,0.5,0)
toggleBtn.AnchorPoint = Vector2.new(1,0.5)
toggleBtn.BackgroundColor3 = Color3.fromRGB(50,50,50)
toggleBtn.TextColor3 = Color3.new(1,1,1)
toggleBtn.Font = Enum.Font.GothamBold
toggleBtn.TextSize = 10
toggleBtn.Text = ">"
toggleBtn.Visible = false
Instance.new("UICorner",toggleBtn).CornerRadius = UDim.new(0,3)
toggleBtn.Parent = textBox
local dropFrame = Instance.new("ScrollingFrame")
dropFrame.Parent = container
dropFrame.Size = UDim2.new(0.85,0,0,80)
dropFrame.Position = textBox.Position + UDim2.new(0,0,0.15,0)
dropFrame.AnchorPoint = Vector2.new(0.5,0)
dropFrame.BackgroundColor3 = Color3.fromRGB(25,25,25)
dropFrame.Visible = false
dropFrame.ScrollBarThickness = 4
dropFrame.CanvasSize = UDim2.new(0,0,0,0)
dropFrame.ZIndex = 5
Instance.new("UICorner",dropFrame).CornerRadius = UDim.new(0,4)
local uiList = Instance.new("UIListLayout",dropFrame)
uiList.SortOrder = Enum.SortOrder.Name
uiList.Padding = UDim.new(0,1)
local function getItems()
if type(itemsSource) == "function" then
local ok, res = pcall(itemsSource)
return ok and res or {}
else
return itemsSource or {}
end
end
local function refreshList()
for _,child in ipairs(dropFrame:GetChildren()) do
if child:IsA("TextButton") then child:Destroy() end
end
local items = getItems()
for _,item in ipairs(items) do
local btn = Instance.new("TextButton")
btn.Parent = dropFrame
btn.Size = UDim2.new(1,0,0,22)
btn.BackgroundColor3 = Color3.fromRGB(40,40,40)
btn.TextColor3 = Color3.new(1,1,1)
btn.Font = Enum.Font.Gotham
btn.TextSize = 10
btn.ZIndex = 6
btn.Text = item
btn.MouseButton1Click:Connect(function()
textBox.Text = btn.Text
dropFrame.Visible = false
toggleBtn.Text = ">"
end)
end
dropFrame.CanvasSize = UDim2.new(0,0,0,#items*23)
end
toggleBtn.MouseButton1Click:Connect(function()
local open = dropFrame.Visible
dropFrame.Visible = not open
toggleBtn.Text = open and ">" or "<"
if not open then refreshList() end
end)
return toggleBtn, dropFrame, refreshList
end
-- YOUR PET LIST
local petsList = {
"bat dragon", "shadow dragon", "evil unicorn", "crow", "giraﬀe",
"parrot", "diamond butterfly", "owl", "frost dragon", "giant panda",
"balloon unicorn", "monkey king", "arctic reindeer", "hedgehog", "flamingo",
"turtle", "kangaroo"
}
-- YOUR TOY LIST
local toysList = {
"rainbow rattle", "candy cannon", "flying broomstick", "tombstone ghostify",
}
-- Dropdowns for Pets and Toys
local petsDropdownBtn, petsDropdownFrame, refreshPetsDropdown =
createListDropdown(toBox, petsList)
local toysDropdownBtn, toysDropdownFrame, refreshToysDropdown =
createListDropdown(toBox, toysList)
-- Player helper for Names tab
local function getPlayerUsernames()
local names = {}
for _,p in ipairs(Players:GetPlayers()) do
if p ~= player then
table.insert(names, p.Name)
end
end
table.sort(names, function(a,b) return a:lower() < b:lower() end)
return names
end
-- Player dropdowns for Names
local playerFromDropdownBtn, playerFromDropdownFrame, refreshPlayerFromDropdown =
createListDropdown(fromBox, getPlayerUsernames)
local playerToDropdownBtn, playerToDropdownFrame, refreshPlayerToDropdown =
createListDropdown(toBox, getPlayerUsernames)
-- =====================================================================
-- TRADE FUNCTIONALITY
-- =====================================================================
local function sendTradeToPlayer(playerName)
local targetPlayer = Players:FindFirstChild(playerName)
if targetPlayer and targetPlayer ~= player then
local success, result = pcall(function()
local Loads = require(ReplicatedStorage.Fsys).load
local RouterClient = Loads("RouterClient")
local SendTradeRequest = RouterClient.get("TradeAPI/SendTradeRequest")
SendTradeRequest:FireServer(targetPlayer)
return true
end)
return success
end
return false
end
-- Buttons - ADJUSTED FOR SMALLER LAYOUT
local swapBtn = Instance.new("TextButton")
swapBtn.Parent = container
swapBtn.Size = UDim2.new(0.85, 0, 0, 18)
swapBtn.Position = UDim2.new(0.5, 0, 0.55, 0)
swapBtn.AnchorPoint = Vector2.new(0.5, 0)
swapBtn.BackgroundColor3 = Color3.fromRGB(55,146,255)
swapBtn.Text = "Swap"
swapBtn.TextColor3 = Color3.new(1,1,1)
swapBtn.Font = Enum.Font.GothamBold
swapBtn.TextSize = 12
Instance.new("UICorner",swapBtn).CornerRadius = UDim.new(0,4)
-- SEND TRADE BUTTON
local sendTradeBtn = Instance.new("TextButton")
sendTradeBtn.Parent = container
sendTradeBtn.Size = UDim2.new(0.4, 0, 0, 18)
sendTradeBtn.Position = UDim2.new(0.55, 0, 0.55, 0)
sendTradeBtn.BackgroundColor3 = Color3.fromRGB(255, 165, 0)
sendTradeBtn.Text = "Trade"
sendTradeBtn.TextColor3 = Color3.new(1,1,1)
sendTradeBtn.Font = Enum.Font.GothamBold
sendTradeBtn.TextSize = 10
sendTradeBtn.Visible = false
Instance.new("UICorner",sendTradeBtn).CornerRadius = UDim.new(0,4)
-- REVERT BUTTON
local revertBtn = Instance.new("TextButton")
revertBtn.Parent = container
revertBtn.Size = UDim2.new(0.85, 0, 0, 18)
revertBtn.Position = UDim2.new(0.5, 0, 0.68, 0)
revertBtn.AnchorPoint = Vector2.new(0.5, 0)
revertBtn.BackgroundColor3 = Color3.fromRGB(206,69,69)
revertBtn.Text = "Revert"
revertBtn.TextColor3 = Color3.new(1,1,1)
revertBtn.Font = Enum.Font.GothamBold
revertBtn.TextSize = 12
Instance.new("UICorner",revertBtn).CornerRadius = UDim.new(0,4)
local status = Instance.new("TextLabel")
status.Parent = container
status.Size = UDim2.new(0.9,0,0,14)
status.Position = UDim2.new(0.5,0,0.88,0)
status.AnchorPoint = Vector2.new(0.5,0)
status.BackgroundTransparency = 1
status.TextColor3 = Color3.new(1,1,1)
status.Font = Enum.Font.Gotham
status.TextSize = 10
status.Text = "Ready"
-- Adopt Me internals
local load = require(ReplicatedStorage:WaitForChild("Fsys")).load
local inventoryDB = load("InventoryDB")
local backupPets, backupToys = {}, {}
-- Functions to change pets/toys
local function changePet(from,to)
local fromIndex, toIndex
for i,v in pairs(inventoryDB.pets) do
if v.name and v.name:lower() == from:lower() then fromIndex = i end
if v.name and v.name:lower() == to:lower() then toIndex = i end
end
if not fromIndex or not toIndex then return false end
backupPets[fromIndex] = table.clone(inventoryDB.pets[fromIndex])
local toClone = table.clone(inventoryDB.pets[toIndex])
for k,_ in pairs(inventoryDB.pets[fromIndex]) do
inventoryDB.pets[fromIndex][k] = toClone[k]
end
return true
end
local function changeToy(from,to)
local fromIndex, toIndex
for i,v in pairs(inventoryDB.toys) do
if v.name and v.name:lower() == from:lower() then fromIndex = i end
if v.name and v.name:lower() == to:lower() then toIndex = i end
end
if not fromIndex or not toIndex then return false end
backupToys[fromIndex] = table.clone(inventoryDB.toys[fromIndex])
local toClone = table.clone(inventoryDB.toys[toIndex])
for k,_ in pairs(inventoryDB.toys[fromIndex]) do
inventoryDB.toys[fromIndex][k] = toClone[k]
end
return true
end
local function revertPets() for i,v in pairs(backupPets) do inventoryDB.pets[i] = v end
backupPets = {} end
local function revertToys() for i,v in pairs(backupToys) do inventoryDB.toys[i] = v end
backupToys = {} end
-- NICKNAME SYSTEM
local LocalPlayer = player
local activeNicknameSwap = nil
-- Function to find trade UI elements
local function findTradeUI()
local tradeApp = LocalPlayer.PlayerGui:FindFirstChild("TradeApp")
if not tradeApp then return nil, nil end
local negotiationFrame = tradeApp.Frame:FindFirstChild("NegotiationFrame")
local confirmationFrame = tradeApp.Frame:FindFirstChild("ConfirmationFrame")
local partnerFrame1, partnerFrame2
if negotiationFrame then
local header = negotiationFrame:FindFirstChild("Header")
if header then
partnerFrame1 = header:FindFirstChild("PartnerFrame") or
header:FindFirstChild("PartnerLabel")
for _, child in pairs(header:GetChildren()) do
if child:IsA("TextLabel") and child.Text ~= "" and child.Text ~= LocalPlayer.Name then
if string.find(child.Text, "Trade") == nil and string.find(child.Text, "Oﬀer") == nil then
partnerFrame1 = partnerFrame1 or child
end
end
end
end
end
if confirmationFrame then
partnerFrame2 = confirmationFrame:FindFirstChild("PartnerLabel")
if not partnerFrame2 then
for _, child in pairs(confirmationFrame:GetDescendants()) do
if child:IsA("TextLabel") and child.Text ~= "" and child.Text ~= LocalPlayer.Name then
if string.find(child.Text, "Trade") == nil and string.find(child.Text, "Oﬀer") == nil then
partnerFrame2 = child
break
end
end
end
end
if not partnerFrame2 then
partnerFrame2 = confirmationFrame:FindFirstChild("PartnerFrame")
end
end
return partnerFrame1, partnerFrame2
end
-- Function to apply nickname
local function applyNickname(newName)
if not newName or newName == "" then return false end
local partnerFrame1, partnerFrame2 = findTradeUI()
local success = false
if partnerFrame1 then
if partnerFrame1.Parent:FindFirstChild("ClonedPartnerFrame") then
partnerFrame1.Parent.ClonedPartnerFrame:Destroy()
end
local clone1 = partnerFrame1:Clone()
clone1.Name = "ClonedPartnerFrame"
clone1.Parent = partnerFrame1.Parent
if clone1:IsA("TextLabel") then
clone1.Text = newName
success = true
else
for _, child in pairs(clone1:GetDescendants()) do
if child:IsA("TextLabel") and (child.Name == "NameLabel" or child.Name == "Name"
or child.Name == "TextLabel") then
child.Text = newName
success = true
break
end
end
end
clone1.Visible = true
partnerFrame1.Visible = false
end
if partnerFrame2 then
if partnerFrame2.Parent:FindFirstChild("ClonedPartnerFrame") then
partnerFrame2.Parent.ClonedPartnerFrame:Destroy()
end
local clone2 = partnerFrame2:Clone()
clone2.Name = "ClonedPartnerFrame"
clone2.Parent = partnerFrame2.Parent
if clone2:IsA("TextLabel") then
clone2.Text = newName
success = true
else
for _, child in pairs(clone2:GetDescendants()) do
if child:IsA("TextLabel") and (child.Name == "NameLabel" or child.Name == "Name"
or child.Name == "TextLabel") then
child.Text = newName
success = true
break
end
end
end
clone2.Visible = true
partnerFrame2.Visible = false
end
return success
end
-- Function to revert nicknames
local function revertNicknames()
local partnerFrame1, partnerFrame2 = findTradeUI()
if partnerFrame1 then
partnerFrame1.Visible = true
if partnerFrame1.Parent:FindFirstChild("ClonedPartnerFrame") then
partnerFrame1.Parent.ClonedPartnerFrame:Destroy()
end
end
if partnerFrame2 then
partnerFrame2.Visible = true
if partnerFrame2.Parent:FindFirstChild("ClonedPartnerFrame") then
partnerFrame2.Parent.ClonedPartnerFrame:Destroy()
end
end
activeNicknameSwap = nil
end
-- TRADE MONITORING
local function monitorTradeWindow()
local tradeApp = LocalPlayer.PlayerGui:WaitForChild("TradeApp")
local negotiationFrame = tradeApp.Frame.NegotiationFrame
local confirmationFrame = tradeApp.Frame.ConfirmationFrame
local function applyNicknameIfNeeded()
if activeNicknameSwap then
wait(0.3)
local success = applyNickname(activeNicknameSwap)
if success then
status.Text = "✓ Applied: " .. activeNicknameSwap
status.TextColor3 = Color3.fromRGB(100, 255, 100)
else
wait(0.5)
success = applyNickname(activeNicknameSwap)
if success then
status.Text = "✓ Applied: " .. activeNicknameSwap
status.TextColor3 = Color3.fromRGB(100, 255, 100)
end
end
else
local fromName = trim(fromBox.Text)
local toName = trim(toBox.Text)
if toName ~= "" and currentTab == "Names" then
if fromName == "" then
local success = applyNickname(toName)
if success then
status.Text = "✓ Auto: " .. toName
status.TextColor3 = Color3.fromRGB(100, 255, 100)
end
else
local partnerFrame1, partnerFrame2 = findTradeUI()
local originalText = ""
if partnerFrame1 then
if partnerFrame1:IsA("TextLabel") then
originalText = partnerFrame1.Text
else
for _, child in pairs(partnerFrame1:GetDescendants()) do
if child:IsA("TextLabel") and (child.Name == "NameLabel" or child.Name ==
"Name") then
originalText = child.Text
break
end
end
end
if originalText:lower() == fromName:lower() then
local success = applyNickname(toName)
if success then
status.Text = "✓ To " .. fromName
status.TextColor3 = Color3.fromRGB(100, 255, 100)
end
end
end
end
end
end
end
negotiationFrame:GetPropertyChangedSignal("Visible"):Connect(function()
if negotiationFrame.Visible then
wait(0.3)
applyNicknameIfNeeded()
else
if not confirmationFrame.Visible then
revertNicknames()
end
end
end)
confirmationFrame:GetPropertyChangedSignal("Visible"):Connect(function()
if confirmationFrame.Visible then
wait(0.4)
applyNicknameIfNeeded()
wait(0.8)
applyNicknameIfNeeded()
else
if not negotiationFrame.Visible then
revertNicknames()
end
end
end)
end
-- Tab switching - ADJUSTED FOR SMALLER GUI
local function switchTab(tabName)
savedInputs[currentTab].From = fromBox.Text
savedInputs[currentTab].To = toBox.Text
currentTab = tabName
for _,b in ipairs(tabs) do b.BackgroundColor3 = Color3.fromRGB(60,60,100) end
for _,b in ipairs(tabs) do if b.Text==tabName then b.BackgroundColor3 =
Color3.fromRGB(80,80,120) end end
fromBox.Text = savedInputs[currentTab].From
toBox.Text = savedInputs[currentTab].To
status.Text = tabName.." tab"
-- Show/hide dropdowns
petsDropdownBtn.Visible = (tabName=="Pets")
petsDropdownFrame.Visible = false
toysDropdownBtn.Visible = (tabName=="Toys")
toysDropdownFrame.Visible = false
playerFromDropdownBtn.Visible = (tabName=="Names")
playerFromDropdownFrame.Visible = false
playerToDropdownBtn.Visible = (tabName=="Names")
playerToDropdownFrame.Visible = false
-- Button layout
if tabName == "Names" then
sendTradeBtn.Visible = true
swapBtn.Visible = true
swapBtn.Size = UDim2.new(0.4, 0, 0, 18)
swapBtn.Position = UDim2.new(0.05, 0, 0.55, 0)
swapBtn.AnchorPoint = Vector2.new(0, 0)
sendTradeBtn.Size = UDim2.new(0.4, 0, 0, 18)
sendTradeBtn.Position = UDim2.new(0.55, 0, 0.55, 0)
revertBtn.Position = UDim2.new(0.5, 0, 0.68, 0)
revertBtn.Size = UDim2.new(0.85, 0, 0, 18)
revertBtn.Text = "Revert Name"
else
sendTradeBtn.Visible = false
swapBtn.Visible = true
swapBtn.Size = UDim2.new(0.85, 0, 0, 18)
swapBtn.Position = UDim2.new(0.5, 0, 0.55, 0)
swapBtn.AnchorPoint = Vector2.new(0.5, 0)
revertBtn.Position = UDim2.new(0.5, 0, 0.68, 0)
revertBtn.Size = UDim2.new(0.85, 0, 0, 18)
revertBtn.Text = "Revert"
end
if tabName=="Names" then
refreshPlayerFromDropdown()
refreshPlayerToDropdown()
end
end
for _,btn in ipairs(tabs) do
btn.MouseButton1Click:Connect(function() switchTab(btn.Text) end)
end
switchTab("Pets")
Players.PlayerAdded:Connect(function() refreshPlayerFromDropdown();
refreshPlayerToDropdown() end)
Players.PlayerRemoving:Connect(function()
refreshPlayerFromDropdown()
refreshPlayerToDropdown()
end)
-- Swap button logic
swapBtn.MouseButton1Click:Connect(function()
local from = trim(fromBox.Text)
local to = trim(toBox.Text)
if to == "" then
status.Text = "Enter To name!"
status.TextColor3 = Color3.fromRGB(255,100,100)
return
end
if currentTab=="Pets" then
local success = changePet(from,to)
status.Text = success and "Pet swapped!" or "Pet failed!"
status.TextColor3 = success and Color3.fromRGB(100,255,100) or
Color3.fromRGB(255,100,100)
elseif currentTab=="Toys" then
local success = changeToy(from,to)
status.Text = success and "Toy swapped!" or "Toy failed!"
status.TextColor3 = success and Color3.fromRGB(100,255,100) or
Color3.fromRGB(255,100,100)
elseif currentTab=="Names" then
activeNicknameSwap = to
if from == "" then
status.Text = "✓ Ready: " .. to
else
status.Text = "✓ For " .. from
end
status.TextColor3 = Color3.fromRGB(100,255,100)
local tradeApp = LocalPlayer.PlayerGui:FindFirstChild("TradeApp")
if tradeApp and tradeApp.Frame.NegotiationFrame.Visible then
wait(0.2)
local success = applyNickname(to)
if success then
status.Text = status.Text .. " ✓"
end
end
end
end)
-- SEND TRADE BUTTON LOGIC
sendTradeBtn.MouseButton1Click:Connect(function()
local fromName = trim(fromBox.Text)
if fromName == "" then
status.Text = "Enter player name!"
status.TextColor3 = Color3.fromRGB(255, 165, 0)
return
end
local success = sendTradeToPlayer(fromName)
if success then
status.Text = "✓ Trade to: " .. fromName
status.TextColor3 = Color3.fromRGB(100, 255, 100)
else
status.Text = "❌ Failed: " .. fromName
status.TextColor3 = Color3.fromRGB(255, 100, 100)
end
end)
-- Revert button logic
revertBtn.MouseButton1Click:Connect(function()
if currentTab=="Pets" then
revertPets()
status.Text = "Pets reverted!"
elseif currentTab=="Toys" then
revertToys()
status.Text = "Toys reverted!"
elseif currentTab=="Names" then
revertNicknames()
activeNicknameSwap = nil
status.Text = "✓ Name reverted!"
end
status.TextColor3 = Color3.fromRGB(100,255,100)
end)
-- MANUAL TRADE DETECTION
spawn(function()
while true do
wait(0.5)
local tradeApp = LocalPlayer.PlayerGui:FindFirstChild("TradeApp")
if tradeApp then
local inNegotiation = tradeApp.Frame.NegotiationFrame.Visible
local inConfirmation = tradeApp.Frame.ConfirmationFrame.Visible
if (inNegotiation or inConfirmation) and activeNicknameSwap then
local success = applyNickname(activeNicknameSwap)
if success then
status.Text = "✓ In trade: " .. activeNicknameSwap
status.TextColor3 = Color3.fromRGB(100, 255, 100)
end
elseif not inNegotiation and not inConfirmation then
revertNicknames()
end
end
end
end)
-- Start monitoring
monitorTradeWindow()
status.Text = "✓ Loaded!"
status.TextColor3 = Color3.fromRGB(100,255,100)
-- Trade Notification Disabler
task.wait(3)
local function disableTradeNotifs()
pcall(function()
local Loads = require(game:GetService("ReplicatedStorage").Fsys).load
local NotificationController = Loads("Client/Controllers/NotificationController")
if NotificationController and NotificationController.Notify then
local originalNotify = NotificationController.Notify
NotificationController.Notify = function(self, notifType, message, ...)
if type(message) == "string" and message:lower():find("trade request sent to") then
return
end
end
end
end)
end
return originalNotify(self, notifType, message, ...)
disableTradeNotifs()
-- Clean notifications
spawn(function()
while task.wait(0.5) do
pcall(function()
for _, gui in pairs({game:GetService("CoreGui"), game.Players.LocalPlayer.PlayerGui})
do
for _, screenGui in pairs(gui:GetChildren()) do
if screenGui:IsA("ScreenGui") then
for _, child in pairs(screenGui:GetDescendants()) do
if (child:IsA("TextLabel") or child:IsA("TextButton")) and child.Text then
if child.Text:lower():find("trade request sent to") then
child:Destroy()
end
end
end
end
end
end
end)
end
end)
