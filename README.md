print("🐳")
local Rayfield = loadstring(game:HttpGet('https://raw.githubusercontent.com/Ddddddadcbw/ddad/refs/heads/main/README.md'))()
local Window = Rayfield:CreateWindow({
    Name = "🐳Whale Hub🐳",
    LoadingTitle = "🐳Whale Hub🐳",
    LoadingSubtitle = "구매 해주셔서 감사합니다 :) -🐳-",
    ConfigurationSaving = {
        Enabled = false,
        FolderName = nil,
        FileName = "Example Hub"
    },
    Discord = {
        Enabled = true,
        Invite = "https://discord.gg/ynFaBp5ehu",
        RememberJoins = true
    },
    KeySystem = true,
    KeySettings = {
        Title = "비밀번호",
        Subtitle = "비밀번호 입력 ",
        Note = "뀨뀨",
        FileName = "YoutubeHubKey1",
        SaveKey = false,
        GrabKeyFromSite = true,
        Key = {"https://raw.githubusercontent.com/Ddddddadcbw/ddada/refs/heads/main/README.md"}
    }
})

Rayfield:Notify({
    Title = "🐳 Whale Hub🐳가 정상적으로 실행됨.",
    Content = "감사합니다 :)🐳🐳",
    Duration = 6.5,
    Image = 4483362458,
})

local MainTab = Window:CreateTab("🐳", nil)
local MainSection = MainTab:CreateSection("🐳Main")

-- 자기 자신을 킥하는 버튼 추가
local kickButton = MainTab:CreateButton({
    Name = "🐳자기 자신 킥🐳",
    Callback = function()
        local player = game.Players.LocalPlayer
        -- 자기 자신을 킥
        player:Kick("🐳")  -- 퇴장 메시지도 설정할 수 있습니다.
    end,
})
local kick3Button = MainTab:CreateButton({
    Name = "🐳CE 올킬🐳",
    Callback = function()
        local ReplicatedStorage = game:GetService("ReplicatedStorage")
        local Players = game:GetService("Players")

        -- Getting the DamageEvent from ACS_Engine
        local ACS_Engine = ReplicatedStorage:WaitForChild("ACS_Engine", 3)
        local DamageEvent = ACS_Engine:WaitForChild("Events"):WaitForChild("Damage")

        -- Iterate over all players and apply damage to them
        for _, plr in pairs(Players:GetPlayers()) do
            if plr ~= Players.LocalPlayer then
                local humanoid = plr.Character and plr.Character:FindFirstChild("Humanoid")
                if humanoid then
                    -- Fire the DamageEvent with proper parameters
                    DamageEvent:FireServer(humanoid, 100000, "Torso", {'nil','Auth','nil','nil'})
                end
            end
        end
    end,
})
local kick34Button = MainTab:CreateButton({
    Name = "CE 경로 체크🐳",
    Callback = function()
        local ReplicatedStorage = game:GetService("ReplicatedStorage")
        local CResource = ReplicatedStorage:WaitForChild("CarbonResource", 3)
        local Players = game:GetService("Players")
        local Events = {}

        -- Kill the player by setting health to 0
        local playerCharacter = Players.LocalPlayer.Character
        if playerCharacter and playerCharacter:FindFirstChild("Humanoid") then
            playerCharacter.Humanoid.Health = 0
        end

        -- Wait for a short time before accessing events
        task.wait(0.3)

        -- Load events from the CarbonResource
        for idx, remote in pairs(CResource.Events:GetChildren()) do
            Events[remote.Name] = remote
        end

        -- Check and fire the DamageEvent if it exists
        local DamageEvent = Events["DamageEvent"]
        
        if DamageEvent then
            -- Fire the DamageEvent to deal damage (adjust parameters accordingly)
            local targetHumanoid = Players.LocalPlayer.Character and Players.LocalPlayer.Character:FindFirstChild("Humanoid")
            
            if targetHumanoid then
                -- Example of how you might fire the event, parameters may vary
                DamageEvent:FireServer(targetHumanoid, 100000, "Torso", {'nil', 'Auth', 'nil', 'nil'})
            else
                warn("No humanoid found in character.")
            end
        else
            warn("DamageEvent not found!")
        end
    end,
})
-- ACS 블럭 설치 버튼
local kick2Button = MainTab:CreateButton({
    Name = "ACS 블럭 설치🐳",
    Callback = function()
        local ReplicatedStorage = game:GetService("ReplicatedStorage")
        local Event = ReplicatedStorage:WaitForChild("ACS_Engine", 3).Events.Breach 
        local Players = game:GetService("Players")
        Event:InvokeServer(3, {Fortified = {}, Destroyable = workspace}, CFrame.new(), CFrame.new(), {
            CFrame = Players.LocalPlayer.Character:GetPivot().Position,
            Size = Vector3.new(10, 10, 10)
        })
    end,
})
-- ACS 올킬 버튼
local aButton = MainTab:CreateButton({
    Name = "ACS 올킬🐳",  -- 수정된 부분 (name → Name)
    Callback = function()
        local ReplicatedStorage = game:GetService("ReplicatedStorage")
        local Event = ReplicatedStorage:WaitForChild("ACS_Engine", 3).Events.Damage 
        local Players = game:GetService("Players")

        local gun = game:FindFirstChild("ACS_Settings", true).Parent
        local module = require(gun.ACS_Settings)

        for idx, plr in pairs(Players:GetPlayers()) do
            if plr ~= Players.LocalPlayer then
                Event:InvokeServer(gun, plr.Character.Humanoid, 25, 1, module, {
                    minDamageMod = 150, 
                    DamageMod = 150 
                }, nil, nil, "키를얻어서넣기")
            end
        end
    end,
})

-- 텔레포트 상태를 제어할 변수
local stopTeleport = false

-- 플레이어 텔레포트 입력 필드
local TPInput = MainTab:CreateInput({
    Name = "플레이어 이름 입력(계속tp)🐳",
    PlaceholderText = "플레이어 이름을 입력하세요🐳",  -- 플레이어 이름을 입력할 필드
    RemoveTextAfterFocusLost = true,
    Callback = function(Text)
        local player = game.Players.LocalPlayer
        local targetPlayerName = Text  -- 텍스트로 입력된 플레이어 이름

        -- 텔레포트할 플레이어 찾기
        local targetPlayer = game.Players:FindFirstChild(targetPlayerName)

        -- 텔레포트 실행
        if targetPlayer then
            -- 플레이어가 존재하면 계속해서 텔레포트 되도록
            stopTeleport = false  -- 텔레포트를 다시 시작하기 전에 멈춘 상태 초기화
            spawn(function()  -- spawn 함수를 사용하여 루프가 독립적으로 실행되도록 함
                while not stopTeleport do
                    if targetPlayer.Character and player.Character then
                        -- 나를 원하는 플레이어에게 계속 텔레포트
                        player.Character.HumanoidRootPart.CFrame = targetPlayer.Character.HumanoidRootPart.CFrame
                    end
                    wait(1)  -- 1초마다 텔레포트
                end
            end)
            
            game.StarterGui:SetCore("SendNotification", {
                Title = "텔레포트 시작🐳🐳🐳",
                Text = "지정된 플레이어에게 계속 텔레포트 됩니다.🐳🐳",
                Duration = 5
            })
        else
            game.StarterGui:SetCore("SendNotification", {
                Title = "텔레포트 실패🐳",
                Text = "플레이어를 찾을 수 없습니다.🐳",
                Duration = 5
            })
        end
    end,
})

-- 텔레포트를 멈추는 버튼 추가
local stopButton = MainTab:CreateButton({
    Name = "텔레포트 멈추기🐳",
    Callback = function()
        stopTeleport = true  -- stopTeleport를 true로 설정하여 텔레포트를 멈춥니다.
        game.StarterGui:SetCore("SendNotification", {
            Title = "텔레포트 종료🐳",
            Text = "텔레포트가 멈췄습니다.🐳",
            Duration = 5
        })
    end,
})

-- 벽 뚫기 상태를 제어할 변수
local canPassThroughWalls = false

local WallButton = MainTab:CreateButton({
   Name = "벽 뚫기 토글(시작/종료)🐳",
   Callback = function()
       local player = game:GetService("Players").LocalPlayer
       local character = player.Character or player.CharacterAdded:Wait()

       -- 벽 통과 상태 전환
       if not canPassThroughWalls then
           canPassThroughWalls = true
           -- 모든 파트의 CanCollide를 비활성화
           for _, part in ipairs(character:GetDescendants()) do
               if part:IsA("BasePart") then
                   part.CanCollide = false  -- 벽 뚫기 활성화
               end
           end
           game.StarterGui:SetCore("SendNotification", {
               Title = "벽 통과 시작🐳",
               Text = "벽을 뚫을 수 있습니다.🐳",
               Duration = 5
           })
       else
           canPassThroughWalls = false
           -- 모든 파트의 CanCollide를 활성화
           for _, part in ipairs(character:GetDescendants()) do
               if part:IsA("BasePart") then
                   part.CanCollide = true  -- 벽 통과 비활성화
               end
           end
           game.StarterGui:SetCore("SendNotification", {
               Title = "벽 통과 종료🐳",
               Text = "벽을 더 이상 뚫을 수 없습니다.🐳",
               Duration = 5
           })
       end
   end
})

-- 무한 점프 버튼
local Button = MainTab:CreateButton({
   Name = "무한 점프(실행 할려면 눌르세요)🐳",
   Callback = function()
       -- Toggles the infinite jump between on or off on every script run
       _G.infinjump = not _G.infinjump

       if _G.infinJumpStarted == nil then
           -- Ensures this only runs once to save resources
           _G.infinJumpStarted = true
           
           -- Notifies readiness
           game.StarterGui:SetCore("SendNotification", {Title="무한 점프🐳", Text="무한 점프가 가동됨.🐳", Duration=5})
           local plr = game:GetService('Players').LocalPlayer
           local m = plr:GetMouse()
           m.KeyDown:connect(function(k)
               if _G.infinjump then
                   if k:byte() == 32 then
                       local humanoid = game:GetService('Players').LocalPlayer.Character:FindFirstChildOfClass('Humanoid')
                       humanoid:ChangeState('Jumping')
                       wait()
                       humanoid:ChangeState('Seated')
                   end
               end
           end)
       end
   end,
})
local Input = MainTab:CreateInput({
    Name = "스피드핵 속도 제한 입력",
    PlaceholderText = "입력",  -- 플레이어 이름을 입력할 필드
    RemoveTextAfterFocusLost = true,
    Callback = function(alpha)
        local ad = alpha
})
local Slider = MainTab:CreateSlider({
   Name = "스피드 핵🐳",
   Range = {1, ad},
   Increment = 1,
   Suffix = "Speed",
   CurrentValue = 16,
   Flag = "sliderws", 
   Callback = function(Value)
        game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = (Value)
   end,
})
local Input = MainTab:CreateInput({
   Name = "Walkspeed",
   PlaceholderText = "숫자",
   RemoveTextAfterFocusLost = true,
   Callback = function(Text)
        game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = (Text)
   end,
})
local KillAllButton = MainTab:CreateButton({
    Name = "재설정",
    Callback = function()
        for _, player in ipairs(game.Players:GetPlayers()) do
            -- 캐릭터가 있을 경우
            if player.Character and player.Character:FindFirstChild("Humanoid") then
                local humanoid = player.Character:FindFirstChild("Humanoid")
                humanoid.Health = 0  -- 플레이어의 체력을 0으로 설정하여 죽임
            end
        end
        
        -- 알림 표시
        game.StarterGui:SetCore("SendNotification", {
            Title = "재설정 신호",
            Text = "재설정 완료!",
            Duration = 5
        })
    end,
 })
-- 플레이어 텔레포트 입력 필드
local TPInput = MainTab:CreateInput({
    Name = "플레이어 이름 입력",
    PlaceholderText = "플레이어 이름을 입력하세요",  -- 플레이어 이름을 입력할 필드
    RemoveTextAfterFocusLost = true,
    Callback = function(Text)
        local player = game.Players.LocalPlayer
        local targetPlayerName = Text  -- 텍스트로 입력된 플레이어 이름

        -- 텔레포트할 플레이어 찾기
        local targetPlayer = game.Players:FindFirstChild(targetPlayerName)
        if targetPlayer and targetPlayer.Character then
            -- 텔레포트 실행
            player.Character.HumanoidRootPart.CFrame = targetPlayer.Character.HumanoidRootPart.CFrame
            game.StarterGui:SetCore("SendNotification", {
                Title = "텔레포트 성공",
                Text = "플레이어 " .. targetPlayerName .. " 에게 텔레포트 되었습니다.",
                Duration = 5
            })
        else
            game.StarterGui:SetCore("SendNotification", {
                Title = "텔레포트 실패",
                Text = "플레이어를 찾을 수 없습니다.",
                Duration = 5
            })
        end
    end,
})
-- 위치 입력을 위한 텍스트 박스
local LocationInput = MainTab:CreateInput({
    Name = "텔레포트 위치 입력",
    PlaceholderText = "X, Y, Z 좌표 입력 (예: 0, 10, 0)",
    RemoveTextAfterFocusLost = true,
    Callback = function(Text)
        -- 텍스트에서 X, Y, Z 좌표를 파싱
        local x, y, z = Text:match("([%-?%d%.]+),%s*([%-?%d%.]+),%s*([%-?%d%.]+)")
        
        -- 입력된 좌표가 올바른지 확인
        if x and y and z then
            x, y, z = tonumber(x), tonumber(y), tonumber(z) -- 문자열을 숫자로 변환

            local player = game.Players.LocalPlayer
            -- 텔레포트
            player.Character.HumanoidRootPart.CFrame = CFrame.new(x, y, z)
            game.StarterGui:SetCore("SendNotification", {
                Title = "위치 텔레포트 성공",
                Text = "지정된 위치로 텔레포트 되었습니다.",
                Duration = 5
            })
        else
            game.StarterGui:SetCore("SendNotification", {
                Title = "잘못된 좌표 형식",
                Text = "좌표를 올바른 형식으로 입력해주세요. (예: 0, 10, 0)",
                Duration = 5
            })
        end
    end,
})

-- 비행 상태와 비행 속도 설정
local isFlying = false
local flyingSpeed = 50 -- 기본 비행 속도
local ascendSpeed = 25 -- 상승 속도
local descendSpeed = -25 -- 하강 속도
local moveSpeed = 25 -- 앞뒤 좌우 이동 속도

-- 비행을 위한 BodyVelocity 인스턴스
local bodyVelocity

-- 비행 토글 버튼
local FlyButton = FlyTab:CreateButton({
    Name = "비행 토글",
    Callback = function()
        local player = game.Players.LocalPlayer
        local character = player.Character or player.CharacterAdded:Wait()

        -- 비행 상태 전환
        if not isFlying then
            isFlying = true

            -- BodyVelocity 인스턴스 생성 (플레이어를 공중으로 띄우기 위해)
            bodyVelocity = Instance.new("BodyVelocity")
            bodyVelocity.MaxForce = Vector3.new(400000, 400000, 400000) -- 힘의 크기 설정 (비행을 위한 최소값)
            bodyVelocity.Velocity = Vector3.new(0, flyingSpeed, 0) -- 기본 비행 속도 설정 (수직으로 띄운다)
            bodyVelocity.Parent = character:WaitForChild("HumanoidRootPart")

            -- 알림 표시
            game.StarterGui:SetCore("SendNotification", {
                Title = "비행 시작",
                Text = "이제 비행할 수 있습니다.",
                Duration = 5
            })
        else
            isFlying = false

            -- 비행을 종료하고 BodyVelocity를 제거
            if bodyVelocity then
                bodyVelocity:Destroy()
            end

            -- 알림 표시
            game.StarterGui:SetCore("SendNotification", {
                Title = "비행 종료",
                Text = "비행이 종료되었습니다.",
                Duration = 5
            })
        end
    end,
})

-- 비행 속도 조정 슬라이더
local FlySpeedSlider = FlyTab:CreateSlider({
    Name = "비행 속도",
    Range = {10, 100},
    Increment = 1,
    Suffix = "Speed",
    CurrentValue = flyingSpeed,
    Flag = "flyspeed", -- 속도 조정 시 저장되는 값
    Callback = function(Value)
        flyingSpeed = Value
        if isFlying and bodyVelocity then
            bodyVelocity.Velocity = Vector3.new(0, flyingSpeed, 0)
        end
    end,
})

-- 상승 버튼
local AscendButton = FlyTab:CreateButton({
    Name = "이륙 (상승)",
    Callback = function()
        if isFlying and bodyVelocity then
            -- 상승 속도 적용
            bodyVelocity.Velocity = Vector3.new(0, ascendSpeed, 0)
            game.StarterGui:SetCore("SendNotification", {
                Title = "상승 시작",
                Text = "비행이 상승 모드로 변경되었습니다.",
                Duration = 5
            })
        else
            game.StarterGui:SetCore("SendNotification", {
                Title = "비행 시작 필요",
                Text = "비행을 먼저 시작해야 합니다.",
                Duration = 5
            })
        end
    end,
})

-- 하강 버튼
local DescendButton = FlyTab:CreateButton({
    Name = "하강",
    Callback = function()
        if isFlying and bodyVelocity then
            -- 하강 속도 적용
            bodyVelocity.Velocity = Vector3.new(0, descendSpeed, 0)
            game.StarterGui:SetCore("SendNotification", {
                Title = "하강 시작",
                Text = "비행이 하강 모드로 변경되었습니다.",
                Duration = 5
            })
        else
            game.StarterGui:SetCore("SendNotification", {
                Title = "비행 시작 필요",
                Text = "비행을 먼저 시작해야 합니다.",
                Duration = 5
            })
        end
    end,
})

-- 비행 중에 방향 조정(앞, 뒤, 좌, 우)
local function updateFlightDirection()
    if not isFlying or not bodyVelocity then return end

    local player = game.Players.LocalPlayer
    local character = player.Character
    local mouse = game:GetService("Players").LocalPlayer:GetMouse()

    -- 마우스 위치를 기준으로 이동 방향 설정
    local mousePos = mouse.Hit.p
    local direction = (mousePos - character.HumanoidRootPart.Position).unit

    -- 키 입력을 통한 앞뒤, 좌우 이동
    local moveDirection = Vector3.new(0, 0, 0)
    if game:GetService("UserInputService"):IsKeyDown(Enum.KeyCode.W) then
        moveDirection = moveDirection + character.HumanoidRootPart.CFrame.LookVector -- 앞으로 이동
    elseif game:GetService("UserInputService"):IsKeyDown(Enum.KeyCode.S) then
        moveDirection = moveDirection - character.HumanoidRootPart.CFrame.LookVector -- 뒤로 이동
    end
    if game:GetService("UserInputService"):IsKeyDown(Enum.KeyCode.A) then
        moveDirection = moveDirection - character.HumanoidRootPart.CFrame.RightVector -- 왼쪽으로 이동
    elseif game:GetService("UserInputService"):IsKeyDown(Enum.KeyCode.D) then
        moveDirection = moveDirection + character.HumanoidRootPart.CFrame.RightVector -- 오른쪽으로 이동
    end

    -- 최종적으로 이동 방향 계산 (수직 비행 속도 + 수평 이동)
    bodyVelocity.Velocity = direction * flyingSpeed + moveDirection * moveSpeed
end

-- 비행 중에 마우스 및 키 입력 처리
game:GetService("UserInputService").InputBegan:Connect(function(input, gameProcessed)
    if not isFlying then return end

    if input.UserInputType == Enum.UserInputType.MouseMovement then
        -- 마우스를 따라가는 비행
        updateFlightDirection()
    end

    if input.UserInputType == Enum.UserInputType.Keyboard then
        -- WASD 키로 이동
        updateFlightDirection()
    end
end)

game:GetService("UserInputService").InputChanged:Connect(function(input, gameProcessed)
    if not isFlying then return end
    if input.UserInputType == Enum.UserInputType.MouseMovement then
        -- 마우스가 이동할 때마다 비행 방향 갱신
        updateFlightDirection()
    end
end)

local FlyTab = Window:CreateTab("Fly System", nil) -- 새로운 탭 생성

-- 비행 상태와 비행 속도 설정
local isFlying = false
local flyingSpeed = 50 -- 기본 비행 속도
local ascendSpeed = 25 -- 상승 속도
local descendSpeed = -25 -- 하강 속도
local moveSpeed = 25 -- 앞뒤 좌우 이동 속도

-- 비행을 위한 BodyVelocity 인스턴스
local bodyVelocity

-- 비행 상태 표시용 변수
local isAscending = false -- 상승 중인지 확인하는 변수
local isDescending = false -- 하강 중인지 확인하는 변수

-- 비행 토글 버튼
local FlyButton = FlyTab:CreateButton({
    Name = "비행 토글",
    Callback = function()
        local player = game.Players.LocalPlayer
        local character = player.Character or player.CharacterAdded:Wait()

        -- 비행 상태 전환
        if not isFlying then
            isFlying = true

            -- BodyVelocity 인스턴스 생성 (플레이어를 공중으로 띄우기 위해)
            bodyVelocity = Instance.new("BodyVelocity")
            bodyVelocity.MaxForce = Vector3.new(400000, 400000, 400000) -- 힘의 크기 설정 (비행을 위한 최소값)
            bodyVelocity.Velocity = Vector3.new(0, flyingSpeed, 0) -- 기본 비행 속도 설정 (수직으로 띄운다)
            bodyVelocity.Parent = character:WaitForChild("HumanoidRootPart")

            -- 알림 표시
            game.StarterGui:SetCore("SendNotification", {
                Title = "비행 시작",
                Text = "이제 비행할 수 있습니다.",
                Duration = 5
            })
        else
            isFlying = false

            -- 비행을 종료하고 BodyVelocity를 제거
            if bodyVelocity then
                bodyVelocity:Destroy()
            end

            -- 알림 표시
            game.StarterGui:SetCore("SendNotification", {
                Title = "비행 종료",
                Text = "비행이 종료되었습니다.",
                Duration = 5
            })
        end
    end,
})

-- 비행 속도 조정 슬라이더
local FlySpeedSlider = FlyTab:CreateSlider({
    Name = "비행 속도",
    Range = {10, 100},
    Increment = 1,
    Suffix = "Speed",
    CurrentValue = flyingSpeed,
    Flag = "flyspeed", -- 속도 조정 시 저장되는 값
    Callback = function(Value)
        flyingSpeed = Value
        if isFlying and bodyVelocity then
            bodyVelocity.Velocity = Vector3.new(0, flyingSpeed, 0)
        end
    end,
})

-- 상승 버튼 (E로 이륙)
local AscendButton = FlyTab:CreateButton({
    Name = "이륙 (상승)",
    Callback = function()
        if isFlying and bodyVelocity then
            -- 상승 속도 적용
            isAscending = true
            bodyVelocity.Velocity = Vector3.new(0, ascendSpeed, 0)
            game.StarterGui:SetCore("SendNotification", {
                Title = "상승 시작",
                Text = "비행이 상승 모드로 변경되었습니다.",
                Duration = 5
            })
        else
            game.StarterGui:SetCore("SendNotification", {
                Title = "비행 시작 필요",
                Text = "비행을 먼저 시작해야 합니다.",
                Duration = 5
            })
        end
    end,
})

-- 하강 버튼 (Q로 하강)
local FlyTab = Window:CreateTab("Fly System", nil) -- 새로운 탭 생성

-- 비행 상태 및 이동 속도 설정
local isFlying = false
local flyingSpeed = 50
local ascendSpeed = 25
local descendSpeed = -25
local moveSpeed = 5 -- 조금씩 움직이도록 낮은 값 설정

-- 비행을 위한 BodyVelocity 인스턴스
local bodyVelocity
local targetVelocity = Vector3.new(0, 0, 0) -- 목표 속도 저장용

-- 비행 토글 버튼
local FlyButton = FlyTab:CreateButton({
    Name = "비행 토글",
    Callback = function()
        local player = game.Players.LocalPlayer
        local character = player.Character or player.CharacterAdded:Wait()

        if not isFlying then
            isFlying = true
            bodyVelocity = Instance.new("BodyVelocity")
            bodyVelocity.MaxForce = Vector3.new(400000, 400000, 400000)
            bodyVelocity.Velocity = Vector3.new(0, 0, 0)
            bodyVelocity.Parent = character:WaitForChild("HumanoidRootPart")

            game.StarterGui:SetCore("SendNotification", {
                Title = "비행 시작",
                Text = "이제 비행할 수 있습니다.",
                Duration = 5
            })

            -- 지속적인 속도 업데이트
            spawn(function()
                while isFlying do
                    if bodyVelocity then
                        bodyVelocity.Velocity = bodyVelocity.Velocity:Lerp(targetVelocity, 0.1) -- 부드러운 속도 변화
                    end
                    wait(0.1) -- 0.1초마다 갱신
                end
            end)
        else
            isFlying = false
            if bodyVelocity then
                bodyVelocity:Destroy()
            end
            game.StarterGui:SetCore("SendNotification", {
                Title = "비행 종료",
                Text = "비행이 종료되었습니다.",
                Duration = 5
            })
        end
    end,
})

-- 키 입력 처리
game:GetService("UserInputService").InputBegan:Connect(function(input, gameProcessed)
    if not isFlying then return end

    local player = game.Players.LocalPlayer
    local character = player.Character

    if input.UserInputType == Enum.UserInputType.Keyboard then
        if input.KeyCode == Enum.KeyCode.W then
            targetVelocity = targetVelocity + character.HumanoidRootPart.CFrame.LookVector * moveSpeed
        elseif input.KeyCode == Enum.KeyCode.S then
            targetVelocity = targetVelocity - character.HumanoidRootPart.CFrame.LookVector * moveSpeed
        elseif input.KeyCode == Enum.KeyCode.A then
            targetVelocity = targetVelocity - character.HumanoidRootPart.CFrame.RightVector * moveSpeed
        elseif input.KeyCode == Enum.KeyCode.D then
            targetVelocity = targetVelocity + character.HumanoidRootPart.CFrame.RightVector * moveSpeed
        elseif input.KeyCode == Enum.KeyCode.E then
            targetVelocity = targetVelocity + Vector3.new(0, ascendSpeed, 0) -- 상승
        elseif input.KeyCode == Enum.KeyCode.Q then
            targetVelocity = targetVelocity + Vector3.new(0, descendSpeed, 0) -- 하강
        end
    end
end)

-- 키를 뗄 때 이동 멈추기
game:GetService("UserInputService").InputEnded:Connect(function(input, gameProcessed)
    if not isFlying then return end

    if input.UserInputType == Enum.UserInputType.Keyboard then
        if input.KeyCode == Enum.KeyCode.W or input.KeyCode == Enum.KeyCode.S then
            targetVelocity = Vector3.new(0, targetVelocity.Y, 0) -- 수직 속도 유지, 수평 속도 제거
        elseif input.KeyCode == Enum.KeyCode.A or input.KeyCode == Enum.KeyCode.D then
            targetVelocity = Vector3.new(0, targetVelocity.Y, 0) -- 수직 속도 유지, 수평 속도 제거
        elseif input.KeyCode == Enum.KeyCode.E or input.KeyCode == Enum.KeyCode.Q then
            targetVelocity = Vector3.new(targetVelocity.X, 0, targetVelocity.Z) -- 수평 속도 유지, 수직 속도 제거
        end
    end
end)
