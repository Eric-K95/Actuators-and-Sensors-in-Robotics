# Actuators-and-Sensors-in-Robotics
Sketches to share my thoughts
//Exercise: color detection sensor
//designed by JIN WOO KIM
//Purpose: The code designed to detect only three colors(Red, Green, Blue), took one of the highest value of colors from the serial monitor, and the highest value of color was displayed on LCD and turned on the LED. This color detection sensor is often used in industrial and manufacturing application for quality control. From this exercise, practice the use  of color detection sensor, make the color appear on the LCD, and turn on the LED that matches that color.

//LCD
#include <Wire.h>
#include <LiquidCrystal_I2C.h>
LiquidCrystal_I2C lcd(0x27,16,2);

//LED pin
int ledRed = 13;
int ledGreen = 12;
int ledBlue = 11;

//Color Detection Sensor
#define S0 3 //Please notice the Pin's define
#define S1 4 //Combination of S0 and S1 decision output signal frequency scaling factor
#define S2 5 //S2 and S3 are let the red, green, blue, what kind of light through a filter
#define S3 6
#define OUT 2

// Stores frequencies read by the photodiodes
int redFrequency = 0;
int greenFrequency = 0;
int blueFrequency = 0;
int frequency = 0;

void setup() {
pinMode(ledRed, OUTPUT);
pinMode(ledGreen, OUTPUT);
pinMode(ledBlue, OUTPUT);
analogWrite(ledRed, LOW);
analogWrite(ledGreen, LOW);
analogWrite(ledBlue, LOW);

Serial.begin(9600);
lcd.backlight();
lcd.begin();
lcd.setCursor(0,0); //first column, row #1
lcd.print("Robotics");
delay(2000); // delay of the message 2000ms
lcd.clear(); // clear screen

pinMode(S0, OUTPUT);
pinMode(S1, OUTPUT);
pinMode(S2, OUTPUT);
pinMode(S3, OUTPUT);
pinMode(OUT, INPUT);
digitalWrite(S0,HIGH);
digitalWrite(S1,LOW); // frequency scaling: 20%
}
void loop() {
digitalWrite(S2,LOW); // Set to read RED filtered photodiodes
digitalWrite(S3,LOW);
redFrequency = pulseIn(OUT, LOW); // Reading frequency for RED

//Remaping the value of the frequency to the RGB Model of 0 to 255
frequency = map(frequency, 25,72,255,0);
Serial.print("R = "); 
Serial.print(redFrequency); // Print the frequency for RED
delay(100);
digitalWrite(S2,HIGH); // Set to read GREEN filtered photodiodes
digitalWrite(S3,HIGH);
greenFrequency = pulseIn(OUT, LOW); // Reading the frequency for GREEN

//Remaping the value of the frequency to the RGB Model of 0 to 255
frequency = map(frequency, 30,90,255,0);

Serial.print(" G = "); // Printing the frequency for GREEN
Serial.print(greenFrequency); 
delay(100);
digitalWrite(S2,LOW); digitalWrite(S3,HIGH); // Set to read BLUE filtered photodiodes
blueFrequency = pulseIn(OUT, LOW); // Reading the frequency for BLUE

//Remaping the value of the frequency to the RGB Model of 0 to 255
frequency = map(frequency, 25,70,255,0);
Serial.print(" B = "); 
Serial.println(blueFrequency); // Print the frequency for BLUE
delay(100);

//print values on lcd screen
    if (greenFrequency > redFrequency && redFrequency < blueFrequency)
    {
      lcd.clear();
      lcd.setCursor(0, 0); //first column, row #1
      lcd.print("Color Detection");
      lcd.setCursor(0, 1); //second column, row #1
      lcd.print("Color : ");
      lcd.print("Red");
      Serial.println(" - (Red Color)");
      digitalWrite(ledRed, HIGH);
      digitalWrite(ledGreen, LOW);
      digitalWrite(ledBlue, LOW);
    }
      else if ( blueFrequency > redFrequency && blueFrequency > greenFrequency && greenFrequency < redFrequency)
  {
    lcd.clear();
    lcd.setCursor(0, 0); //first column, row #1
    lcd.print("Color Detection");
    lcd.setCursor(0, 1); //second column, row #1
    lcd.print("Color : ");
    lcd.print("Green");
    Serial.println(" - (Green Color)");
    digitalWrite(ledGreen, HIGH);
    digitalWrite(ledBlue,LOW);
    digitalWrite(ledRed,LOW);
  }
    else if (redFrequency > greenFrequency && greenFrequency > blueFrequency && blueFrequency < greenFrequency)
    {
    lcd.clear();
    lcd.setCursor(0, 0); //first column, row #1
    lcd.print("Color Detection");
    lcd.setCursor(0, 1); //second column, row #1
    lcd.print("Color : ");
    lcd.print("Blue");
    Serial.println(" - (Blue Color)");
    digitalWrite(ledBlue, HIGH);
    digitalWrite(ledRed, LOW);
    digitalWrite(ledGreen, LOW);
  }
  Serial.println();
  delay(1000); // delay 1000ms
}
