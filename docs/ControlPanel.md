#  Robot With InterFace

[Play It your self Here](https://www.roblox.com/games/70853747265443/Portfolio-Game)



## Interface

=== "About"
    This is an NPC/AI/Robot Controll panel, with an AI robot





=== "Code Snippit"

    ```lua
      function Menu_Camera()
	local Camera = workspace.CurrentCamera
	Camera.CameraType = Enum.CameraType.Scriptable
	Inputs.Starting_CFrame = Camera.CFrame
	local TweenInfo = TweenInfo.new(2,Enum.EasingStyle.Sine,Enum.EasingDirection.In)
	local Tween = Tween_Service:Create(Camera,TweenInfo,{CFrame = Menu.Part.CFrame})
	Tween:Play()
	
	
	
	
	
	
    end

    function Camera_Player()
        
        local TweenInfo = TweenInfo.new(2,Enum.EasingStyle.Sine,Enum.EasingDirection.In)
        local Camera = workspace.CurrentCamera
        local Tween =  Tween_Service:Create(Camera,TweenInfo,{CFrame = Inputs.Starting_CFrame})
        Tween:Play()
        Tween.Completed:Wait()
        
        
        Camera.CameraType = Enum.CameraType.Custom
    end

    ```
=== "Video"
    <video width="100%" height="300" controls>
  <source src="Assets/Robot/Menu.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>


## Robot Wonder

=== "About"
    This robot servers no real purpose in the highlight of this game, but shows Ai interfacing and working. The section will demonstrate the wonder feature.
=== "Code Snippit"

    ```lua
    	task.spawn(function()
		local Owner:Player = self.Owner 
		local Robot_Model:Model = self.Model
		local Owner_HRP:Part = Owner.Character.HumanoidRootPart
		local Robot_HRP:Part = Robot_Model.HumanoidRootPart
		local Humanoid_Robot:Humanoid = Robot_Model.Humanoid
		local Working = false
		if self.Owner.Character == nil then
			repeat
				task.wait()
			until self.Owner.Character ~= nil
		end
		while self.Task == "Wonder" do
			local Position = self.Model.HumanoidRootPart.Position + Vector3.new(math.random(-10,10),0,math.random(-10,10))
			local Waypoints = self:GetPath_Wonder(Position)
				
				for i,WayPoint in ipairs(Waypoints) do
					if WayPoint.Action == Enum.PathWaypointAction.Jump then
						Humanoid_Robot.Jump = true
					end
					self:AttemptAnimation("Walk")
					Humanoid_Robot:MoveTo(WayPoint.Position)
					if self.Task ~= "Wonder" then
						break
					end
				
					Humanoid_Robot.MoveToFinished:Wait()
					
				end
			

			
			local Check = math.random(0,1)
			
			if Check == 1 then
				local X = math.random(0,5)
				repeat
				task.wait(1)
				if self.Task ~= "Wonder" then
					break
				end
				X -= 1
			until X <= 0
			end
			

		end
	end)
    ```
=== "Video"
      <video width="100%" height="300" controls>
  <source src="Assets/Robot/Wonder.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>

## Robot Follow 

=== "About"
	This feature is to call the robot back to you, after setting it to stay or wonder.
=== "Code Snippet"
	```lua
			task.spawn(function()
				if self.Owner.Character == nil then
					repeat
						task.wait()
					until self.Owner.Character ~= nil
				end
				local Owner:Player = self.Owner 
				local Robot_Model:Model = self.Model
				local Owner_HRP:Part = Owner.Character.HumanoidRootPart
				local Robot_HRP:Part = Robot_Model.HumanoidRootPart
				local Humanoid_Robot:Humanoid = Robot_Model.Humanoid
				local Working = false
				local Last_Pos = nil
				while self.Task == "Follow" do
					if (Owner_HRP.Position - Robot_HRP.Position).magnitude >= 5 and Working == false then
						
						Last_Pos = Owner_HRP.Position
						local WayPosints = self:GetPath_Follow()
						
						repeat
						
						
							if WayPosints[#WayPosints] == nil then
									Working =false
									self:AttemptAnimation("Idel")
									break
							end
								self:AttemptAnimation("Walk")
								Humanoid_Robot:MoveTo(WayPosints[#WayPosints].Position)
								
								if (WayPosints[#WayPosints].Position - Owner_HRP.Position).magnitude >= 6 then
									WayPosints = self:GetPath_Follow()
								end
								
								
								
						
							
							
						
							
							task.wait(.05)
						until Working == false
						
						
						
						
					end
					task.wait()
					
						
				end
				
				
				


			end)
		end
	```

=== "Video"
	<video width="100%" height="300" controls>
  <source src="Assets/Robot/Follow.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>


## Robot Stay


=== "About"

	This functionality forces the robot to stay put, the animation may be a little wonky. But I love it.
=== "Code Snippit"
	```lua
			function Robot:Stay()
				self:AttemptAnimation("Stay")
			end
	```
=== "Video"
	<video width="100%" height="300" controls>
  <source src="Assets/Robot/Stay.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>

	 