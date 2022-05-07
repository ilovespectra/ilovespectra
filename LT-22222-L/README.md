<!DOCTYPE html>
<html lang="en" dir="ltr">
  <head>
    <meta charset="utf-8">
    <link href="styles/styles.css" rel="stylesheet" type="text/css"
  </head>

  <body>
    <h1>Dragino I/O Relay LT-22222-L Walkthrough</h1>
    <h2>for US/AU915mhz</h2>
    <h4>For remote power cycle of 1 or 2 devices
      and voltage monitoring over LoRaWAN</h4>
    <h5>by @tanny7241 with special thanks to @tteague#3838
      (Updated May 7th, 2022)
    <h4>
      The Dragino LT-22222-L ships as a Class C device. For Helium, we need to change that to Class A. In additon, the default settings for US/AU915mhz, as well as for CN470, are to lazily scan all 72 potential channels in your region’s band, when most LoRaWAN gateways only use 8. When the device joins, the server will issue a downlink telling the device how to behave. I’ve included links to the, USB converter, driver and application you’ll need in this walkthrough to make your life a little easier.</h4><br>
      <h2>End Goals:</h2>
    <ul>
      <li>
        Set the device from Class C to Class A so it can work with Helium
      </li>
      <li>
        Configure the end node to work in sub-band 2
      </li>
      <li>
        Join the Helium Network
      </li>
      <li>
        Control the relay on Datacake
      </li>
      <li>
        Wire the node for power relay and voltage monitoring
      </li>
    </ul>
    <h3><i>This is a 102 crash course and will be much easier for folks already orientated with Helium Console / Datacake, but the walkthrough is novice-friendly.</i></h3>
    <img src="images/pwoa.png" alt="Electrical Warning: Yellow Triangle with Exclaimation" width="70" height="70"><h3>ALWAYS USE CAUTION WHEN WORKING WITH ELECTRICITY. NEVER PLUG WIRES DIRECTLY INTO A SOCKET, ALWAYS USE A DC ADAPTER RATED FOR YOUR DEVICE.</h3>
    <h2>You Will Need:</h2>
    <ul>
      <li>
        The Dragino LT-22222-L, in your region's band, antenna attached
      </li>
      <li>
        The 1/8" programming plug it comes with
      </li>
      <li>
        12v power supply, wired Hot to VIN, Neutral to GND.
      </li>
    <h3>Which wire is positive on 12v adapter?</h3>
    <h5>If the multi-colored wire is black and red, the black wire is the negative wire, while the red one is positive. If both wires are black but one has a white stripe, the positive wire should be the one with the white stripe, and the negative wire should be black. RECOMMENDED: TEST WITH VOLTMETER </h5>
    <img src="images/ltpower.jpeg" alt="Power cable orientation for the LT-22222-L" width="500px" height="auto">
    <p>
    </p>
      <li>
        A USB to TTL Converter, use a <a href="https://www.amazon.com/IZOKEE-CP2102-Converter-Adapter-Downloader/dp/B07D6LLX19/ref=sr_1_3?keywords=usb+to+ttl+adapter&qid=1647726342&sr=8-3" target="_blank">CP2102</a> if you want to match the walkthrough:<br><br>
    <img src="images/serial.png" alt="Serial Port connection demonstration with description" width="500px" height="auto">
      </li>
      <li>
        Download and install <a href="https://www.pololu.com/docs/0J7/all" target="_blank">the USB drivers</a> for the CP2102
      </li>
      <li>
        Download and install <a href="https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html" target="_blank">PuTTY</a>
      </li>
      <li>
        Connect the programming cable to USB adapter, plug in and power the LT-22222-L<br>
        (Node < headphone w/ quick connect > CP-2102 > PC)
      </li>
    </ul>
      <h4>This part is finicky, follow these guidelines:<br>
 
- Do not type " or < and > into the console.<br>
 
- The word Enter always means the button.<br>
 
- Do not hit backspace in the terminal: If you have a typo, close and start over.<br>
 
- Use ALL caps<br>
      </h4>
<h4>First, navigate to your Device Manager to find the COM port for your device</h4>
<img src="images/comport.jpg" alt="Serial Port screenshot" width="450px" height="auto">
    <p>
    </p><h4>Open Putty. Click "Serial" under connection type and type in the COM port provided to you in Device Manager. ("COM3" in the example)</h4>
<img src="images/putty.jpg" alt="PuTTY screenshot" width="500px" height="auto">
    <h4>Click "Terminal" under "Category" and select Local Echo: "Force On"</h4>
    <img src="images/echo.png" alt="Force Echo screenshot" width="500px" height="auto">
    <h4>Click "Serial" under "Category", make sure it looks like this, it should already:<br>
<br>
Baud Rate :9600<br>
Data Bits: 8<br>
Stop Bits: 1<br>
Parity: None<br>
Flow Control: XON/XOFF<br>
    </h4>
    <img src="images/baud.png" alt="Baud screenshot" width="500px" height="auto">
    <h4>Click "Open"</h4>
      <p><i>Below is a preview of the AT terminal. Fun fact, AT commands are called that because ATtention.</i></p>
    <img src="images/atz.jpg" alt="Baud screenshot" width="600px" height="auto">
    <h4>The default password is "123456", when <tt>Incorrect Password</tt> appears, just type <tt>123456</tt> and hit enter.
    <p>To help clarify, <tt>" "</tt> indicate a command you enter. If you get an error message, enter the command again.<br>
      If it interrupts your commands with a Tx/Rx feed while you're typing,wait for it to stop, and enter the command again:<br>
      <br><br>Follow this dialogue carefully,<br><br>
    </p>
    <p>Enter the default password<br><br>
      <tt>"123456"</tt> Enter<br><br>
<tt>Correct Password</tt><br><br>
<tt>"ATZ"</tt> Enter<br><br>
<br>
  <tt>DRAGINO XX Device</tt><br>
    <tt>Image Version: XXX…</tt><br>
      <tt>Frequency Band: XXX… </tt>  (Make sure the band reflects your region's)<br>
<tt>DevEui= XX XX XX XX XX XX XX XX</tt><br>
<tt>Enter Password to Active AT Commands</tt><br>
<tt><i>followed by the Tx and Rx information</i></tt><br><br>
<br><br><tt>"AT+CLASS?"</tt> Enter<br><br>
<tt>AT+CLASS: Get or Set the Device Class</tt><br><br>
<tt>"AT+CLASS=A"</tt> Enter<br><br>
<tt>OK</tt><br><br>
<tt>"AT+CHE?"</tt> Enter<br><br>
      <tt>AT+CHE: Get or Set eight channels mode, Only for US915,AU915,CN470</tt><br><br>
<tt>"AT+CHE=2"</tt> Enter<br><br>
<tt>OK</tt>(Or After reset of the ATZ)<br><br>
<tt>"ATZ?"</tt>Enter [To reset]<br><br>
<tt>OK</tt><br><br>
<p>
  <p><i>Enter this last command, and check the lines that have been indicated with a ?</i></p><br><br>
      <tt>"AT+CFG"</tt>Enter<br><br>
<tt>Stop Tx events, Please wait for all configurations to print</tt><br>
<tt>AT+DEUI=1234567890123…</tt><br>
<tt>AT+DADDR=123456789012…</tt><br>
<tt>AT+APPKEY=12345678901…</tt> }These will all be unique to your device<br>
<tt>AT+NWKSKEY=1234567890…</tt><br>
<tt>AT+APPSKEY=1234567890…</tt><br>
<tt>AT+APPEUI=12345678901…</tt><br>
<tt>AT+ADR=1</tt><br>
<tt>AT+TXP=0</tt><br>
<tt>AT+DR=4</tt><br>
<tt>AT+DCS=0</tt><br>
<tt>AT+PNM=1</tt><br>
<tt>AT+RX2FQ=923300000</tt><br>
<tt>AT+RX2DR=8</tt><br>
<tt>AT+RX1DL=1000</tt><br>
<tt>AT+RX2DL=2000</tt><br>
<tt>AT+JN1DL=5000</tt><br>
<tt>AT+JN2DL=6000</tt><br>
<tt>AT+NJM=1</tt><br>
<tt>AT+NWKID=00 00 00 00</tt><br>
<tt>AT+FCU=0</tt><br>
<tt>AT+FCD=0</tt><br>
<tt>AT+CLASS=A</tt> Yours says CLASS A?<br>
<tt>AT+NJS=0</tt><br>
<tt>AT+RECVB=0:</tt><br>
<tt>AT+RECV=0:</tt><br>
<tt>AT+VER=v1.5.6 US915</tt> Yours says US915?<br>
<tt>AT+CFM=0</tt><br>
<tt>AT+CFS=0</tt><br>
<tt>AT+SNR=0</tt><br>
<tt>AT+RSSI=0</tt><br>
<tt>AT+RJTDC=20</tt><br>
<tt>AT+RPL=0</tt><br>
<tt>AT+TDC=600000</tt><br>
<tt>AT+PORT=2</tt><br>
<tt>AT+RX1WTO=83</tt><br>
<tt>AT+RX2WTO=7</tt><br>
<tt>AT+CHS=0</tt><br>
<tt>AT+CHE=2</tt> Yours says 2?<br>
<tt>903.9 904.1 904.3 904.5 904.7 904.9 905.1 905.3</tt><br>
<tt>AT+PWORD=123456</tt><br>
<tt>AT+MOD=1</tt><br>
<tt>AT+TRIG1=0,0</tt><br>
<tt>AT+TRIG2=0,0</tt><br>
<tt>AT+VOLMAX=0,0</tt><br>
<tt>AT+COUTIME=600</tt><br>
<tt>AT+ADDMOD6=0</tt><br>
<tt>AT+DTRI=0,0</tt><br>
<tt>AT+AVLIM=0,0,0,0</tt><br>
<tt>AT+ACLIM=0,0,0,0</tt><br>
<tt>AT+ATDC=5</tt><br><br>
<br>
<p><i>If so, then you’re done. If any of yours look different, enter the related command again.</i></p>
<p>Once you’re done click the computer icon in the top left corner and click "Copy all to Clipboard." Paste in a text editing app. [Delete the spaces in your Keys/EUIs to make copy and pasting them a breeze.]</p><br>
<br>
Pat yourself on the back, hacker. You've finished the hard part.<br>
<br>
You can close the Putty window<h3>Unplug the LT-22222-L
[Vital step here, because the join uplink is sent when the device powers on.]</h3>
<br><br><h2>Using the Relay</h2>
  <ul>
    <li>
      Go to <a href="https://console.helium.com/welcome" target="_blank">console.helium.com</a> [Create an account if it's your first time]<br>
    </li>
    <li>
      Add a device using the AppEUI, DevEDUI, and APPKey provided with your LT-22222-L, <i>or that you hopefully just copied from Putty into a text app. Don't forget to delete all spaces in your Keys/EUIs.</i> (There is a pending state when adding the device and you will not see any data flow/joins/etc.during this period. Wait about 20 minutes to power it on)
    </li>
    <li>
      Power on the device and scroll to the device Event Log at the bottom of the device’s page to confirm you got the Uplink, it should be nearly simultaneous with a downlink.<i>Welcome to the Helium network! You did it  :)</i>
    </li>
    <li>In a separate tab or window, go to <a href="https://datacake.co/" target="_blank">datacake.co</a>
    </li>
  </ul>
  <h4>Navigate to your <a href="https://app.datacake.de/" target="_blank">Datacake dashboard</a> and add the device: Select “LoRaWAN” / “Helium”</h4>
  <img src="images/datacake1.png" alt="Datacake LoRaWan Selection" width="500px" height="auto">
  <img src="images/datacake2.png" alt="Datacake Helium Selection" width="500px" height="auto">
  <img src="images/datacake3.png" alt="Datacake Dragino LT-22222-L Selection" width="600px" height="auto">
  <h4>Now navigate back over to your Helium Console and click “Integrations” > “Add Integration” > “Datacake HTTP”</h4>
  <img src="images/console1.png" alt="Datacake on Helium Console" width="600px" height="auto">
  <h4>Navigate back to your Datacake dashboard and click your user name in the top left corner > click “Edit Profile” > “API” > “Show Token” > Now copy that token to your clipboard.</h4>
  <img src="images/datacake5.png" alt="Datacake API Token" width="600px" height="auto">
  <h4>and paste it to the “ENDPOINT DETAILS” in your Helium Integration</h4>
  <img src="images/endpoint.png" alt="Endpoint Details" width="750px" height="auto">
  <h4>Navigate back over to Datacake and click “Configuration”> Scroll down to “Network Server” and click “Change” > Scroll down to your “Uplink URL” and copy it to your clipboard.</h4>
  <img src="images/uplinkurl.png" alt="Uplink URL" width="750px" height="auto">
  <h4>Navigate back over to Helium Console and click “Integrations” > Select your LT-22222-L > Scroll down to “Endpoint URL” and paste the “Uplink URL” you just copied from Datacake and click “Update Details”</h4>
  <img src="images/endpoint2.png" alt="Endpoint Details 2" width="750px" height="auto">
  <h4>Click “Flows” > “+ Nodes” > Drag your device out of that menu and it'll stick to the background.</h4>
  <img src="images/flows.png" alt="Flows example on Helium Console" width="600px" height="auto">
  <h4>Now connect the dots! Click to draw a line connecting the two. It should appear dotted indicating data flow. This is how your Flows board will work for all integrations. <i>It’s sexy, isn't it?</i></h4>
  <h4>Navigate back over to your Datacake dashboard and click “Downlinks” and click “Switch on all Relays”. You should receive a message that says “Downlink sent to the LNS successfully”</h4>
  <img src="images/downlink.png" alt="Downlink example on Datacake" width="600px" height="auto">
  <h4>Go over to Helium Console, click “Devices” > LT-22222-L > and scroll down to your “Event Log” and make sure you see a red Downlink Queued.</h4>
  <img src="images/downlink2.png" alt="Downlink example on Helium" width="600px" height="auto">
  <h4>It could take around 10 minutes for you to see the RO1 and RO2 lights on your LT-22222-L illuminate. If they do, you’re all done!</h4>
  <p><i>If you are not getting the downlinks, the easiest thing to do will be to remove the device from Helium console, and start over. You’ve got this!</i></p>
  <h2>Relay Wiring Configuration-</h2>
  <img src="images/pwoa.png" alt="Electrical Warning: Yellow Triangle with Exclaimation" width="100" height="100">
  <h3>Note the use of a socket in this diagram is for illustrative purposes only. USE CAUTION WHEN WORKING WITH ELECTRICITY. DO NOT PLUG WIRES DIRECTLY INTO A SOCKET, ALWAYS USE A DC ADAPTER RATED FOR YOUR DEVICE.</h3>
  <img src="images/config.png" alt="Electrical configuration of LT to router and hotspot." width="750px" height="auto">
  <p><i>The steps below apply specifically to the off-grid configuration provided in an https://www.iotoffgrid.com/ solar enclosure kit. The LT-22222-L can be powered directly by a 7-24v DC adapter for on-the-grid indoor and enclosure applications.</i></p>
  <img src="images/wiring.jpeg" alt="Electrical configuration of LT to router and hotspot." width="750px" height="auto">
  <p>The black wires don’t need to be cut. If they are, splice/crimp carefully.</>
  <ul>
    <li>
      Power off
    </li>
    <li>
      Daisy chain batteries together with charging splitter (if  >1) [img next page]
    </li>
    <li>
      Connect 1 battery lead to solar controller
    </li>
    <li>
      Connect opposite battery lead to LT-22222-2 (+VIN,-GND)
    </li>
    <li>
      Connect appliance lead from controller to regulator
    </li>
    <li>
      Run the ( + ) miner wire into RO1-1 and out RO1-2 to miner
    </li>
    <li>
      Run the ( - ) miner wire from controller to miner
    </li>
    <li>
      Run the ( + ) router wire from regulator to RO2-1 and out RO-2 to router
    </li>
    <li>
      Run the ( - ) router wire from regulator to router
    <li>
      *OPTIONAL: analog voltage monitoring (battery voltage)
      $nbsp <12” ~14-16 AWG wire from ( + ) battery input of controller to AVI1
    </li>
  </ul>
  <img src="images/splitters.png" alt="Diagram of the splitter configuration." width="650px" height="auto">
  <p><i>This is the recommended configuration provided by @Pirate_ProfTK#1062 for the 2 battery / splitter / controller / LT. Chances are you’ll need to cut the plug off the lead to the LT and splice a <12” ( + ) and ( - ) wire to power the LT-22222-L</i></p>
  <br>
  <p>The image below illustrates how to wire the LT-22222-L on-the-grid with wall power supply for the node and miner. Repeat the process for the router. </p>
  <img src="images/relaywiring.jpeg" alt="Illustration of wiring the relay." width="650px" height="auto">
  <p>Datacake will allow you to control the Relays individually, but it’s easier to select “All On” or “All Off” from the provided Downlinks. The device sends its uplink every 10 minutes or so, you will not get an immediate response from your relay. The ON and OFF packets are different Downlinks and they should be sent one at a time. I will be updating this walkthrough with any corrections, as well as the code I’m trying to develop for a custom downlink to power cycle with a single command. Wish me luck!</p>
  <br>
  <p>The stock antenna did not cut it in my first deployment. If you’re planning on placing your LT indoors, perhaps consider putting the LT in an outdoor enclosure instaed, or running coax to an aftermarket antenna.</p>
  <p><i>When you do cable runs, think ‘teapot’- short, and stout. Thicker cable for antennas has better insulation against static accumulation, and thicker cable for electrical systems means less voltage drop.</i></p>
  <br>
  <p><i>Eample of a custom enclosure by @tteague: </i></p>
  <img src="images/enclosure.png" alt="Example of an enclosure for the LT by tteague." width="650px" height="auto">
  <p>This is the piece you’ll need to upgrade the antenna, it’s called “IPD/u.fl to N Type” You can also use the existing SMA out with an adapter. You’ll replace the pigtail on the board like a snap button on your pants, pop it off, snap it on.</p>
  <h3>EDIT: The IPEX/u.fl cable I ordered did not fit the ufl board on the LT. The next easiest solution is to simply order an SMA adapter that matches your cable configuration. The SMA out on the LT is an SMA Female (Not reverse polarity, like on a RAK miner for example)</h3>
  <img src="images/ufl.png" alt="Image of a ufl/ipex cable." width="550px" height="auto">
  <p><i>If your LT will be in sparse LoRaWAN coverage or indoors, highly consider running an aftermarket antenna. Any antenna that would work on your Helium miner would work on the LT, just be sure it’s in your region’s band.</i></p>
  <br>
  <p>Now you've got this incredible tool at your fingertips, create a folder on your phone with Helium Geek and a Safari bookmark to Datacake. Whenever you're ~500 blocks behind, start keeping an eye out. If your internet has been down and needs a reboot, your miner could probably benefit from a power cycle refreshing the snapshot. So if your router is offline and unresponsive, or if your miner gets stuck, and is behind by 2000-2500 blocks or so, cowboy boot those babies! <i>Do not abuse the ability to power cycle your hardware, never do so more than once a day.</i><p>
  <br>
  <p>Thanks for sticking through this walkthrough. Hopefully it helps you, if you got stuck anywhere along the way or have any questions, I'm @tanny#7241 on Discord, and @iLoveSpectra on Twitter.</p>
  <br>
  <p>If you’re feeling grateful, my HNT wallet address is provided below:<p>
  <h4>14fjXeSHN1t9f1TJLX5zWVC1P8iVPZ2M96Ke6iFDPwjkxvK3fZq</h4>
  <img src="images/qr.png" alt="tanny donation wallet qr code" width="250px" height="auto">
  <br>
  <h4><i>Hopefully you’ve reached the end of this walkthrough for the first time without having even ordered your LT-22222-L yet. If you haven't used the Helium network by creating applications on Helium Console for LoRaWAN sensors, there's no need to start with this I/O Relay. There are countless sensors with limitless applications that allow you to nitpick every element of the physical environment, almost anywhere. The steps Using the Relay are the same for any node, just follow the bread crumbs from your makers. Every off-grid, or even every out-door enclosure period, should be equipped with a temperature/humitidy sensor. Electronics are prone to static under 30% humidity, and moisture issues over 50%. Tow that fine line and you'll extend the life of your hardware by years.</i></h4>
  <br><br><br><br><br><br><br><br>
  </body>

</html>
