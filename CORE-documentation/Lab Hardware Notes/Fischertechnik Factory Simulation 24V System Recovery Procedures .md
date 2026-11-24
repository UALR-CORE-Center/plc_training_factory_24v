Fischertechnik Factory Simulation 24V System Recovery Procedures

1. Restore the PLC program   
   1. Toggle the switch on the PLC to the center position.  
   2. Link to download the program file: [https\://github.com/UALR-CORE-Center/plc\_training\_factory\_24v/blob/docs-marty/CORE-LearningFactory\_4\_0\_24V\_V14\_TP\_V18\_V21/LearningFactory\_4\_0\_24V\_V14\_TP\_V18\_V21.ap21](https://github.com/UALR-CORE-Center/plc_training_factory_24v/blob/docs-marty/CORE-LearningFactory_4_0_24V_V14_TP_V18_V21/LearningFactory_4_0_24V_V14_TP_V18_V21.ap21)  
   3. Locate the computer (engineer workstation) with TIA Portal Step 7 installed.  
      1. Open program file in TIA Portal Step 7 (image of step 7 logo)  
      2. Disconnect ethernet cables from PLC to pi and router  
      3. Connect ethernet cable to only the PLC (or connect to router wirelessly)  
      4. "Go Online" in Step 7   
      5. Upload the program (up arrow).

2. Restore the Raspberry Pi 4b image  
   1. Link to download the image file: [https\://github.com/fischertechnik/plc\_training\_factory\_24v/releases/download/V04/2022-01-11-bullseye-IOTpi2.zip](https://github.com/fischertechnik/plc_training_factory_24v/releases/download/V04/2022-01-11-bullseye-IOTpi2.zip) (UPDATE with new custom image when it is ready)  
   2. Remove the micro SD card from the Raspberry Pi 4b.  
   3. Place the micro SD card in the slot on the right side of the laptop.  
   4. Use the Raspberry Pi Imager [https\://www\.raspberrypi.com/software/](https://www.raspberrypi.com/software/) to write the Raspberry Pi 4 image to the \>=4GB micro SD card.  
        
3. Restore the TXT 4.0 Controller Node Red Programs  
   1. Link to download the Node RED program files: [https\://github.com/fischertechnik/plc\_training\_factory\_24v/tree/master/GatewayPLC](https://github.com/fischertechnik/plc_training_factory_24v/tree/master/GatewayPLC)  
   2. Locate the USB to Mini-USB cable and plug into laptop and TXT 4.0 Controller  
   3. Install ROBO Pro Coding software (Windows): [https\://apps.microsoft.com/detail/9mxpk52r734c?hl=en-US\&gl=US](https://apps.microsoft.com/detail/9mxpk52r734c?hl=en-US&gl=US)  
   4. Once installed and the ROBO Pro Coding software is running, select the controller icon in the top right corner and connect to the device.  
   5. Load the [GatewayPLC.ft](http://GatewayPLC.ft) program in the editor  
   6. Press upload button (up arrow on folder) in top right corner of ROBO Pro Coding software  
   7. Finally follow these instructions to upload the configuration files: [https\://github.com/fischertechnik/plc\_training\_factory\_24v/blob/master/howto\_new\_TXT.md](https://github.com/fischertechnik/plc_training_factory_24v/blob/master/howto_new_TXT.md)  
        
4. (Optional) Factory Reset the TP Link Router  
   1. Find a push pin and use it to press the reset button on the back of the router until it power-cycles.  
   2. Go to [tplinkrouter.net](http://tplinkrouter.net), set a new password, and then set the IP address to 192.168.0.252.

