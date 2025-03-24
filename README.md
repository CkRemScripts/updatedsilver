local LocalPlayer = game.Players.LocalPlayer

-- Setting the images for MagicHealth and Glow
LocalPlayer.PlayerGui.ScreenGui.MagicHealth.Health.Bar.Bar.Image = "rbxassetid://17136251960"
LocalPlayer.PlayerGui.ScreenGui.MagicHealth.Health.Glow.Image = "rbxassetid://17136251821"

-- Change the RGB color for the Health Bar
LocalPlayer.PlayerGui.ScreenGui.MagicHealth.Health.Bar.Bar.ImageColor3 = Color3.fromRGB(159, 161, 172)

local player = game.Players.LocalPlayer
local char = player.Character
local humanoid = char:WaitForChild("Humanoid")
local hot = player.PlayerGui:WaitForChild("Hotbar")
local hotbar = hot:WaitForChild("Backpack"):WaitForChild("Hotbar")

local buttonData = {
    {name = "1", text = "Psychic Punch"},
    {name = "2", text = "Telekinetic Barrage"},
    {name = "3", text = "Agile Kick"},
    {name = "4", text = "Silver's Uppercut"},
}

for _, data in pairs(buttonData) do
    local baseButton = hotbar:FindFirstChild(data.name).Base
    local ToolName = baseButton.ToolName
    ToolName.Text = data.text
end

local replacementAnimations = {
    ["10468665991"] = {id = "rbxassetid://13294790250", speed = 2},   -- Psychic Punch (or custom animation)
    ["10466974800"] = {id = "rbxassetid://17799224866", speed = 1},
    ["10471336737"] = {id = "rbxassetid://17838619895", speed = 1},
    ["12510170988"] = {id = "rbxassetid://18179181663", speed = 1},  -- Basic Animations
    
    ["10469493270"] = {id = "rbxassetid://17889458563", speed = 1.3},  -- Telekinetic Barrage
    ["10469630950"] = {id = "rbxassetid://17889461810", speed = 1.3}, 
    ["10469639222"] = {id = "rbxassetid://17889471098", speed = 1.3}, 
    ["10469643643"] = {id = "rbxassetid://17889290569", speed = 1.3},  

    ["11343318134"] = {id = "rbxassetid://17420452843", speed = 1.6},  -- Uppercut
    ["11365563255"] = {id = "rbxassetid://125518819402716", speed = 0.5},
    ["12983333733"] = {id = "rbxassetid://140164642047188", speed = 1.2},  -- Ultimate Animation
    ["13927612951"] = {id = "rbxassetid://136370737633649", speed = 0.5}, 
    ["12447707844"] = {id = "rbxassetid://119169968232874", speed = 2},  -- Ultimate Activation Animation

    ["15955393872"] = {id = "rbxassetid://16310343179", speed = 1},  -- Wall Combo 
    ["10503381238"] = {id = "rbxassetid://12510170988", speed = 1.0},  -- Uppercut
    ["10470104242"] = {id = "rbxassetid://18464372850", speed = 2.7},  -- Downslam 
    ["10479335397"] = {id = "rbxassetid://17838006839", speed = 1},  -- FDash
    ["10480793962"] = {id = "rbxassetid://10480796021", speed = 1.0},  -- RightDash
    ["10480796021"] = {id = "rbxassetid://10480793962", speed = 1.0},  -- LeftDash
    ["10470389827"] = {id = "rbxassetid://", speed = 1.0},  -- Block 
}

local function onAnimationPlayed(animationTrack)
    local animationId = animationTrack.Animation.AnimationId:match("%d+")
    local replacement = replacementAnimations[animationId]

    if replacement then
        animationTrack:Stop()
        local newAnimation = Instance.new("Animation")
        newAnimation.AnimationId = replacement.id
        local newTrack = humanoid:LoadAnimation(newAnimation)
        newTrack:Play()
        newTrack:AdjustSpeed(replacement.speed) -- Apply speed to the new animation
    end
end

humanoid.AnimationPlayed:Connect(onAnimationPlayed)

player.CharacterAdded:Connect(function(newCharacter)
    character = newCharacter
    humanoid = newCharacter:WaitForChild("Humanoid")
    humanoid.AnimationPlayed:Connect(onAnimationPlayed)
end)

local OriginalName1 = "Psychic Punch"
local OriginalName2 = "Telekinetic Barrage"
local OriginalName3 = "Agile Kick"
local OriginalName4 = "Silver's Uppercut"

local UltMovesetName1 = "Ultimate Psychic Punch"
local UltMovesetName2 = "Silver's Ultimate Barrage"
local UltMovesetName3 = "Silver's Master Kick"
local UltMovesetName4 = "Silver's Ultimate Uppercut"

local toolNamesToReplace = {
    ["1"] = {original = OriginalName1, new = UltMovesetName1},
    ["2"] = {original = OriginalName2, new = UltMovesetName2},
    ["3"] = {original = OriginalName3, new = UltMovesetName3},
    ["4"] = {original = OriginalName4, new = UltMovesetName4}
}

local function checkAndReplaceToolName()
    while char.Humanoid.Health > 0 do
        local hotbar = playerGui:FindFirstChild("Hotbar")
        if hotbar then
            local backpack = hotbar:FindFirstChild("Backpack")
            if backpack then
                local hotbarFrame = backpack:FindFirstChild("Hotbar")
                if hotbarFrame then
                    for buttonName, toolData in pairs(toolNamesToReplace) do
                        local baseButton = hotbarFrame:FindFirstChild(buttonName) and hotbarFrame[buttonName]:FindFirstChild("Base")
                        if baseButton then
                            local toolName = baseButton:FindFirstChild("ToolName")
                            if toolName and toolName.Text == toolData.original then
                                toolName.Text = toolData.new
                            end
                        end
                    end
                end
            end
        end
        wait()
    end
end

checkAndReplaceToolName()
