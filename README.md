# Initial development for Uduino Uno R3

<div><img src= https://docs.arduino.cc/static/bbceab04f8e0726194ef4dfe2457097f/image.svg width=300 align="left" /></div>

|          **Board**          |          Arduino UNO R3          |
|:---------------------------:|:--------------------------------:|
|        **Microcontroller**  |          ATmega328P              |
|          **USB connector**  |         USB-B                    |
|     **Built-in LED Pin**    |                13                |
|     **Digital I/O Pins**    |                14                |
|    **Analog input pins**    |                 6                |
|         **PWM pins**        |                 6                |
|           **UART**          |                Yes               |
|           **I2C**           |                Yes               |
|           **SPI**           |                Yes               |
|       **I/O Voltage**       |                5V                |
| **Input voltage (nominal)** |               7-12V              |
|  **DC Current per I/O Pin** |               20 mA              |
|  **Power Supply Connector** |            Barrel Plug           |
|      **Main Processor**     |         ATmega328P 16 MHz        |
|   **USB-Serial Processor**  |         ATmega16U2 16 MHz        |
|        **Memory**       | 2KB SRAM, 32KB FLASH, 1KB EEPROM |


---
## Uduino Uno R3 V1.0

Documentation &rarr; [Arduino UNO R3](https://docs.arduino.cc/hardware/uno-rev3)

Configuring Visual Studio with [PlatformIO Extension](https://platformio.org/install/ide?install=vscode). Remember to set the **platform.ini** file with:
```
[env:uno]
platform = atmelavr
board = uno
framework = arduino
```
## Simple Projects
### Branch: HelloWorld-Blinky 
```
#include <Arduino.h>
//Blinky - Hello World

// defines
#define LED1 7 //LED Digital Port 7
#define LED2 8 // LED Digital Port 8

void setup() {
  //Pins Setup
  pinMode(LED1,OUTPUT);
  digitalWrite(LED1, LOW);
  pinMode(LED2,OUTPUT);
  digitalWrite(LED2, LOW);
}

void loop() {
    //HELLO WORLD
    digitalWrite(LED1,HIGH);
    digitalWrite(LED2,LOW);
    delay(100);
    digitalWrite(LED1,LOW);
    digitalWrite(LED2,HIGH);
    delay(100);

}
```

### Branch: Button-Blinky 
```
#include <Arduino.h>
//Press the Button to turn both LEDs ON/OFF

// defines
#define LED1 7 //LED Digital Port 7
#define LED2 8 // LED Digital Port 8
#define BUTTON1 9 // Button Digital Port 9

//global var
unsigned int state = 0; //change LED state
unsigned int pressB = 0; //button presses


void setup() {
  //Pins Setup
  pinMode(LED1,OUTPUT);
  digitalWrite(LED1, LOW);
  pinMode(LED2,OUTPUT);
  digitalWrite(LED2, LOW);
  pinMode(BUTTON1, INPUT);
  Serial.begin(9600);
  Serial.println("Begin...");
}

void loop() {
  //check if the button is pressed then change the state of the LEDs to ON/OFF
  if(digitalRead(BUTTON1))
  {
    state = !state;
    Serial.println("Button");
    digitalWrite(LED1,state);
    digitalWrite(LED2,state);
    pressB++; //increment button press
    Serial.println(pressB);
    delay(100); //debounce
  }
  delay(100); //debounce
}
```

### Branch: Debounce-Button
```
// defines
#define LED1 7 //LED Digital Port 7
#define LED2 8 // LED Digital Port 8
#define BUTTON1 9 // Button Digital Port 9

// Variables will change:
int ledState = HIGH;        // the current state of the output pin
int buttonState;            // the current reading from the input pin
int lastButtonState = LOW;  // the previous reading from the input pin

// the following variables are unsigned longs because the time, measured in
// milliseconds, will quickly become a bigger number than can be stored in an int.
unsigned long lastDebounceTime = 0;  // the last time the output pin was toggled
unsigned long debounceDelay = 50;    // the debounce time; increase if the output flickers

void setup() {
  pinMode(LED1,OUTPUT);
  digitalWrite(LED1, LOW);
  pinMode(LED2,OUTPUT);
  digitalWrite(LED2, LOW);
  pinMode(BUTTON1, INPUT);

}

void loop() {
  // read the state of the switch into a local variable:
  int reading = digitalRead(BUTTON1);

  // check to see if you just pressed the button
  // (i.e. the input went from LOW to HIGH), and you've waited long enough
  // since the last press to ignore any noise:

  // If the switch changed, due to noise or pressing:
  if (reading != lastButtonState) {
    // reset the debouncing timer
    lastDebounceTime = millis();
  }

  if ((millis() - lastDebounceTime) > debounceDelay) {
    // whatever the reading is at, it's been there for longer than the debounce
    // delay, so take it as the actual current state:

    // if the button state has changed:
    if (reading != buttonState) {
      buttonState = reading;

      // only toggle the LED if the new button state is HIGH
      if (buttonState == HIGH) {
        ledState = !ledState;
      }
    }
  }

  // set the LED:
  digitalWrite(LED1, ledState);
  digitalWrite(LED2, ledState);

  // save the reading. Next time through the loop, it'll be the lastButtonState:
  lastButtonState = reading;
}

```
