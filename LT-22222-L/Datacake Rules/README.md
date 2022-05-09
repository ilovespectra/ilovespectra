<!DOCTYPE html>
<html lang="en" dir="ltr">
  <head>
    <meta charset="utf-8">
  </head>
  <body>
    <h2>Datacake Battery / Temperature /Humidity Rules for Hardware Safety</h2>
    <p><b>Highly Reccomended</b> <i>(Feel free to edit integers to your discretion.)</i></p>
    <p>These rules will instruct your LT to power down your miner and router due
to low battery or high temp/humidity, as well as turn them back on when it
is safe to do so. The LT can operate on as ittle as 7 volts, which will keep
you in control of your hardware even if the voltage is too low to power the
12v solar controller. This will prevent your miner/router from repeatedly
killing the battery in low solar environments, potentially damaging your
battery, or costing you expensive hardware failure due to heat or moisture.</p>
    <br>
    <p>The first four rules are for the temperature/humidity sensor. I'm using a <a href="https://www.dragino.com/products/temperature-humidity-sensor/item/151-lht65.html" target="_blank">Dragino LT-65.</a> and the <a href="https://www.dragino.com/products/lora-lorawan-end-node/item/156-lt-22222-l.html" target="_blank">Dragino LT-22222-L.</a> from <a href="https://github.com/ilovespectra/ilovespectra/tree/main/LT-22222-L" target="_blank">this walkthrough</a><i> You’ll select each device / parameter specifically, make the appropriate selection.</i></p>
    <img src="images/tempmaint.png" alt="Temperature maintenance rule" width=670 height=auto>
    <img src="images/temprestart.png" alt="Temperature restart rule" width=670 height=auto>
    <img src="images/humidmaint.png" alt="Humidity maintenance rule" width=670 height=auto>
    <img src="images/humidrestart.png" alt="Humidity restart rule" width=670 height=auto>
    <h4>These two are for the LT itself</h4>
    <img src="images/batmaint.png" alt="Battery maintenance rule" width=670 height=auto>
    <img src="images/batrestart.png" alt="Battery restart rule" width=670 height=auto>
    <p>That’s it. Hopefully that saves you a lot of stress, a little headache, and a ton of
money. There are endless combinations of sensors and rules you could come up with, be clever. The
more sensors the better. Water detection sensor, door open/close sensor, etc… 
    </p>
  </body>
</html>
