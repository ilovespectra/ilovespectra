<!DOCTYPE html>
<html lang="en" dir="ltr">
  <head>
    <meta charset="utf-8">
  </head>

  <body>
    <h2>RAK4631 Weather-Monitor <i>*Additional Help</i></h2>
    <p><i>Information to help setup a weather monitor (US915) using the RAK4631 and the walkthrough provided by RAK.</i></p>
    <p>I wound up using a half a dozen sources to try to get this all setup. Hoping consolidating that data makes your life a little easier. I do not include the decoding for the light sensor, it was not needed for my application.</p>
    <P>First things first, be sure you've gone through the steps in 
      <a href="https://github.com/RAKWireless/WisBlock/tree/master/examples/RAK4630/solutions/Weather_Monitoring" target="_blank">this walkthrough</a>. 
      It will help you physically assemble the weather sensor, as well as setup an IDE with the boards and libraries you'll need. I use Arduino IDE for this walkthrough.
      Once you've got the monitor assembled and everything all set on Arduino IDE, head on over <a href="https://github.com/RAKWireless/WisBlock/tree/master/PlatformIO/RAK4630" target="_blank">here</a> to get your RAK4631 all setup with the latest bootloader.</p>
    <br><p><i>Welcome back</i></p>
    <p>Now you're ready to start setting things up. Head over to <a href="https://console.helium.com/" target="_blank">Helium Console</a> and setup an account if you 
      don't have one yet. Click "Add New Device" and enter a name for your Weather-Monitor.<br><br><img src="images/heliumadddevice.png" alt="" width="300px" height="auto"><br>Copy and paste your DevEui, AppEui, and AppKey into a text editing app and 
      save the file as something like "Weather Monitor Keys".<br><br><img src="images/heliumedeveui.png" alt="" width="500px" height="auto"><br><br>You may have seen the Sketch File in the Weather Monitor walkthrough I provided above, it may have looked 
      like total gibberish at the time, but we'll make a hacker out of you yet! Copy and paste that code into your Arduino IDE, it should look like this:</p>
    <img src="images/arduino1.png" alt="" width="700px" height="auto">
    <p><b>Here are the things you'll need to modify:</b><br><br>Replace line 48 with:</p>
    
      #define LORAWAN_DATERATE DR_3                   /*LoRaMac datarates definition, from DR_0 to DR_5*/
    
   <p>Replace line 51 with:</p>
    
     DeviceClass_t g_CurrentClass = CLASS_A;          /* class definition*/

   <p>Replace line 52 with:</p>
      
    LoRaMacRegion_t g_CurrentRegion = LORAMAC_REGION_US915;    /* Region:US915*/
      
   <p>Lines 73, 74, and 75 are to be filled in using the Euis/Keys you saved from Helium Console earlier. Only replace the blank areas indicated below:</p>
     
     uint8_t nodeDeviceEUI[8] = {0x__, 0x__,...};
     uint8_t nodeAppEUI[8] = {0x__, 0x__,...};
     uint8_t nodeAppKey[16] = {0x__, 0x__,...};
    
 <p>It should look like this:</p>
<img src="images/arduinomod.png" alt="" width="800px" height="auto">
   <p>Click the check in the corner to verify, or compile, the sketch. You shouldn't receive any errors. If you do, simply use the debug at the bottom of the application to track down your typo.</p>     
<br><h2>Configuring Datacake</h2>
    <p>We'll be using <a href="https://datacake.co/" target="_blank">Datacake.co</a> to read the data from our sensor. Head over and create an account if you don't have one already.</p>
    <p>Navigate to "Devices" and select "Add Device"</p><img src="images/datacakeadddevice.png" alt="" width="550px" height="auto"><br>Select "LoraWan" > "RAK Wisnode Starting Template" > "Helium"</p><br>
<img src="images/datacakelora.png" alt="" width="500px" height="auto"><img src="images/datacakerak.png" alt="" width="500px" height="auto"><img src="images/datacakehelium.png" alt="" width="500px" height="auto">
    <p>Then enter your Device EUI and click "Next"</p><br><img src="images/datacakedeveui.png" alt="" width="500px" height="auto"> 

Navigate to Configuration and down to your Decoder. Scroll down to line 60 of your Decoder, replace it with '}' and hit enter. Paste the following function:


    function Decoder(bytes, port) {
    var decoded = {};

    decoded.temperature = (bytes[1] << 8 | (bytes[2])) / 100;
    decoded.humidity = (bytes[3] << 8 | (bytes[4])) / 100;
    decoded.pressure = (bytes[8] | (bytes[7] << 8) | (bytes[6] << 16) | (bytes[5] << 24)) / 100;
    decoded.light = bytes[12] | (bytes[11] << 8) | (bytes[10] << 16) | (bytes[9] << 24);

    return decoded;

  <p>Place another '}' below, and hit enter again. It should look like this:</p><br><img src="images/datacakefunction.png" alt="" width="850px" height="auto">Then scroll down past your Decoder in your "Fields" to "Temperature" and select "Add Mapping Field"</p><br><img src="images/datacakeaddmap.png" alt="" width="900px" height="auto"><br>
<p>Enter the following information: "Type"- Float, "Name"- <i>any, ie. Temperature Fahrenheit</i>, "Source"- set 0 to 100, "Target"- set 32 to 212 </p><img src="images/datacakemapfield.png" alt="" width="500px" height="auto">
<p>Now head over to your dashboard and toggle the edit switch in the right of "Permissions" and edit it however you like, this is my configuration. You can duplicate or add fields, and customize their data and appearence really easily. <i>Piece of Datacake!</i></p>
  <br><img src="images/datacakedash.png" alt="" width="900px" height="auto">
  <p>Enjoy!</p>
  <p><i>(Edit: The LoRa data fields no longer appear to function after editing the code, but again I do not need the data. You'll need to troubleshoot that if you do.)</i></p>
    
  </body>

</html>
