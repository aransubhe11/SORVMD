# SORVMD
Final year project solar operated robotic robotic vehicle for metal detection arduino program
#include <TinyGPS++.h>
#include <SoftwareSerial.h>
#define rxGPS 3
#define txGPS 2
SoftwareSerial gpsSerial(rxGPS, txGPS);
TinyGPSPlus gps;
int Pin = 7;
int Buzz = 8;
int m1 = 9;
int m11 = 10;
int m2 = 11;
int m21 = 12;
int state;
void setup() {
 pinMode(Pin, INPUT);
 pinMode(Buzz, OUTPUT);
 pinMode(m1, OUTPUT);
 pinMode(m11, OUTPUT);
 pinMode(m2, OUTPUT);
pinMode(m21, OUTPUT);
 Serial.begin(9600);
 gpsSerial.begin(9600);
}
void loop() {
 Metal();
 if (Serial.available() > 0) 
 {
 state = Serial.read();
 if (state == '1')
 {
 digitalWrite(m1, LOW);
 digitalWrite(m11, HIGH);
 digitalWrite(m2, LOW);
 digitalWrite(m21, HIGH);
 }
 else if (state == '2')
 {
 digitalWrite(m1, HIGH);
 digitalWrite(m11, LOW);
 digitalWrite(m2, HIGH);
 digitalWrite(m21, LOW);
 }
 else if (state == '3')
 {
 digitalWrite(m1, LOW);
 digitalWrite(m11, HIGH);
 digitalWrite(m2, LOW);
 digitalWrite(m21, LOW);
 }
 else if (state == '4')
 {
digitalWrite(m1, LOW);
 digitalWrite(m11, LOW);
 digitalWrite(m2, LOW);
 digitalWrite(m21, HIGH);
 }
 else if (state == '5')
 {
 digitalWrite(m1, LOW);
 digitalWrite(m11, LOW);
 digitalWrite(m2, LOW);
 digitalWrite(m21, LOW);
 }
 }
}
void Metal() {
 int sensorValue = digitalRead(Pin);
 if (sensorValue == LOW) {
 digitalWrite(Buzz , LOW);
 delay(500);
 }
 else {
 Serial.println("Object Detected");
 digitalWrite(m1, LOW);
 digitalWrite(m11, LOW);
 digitalWrite(m2, LOW);
 digitalWrite(m21, LOW);
 digitalWrite(Buzz , HIGH);
 Gps();
 delay(500);
 }
}
void Gps()
{
 while (gpsSerial.available()) 
 {
 if (gps.encode(gpsSerial.read())) 
 {
 Serial.print("Latitude " + String(gps.location.lat(), 6) +" ,
 " + " Longitude " + String(gps.location.lng(), 6));
 delay(4000);
 }
 }
}
