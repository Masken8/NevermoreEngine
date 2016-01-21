# About
Nevermore is a collection of useful libraries for ROBLOX development.


## Features 
* **OOP** - Many of the libraries in Nevermore are OOP
* **Lazy loading** - Loads libraries only when you ask for them
* **Lots of libraries** - Lots of libraries that handle all sorts of logical issues that may occur during ROBLOX game development
* **Tested** - Used in several games that have made it to the front page, NevermoreEngine works without lots of testing
* **Simple** - Nevermore is essential a big group of libraries with lots of utility functions. It's designed with simplicity in mind.
	* The main module is only 200 lines long
	* Fast install, just one line in the command line
* **Open source** - Nevermore is open source
* **Built for ROBLOX** - Nevermore is built for ROBLOX
* **Works well with frameworks** - Nevermore works alongside your existing frameworks because it's just a collection of libraries and a loader.

## A sample of libraries
You don't have to use a library unless you load it. Nevermore should not impact performance (it can sit in a game without taking up processing power, as it's just a ton of libraries). Here are a few of the libraries Nevermore offers


* Additive Camera effects 
* Admin commands
* Pseudo chat
* Welding, CFrame manipulation code, quaternion slerp
* Bezier Curves
* Player manipulation utilities (check teammates, et cetera)
* Type checkers
* GUI transparency and Color3 animation code
* Material design UI code
    * Ripple
    * Snackbar
* Kinetic scrolling frame (mobile inertia scrollling)
* Compass code
* TimeSync between server and client
* Custom Signals
* Promises
* HeldInputs
* Projectile physics
* 3D rendering
* Rotating text labels
* Screen cover effects
* Title generation


# Get Nevermore
Paste the following code into your command bar in ROBLOX Studio to install Nevermore.


```lua
local a=game:GetService("HttpService")local b=game:GetService("ReplicatedStorage")local c=game:GetService("ServerScriptService")local d=a.HttpEnabled;a.HttpEnabled=true;local function e(f)f=f:gsub("\\","/"):gsub("^%s*(.-)%s*$","%1")return a:GetAsync("https://raw.githubusercontent.com/Quenty/NevermoreEngine/master/"..f)end;local function g(h,f)local i=f:gmatch("(%w+)%.lua")()local j=h:FindFirstChild(i)or Instance.new("ModuleScript",h)j.Name=i;j.Source=e(f)or error("Unable to load script")return j end;local function k(h,f)local l=f:gmatch("%w+\\")()if l then l=l:sub(1,#l-1)local j=h:FindFirstChild(l)or Instance.new("Folder",h)j.Name=l;return k(j,f:sub(#l+2,#f))else return h end end;print("Loading Nevermore")g(b,"App/NevermoreEngine.lua")print("Loading sublibraries")local m=k(c,"Nevermore\\")local n={}for o in e("Modules/ModuleList.txt"):gmatch("[^\r\n]+")do n[#n+1]=o end;for p,o in pairs(n)do g(k(m,o),"Modules/"..o)print(p,"/",#n)end;a.HttpEnabled=d;print("Done loading")
```
## What you just got

* The main NevermoreModule in `ReplicatedStorage`
* All the modules in `ServerScriptStorage`

Please note that installing Nevermore will **not** change any behavior in your game. Nevermore is a collection of libraries. If you want to see the power of Nevermore try using some [Admin Commmands](https://github.com/Quenty/NevermoreEngine/tree/master/Modules/QuentyAdminCommands)


# Updating Nevermore

To update Nevermore, back up your place file and run the install code above. It should preserve all of your existing code (although code changes to core Nevermore libraries will be overridden).

----


## Usage
Load up Nevermore on your server and client. This is the header code you need.


```lua
local ReplicatedStorage = game:GetService("ReplicatedStorage")

local NevermoreEngine   = require(ReplicatedStorage:WaitForChild("NevermoreEngine"))
local LoadCustomLibrary = NevermoreEngine.LoadLibrary
```

### Loading a library
With the above code, you can easily load a library and all dependencies

```lua
local qSystems = LoadCustomLibrary("qSystems")
```

Libraries have different functions with a variety of useful methods. For example, let's say we want to make a lava brick

Vanilla ROBLOX Lua to turn all red parts into killing bricks
```lua
local function HandleTouch(Part)
	-- Recursively find the humanoid
	local Humanoid = Part:FindFirstChild("Humanoid")
	if not Humanoid then
		if Part.Parent then
			return HandleTouch(Part.Parent)
		end
	elseif Humanoid:IsA("Humanoid") then
		Part.Humanoid:TakeDamage(100)
	end
end

local function RecurseApplyLava(Parent)
	for _, Item in pairs(Parent:GetChildren()) do
		if Item:IsA("BasePart") then
			Item.Touched:connect(HandleTouch)
		end

		RecurseApplyLava(Item)
	end
end

RecurseApplyLava(workspace)
```

With NevermoreEngine, this is easier:

```lua
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local NevermoreEngine   = require(ReplicatedStorage:WaitForChild("NevermoreEngine"))
local LoadCustomLibrary = NevermoreEngine.LoadLibrary
local qSystems = LoadCustomLibrary("qSystems")

local function HandleTouch(Part)
	local Humanoid = qSystems.GetHumanoid(Part)
	if Humanoid then
		Humanoid:TakeDamage(100)
	end
end

qSystems.CallOnChildren(workspace, function(Item)
	if Item:IsA("BasePart") then
		Item.Touched:connect(HandleTouch)
	end
end)
```


## Manual Installation
Put `NevermoreEngine.lua`'s content's in `game.ReplicatedStorage` in a ModuleScript name `NevermoreEngine`

Put all the modules in a folder in `game.ServerScriptStorage` and name them the names of their script, but without
.lua

```
game
	ReplicatedStorage
		`ModuleScript` NevermoreEngine
	ServerScriptStorage
		`Folder` Nevermore
			`Folder` qSystems
				`ModuleScript` qSystems
				... more libraries
			... more folders and libraries

```

## Libraries
Nevermore loads libraries from name. 

## Using NevermoreEngine

See App/readme.md

## Update / Change Log
This change log is *strictly* for Nevermore's module and documentation only.
##### December 23rd, 2015 [0.4.0.0]
- Moved NevermoreEngine into a simplified module that had 3 API components, for loading Libraries, RemoteEvents, and RemoteFunctions

##### November 21st, 2014 [0.3.0.0]
- Removed :Import() syntax
- Lots of bug fixes
- Redesign of class architecture should be coming soon
	- Specifically designed for testing
	- Existing implimentation has lots of errors
- Lots of bug fixes
- Was **not** updated to use ReplicatedFirst because of the way ROBLOX currently handles replicated first
	- Updates planned to make Nevermore more accessible
- Documentation added, more to come.

##### February 9th, 2014 [0.2.0.3]
- Fixed `RemoteEvent` Firing in server
- Updated 

##### February 8th, 2014 [0.2.0.2]
- Pushed to github
- Fixed release notes for MD

##### February 7th, 2014 [0.2.0.1]
- Made Parent argument in GetDataStreamObject optional. Defaults to bin.
- Made Parent argument in GetEventStreamObject optional. Defaults to bin.
- Added Workspace.FilteringEnabled as a property on Configuration

##### February 6th, 2014 [0.2.0.0]
- Updated system to work with Workspace.FilteringEnabled. 
- Updated so it does not warn when unregistered requests come through to prevent 
  bug with output streams looping output
  from the server on error. (ehhhh, I'm not sure how I fix that.).
- Now clients wait for DataStreamObject's to replicate from the server, instead 
  of creating them themselves, because they cannot create them themselves. 

##### February 4th, 2014 [0.1.0.9]
- Fixed character loading issue
- Moved events with character load
- Removed client loader dependency

##### February 3rd, 2014 [0.1.0.8]
- Added GetSplashEnabled function

##### February 2nd, 2014 [0.1.0.7]
- Fixed firing client bug

##### February 1st, 2014 [0.1.0.6]
- Fixed serverside bug with event storage

##### January 24th, 2014 [0.1.0.5]
- Added EventStream 
- Added new setting "EventStreamName"
- Added new bin in the replicated bin thing for EventStreams
- Add GetDataStreamObject to public API
- Add GetEventStreamObject and make it public

##### January 23th, 2014 [0.1.0.4]
- Added more documentation

##### January 20th, 2014 [0.1.0.3]
- Updated networking against. 
- Removed ypcall wrapping as ModuleScripts have been fixed.
- Fixed data replication package being cleared / nilled
- Debugged networking in server mode. 

##### January 19th, 2014 [0.1.0.2]
- Fixed problem with client / server networking

##### Janurary 4th, 2014 [0.1.0.1]
- Added SendSpawn property to DataStreams, as a networking option.
- Recursion added to modules, will now recurse through everything not a script, 
  local script, or module script in search of resources.
- ypcall wrapped Nevermore for debugging, until modulescripts are fixed.

##### Janurary 2nd, 2013 [0.1.0.0]
- Nevermore works as expected in solo mode and solotest mode
