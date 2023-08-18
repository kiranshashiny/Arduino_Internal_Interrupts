# Arduino_Internal_Interrupts


This code counts from 1 to 5 and stops counting.

No hard ware required.

Uses Timer1 ISR, 
Timer1.initialize() to start, 
Timer1.detachInterrupt () to stop.


Use Volatile if you have a variable to play with in the ISR.
Do not use Serial print lines in ISRs.
Timer.initialize takes an argument that's 1 second.

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
