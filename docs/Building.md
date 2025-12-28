### Self Building
[Play It your self Here](https://www.roblox.com/games/131592986004949/self-building)

__About __<br>
This is a Building, that selfs build when told. <br>

<video width="4000" height = 300 controls> <source src="/Assets/Self Building.mp4" type="video/mp4"> </video>




## Client

=== "Small Explination"
    The actual movement of this was done on the client to reduce stress on the server

=== "Code Snippet"
	```lua
	function Rebuild.RebuildBuilding(Table,Postion)
	    local Sound:Sound = script.lego:Clone()
	    if Sound.IsPlaying ~= true then
		    Sound.Parent = Table[1]
		    Sound:Play()
	    end
	
        task.spawn(function()
            


                for i,Part:Part in Table do
                    Part.Transparency = 1
                end
            
                
                

                for i,Part:Part in Table do
                    task.wait()
                    spawn(function()
                        local NewPart = MakePart(Part,Postion)
                        local BrazeierTween = MakeBrezzier(Postion,Part,NewPart)
                        local Tween = BrazeierTween:CreateCFrameTween(NewPart,{"CFrame"},TweenStuff,true)
                        Tween:Play()
                        Tween.Completed:Connect(function()
                            setmetatable(BrazeierTween,nil)
                            Part.Transparency = 0
                            NewPart:Destroy()
                        end)


                    end)
                end
                Sound:Stop()
                Sound:Destroy()
            end)
    end
	```


## Server


=== "Small Explination"
    The server handles a few things, the first being remote throtelling, back when this system was orginally made there weren't any good Libarys to handle it. Thus I had to make one for adding more buildings.

=== "Remote throttle"
	```lua
            function RemoteHandler.HandleOverFlow()
                if HandlingOverFlow then return end
                HandlingOverFlow = true
                while #Throttled > 0 do
                task.wait(ThrottleTime)
                    if #Throttled >0 then
                    
                    Throttled[1][1]:Ready()
                    local SendTable = {Throttled[1][2],Throttled[1][3]}
                    
                    Remote:FireAllClients(SendTable,Throttled[1][4])

                        table.remove(Throttled,1)
                    end
                    
                    
                    
                end
                
                
            
            
        end




        function RemoteHandler.SendBuild(Object,Table,Part,Action)
                    
                    
                table.insert(Throttled,{Object,Table,Part,Action})	
                
                RemoteHandler.HandleOverFlow()

        end

	```
=== "Building Object"

    ```lua
        function Building.New(BuildingInstance)
                local TotalDeletedParts = Instance.new("IntValue",BuildingInstance.PrimaryPart)
                self = setmetatable({
                    Inst = BuildingInstance,
                    MaxMisingParts = math.round(#BuildingInstance:GetChildren()/2),
                    CurrentParts = #BuildingInstance:GetChildren(),
                    CanDestruct = true,
                    DestroyedParts = 0,
                    Proximity = BuildingInstance.Rebuild.rebuild
                },Building)
                Objects[BuildingInstance] = self
                return self
        end


        function Building:DestroyParts(Table)
            for i,v:Part in Table do
                
                v.Transparency = 1
                self.DestroyedParts +=1
            end
            
            if self.DestroyedParts >= self.MaxMisingParts then
                
                return	self:Rebuild()
            end
            
        end

        function Building:Rebuild()

            local Table = {}
            self.CanDestruct = false
            self.DestroyedParts = 0
            
            for i,v:Part in self.Inst:GetChildren() do
                table.insert(Table,v)
            end
            
            RebuildRemote.SendBuild(self,Table,self.Inst.PartBasin,"Rebuild")
            return true
        end
        function Building:Ready()
            for i,v in self.Inst:GetChildren() do
                v.Transparency = 0 
            end
            self.CanDestruct = true
        end

	```


