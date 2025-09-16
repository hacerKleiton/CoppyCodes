-- Dumper de Scripts Roblox Mobile v2
-- Por favor use apenas para jogos que você criou

return function()
    local output = ""
    local scriptCount = 0
    
    local function scan(obj, depth)
        depth = depth or 0
        local indent = string.rep("  ", depth)
        
        if obj:IsA("LuaSourceContainer") then
            local success, source = pcall(function()
                return obj.Source
            end)
            
            if success and source and source ~= "" then
                output = output .. indent .. "-- " .. obj:GetFullName() .. "\n"
                output = output .. indent .. source .. "\n\n"
                scriptCount = scriptCount + 1
            end
        end
        
        for _, child in pairs(obj:GetChildren()) do
            scan(child, depth + 1)
        end
    end

    -- Escaneia locais importantes (versão mobile otimizada)
    local importantPlaces = {
        game:GetService("ReplicatedStorage"),
        game:GetService("Workspace"),
        game:GetService("StarterPlayer"),
        game:GetService("StarterGui"),
        game:GetService("Lighting")
    }
    
    for _, place in pairs(importantPlaces) do
        scan(place)
    end
    
    return output, scriptCount
end
