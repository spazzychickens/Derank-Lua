local derankcheck = ui.new_checkbox("config", "LUA", "Derank")
local js = panorama.open()
local compapi = js.CompetitiveMatchAPI
local gamestateapi = js.GameStateAPI
local friendsapi = js.FriendsListAPI

local function reconnectfunc()
    local derankcheck = ui.get(derankcheck)
    if (derankcheck == true) then
        if compapi.HasOngoingMatch() then
            return compapi.ActionReconnectToOngoingMatch( '', '', '', '' ), derankcheck
        end
    end
end

client.set_event_callback("round_freeze_end", function()
    
        if ui.get(derankcheck) then
            local local_player, players, teammates = entity.get_local_player(), entity.get_players(), {}
            local local_team, local_steam64 = entity.get_prop(local_player, "m_iTeamNum"), entity.get_steam64(local_player)
            for i=1, #players do
                if entity.get_prop(players[i], "m_iTeamNum") == local_team then
                    table.insert(teammates, entity.get_steam64(players[i]))
                end
            end
            table.sort(teammates)
        
            local team_index
            for i=1, #teammates do
                if teammates[i] == local_steam64 then
                    team_index = i
                end
            end
            local rounds_won_enemy = entity.get_prop(entity.get_game_rules(), "m_totalRoundsPlayed")
        
            local delay = 5 + ((rounds_won_enemy+team_index-1) % #teammates)*0.1
            client.delay_call(delay, client.exec, "disconnect")
            client.delay_call(delay+0.2, reconnectfunc)
        else
            return
        end
end)
