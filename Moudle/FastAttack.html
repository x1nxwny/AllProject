local _ENV = (getgenv or getrenv or getfenv)()

local Connections = {}

local ReplicatedStorage: ReplicatedStorage = game:GetService("ReplicatedStorage")
local Modules = ReplicatedStorage:WaitForChild("Modules")
local Net = Modules:WaitForChild("Net")
local Players = game:GetService("Players")
local Player = Players.LocalPlayer
local VirtualInputManager = game:GetService("VirtualInputManager")
local SeaBeasts = workspace:WaitForChild("SeaBeasts")
local Boats = workspace:WaitForChild("Boats")

local Remotes = ReplicatedStorage:WaitForChild("Remotes")
local getupvalue = getupvalue or (debug and debug.getupvalue)
local GunValidator = Remotes:WaitForChild("Validator2")
local Characters = workspace:WaitForChild("Characters")
local Enemies = workspace:WaitForChild("Enemies")

local RunService = game:GetService("RunService")
local Stepped = RunService.Stepped

local Module = {}

local HIDDEN_SETTINGS = {
	SKILL_COOLDOWN = 0.5,
	CLEAR_AFTER = 50
}

local function CreateNewClear()
	local COUNT_NEWINDEX = 0

	return {
		__newindex = function(self, index, value)
			if COUNT_NEWINDEX >= HIDDEN_SETTINGS.CLEAR_AFTER then
				for key, cache in pairs(self) do
					if typeof(cache) == "Instance" and not cache:IsDescendantOf(game) then
						rawset(self, key, nil)
					end
				end
				COUNT_NEWINDEX = 0
			end

			COUNT_NEWINDEX += 1
			return rawset(self, index, value)
		end
	}
end

local function CheckPlayerAlly(__Player)
	if tostring(__Player.Team) == "Marines" and __Player.Team == Player.Team then
		return false
	end

	return true
end


local Cached = {
	Closest = nil,
	Equipped = nil,
	Humanoids = setmetatable({}, CreateNewClear()),
	Enemies = {}, -- setmetatable({}, CreateNewClear()),
	Progress = {},
	Bring = {},
	Tools = {}
}

Module.Cached = Cached
Module.AttackCooldown = 0


function Module.IsAlive(Character)
	if Character then
		local Humanoids = Cached.Humanoids
		local Parent = Character.Parent
		local Humanoid = Humanoids[Character] or Character:FindFirstChild(if Parent == SeaBeasts then "Health" else "Humanoid")

		if Humanoid then
			if not Humanoids[Character] then
				Humanoids[Character] = Humanoid
			end

			return Humanoid[if Humanoid.ClassName == "Humanoid" then "Health" else "Value"] > 0
		end

		return Parent == Boats
	end
end

Module.FastAttack = (function()
	local FastAttack = {
		Distance = 50,
		attackMobs = true,
		attackPlayers = true,
		Equipped = nil,
		Debounce = 0,
		ComboDebounce = 0,
		ShootDebounce = 0,
		M1Combo = 0,

		Overheat = {
			["Dragonstorm"] = {
				MaxOverheat = 3,
				Cooldown = 0,
				TotalOverheat = 0,
				Distance = 350,
				Shooting = false
			}
		},
		ShootsPerTarget = {
			["Dual Flintlock"] = 2
		},
		SpecialShoots = {
			["Skull Guitar"] = "TAP",
			["Bazooka"] = "Position",
			["Cannon"] = "Position",
			["Dragonstorm"] = "Overheat"
		},
		HitboxLimbs = {"RightLowerArm", "RightUpperArm", "LeftLowerArm", "LeftUpperArm", "RightHand", "LeftHand"}
	}

	local RE_RegisterAttack = Net:WaitForChild("RE/RegisterAttack")
	local RE_ShootGunEvent = Net:WaitForChild("RE/ShootGunEvent")
	local RE_RegisterHit = Net:WaitForChild("RE/RegisterHit")
	local Events = ReplicatedStorage:WaitForChild("Events")

	local SUCCESS_FLAGS, COMBAT_REMOTE_THREAD = pcall(function()
		return require(Modules.Flags).COMBAT_REMOTE_THREAD or false
	end)

	local SUCCESS_SHOOT, SHOOT_FUNCTION = pcall(function()
		return getupvalue(require(ReplicatedStorage.Controllers.CombatController).Attack, 9)
	end)

	local HIT_FUNCTION; task.defer(function()
		local PlayerScripts = Player:WaitForChild("PlayerScripts")
		local LocalScript = PlayerScripts:FindFirstChildOfClass("LocalScript")

		while not LocalScript do
			Player.PlayerScripts.ChildAdded:Wait()
			LocalScript = PlayerScripts:FindFirstChildOfClass("LocalScript")
		end

		if getsenv then
			local Success, ScriptEnv = pcall(getsenv, LocalScript)

			if Success and ScriptEnv then
				HIT_FUNCTION = ScriptEnv._G.SendHitsToServer
			end
		end
	end)

	local IsAlive = Module.IsAlive

	FastAttack.ShootsFunctions = {
		["Skull Guitar"] = function(self: FastAttack, Equipped: Tool, Position: Vector3)
			Equipped.RemoteEvent:FireServer("TAP", Position) -- Events.ShootSoulGuitar:Invoke(Position)
		end
	}

	local function ExpandsHitBox(Enemies)
		for i = 1, #Enemies do
			Enemies[i][2].Size = Vector3.one * 50
			Enemies[i][2].Transparency = 1
		end
	end

	function FastAttack:ShootInTarget(TargetPosition)
		local Equipped = IsAlive(Player.Character) and Player.Character:FindFirstChildOfClass("Tool")

		if Equipped and Equipped.ToolTip == "Gun" then
			if Equipped:FindFirstChild("Cooldown") and (tick() - self.ShootDebounce) >= Equipped.Cooldown.Value then
				if self.ShootsFunctions[Equipped.Name] then
					return self.ShootsFunctions[Equipped.Name](self, Equipped, TargetPosition)
				end

				if SUCCESS_SHOOT and SHOOT_FUNCTION then
					local ShootType = self.SpecialShoots[Equipped.Name] or "Normal"

					if ShootType == "Position" or (ShootType == "TAP" and Equipped:FindFirstChild("RemoteEvent")) then
						Equipped:SetAttribute("LocalTotalShots", (Equipped:GetAttribute("LocalTotalShots") or 0) + 1)
						GunValidator:FireServer(self:GetValidator2())

						if ShootType == "TAP" then
							Equipped.RemoteEvent:FireServer("TAP", TargetPosition)
						else
							RE_ShootGunEvent:FireServer(TargetPosition)
						end

						self.ShootDebounce = tick()
					end
				else
					VirtualInputManager:SendMouseButtonEvent(0, 0, 0, true, game, 1);task.wait(0.05)
					VirtualInputManager:SendMouseButtonEvent(0, 0, 0, false, game, 1);task.wait(0.05)
					self.ShootDebounce = tick()
				end
			end
		end
	end

	function FastAttack:CheckStun(ToolTip, Character, Humanoid)
		local Stun = Character:FindFirstChild("Stun")
		local Busy = Character:FindFirstChild("Busy")

		if Humanoid.Sit and (ToolTip == "Sword" or ToolTip == "Melee" or ToolTip == "Gun") then
			return false
			-- elseif Stun and Stun.Value > 0 then {{ or Busy and Busy.Value }}
			--	 return false
		end

		return true
	end

	function FastAttack:Process(assert, Enemies, BladeHits, Position, Distance)
		if not assert then return end

		local HitboxLimbs = self.HitboxLimbs
		local Mobs = Enemies:GetChildren()

		for i = 1, #Mobs do
			local Enemy = Mobs[i]
			local BasePart = Enemy:FindFirstChild(HitboxLimbs[math.random(#HitboxLimbs)]) or Enemy.PrimaryPart

			if not BasePart then continue end

			local CanAttack = Enemy.Parent == Characters and CheckPlayerAlly(Players:GetPlayerFromCharacter(Enemy))

			if Enemy ~= Player.Character and (Enemy.Parent ~= Characters or CanAttack) then
				if IsAlive(Enemy) and (Position - BasePart.Position).Magnitude <= Distance then
					if not self.EnemyRootPart then
						self.EnemyRootPart = BasePart
					else
						table.insert(BladeHits, { Enemy, BasePart })
					end
				end
			end
		end
	end

	function FastAttack:GetAllBladeHits(Character: Character, Distance)
		local Position = Character:GetPivot().Position
		local BladeHits = {}
		Distance = Distance or self.Distance

		self:Process(self.attackMobs, Enemies, BladeHits, Position, Distance)
		self:Process(self.attackPlayers, Characters, BladeHits, Position, Distance)

		return BladeHits
	end

	function FastAttack:GetClosestEnemy(Character, Distance)
		local BladeHits = self:GetAllBladeHits(Character, Distance)

		local Distance, Closest = math.huge

		for i = 1, #BladeHits do
			local Magnitude = if Closest then (Closest.Position - BladeHits[i][2].Position).Magnitude else Distance

			if Magnitude <= Distance then
				Distance, Closest = Magnitude, BladeHits[i][2]
			end
		end

		return Closest
	end

	function FastAttack:GetGunHits(Character: Character, Distance)
		local BladeHits = self:GetAllBladeHits(Character, Distance)
		local GunHits = {}

		for i = 1, #BladeHits do
			if not GunHits[1] or (BladeHits[i][2].Position - GunHits[1].Position).Magnitude <= 10 then
				table.insert(GunHits, BladeHits[i][2])
			end
		end

		return GunHits
	end

	function FastAttack:GetCombo()
		local Combo = if tick() - self.ComboDebounce <= 0.4 then self.M1Combo else 0
		Combo = if Combo >= 4 then 1 else Combo + 1

		self.ComboDebounce = tick()
		self.M1Combo = Combo

		return Combo
	end

	function FastAttack:UseFruitM1(Character, Equipped, Combo)
		if self.UsingM1 then return end

		local M1Active = Equipped:FindFirstChild("M1Active")
		local Position = Character:GetPivot().Position
		local EnemyList = Enemies:GetChildren()

		for i = 1, #EnemyList do
			local Enemy = EnemyList[i]
			local PrimaryPart = Enemy.PrimaryPart
			if IsAlive(Enemy) and PrimaryPart and (PrimaryPart.Position - Position).Magnitude <= 50 then
				local Direction = (PrimaryPart.Position - Position).Unit
				Equipped.LeftClickRemote:FireServer(Direction, Combo)
			end
		end
	end

	function FastAttack:UseNormalClick(Humanoid, Character, Cooldown)
		self.EnemyRootPart = nil
		local BladeHits = self:GetAllBladeHits(Character)
		local EnemyHitBox = self.EnemyRootPart

		if EnemyHitBox then
			if SUCCESS_FLAGS and COMBAT_REMOTE_THREAD and HIT_FUNCTION then
				RE_RegisterAttack:FireServer(Cooldown)
				HIT_FUNCTION(EnemyHitBox, BladeHits)
			elseif SUCCESS_FLAGS and not COMBAT_REMOTE_THREAD then
				RE_RegisterAttack:FireServer(Cooldown)
				RE_RegisterHit:FireServer(EnemyHitBox, BladeHits)
			else
				table.insert(BladeHits, { Enemy, EnemyHitBox })
				ExpandsHitBox(BladeHits)

				VirtualInputManager:SendMouseButtonEvent(0, 0, 0, true, game, 1);task.wait(0.05)
				VirtualInputManager:SendMouseButtonEvent(0, 0, 0, false, game, 1)
			end
		end
	end

	function FastAttack:GetValidator2()
		local v1 = getupvalue(SHOOT_FUNCTION, 15) -- v40, 15
		local v2 = getupvalue(SHOOT_FUNCTION, 13) -- v41, 13
		local v3 = getupvalue(SHOOT_FUNCTION, 16) -- v42, 16
		local v4 = getupvalue(SHOOT_FUNCTION, 17) -- v43, 17
		local v5 = getupvalue(SHOOT_FUNCTION, 14) -- v44, 14
		local v6 = getupvalue(SHOOT_FUNCTION, 12) -- v45, 12
		local v7 = getupvalue(SHOOT_FUNCTION, 18) -- v46, 18

		local v8 = v6 * v2									-- v133
		local v9 = (v5 * v2 + v6 * v1) % v3 -- v134

		v9 = (v9 * v3 + v8) % v4
		v5 = math.floor(v9 / v3)
		v6 = v9 - v5 * v3
		v7 = v7 + 1

		setupvalue(SHOOT_FUNCTION, 15, v1) -- v40, 15
		setupvalue(SHOOT_FUNCTION, 13, v2) -- v41, 13
		setupvalue(SHOOT_FUNCTION, 16, v3) -- v42, 16
		setupvalue(SHOOT_FUNCTION, 17, v4) -- v43, 17
		setupvalue(SHOOT_FUNCTION, 14, v5) -- v44, 14
		setupvalue(SHOOT_FUNCTION, 12, v6) -- v45, 12
		setupvalue(SHOOT_FUNCTION, 18, v7) -- v46, 18

		return math.floor(v9 / v4 * 16777215), v7
	end

	function FastAttack:UseGunShoot(Character, Equipped)
		if not Equipped.Enabled then return end

		local ShootType = self.SpecialShoots[Equipped.Name] or "Normal"

		if ShootType == "Normal" or ShootType == "Overheat" then
			if ShootType == "Overheat" then
				local Data = self.Overheat[Equipped.Name]

				if Data.Shooting then
					return nil
				end

				local Target = self:GetClosestEnemy(Character, Data.Distance or 100)

				if Target then
					Data.Shooting = true

					while Equipped and Equipped.Parent == Player.Character and Data.TotalOverheat < Data.MaxOverheat do
						if Target and Target.Parent and IsAlive(Target.Parent) then
							Equipped:SetAttribute("LocalTotalShots", (Equipped:GetAttribute("LocalTotalShots") or 0) + 1)
							GunValidator:FireServer(self:GetValidator2())
							RE_ShootGunEvent:FireServer(Target.Position, { Target })
							Data.TotalOverheat += task.wait(Data.Cooldown)
						else
							break
						end
					end

					while Data.TotalOverheat > 0 do
						Data.TotalOverheat = math.clamp(Data.TotalOverheat - task.wait(), 0, Data.MaxOverheat)
					end

					Data.Shooting = false
				end
			else
				local Hits = self:GetGunHits(Character, 120)
				local Target = Hits[1] and Hits[1].Position

				if Target then
					Equipped:SetAttribute("LocalTotalShots", (Equipped:GetAttribute("LocalTotalShots") or 0) + 1)
					GunValidator:FireServer(self:GetValidator2())

					for i = 1, (self.ShootsPerTarget[Equipped.Name] or 1) do
						RE_ShootGunEvent:FireServer(Target, Hits)
					end
				end
			end
		elseif ShootType == "Position" or (ShootType == "TAP" and Equipped:FindFirstChild("RemoteEvent")) then
			local Target = self:GetClosestEnemy(Character, 200)

			if Target then
				if self.ShootsFunctions[Equipped.Name] then
					return self.ShootsFunctions[Equipped.Name](self, Equipped, Target.Position)
				end

				Equipped:SetAttribute("LocalTotalShots", (Equipped:GetAttribute("LocalTotalShots") or 0) + 1)
				GunValidator:FireServer(self:GetValidator2())

				if ShootType == "TAP" then
					Equipped.RemoteEvent:FireServer("TAP", Target.Position)
				else
					RE_ShootGunEvent:FireServer(Target.Position)
				end
			end
		end
	end

	function FastAttack.attack()
		if not _G["Fast Attack"] or (tick() - Module.AttackCooldown) <= 1 then return end
		if not IsAlive(Player.Character) then return end

		local self = FastAttack
		local Character = Player.Character
		local Humanoid = Character.Humanoid

		local Equipped = Character:FindFirstChildOfClass("Tool")
		local ToolTip = Equipped and Equipped.ToolTip
		local ToolName = Equipped and Equipped.Name

		if not Equipped or (ToolTip ~= "Gun" and ToolTip ~= "Melee" and ToolTip ~= "Blox Fruit" and ToolTip ~= "Sword") then
			return nil
		end

		local Cooldown = Equipped:FindFirstChild("Cooldown") and Equipped.Cooldown.Value or 0.3

		if (tick() - self.Debounce) >= Cooldown and self:CheckStun(ToolTip, Character, Humanoid) then
			local Combo = self:GetCombo()
			Cooldown += if Combo >= 4 then 0.05 else 0

			self.Equipped = Equipped
			self.Debounce = if Combo >= 4 and ToolTip ~= "Gun" then (tick() + 0.05) else tick()

			if ToolTip == "Blox Fruit" then
				if ToolName == "Ice-Ice" or ToolName == "Light-Light" then
					return self:UseNormalClick(Humanoid, Character, Cooldown)
				elseif Equipped:FindFirstChild("LeftClickRemote") then
					return self:UseFruitM1(Character, Equipped, Combo)
				end
			elseif ToolTip == "Gun" then
				if SUCCESS_SHOOT and SHOOT_FUNCTION and _G["Auto Shoot"] then
					return self:UseGunShoot(Character, Equipped)
				end
			else
				return self:UseNormalClick(Humanoid, Character, Cooldown)
			end
		end
	end

	table.insert(Connections, Stepped:Connect(FastAttack.attack))

	task.spawn(function()
		while task.wait() do
			if (tick() - Module.AttackCooldown) < 0 then continue end
			if not _G["Fast Attack"] then continue end
			FastAttack.attack()
		end
	end)

	return FastAttack
end)()
