# MAD STUDIO - Replica

Replica is a Roblox server to client state replication solution which lets the developer subscribe certain players to certain states.

Individual states in the Replica module are called "replicas". Replicas can only be created and changed server-side, both server
and client can connect cleanup tasks for the moment of replica destruction, state changes can trigger listeners on the client-side.

# Changes: Server side listeners

The goal of this fork is to create a way to listen for server side changes to individual `Replica` instance `data` changes.

Example:

`Server.luau`

```
local ServerScriptService = game:GetService("ServerScriptService")
local Replica = require(ServerScriptService.ReplicaServer)

Replica.NewReadyPlayer:Connect(function(player)
    local playerDataReplica = Replica.New({
        Token = Token,
        Tags = {UserId = player.UserId},
        Data = {
            Health = 0
        },
    })
    
    playerDataReplica:OnSet({"Health"}, function(newValue: number)
        print("player health changed to: %d", newValue)
    end)

    playerDataReplica:Subscribe(player)
end)

```