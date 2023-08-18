# Arduino_Internal_Interrupts


This code counts from 1 to 5 and stops counting.

The basics for this Interrupts is from this link.

https://www.circuitbasics.com/how-to-use-hardware-interrupts-and-timer-interrupts-on-the-arduino/

https://www.pjrc.com/teensy/td_libs_TimerOne.html


No hard ware required.

Uses Timer1 ISR, 
Timer1.initialize() to start, 
Timer1.detachInterrupt () to stop.


Use Volatile if you have a variable to play with in the ISR.
Do not use Serial print lines in ISRs.
Timer.initialize takes an argument that's 1 second.

## 1st program - no hardware required.  See following program ifyou want to see the action when the button is pressed.

```
#include <TimerOne.h>

volatile int count=0;
unsigned long previousMillis = 0;
const long interval = 1000;  

void setup() {
  pinMode(LED_BUILTIN, OUTPUT);
  pinMode(5, OUTPUT);
  
  Timer1.initialize(1000000); 
  //Timer1.initialize(5000000); 
     
  Timer1.attachInterrupt(longBlink);  
  Serial.begin (9600);
}

void longBlink() {
  //digitalWrite(10, !digitalRead(10));
  count++;
}

void loop() {

  if ( count >= 5) { Timer1.detachInterrupt(); }
  unsigned long currentMillis = millis();

  if (currentMillis - previousMillis >= interval) {
    // save the last time you blinked the LED
    previousMillis = currentMillis;

    // if the LED is off turn it on and vice-versa:
    Serial.println(count);
  }
}

```


To count down ,

initialize count to 10, and count-- in loop() and if count == 0  stop interrupt.


## Second program - here the counting starts when I press the button.
counts upto 5 and stops, Needs improvement.

```
#include <TimerOne.h>

volatile int count=0;
unsigned long previousMillis = 0;
const long interval = 1000;  

void setup() {
  pinMode(LED_BUILTIN, OUTPUT);
  pinMode(5, OUTPUT);
  
  Timer1.initialize(1000000); 
  //Timer1.initialize(5000000); 
     
  Timer1.attachInterrupt(longBlink);  
  Serial.begin (9600);
}

void longBlink() {
  //digitalWrite(10, !digitalRead(10));
  count++;
}

void loop() {

  if ( count >= 5) { Timer1.detachInterrupt(); }
  unsigned long currentMillis = millis();

  if (currentMillis - previousMillis >= interval) {
    // save the last time you blinked the LED
    previousMillis = currentMillis;

    // if the LED is off turn it on and vice-versa:
    Serial.println(count);
  }
}

```

### Third program - a bit better than earlier one - counts upto 5, resets counter.

```
/*
  Button

  Turns on and off a light emitting diode(LED) connected to digital pin 13,
  when pressing a pushbutton attached to pin 2.

  The circuit:
  - LED attached from pin 13 to ground through 220 ohm resistor
  - pushbutton attached to pin 2 from +5V
  - 10K resistor attached to pin 2 from ground

  - Note: on most Arduinos there is already an LED on the board
    attached to pin 13.

  created 2005
  by DojoDave <http://www.0j0.org>
  modified 30 Aug 2011
  by Tom Igoe

  This example code is in the public domain.

  https://www.arduino.cc/en/Tutorial/BuiltInExamples/Button
*/
#include <TimerOne.h>

// constants won't change. They're used here to set pin numbers:
const int buttonPin = 2;     // the number of the pushbutton pin
const int ledPin =  13;      // the number of the LED pin
unsigned long previousMillis = 0;
const long interval = 200;  

volatile int count=0;

// variables will change:
int buttonState = 0;         // variable for reading the pushbutton status

void setup() {
  // initialize the LED pin as an output:
  pinMode(ledPin, OUTPUT);
  // initialize the pushbutton pin as an input:
  pinMode(buttonPin, INPUT);
  Serial.begin(9600);
}

void longBlink() {
  count++;
}

void loop() {

 if ( count > 5) { Timer1.detachInterrupt(); count=-1; }

  
  // read the state of the pushbutton value:
  unsigned long currentMillis = millis();

  if (currentMillis - previousMillis >= interval) {
    // save the last time you blinked the LED
    previousMillis = currentMillis;

    buttonState = digitalRead(buttonPin);
    if( count >0){
    Serial.println ( count);
    }
    // check if the pushbutton is pressed. If it is, the buttonState is HIGH:
    if (buttonState == HIGH) {
      // turn LED on:
      digitalWrite(ledPin, HIGH);
      Serial.println ("pressed");
      Timer1.initialize(1000000); 
      Timer1.attachInterrupt(longBlink);  

    } else {
      // turn LED off:
      digitalWrite(ledPin, LOW);
    }
  }
}

```
