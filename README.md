# Uduino Uno R3 - Hello World
 Initial development for **Uduino Uno R3 V1.0**


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
