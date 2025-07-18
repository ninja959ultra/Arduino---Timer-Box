# Arduino---Timer-Box


# Code Below:
```cpp
#include <LiquidCrystal_I2C.h>
#include <Servo.h>

Servo servo;
LiquidCrystal_I2C lcd(0x27, 16, 2);

byte increasSec = 2;
byte decreasSec = 3;
byte increasMin = 5;
byte decreasMin = 6;
byte start = 4;
byte blueLED = 7;
unsigned long time = 50000;
unsigned long totalSeconds = time / 1000;


int seconds, minutes;
unsigned long timeStart, lastTime;


void showOnLCD() {
  if (minutes == 0) {
    lcd.setCursor(5, 0);
    lcd.print("  ");
    lcd.setCursor(5, 0);
    lcd.print("00");
  }
  
  else if (minutes < 10) {
    lcd.setCursor(5, 0);
    lcd.print("  ");
    lcd.setCursor(6, 0);
    lcd.print(minutes);
  }

  else{
    lcd.setCursor(5, 0);
    lcd.print("  ");
    lcd.setCursor(5, 0);
    lcd.print(minutes);
  }

  if (seconds == 0) {
    lcd.setCursor(8, 0);
    lcd.print("  ");
    lcd.setCursor(8, 0);
    lcd.print("00");
  }

  else if (seconds < 10) {
    lcd.setCursor(8, 0);
    lcd.print("  ");
    lcd.setCursor(8, 0);
    lcd.print('0');
    lcd.setCursor(9, 0);
    lcd.print(seconds);
  }

  else{
    lcd.setCursor(8, 0);
    lcd.print("  ");
    lcd.setCursor(8, 0);
    lcd.print(seconds);
  }
}


void setup() {
  for (byte i=2; i<7; i++) { // Setup buttons
    pinMode(i, INPUT_PULLUP);
  }

  pinMode(blueLED, OUTPUT);

  lcd.init();
  lcd.backlight();
  Serial.begin(9600);
  lcd.setCursor(5,0);
  lcd.print("00:00");
}

void loop() {
  totalSeconds = time / 1000;

  minutes = totalSeconds / 60;
  seconds = totalSeconds % 60;

  if (digitalRead(increasSec) == LOW) {
    time+=1000;
  }

  if (digitalRead(decreasSec) == LOW && time > 0) {
    time-=1000;
  }

  if (digitalRead(increasMin) == LOW) {
    time+=60000;
  }

  if (digitalRead(decreasMin) == LOW && minutes > 0) {
    time-=60000;
  }

  if (digitalRead(start) == LOW) {
    timeStart = millis();
    while (time > 0){
      if (millis() - lastTime > 1000){
        lastTime = millis();
        totalSeconds--;
        minutes = totalSeconds / 60;
        seconds = totalSeconds % 60;
        showOnLCD();
        Serial.print("Minutes= ");
  Serial.println(minutes);
  Serial.print("Seconds= ");
  Serial.println(seconds);
  Serial.print("Time = ");
  Serial.println(time);
      }

    } // End while loop

  } // END start button


  showOnLCD();

  Serial.print("Minutes= ");
  Serial.println(minutes);
  Serial.print("Seconds= ");
  Serial.println(seconds);
  Serial.print("Time = ");
  Serial.println(time);
  delay(100);
}

```
