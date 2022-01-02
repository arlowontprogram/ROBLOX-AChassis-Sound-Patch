# ROBLOX AChassis Sound Patch
#### This prevents non-drivers from changing sounds for a vehicle.

###### Source from Handler in:
###### /Car Model/A-Chassis Tune/Plugins/A6_Stock_Sound/AC6_FE_Sounds/Handler
```lua
local car = script.Parent.Parent
local Sounds = {}
local F = {}

F.newSound = function(name,par,id,pitch,volume,loop)
	for i,v in pairs(Sounds) do
		if i==name then
			v:Stop()
			v:Destroy()
		end
	end
	local sn = Instance.new("Sound",par)
	sn.Name = name
	sn.SoundId = id
	sn.Pitch = pitch
	sn.Volume = volume
	sn.Looped = loop
	Sounds[name]=sn
end

F.updateSound = function(sound,id,pit,vol)
	local sn = Sounds[sound]
	if id~=sn.SoundId then sn.SoundId = id end
	if pit~=sn.Pitch then sn.Pitch = pit end
	if vol~=sn.Volume then sn.Volume = vol end
end

F.playSound = function(sound)
	Sounds[sound]:Play()
end

F.pauseSound = function(sound)
	Sounds[sound]:Pause()
end

F.stopSound = function(sound)
	Sounds[sound]:Stop()
end

F.removeSound = function(sound)
	Sounds[sound]:Stop()
	Sounds[sound]:Destroy()
	Sounds[sound]=nil
end

script.Parent.OnServerEvent:connect(function(pl,Fnc,...)
	if pl.Character == car.DriveSeat.Occupant.Parent then
		F[Fnc](...)
	else
		warn(pl.Name .. " has been sus by activating sounds when not driving :thinking:")
                -- do stuff
	end
end)

car.DriveSeat.ChildRemoved:connect(function(child)
	if child.Name=="SeatWeld" then
		F.removeSound("Rev")
	end
end)
```
