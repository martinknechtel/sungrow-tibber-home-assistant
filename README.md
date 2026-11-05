# Sungrow + Tibber to save money 💴 (and the planet) 🌍

## ARCHVIVED
I will close this repo, since meanwhile [evcc Optimizer] covers the goal. See the following picture and compare it with the Goal below. All covered: charge battery, consume from battery, consume from grid - all based on grid price and predicted consumption and solar production. So my stop-gap solution is not required anymore and I archive this repo.

![charge battery, consume from battery, consume from grid - all based on grid price and predicted consumption and solar production](evcc Optimizer.PNG)

## Goal
Charge your home battery in cheap Tibber hours, consume from the grid in average price times, consume from home battery in expensive hours.

![Diagram with Tibber price over time and charging your home battery at the right time](Tibber_2024-12-12.jpg)

In this example, the result we want to achieve would look like this. The battery has been charged during the cheapest hours 2:00-4:00, consumed in the expensive hours 8:00-21:00, and the rest of the day battery was bypassed and the grid powered the house at a relatively low price where a charge-discharge battery cycle would not pay off.
![Animation showing successful battery and grid use times](Tibber_2024-12-12_sungrow.gif)

## Manual Alternative
At first I used the Sungrow app iSolarCloud. I manually scheduled time windows to charge and discharge. Look for Forced Charging, Battery Use Time, Weekend Discharging in the app

![Screenshot of iSolarCloud](iSolarCloud_Forced_Charging_and_Battery_Use_Time.jpg)

## Prerequisites
* https://github.com/mkaiser/Sungrow-SHx-Inverter-Modbus-Home-Assistant
* https://www.home-assistant.io/integrations/tibber

## Simulate first
Create the automation [automation-simulate.yaml](automation-simulate.yaml). Have no fear since for now it only posts notifications to your Home Assistant UI, we will extend this simulation mode with real action in next section. To install, create a new Automation in the UI, switch to yaml view, and copy&paste. If you want to learn more about creating Automations, [read the documentation](https://www.home-assistant.io/docs/automation/editor/).

![Notifications only, no action yet](Simulate.jpg)

## Action!
Make your automation do something by calling the scripts that come with the Sungrow modbus integration from the Prerequisites section: [automation-productive.yaml](automation-productive.yaml). If you want to debug it, you can check the Traces of your Automation.

![Traces of the Automation](Traces_of_the_Automation.jpg)

## Add further conditions to refine
Here is [automation-extended.yaml](automation-extended.yaml) with the following extensions:
* I realized, the battery is not charged while in bypass mode, so I use the already existing sensor `exporting_power` as trigger.
* And I realized in some weeks I do not want to charge my battery from the grid, the sun delivers enough if I consume from the battery only in expensive hours, so I added a toggle `battery_recharge_when_cheap_tibber_price`. Creating a plain new toggle is easily done as https://www.home-assistant.io/integrations/input_boolean/, remember to use the same name as I did or change it in your automation yaml.

![Sensor for Exporting Power existed and I added a toggle to control Forced Charge](Toggle_Recharge.jpg)

## You don't have Tibber yet? \[affiliate links\]
* English short version:
  * Use my invite code https://invite.tibber.com/r664wqgc
  * And if you are looking for an ecological gas supplier have a look at https://www.polarstern-energie.de/oekogas/?linkId=lp_744447&sourceId=martin-knechtel&tenantId=polarstern 
* German long version:
  * Strom ist immer dann billig, wenn besonders viel Solar und Windstrom im Netz ist. Doppelter Nutzen also! Du kannst Geld sparen, die Umwelt schonen und gleichzeitig unnötigen Energieverbrauch vermeiden. Wie das geht? Mit den stundenaktuellen Strompreisen von Tibber kannst du deinen Verbrauch einfach auf die günstigsten Stunden verlagern und teure Stunden vermeiden. Probiere es aus, und wir bekommen beide 50 € als Bonus, den wir im Tibber Store einlösen können. ⚡ Lies hier mehr dazu: https://invite.tibber.com/r664wqgc
  * Und falls bei dir wie bei mir keine Wärmepumpe geht, dann gibt es 100% Ökogas. Polarstern ist ein unabhängiger Ökoenergieversorger, den ich dir ans Herz legen möchte. Die bieten nur saubere Energie, fördern die Energiewende weltweit und sind von der Gemeinwohl-Ökonomie zertifiziert. Ich find's super. Und der Wechsel ist in wenigen Minuten erledigt - wirklich! https://www.polarstern-energie.de/oekogas/?linkId=lp_744447&sourceId=martin-knechtel&tenantId=polarstern
