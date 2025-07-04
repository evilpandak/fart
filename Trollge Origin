local Players = game:GetService('Players')
local player = Players.LocalPlayer
local playerGui = player:WaitForChild('PlayerGui')
local autoBankEquipActive = false
local autoEquipToolName = ''
local autoBankEquipLoop = nil
setsimulationradius(999,999)
local gui = Instance.new('ScreenGui')
gui.Name = 'ModularGUI'
gui.ResetOnSpawn = false
gui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
gui.DisplayOrder = 10
gui.IgnoreGuiInset = true
gui.Parent = playerGui

local colors = {
    background = Color3.fromRGB(20, 20, 20),
    header = Color3.fromRGB(30, 30, 30),
    text = Color3.fromRGB(255, 255, 255),
    subtext = Color3.fromRGB(200, 200, 200),
    button = Color3.fromRGB(50, 50, 50),
    buttonHover = Color3.fromRGB(80, 80, 80),
    toggleOff = Color3.fromRGB(50, 50, 50),
    toggleOn = Color3.fromRGB(0, 200, 150),
    input = Color3.fromRGB(40, 40, 40),
    border = Color3.fromRGB(70, 70, 70),
    tabActive = Color3.fromRGB(40, 40, 40),
    tabInactive = Color3.fromRGB(30, 30, 30),
    resizeHandle = Color3.fromRGB(100, 100, 100),
}

-- Main frame with initial size
local mainFrame = Instance.new('Frame')
mainFrame.Name = 'MainFrame'
mainFrame.AnchorPoint = Vector2.new(0.5, 0.5)
mainFrame.BackgroundColor3 = colors.background
mainFrame.BorderSizePixel = 1
mainFrame.BorderColor3 = colors.border
mainFrame.Position = UDim2.new(0.5, 0, 0.5, 0)
mainFrame.Size = UDim2.new(0, 350, 0, 400) -- Larger initial size
mainFrame.ClipsDescendants = true
mainFrame.Parent = gui

-- Title bar
local titleBar = Instance.new('Frame')
titleBar.Name = 'TitleBar'
titleBar.BackgroundColor3 = colors.header
titleBar.BorderSizePixel = 0
titleBar.Size = UDim2.new(1, 0, 0, 28)
titleBar.Parent = mainFrame

local title = Instance.new('TextLabel')
title.Name = 'Title'
title.BackgroundTransparency = 1
title.Position = UDim2.new(0, 10, 0, 0)
title.Size = UDim2.new(1, -40, 1, 0)
title.Font = Enum.Font.Code
title.Text = 'BasicIsBetter! SUBSCRIBE!'
title.TextColor3 = colors.text
title.TextSize = 14
title.TextXAlignment = Enum.TextXAlignment.Left
title.Parent = titleBar

local closeButton = Instance.new('TextButton')
closeButton.Name = 'CloseButton'
closeButton.AnchorPoint = Vector2.new(1, 0.5)
closeButton.BackgroundTransparency = 1
closeButton.Position = UDim2.new(1, -10, 0.5, 0)
closeButton.Size = UDim2.new(0, 20, 0, 20)
closeButton.Font = Enum.Font.Code
closeButton.Text = 'X'
closeButton.TextColor3 = colors.text
closeButton.TextSize = 14
closeButton.Parent = titleBar

closeButton.MouseButton1Click:Connect(function()
    gui:Destroy()
end)

-- Make GUI draggable (works for both mouse and touch)
local dragging, dragInput, dragStart, startPos
local function updateDrag(input)
    local delta = input.Position - dragStart
    mainFrame.Position = UDim2.new(
        startPos.X.Scale,
        startPos.X.Offset + delta.X,
        startPos.Y.Scale,
        startPos.Y.Offset + delta.Y
    )
end

titleBar.InputBegan:Connect(function(input)
    if
        input.UserInputType == Enum.UserInputType.MouseButton1
        or input.UserInputType == Enum.UserInputType.Touch
    then
        dragging = true
        dragStart = input.Position
        startPos = mainFrame.Position

        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)

titleBar.InputChanged:Connect(function(input)
    if
        input.UserInputType == Enum.UserInputType.MouseMovement
        or input.UserInputType == Enum.UserInputType.Touch
    then
        dragInput = input
    end
end)

game:GetService('UserInputService').InputChanged:Connect(function(input)
    if input == dragInput and dragging then
        updateDrag(input)
    end
end)

-- Tab system with scrolling for many tabs
local tabContainer = Instance.new('Frame')
tabContainer.Name = 'TabContainer'
tabContainer.BackgroundColor3 = colors.header
tabContainer.BorderSizePixel = 0
tabContainer.Position = UDim2.new(0, 0, 0, 28)
tabContainer.Size = UDim2.new(1, 0, 0, 28)
tabContainer.ClipsDescendants = true
tabContainer.Parent = mainFrame

local tabScrollingFrame = Instance.new('ScrollingFrame')
tabScrollingFrame.Name = 'TabScroller'
tabScrollingFrame.BackgroundTransparency = 1
tabScrollingFrame.Size = UDim2.new(1, 0, 1, 0)
tabScrollingFrame.CanvasSize = UDim2.new(0, 0, 0, 0)
tabScrollingFrame.ScrollBarThickness = 4
tabScrollingFrame.ScrollingDirection = Enum.ScrollingDirection.X
tabScrollingFrame.AutomaticCanvasSize = Enum.AutomaticSize.X
tabScrollingFrame.Parent = tabContainer

local tabContentContainer = Instance.new('Frame')
tabContentContainer.Name = 'TabContentContainer'
tabContentContainer.BackgroundTransparency = 1
tabContentContainer.Position = UDim2.new(0, 10, 0, 56)
tabContentContainer.Size = UDim2.new(1, -20, 1, -76) -- Adjusted for resize handle
tabContentContainer.ClipsDescendants = true
tabContentContainer.Parent = mainFrame

-- Add resize handle (bottom right corner)
local resizeHandle = Instance.new('Frame')
resizeHandle.Name = 'ResizeHandle'
resizeHandle.BackgroundColor3 = colors.resizeHandle
resizeHandle.BorderSizePixel = 0
resizeHandle.Size = UDim2.new(0, 16, 0, 16)
resizeHandle.Position = UDim2.new(1, -16, 1, -16)
resizeHandle.Parent = mainFrame

-- Resize functionality (works for both mouse and touch)
local resizing = false
local resizeStartSize, resizeStartPos

local function updateResize(input)
    local delta = input.Position - resizeStartPos
    local newWidth = math.max(300, resizeStartSize.X.Offset + delta.X)
    local newHeight = math.max(200, resizeStartSize.Y.Offset + delta.Y)
    mainFrame.Size = UDim2.new(0, newWidth, 0, newHeight)
end

resizeHandle.InputBegan:Connect(function(input)
    if
        input.UserInputType == Enum.UserInputType.MouseButton1
        or input.UserInputType == Enum.UserInputType.Touch
    then
        resizing = true
        resizeStartSize = mainFrame.Size
        resizeStartPos = input.Position

        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                resizing = false
            end
        end)
    end
end)

resizeHandle.InputChanged:Connect(function(input)
    if
        input.UserInputType == Enum.UserInputType.MouseMovement
        or input.UserInputType == Enum.UserInputType.Touch
    then
        dragInput = input
    end
end)

game:GetService('UserInputService').InputChanged:Connect(function(input)
    if input == dragInput and resizing then
        updateResize(input)
    end
end)

-- Tab management
local tabs = {}
local currentTab = nil

local function UpdateFrameSize()
    if currentTab then
        -- Auto-size the content frame based on its contents
        currentTab.content.Size = UDim2.new(
            1,
            0,
            0,
            currentTab.currentYPosition
        )
    end
end

local function SwitchTab(tab)
    if currentTab then
        currentTab.content.Visible = false
        currentTab.button.BackgroundColor3 = colors.tabInactive
    end

    currentTab = tab
    currentTab.content.Visible = true
    currentTab.button.BackgroundColor3 = colors.tabActive
end

function CreateTab(tabName)
    local tabButton = Instance.new('TextButton')
    tabButton.Name = tabName .. 'TabButton'
    tabButton.BackgroundColor3 = colors.tabInactive
    tabButton.BorderSizePixel = 0
    tabButton.Size = UDim2.new(0, 100, 1, 0)
    tabButton.Font = Enum.Font.Code
    tabButton.Text = tabName
    tabButton.TextColor3 = colors.text
    tabButton.TextSize = 14
    tabButton.Parent = tabScrollingFrame

    -- Position the tab button after the previous tabs
    if #tabs > 0 then
        local lastTab = tabs[#tabs].button
        tabButton.Position = UDim2.new(
            0,
            lastTab.AbsolutePosition.X
                + lastTab.AbsoluteSize.X
                - tabScrollingFrame.AbsolutePosition.X,
            0,
            0
        )
    else
        tabButton.Position = UDim2.new(0, 0, 0, 0)
    end

    local contentFrame = Instance.new('ScrollingFrame')
    contentFrame.Name = tabName .. 'Content'
    contentFrame.BackgroundTransparency = 1
    contentFrame.Size = UDim2.new(1, 0, 1, 0)
    contentFrame.Visible = false
    contentFrame.ScrollBarThickness = 4
    contentFrame.ScrollingDirection = Enum.ScrollingDirection.Y
    contentFrame.AutomaticCanvasSize = Enum.AutomaticSize.Y
    contentFrame.Parent = tabContentContainer

    local contentLayout = Instance.new('UIListLayout')
    contentLayout.Padding = UDim.new(0, 5)
    contentLayout.Parent = contentFrame

    local tab = {
        name = tabName,
        button = tabButton,
        content = contentFrame,
        currentYPosition = 0,
        elementPadding = 3,
        sectionPadding = 10,
    }

    table.insert(tabs, tab)

    tabButton.MouseButton1Click:Connect(function()
        -- Hide all other tab contents first
        for _, otherTab in ipairs(tabs) do
            otherTab.content.Visible = false
            otherTab.button.BackgroundColor3 = colors.tabInactive
        end

        -- Show this tab's content
        contentFrame.Visible = true
        tabButton.BackgroundColor3 = colors.tabActive
    end)

    -- If this is the first tab, make it active
    if #tabs == 1 then
        contentFrame.Visible = true
        tabButton.BackgroundColor3 = colors.tabActive
    end

    -- Create functions for this tab
    local tabFunctions = {}

    function tabFunctions.CreateSection(titleText)
        local sectionFrame = Instance.new('Frame')
        sectionFrame.BackgroundTransparency = 1
        sectionFrame.Size = UDim2.new(1, 0, 0, 26)
        sectionFrame.LayoutOrder = tab.currentYPosition
        sectionFrame.Parent = tab.content

        local label = Instance.new('TextLabel')
        label.BackgroundTransparency = 1
        label.Size = UDim2.new(1, 0, 1, 0)
        label.Font = Enum.Font.Code
        label.Text = titleText
        label.TextColor3 = colors.text
        label.TextSize = 16
        label.TextXAlignment = Enum.TextXAlignment.Left
        label.TextStrokeTransparency = 0.9
        label.Parent = sectionFrame

        local divider = Instance.new('Frame')
        divider.BackgroundColor3 = colors.border
        divider.BorderSizePixel = 0
        divider.Position = UDim2.new(0, 0, 1, -1)
        divider.Size = UDim2.new(1, 0, 0, 1)
        divider.Parent = sectionFrame

        tab.currentYPosition = tab.currentYPosition + 26 + tab.elementPadding
        return sectionFrame
    end
    -- [Previous code remains the same until the CreateTab function]

    function tabFunctions.CreateButton(buttonName, callback)
        local button = Instance.new('TextButton')
        button.BackgroundColor3 = colors.button
        button.BorderSizePixel = 0
        button.Size = UDim2.new(1, 0, 0, 24)
        button.LayoutOrder = tab.currentYPosition
        button.Font = Enum.Font.Code
        button.Text = '> ' .. buttonName
        button.TextColor3 = colors.text
        button.TextSize = 14
        button.TextXAlignment = Enum.TextXAlignment.Left
        button.ClipsDescendants = true
        button.Parent = tab.content

        local originalColor = button.BackgroundColor3
        local hoverColor = colors.buttonHover

        button.MouseEnter:Connect(function()
            button.BackgroundColor3 = hoverColor
        end)

        button.MouseLeave:Connect(function()
            button.BackgroundColor3 = originalColor
        end)

        button.MouseButton1Click:Connect(function()
            callback()
        end)

        tab.currentYPosition = tab.currentYPosition + 24 + tab.elementPadding
        return button
    end

    function tabFunctions.CreateToggle(toggleName, defaultState, callback)
        local frame = Instance.new('Frame')
        frame.BackgroundTransparency = 1
        frame.Size = UDim2.new(1, 0, 0, 24)
        frame.LayoutOrder = tab.currentYPosition
        frame.Parent = tab.content

        local label = Instance.new('TextLabel')
        label.BackgroundTransparency = 1
        label.Size = UDim2.new(0.6, 0, 1, 0)
        label.Font = Enum.Font.Code
        label.Text = toggleName
        label.TextColor3 = colors.text
        label.TextSize = 14
        label.TextXAlignment = Enum.TextXAlignment.Left
        label.Parent = frame

        local toggle = Instance.new('TextButton')
        toggle.AnchorPoint = Vector2.new(1, 0)
        toggle.Position = UDim2.new(1, 0, 0, 0)
        toggle.Size = UDim2.new(0.35, 0, 1, 0)
        toggle.BackgroundColor3 = defaultState and colors.toggleOn
            or colors.toggleOff
        toggle.Text = defaultState and 'ON' or 'OFF'
        toggle.Font = Enum.Font.Code
        toggle.TextColor3 = colors.text
        toggle.TextSize = 14
        toggle.BorderSizePixel = 0
        toggle.Parent = frame

        local state = defaultState
        toggle.MouseButton1Click:Connect(function()
            state = not state
            toggle.Text = state and 'ON' or 'OFF'
            toggle.BackgroundColor3 = state and colors.toggleOn
                or colors.toggleOff
            callback(state)
        end)

        tab.currentYPosition = tab.currentYPosition + 24 + tab.elementPadding
        return toggle
    end

    function tabFunctions.CreateDropdown(titleText, getOptionsFunc, callback)
        local dropdownFrame = {}
        local dropdownButton = Instance.new('TextButton')
        dropdownButton.BackgroundColor3 = colors.input
        dropdownButton.BorderSizePixel = 0
        dropdownButton.Size = UDim2.new(1, 0, 0, 24)
        dropdownButton.LayoutOrder = tab.currentYPosition
        dropdownButton.Font = Enum.Font.Code
        dropdownButton.Text = titleText .. ': [Select]'
        dropdownButton.TextColor3 = colors.text
        dropdownButton.TextSize = 14
        dropdownButton.TextXAlignment = Enum.TextXAlignment.Left
        dropdownButton.ClipsDescendants = true
        dropdownButton.Parent = tab.content

        local optionHolder = Instance.new('ScrollingFrame')
        optionHolder.BackgroundColor3 = colors.background
        optionHolder.BorderSizePixel = 1
        optionHolder.BorderColor3 = colors.border
        optionHolder.Size = UDim2.new(0, 0, 0, 0) -- start hidden with 0 size
        optionHolder.CanvasSize = UDim2.new(0, 0, 0, 0)
        optionHolder.ScrollBarThickness = 4
        optionHolder.ScrollingDirection = Enum.ScrollingDirection.Y
        optionHolder.ZIndex = 2
        optionHolder.Visible = false
        optionHolder.Parent = gui -- parent outside UIListLayout to control position freely
        optionHolder.AutomaticCanvasSize = Enum.AutomaticSize.Y
        optionHolder.ClipsDescendants = true

        local uiList = Instance.new('UIListLayout')
        uiList.Padding = UDim.new(0, 2)
        uiList.SortOrder = Enum.SortOrder.LayoutOrder
        uiList.Parent = optionHolder

        local isOpen = false
        local optionButtons = {}
        local selectedOption = nil

        local maxHeight = 100
        local optionHeight = 22 + 2 -- option button height + padding

        function dropdownFrame:ClearOptions()
            for _, btn in ipairs(optionButtons) do
                btn:Destroy()
            end
            optionButtons = {}
        end

        function dropdownFrame:PopulateOptions()
            self:ClearOptions()
            local options = getOptionsFunc()
            for _, option in ipairs(options) do
                local opt = Instance.new('TextButton')
                opt.BackgroundColor3 = colors.input
                opt.BorderSizePixel = 0
                opt.Size = UDim2.new(1, 0, 0, 22)
                opt.Font = Enum.Font.Code
                opt.Text = '→ ' .. option
                opt.TextColor3 = colors.subtext
                opt.TextSize = 13
                opt.TextXAlignment = Enum.TextXAlignment.Left
                opt.ClipsDescendants = true
                opt.Parent = optionHolder

                opt.MouseButton1Click:Connect(function()
                    selectedOption = option
                    dropdownButton.Text = titleText .. ': [' .. option .. ']'
                    callback(option)
                    isOpen = false
                    optionHolder.Visible = false
                    optionHolder.Size = UDim2.new(0, 0, 0, 0)
                    if renderStepConn then
                        renderStepConn:Disconnect()
                        renderStepConn = nil
                    end
                end)

                table.insert(optionButtons, opt)
            end

            local totalHeight = math.min(#options * optionHeight, maxHeight)
            optionHolder.Size = UDim2.new(
                dropdownButton.AbsoluteSize.X / gui.AbsoluteSize.X,
                0,
                0,
                totalHeight
            )
        end

        local RunService = game:GetService('RunService')
        local renderStepConn = nil

        local function updateOptionHolderPosition()
            local buttonPos = dropdownButton.AbsolutePosition
            local buttonSize = dropdownButton.AbsoluteSize
            local guiPos = gui.AbsolutePosition

            -- Calculate position relative to the GUI
            local relativeX = buttonPos.X - guiPos.X
            local relativeY = (buttonPos.Y - guiPos.Y) + buttonSize.Y

            optionHolder.Position = UDim2.new(0, relativeX, 0, relativeY)
            optionHolder.Size = UDim2.new(
                0,
                buttonSize.X,
                optionHolder.Size.Y.Scale,
                optionHolder.Size.Y.Offset
            )
        end

        dropdownButton.MouseButton1Click:Connect(function()
            isOpen = not isOpen
            if isOpen then
                dropdownFrame:PopulateOptions()
                optionHolder.Visible = true
                updateOptionHolderPosition()
                -- Keep dropdown positioned while GUI moves
                renderStepConn = RunService.RenderStepped:Connect(
                    updateOptionHolderPosition
                )
            else
                optionHolder.Visible = false
                optionHolder.Size = UDim2.new(0, 0, 0, 0)
                if renderStepConn then
                    renderStepConn:Disconnect()
                    renderStepConn = nil
                end
            end
        end)

        tab.currentYPosition = tab.currentYPosition + 24 + tab.elementPadding
        return dropdownFrame
    end

    function tabFunctions.CreateLabel(labelText)
        local label = Instance.new('TextLabel')
        label.BackgroundTransparency = 1
        label.Size = UDim2.new(1, 0, 0, 20)
        label.LayoutOrder = tab.currentYPosition
        label.Font = Enum.Font.Code
        label.Text = labelText
        label.TextColor3 = colors.subtext
        label.TextSize = 12
        label.TextXAlignment = Enum.TextXAlignment.Left
        label.Parent = tab.content

        tab.currentYPosition = tab.currentYPosition + 20 + tab.elementPadding
        return label
    end

    function tabFunctions.CreateTextbox(labelText, defaultText, callback)
        local frame = Instance.new('Frame')
        frame.BackgroundTransparency = 1
        frame.Size = UDim2.new(1, 0, 0, 24)
        frame.LayoutOrder = tab.currentYPosition
        frame.Parent = tab.content

        local label = Instance.new('TextLabel')
        label.BackgroundTransparency = 1
        label.Size = UDim2.new(0.4, 0, 1, 0)
        label.Font = Enum.Font.Code
        label.Text = labelText
        label.TextColor3 = colors.text
        label.TextSize = 14
        label.TextXAlignment = Enum.TextXAlignment.Left
        label.Parent = frame

        local textbox = Instance.new('TextBox')
        textbox.AnchorPoint = Vector2.new(1, 0)
        textbox.Position = UDim2.new(1, 0, 0, 0)
        textbox.Size = UDim2.new(0.6, 0, 1, 0)
        textbox.BackgroundColor3 = colors.input
        textbox.Text = defaultText or ''
        textbox.ClearTextOnFocus = false
        textbox.Font = Enum.Font.Code
        textbox.TextColor3 = colors.text
        textbox.TextSize = 14
        textbox.TextXAlignment = Enum.TextXAlignment.Left
        textbox.BorderSizePixel = 0
        textbox.Parent = frame

        local function submitText()
            local text = textbox.Text
            if text and text:match('%S') then -- text contains at least one non-space char
                local success, err = pcall(function()
                    callback(text)
                end)
                if success then
                    textbox.Text = '' -- clear only on success
                else
                    warn('Error in callback:', err)
                end
            end
        end

        textbox.FocusLost:Connect(function(enterPressed)
            if enterPressed then
                submitText()
            end
        end)

        tab.currentYPosition = tab.currentYPosition + 24 + tab.elementPadding
        return textbox
    end

    return tabFunctions
end
--ETC
local ESP = {
    enabled = false,
    objects = {},
    highlightColor = Color3.fromRGB(255, 100, 100), -- Soft red color
    textColor = Color3.fromRGB(255, 255, 255), -- White text
}

-- Create highlight for a single player
function ESP:Create(player)
    if self.objects[player] then
        return
    end -- Skip if already exists

    local function setupESP(character)
        -- Clean up existing ESP if any
        if self.objects[player] then
            for _, obj in pairs(self.objects[player]) do
                obj:Destroy()
            end
        end
        self.objects[player] = {}

        -- Wait for character parts
        local head = character:WaitForChild('Head')

        -- Create full-body highlight
        local highlight = Instance.new('Highlight')
        highlight.Name = 'ESP_Highlight'
        highlight.Adornee = character
        highlight.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
        highlight.FillColor = self.highlightColor
        highlight.FillTransparency = 0.7
        highlight.OutlineColor = self.highlightColor
        highlight.OutlineTransparency = 0.3
        highlight.Enabled = self.enabled
        highlight.Parent = character
        table.insert(self.objects[player], highlight)

        -- Create name tag
        local billboard = Instance.new('BillboardGui')
        billboard.Name = 'ESP_Name'
        billboard.Adornee = head
        billboard.Size = UDim2.new(0, 200, 0, 50)
        billboard.StudsOffset = Vector3.new(0, 3.5, 0)
        billboard.AlwaysOnTop = true
        billboard.Enabled = self.enabled
        billboard.Parent = head

        local nameLabel = Instance.new('TextLabel')
        nameLabel.Text = player.Name
        nameLabel.Size = UDim2.new(1, 0, 1, 0)
        nameLabel.BackgroundTransparency = 1
        nameLabel.TextColor3 = self.textColor
        nameLabel.TextStrokeTransparency = 0
        nameLabel.Font = Enum.Font.Code
        nameLabel.TextSize = 18
        nameLabel.Visible = true
        nameLabel.Parent = billboard
        table.insert(self.objects[player], billboard)

        -- Handle character cleanup
        character.AncestryChanged:Connect(function(_, parent)
            if not parent then -- Character was destroyed
                if self.objects[player] then
                    for _, obj in pairs(self.objects[player]) do
                        obj:Destroy()
                    end
                    self.objects[player] = nil
                end
            end
        end)
    end

    -- Set up for current character
    if player.Character then
        setupESP(player.Character)
    end

    -- Set up for future characters
    player.CharacterAdded:Connect(function(character)
        setupESP(character)
    end)
end

-- Toggle ESP visibility
function ESP:SetEnabled(state)
    self.enabled = state
    for _, objects in pairs(self.objects) do
        for _, obj in pairs(objects) do
            if obj:IsA('Highlight') then
                obj.Enabled = state
            elseif obj:IsA('BillboardGui') then
                obj.Enabled = state
            end
        end
    end
end

-- Initialize ESP system
function ESP:Init()
    -- Existing players
    for _, player in ipairs(game:GetService('Players'):GetPlayers()) do
        if player ~= game:GetService('Players').LocalPlayer then
            self:Create(player)
        end
    end

    -- New players
    game:GetService('Players').PlayerAdded:Connect(function(player)
        self:Create(player)
    end)

    -- Player leaving cleanup
    game:GetService('Players').PlayerRemoving:Connect(function(player)
        if self.objects[player] then
            for _, obj in pairs(self.objects[player]) do
                obj:Destroy()
            end
            self.objects[player] = nil
        end
    end)
end

-- Initialize the ESP system
ESP:Init()
--ETC
local ownedNPCs = {} -- key = model name, value = model instance

-- Function to get current list of owned NPCs
local function getOwnedNPCNames()
    local localPlayer = game.Players.LocalPlayer
    ownedNPCs = {}

    for _, obj in ipairs(game.Workspace:GetDescendants()) do
        if
            obj:IsA('Model')
            and obj ~= localPlayer.Character
            and obj:FindFirstChild('Humanoid')
            and obj:FindFirstChild('HumanoidRootPart')
            and isnetworkowner(obj.HumanoidRootPart)
        then
            ownedNPCs[obj.Name] = obj
        end
    end

    local names = {}
    for name, _ in pairs(ownedNPCs) do
        table.insert(names, name)
    end
    table.sort(names)
    return names
end
-- Create tabs and populate them with content
local mainTab = CreateTab('Universal')
local trollgetab = CreateTab('Trollge Origin')
local settingsTab = CreateTab('Settings')

-- Main Tab Content
mainTab.CreateLabel('Like and Subscribe to BasicIsBetter!')
mainTab.CreateSection('Main Functions')

mainTab.CreateTextbox('Teleport To Player', 'Name Here', function(input)
    local inputTrimmed = input:match('^%s*(.-)%s*$') -- trim whitespace
    if inputTrimmed == '' then
        error('Please enter a valid player name.')
        return
    end

    local inputLower = inputTrimmed:lower()
    local matchedPlayer = nil

    -- Try to find player by exact name (case insensitive)
    for _, plr in pairs(game.Players:GetPlayers()) do
        if plr.Name:lower() == inputLower then
            matchedPlayer = plr
            break
        end
    end

    -- If not exact match, try partial match
    if not matchedPlayer then
        for _, plr in pairs(game.Players:GetPlayers()) do
            if plr.Name:lower():sub(1, #inputLower) == inputLower then
                matchedPlayer = plr
                break
            end
        end
    end

    if not matchedPlayer then
        error('Player not found: ' .. inputTrimmed)
    end

    print('Teleporting to player:', matchedPlayer.Name)
    local character = game.Players.LocalPlayer.Character
    if character and character:FindFirstChild('HumanoidRootPart') then
        character.HumanoidRootPart.CFrame = matchedPlayer.Character.HumanoidRootPart.CFrame
            + Vector3.new(0, 5, 0)
    else
        error('Your character is not loaded or missing HumanoidRootPart')
    end
end)

mainTab.CreateButton('Speed Boost', function()
    game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = game.Players.LocalPlayer.Character.Humanoid.WalkSpeed
        + 5
end)

mainTab.CreateToggle('Player ESP', false, function(state)
    ESP:SetEnabled(state)
    print('Player ESP:', state and 'ON' or 'OFF')
end)
-- Settings Tab Content
settingsTab.CreateSection('Options')
settingsTab.CreateLabel('Settings will NOT be saved automatically.. for now :3')

trollgetab.CreateSection('Farming')
trollgetab.CreateToggle('TP To Chests', false, function(state)
    tpCollectActive = state
    local player = game.Players.LocalPlayer

    if not tpCollectActive then
        return
    end

    if tpCollectLoop then
        tpCollectLoop = nil
    end

    tpCollectLoop = coroutine.create(function()
        while tpCollectActive do
            local character = player.Character
            local hrp = character
                and character:FindFirstChild('HumanoidRootPart')
            if not hrp then
                break
            end

            local chests = game.Workspace.chests:GetChildren()
            for _, chest in ipairs(chests) do
                if not tpCollectActive then
                    break
                end
                if
                    chest:IsA('BasePart')
                    and chest:FindFirstChild('ProximityPrompt')
                then
                    hrp.CFrame = chest.CFrame + Vector3.new(0, 2, 0)

                    repeat
                        fireproximityprompt(chest.ProximityPrompt, true)
                        task.wait() -- yield
                    until not chest.Parent or not tpCollectActive
                end
            end

            task.wait(0.1) -- quick cd
        end
    end)

    coroutine.resume(tpCollectLoop)
end)

trollgetab.CreateToggle('TP To Gold/other', false, function(state)
    tpCollectActive = state
    local player = game.Players.LocalPlayer
    local hrp = player.Character
        and player.Character:FindFirstChild('HumanoidRootPart')

    if not tpCollectActive then
        return -- stop here if turned off
    end

    if tpCollectLoop then
        tpCollectLoop = nil -- Reset old coroutine
    end

    tpCollectLoop = coroutine.create(function()
        while tpCollectActive do
            if
                not player.Character
                or not player.Character:FindFirstChild('HumanoidRootPart')
            then
                break
            end

            hrp = player.Character.HumanoidRootPart

            local gold = game.Workspace.other:GetChildren()
            for _, coin in ipairs(gold) do
                if not tpCollectActive then
                    break
                end
                if
                    coin:IsA('BasePart')
                    and coin:FindFirstChild('ProximityPrompt')
                then
                    hrp.CFrame = coin.CFrame + Vector3.new(0, 2, 0)
                    task.wait(0.15)
                    fireproximityprompt(coin.ProximityPrompt)
                    task.wait(0.1)
                end
            end

            task.wait(0.5)
        end
    end)
    coroutine.resume(tpCollectLoop)
end)
trollgetab.CreateToggle('Auto Collect Nearby Chests', false, function(state)
    autocollectActive = state
    local player = game.Players.LocalPlayer
    local hrp = player.Character
        and player.Character:FindFirstChild('HumanoidRootPart')

    if autocollectLoop then
        autocollectLoop:Disconnect()
        autocollectLoop = nil
    end

    if autocollectActive then
        autocollectLoop = game
            :GetService('RunService').Heartbeat
            :Connect(function()
                if not autocollectActive then
                    return
                end

                if
                    not player.Character
                    or not player.Character:FindFirstChild('HumanoidRootPart')
                then
                    return
                end
                hrp = player.Character.HumanoidRootPart

                for _, chest in pairs(game.Workspace.chests:GetChildren()) do
                    if
                        chest:IsA('BasePart')
                        and chest:FindFirstChild('ProximityPrompt')
                    then
                        local distance =
                            (chest.Position - hrp.Position).Magnitude
                        if distance < 22 then
                            fireproximityprompt(chest.ProximityPrompt)
                        end
                    end
                end
            end)
    end
end)
trollgetab.CreateToggle('Auto Collect Nearby Gold/other', false, function(state)
    autocollectActive = state
    local player = game.Players.LocalPlayer
    local hrp = player.Character
        and player.Character:FindFirstChild('HumanoidRootPart')

    if autocollectLoop then
        autocollectLoop:Disconnect()
        autocollectLoop = nil
    end

    if autocollectActive then
        autocollectLoop = game
            :GetService('RunService').Heartbeat
            :Connect(function()
                if not autocollectActive then
                    return
                end

                if
                    not player.Character
                    or not player.Character:FindFirstChild('HumanoidRootPart')
                then
                    return
                end
                hrp = player.Character.HumanoidRootPart

                for _, gold in pairs(game.Workspace.other:GetChildren()) do
                    if
                        gold:IsA('BasePart')
                        and gold:FindFirstChild('ProximityPrompt')
                    then
                        local distance =
                            (gold.Position - hrp.Position).Magnitude
                        if distance < 22 then
                            fireproximityprompt(gold.ProximityPrompt)
                        end
                    end
                end
            end)
    end
end)
trollgetab.CreateSection('misc functions')
trollgetab.CreateButton('Grab All Dropped "Tools"', function()
    for i, v in pairs(game.Workspace:GetChildren()) do
        if v:IsA('Tool') and v:FindFirstChild('Handle') then
            wait()
            v:FindFirstChild('Handle').CFrame =
                game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame
        end
    end
end)
local toolNameInput = trollgetab.CreateTextbox(
    'Tool Name:',
    'ToolNameHere',
    function(inputText)
        -- This callback will be used when the text is submitted
        -- We'll store the input text for the toggle to use
        autoEquipToolName = inputText
    end
)

trollgetab.CreateToggle('Auto Equip For Bank', false, function(state)
    autoBankEquipActive = state
    local player = game.Players.LocalPlayer
    local character = player.Character or player.CharacterAdded:Wait()
    local inventory = player:FindFirstChild('Backpack')

    if not inventory then
        warn('Backpack not found!')
        return
    end

    -- Helper function to find and equip one item
    local function equipNext()
        if not autoBankEquipActive then
            return
        end

        -- Ensure we have a valid character reference
        character = player.Character or player.CharacterAdded:Wait()
        if not character then
            return
        end

        -- Check if already holding something
        local tool = character:FindFirstChildOfClass('Tool')
        if tool then
            return
        end

        -- Use the stored tool name or the current textbox value
        local targetToolName = autoEquipToolName or toolNameInput.Text
        if not targetToolName or targetToolName == '' then
            warn('No tool name specified for auto-equip!')
            return
        end

        -- Search backpack for matching tool
        for _, item in pairs(inventory:GetChildren()) do
            if
                item:IsA('Tool')
                and item.Name:lower() == targetToolName:lower()
            then
                item.Parent = character
                break
            end
        end
    end

    -- Cleanup previous connection
    if autoBankEquipLoop then
        autoBankEquipLoop:Disconnect()
        autoBankEquipLoop = nil
    end

    if autoBankEquipActive then
        -- Listen for when a tool is removed (banked)
        autoBankEquipLoop = character.ChildRemoved:Connect(function(child)
            if not autoBankEquipActive then
                return
            end
            if child:IsA('Tool') then
                local targetToolName = autoEquipToolName or toolNameInput.Text
                if child.Name:lower() == targetToolName:lower() then
                    task.wait(0.2) -- Small delay before re-equipping
                    equipNext()
                end
            end
        end)

        -- Equip first one immediately
        equipNext()
    end
end)
trollgetab.CreateDropdown('Kill NPC', getOwnedNPCNames, function(selectedName)
    local npc = ownedNPCs[selectedName]
    if npc and npc:FindFirstChild('Humanoid') then
        npc.Humanoid.Health = 0
    end
end)
--small little trollge origin update!!! killing NPCs only works if they're close enough / you're owner of their "parts"
--LIKE AND SUBSCRIBE!!!! HAVE A GOOD ONE
