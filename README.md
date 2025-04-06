**ECONET ETHERNET TEST, PLEASE REPORT IF YOU HAVE THESE DEVICES AND ECONET DEVICE TO TEST**
    
    
  I have a m5stack Core basic v2.7 controller (SKU: k001-v27) and the PoE LAN module (SKU: k012-c-v12) and want to use my controller for the econet's team project for interface of econet devices (i.e. Rheem water tanks, etc) via ethernet instead of WiFi. Since the base files of their project already reference tx/rx pins and both ethernet and rs485 need separate buses/pins then the original files needed to be edited.

  The econet base yaml was modified to change the rx/tx pin mapping to busRx_pin and busTx_pin. This allows the user to substitute the standard rx/tx pin in substitutions for the ethernet controller and also modify the pin map specifically for the uart rs485 interface. The econet base yaml was also modified to deleted the WiFi and Captive components (because it conflicts with ethernetcomponent). So in order to properly map your device you need to do the following:

[yaml example.txt](https://github.com/user-attachments/files/19618777/yaml.example.txt)

This picture below is the example of a pinmap from m5stack. You can sometimes find this at the bottom of the documents section of their website. Note how that pinmap compares to the yaml example. You need to confirm a few things first before buying a controller and modules.
  1. There has to be a dedicated pin pair for rx/tx of your rs485. These pins must be available and cannot share any other component or device (such as ethernet, i2c, or be the sole rx/tx)
  2. There has to be a separate dedicate pin pair for rx/tx of your controller/ethernet component
  3. This yaml is for w5500 ethernet chip using SPI interface and it needs dedicated pins for MISO, MIOS, CS, rst, int, and clock. By using this module you cannot use the screen because most if not all screens use SPI interace

![Screenshot_5-4-2025_19140_docs m5stack com](https://github.com/user-attachments/assets/a6ef1023-4931-4f6d-b34c-4589e43ac2c3)


  In order to make the Core basic v2.7 and PoE LAN module to work you have to solder the rs485 chip onto the m5stack lan board. This locks the rs485 tx/rx to pins 5 and 15. You also have the txd0/rxd0 and txd2/txd2. In the example I set base substitution rx/tx to the txd2/rxd2 because I felt like it. After you have everything soldered and connected your yaml configured and flashed to your device you have to wire it up. PoE connection will power and allow you to connect to the controller via Home Assistant. However, in order for this to work you have to have the A/B AND the ground from your rs485 device(s) connected in order for your device to communicate with HA. If you are using PoE and don't have the ground from your rs485 device ground to the interface on your controller then the rs485 will try to ground to the same as the ethernet controller and it prevents the controller from communicating. (I don't know why, maybe it cross talks and cancels out transmission). 




**USING THIS DEVICE AND MODULE PAIR AS A rs485 BUS FOR rs485 SENSORS**


to be continued, reference https://community.home-assistant.io/t/modbus485-sht30-rh-and-temp-sensor-with-atoms3lite/866825/6 and https://community.home-assistant.io/t/entities-available-on-wifi-component-but-not-on-ethernet-component/872677 to get started
