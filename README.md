# cloud_illegal

![banner](https://i.imgur.com/yIQs4xi.jpg)

Esx_Illegal + Kypo_Drug_Effects w/ Overdose by **Cloud10Dev**

___________________________

**Good Afternoon dear community, today i will realease my first modification on a script that i didnt found here on the forum.**

So, this is basically the **esx_illegal by Xovos with kypo-drug-effects**.

How i made it?

___________________________

![step1](https://imgur.com/a/1vpqKwW)

Modify the **esx_basicneeds** (included on the zip im giving), changed every **“stress”** to **“drug”**.

ORIGINAL:
````
AddEventHandler('esx_basicneeds:resetStatus', function()
	TriggerEvent('esx_status:set', 'hunger', 500000)
	TriggerEvent('esx_status:set', 'thirst', 500000)
	TriggerEvent('esx_status:set', 'stress', 100000)
end)
````

MODIFIED:
````
AddEventHandler('esx_basicneeds:resetStatus', function()
	TriggerEvent('esx_status:set', 'hunger', 500000)
	TriggerEvent('esx_status:set', 'thirst', 500000)
	TriggerEvent('esx_status:set', 'drug', 100000)
end)
````
___________________________

![step1](https://imgur.com/a/FsR9oMf)

Go to **kypo-drug-effect** and on **server.lua**, on every drug you have add this line above the other trigger:

**TriggerClientEvent(‘esx_status:add’, source, ‘drug’, 430000)** - 430000 is the ammout that it will give as drug

**Like this:**

````
ESX.RegisterUsableItem('coke', function(source)
        
        local _source = source
	local xPlayer = ESX.GetPlayerFromId(_source)
	xPlayer.removeInventoryItem('coke', 1)


	TriggerClientEvent('esx_status:add', source, 'drug', 430000)
	TriggerClientEvent('kypo-drug-effect:onCoke', source)
end)
````

___________________________

With my share, these are the stats of the drugs:

**EFFECTS:**
- Weed: Give Health Boost +Gives Half Armour! (easy to get the drug)
- Coke: Gives Speed Boost! (easy to get the drug)
- Meth: Full Health + Full Armour! (hard to get the drug)
- Heroin: Lowers health to half + Full Armour! (easy to get the drug)

**OVERDOSE:**
- Weed: dies with 10 doses
- Coke: dies with 3 doses
- Meth: dies with 2 doses
- Heroin: dies with 2 doses

___________________________

What if you just want to add the overdose to your drug?

Add to **client.lua** where you make drugs usable:

````
local IsAlreadyDrug = false
local DrugLevel = -1
````

Then, bellow esx:getSharedObject, add:
````
AddEventHandler('esx_status:loaded', function(status)

  TriggerEvent('esx_status:registerStatus', 'drug', 0, '#9ec617', 
    function(status)
      if status.val > 0 then
        return true
      else
        return false
      end
    end, function(status)
      status.remove(1500)
    end)

	Citizen.CreateThread(function()
		while true do

			Wait(1000)

			TriggerEvent('esx_status:getStatus', 'drug', function(status)

		if status.val > 0 then
          local start = true

          if IsAlreadyDrug then
            start = false
          end

          local level = 0

          if status.val <= 999999 then
            level = 0
          else
            overdose()
          end

          if level ~= DrugLevel then
          end

          IsAlreadyDrug = true
          DrugLevel = level
		end

		if status.val == 0 then
          
          if IsAlreadyDrug then
            Normal()
          end

          IsAlreadyDrug = false
          DrugLevel     = -1
		end
			end)
		end
	end)
end)

--When effects ends go back to normal
function Normal()

  Citizen.CreateThread(function()
    local playerPed = GetPlayerPed(-1)
			
    ClearTimecycleModifier()
    ResetScenarioTypesEnabled()
   -- ResetPedMovementClipset(playerPed, 0) <- it might cause the push of the vehicles
   -- SetPedIsDrug(playerPed, false)
    SetPedMotionBlur(playerPed, false)
  end)
end

--In case too much drugs dies of overdose set everything back
function overdose()

  Citizen.CreateThread(function()
    local playerPed = GetPlayerPed(-1)
	
    SetEntityHealth(playerPed, 0)
    ClearTimecycleModifier()
    ResetScenarioTypesEnabled()
    ResetPedMovementClipset(playerPed, 0)
    SetPedIsDrug(playerPed, false)
    SetPedMotionBlur(playerPed, false)
  end)
end
````
___________________________

**REMEMBERING THAT THE DRUGS SPOTS ARE WHERE I WANTED THEM, YOU MAYBE WONT HAVE THE YMAPS/MLO FOR THEM, SO, YOU GOTTA CHANGE THE POSITIONS OF THEM. ALSO, I DISABLED LSD/LSA/CHEMICALS**

___________________________


Thank you, and i will try to give support!

**Discord:** CloudDev#0810

