# ATWINC3400 Driver for Atmel Start / Atmel Software Framework 4 (ASF4)

Anyone who attempts to use the ATWINC3400 will quickly notice that Microchip does not support it in Atmel Start / Atmel Software Framework 4 (ASF4). It is only supported in ASF3 or in their MP Lab / Hamrony product. This a curious business decision which is likely due to them trying to drive users towards the MP Lab / Harmony development environment, but really this just ends up alienating users and making it more difficult to use their products.

In a similar vein, Microchip has also made the curious decision to not support the SAMD51 microcontroller in ASF3. Rather, it is only supported in ASF4. Basically this means that if you want to use a SAMD51 microcontroller with an ATWINC3400, you're out of luck and Microchip won't give you any help at all.

This repository is based on the ATWINC3400 driver v1.1.0 available in ASF3. I have also used the ATWINC1500 driver available in ASF4 to help in the conversion process to ASF4.

If you are interested in incorporating the ATWINC3400 into an Arduino project, we have also ported the ATWINC3400 driver to Arduino. You can find it here: https://github.com/Vulintus/ATWINC3400_Driver_Arduino

# Usage instructions

- If the instructions below are confusing, you can reference the following resources:
    - Tutorial/example code: https://github.com/davepruitt/samd_tutorials
    - Tutorial video: https://www.youtube.com/watch?v=8QXpHtQB0DE

1. In Atmel Start (https://start.atmel.com/), configure your project as you typically would using an ATWINC1500 module. This is important. Do not forget to add the "wifi middleware" to your project (which is the ATWINC1500 driver), and configure all of the correct pins. Remember to configure your clocks, and the SPI instance should be clocked at 48 MHz.

2. In Atmel Start, you MUST make sure the following configuration options are set:

- In the SPI instance, set the baud rate to **4000000**. If you're wondering why it should be that number, it is based on what the ATWINC1500 example projects use, and also based on the following documentation: https://microchipsupport.force.com/s/article/SPI-baud-rate-setting-for-SAME-S-V-devices-using-Atmel-Start
- In the SPI instance, enable "Advanced Configuration", and then set the "dummy byte" to be **0x100.**
- In the SPI instance, make sure to set all of your component signals (MISO, MOSI, and SCK).
- In the EXTIRQ instance, select the interrupt pin that you are using based on your board design.make sure to enable the interrupt that corresponds to the pin you are using on your board.
- In the EXTIRQ instance, enable the interrupt settings for the interrupt you are using, and make sure that **"Falling-edge detection"** is selected for the "sense configuration".
- In the WIFI instance, make sure to set all your component signals (CE pin, CS pin, and RESET pin).

3. Export your project to Microchip Studio/Atmel Studio

4. After opening your project in Atmel Studio, remove the "winc1500" folder from your project. You won't be needing that code since you're not using an ATWINC1500.

5. In the project file, or in your project settings, remove the "winc1500" folder from your list of "include directories".

6. Add the winc3400 driver from this repository into your project, and add the winc3400 folder to your list of "include directories" in your project.

7. In the file **nm_bsp_samd51.c**, define the value of CONF_WINC_EXT_INT_PIN to be the pin you selected as your interrupt pin. You can find the name of this pin in **atmel_start_pins.h**.

8. Build your project and use the winc3400 API!
