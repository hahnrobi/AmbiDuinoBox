# AmbiDuinoBox

This may have limited interest to most AtmoLight users, as you will normally setup your ardiuino for the light system you wish to use then forget about it.

However, due to various reasons (like testing) I wanted to be able to leave the arduino alone and run up AtmoWin (in either AtmoDuino or V2 mode) or Ambibox and have them still work. Because they have different headers, this requires something a little different. The solution here will accept any header from the above (could be more later) through a matching method, it does appear to work VERY well at speed.

To use:
1. Please already know your FastLED config before trying this, as it's always best to get the lights working with the 'FirstLight' example :)

//-----------------------EDITABLE CONFIGURATION

#define NUM_LEDS 176

#define LED_TYPE WS2812B

#define DATA_PIN 11

#define CLOCK_PIN 0 //leave at zero if no clock pin used

#define COLOUR_ORDER RGB

#define SERIAL_SPEED 512000

//-----------------------EDITABLE CONFIGURATION

2. Edit the top section with your configuration, if you use LEDs with a clock pin, change the number from zero to use the correct addLEDs call, or leave it at zero to ignore

3. The default speed I set here is 512,000, which is supported in the following ways:

    Ambibox: 'AtmoDuino' section from SerialPortConfig.ini in the Ambibox program files dir
    
    Atmowin: Requires 1.0.0.9 from AtmoWin thread, change speed in device configuration option.
    
    
4. Try other speed values. If you have only a few LEDs then the standard arduino 115,200 will work ok, however that is not enough bandwidth for larger amounts of LEDs. It is also common to have some lower speeds fail despite higher ones working, most Unos can go pretty fast.

5. Hope it works for you



Information about the detection for those interested:

I worked out how to calculate the correct 4 bytes for each header type, this means you only need to enter the NUM_LEDS value:
AtmoDuinoV2 - 0xD7, 0xEE, 0x23, NUM_LEDS - 1

AtmoDuino on Ambibox - 0xFF, 0x00, 0x00, NUM_LEDS * 3 - this value can overflow, but when truncated the byte leftover is matching ambibox, so that is what ambibox appears to do too

AtmoDuino on AtmoWin - 0xFF, 0x00, 0x00, NUM_LEDS


So that the loop is not complex, it is only ensuring each header part received matches at least one predefined header, but it could do better. If anyone better at that sort of thing (I tried a success array but it took too long) then please let me know.
