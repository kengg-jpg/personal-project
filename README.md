-- I had this as my server code. Server code as in when a client requests something to a server. Soemthing like that
local replicatedStorage = game:GetService("ReplicatedStorage")
local mainEvent = replicatedStorage:WaitForChild("Main")
local generalAnimations = replicatedStorage:WaitForChild("Animations")
local weldSettings = { -- C1 ONLY [note] || this is a data table for welds // attaching 
	["Sheathe"] =  CFrame.new(-2.09984207, 1.00043821, -0.0260119177, -0.0123886019, 7.4505806e-09, -0.9999246, -0.0168127045, 0.999859393, 0.000208067708, 0.999782681, 0.0168139916, -0.0123866275),
	["Unsheathe"] = CFrame.new( 0, 7.45058071e-10, -0.5, -0.801143944, -0.598472297, 0, 0.598472416, -0.801144004, 7.4505806e-09, -4.45896609e-09, 5.96898753e-09, 1.00000012),
}

local function newWeld(part1, part0, c1, c0) -- katana will be part1 [[ just some notes]] || this is a function for welding // attaching
	local Weld = Instance.new("Weld", part1)
	Weld.Part0 = part0
	Weld.Part1 = part1
	Weld.C1 = c1
	Weld.C0 = c0
	return Weld
end

local function findKatana(char)
	if char:FindFirstChild("Katana") then -- IF the server finds "Katana" in the player's character
		return char.Katana -- it will return the Katana
	else
		return nil --  else, it will return nothing
	end
end

mainEvent.OnServerEvent:connect(function(player, request) -- when the client is requesting for something
	local playerKatana = findKatana(player.Character)
	if not playerKatana then -- if the server doesn't find the player's katana using the function as requested, it will request the server to return
		return
	end
	if request == "Unsheathe" then -- client requests
		playerKatana.Weld.C1 = weldSettings.Unsheathe -- setting the weld as sheathe/unsheathe
	end
	if request == "Sheathe" then
		playerKatana.Weld.C0 = weldSettings.Sheathe
	end
end)
