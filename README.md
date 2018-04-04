Sound: (Arduino + MP3 Shield)
#include <BlockDriver.h>
#include <FreeStack.h>
#include <MinimumSerial.h>
#include <SdFat.h>
#include <SdFatConfig.h>
#include <SysCall.h>
#include <SFEMP3Shield.h>
#include <SFEMP3ShieldConfig.h>
#include <SFEMP3Shieldmainpage.h>
#include <SPI.h>
SdFat sd;
SFEMP3Shield MP3player; //mp3

const int P1 = 0;
const int P2 = 1;
const int P3 = 2;
const int P4 = 3;
const int P5 = 4;
int Value1= 0;
int Value2= 0;
int Value3= 0;
int Value4= 0;
int Value5= 0;
const uint8_t volume = 5;



void setup() {
Serial.begin(9600); // Starts the serial communication
sd.begin(SD_SEL, SPI_HALF_SPEED);
MP3player.begin();
}

void loop() {

Serial.print ("V1:");
Serial.print (Value1); 
Serial.print (" ");
Serial.print ("V2:");
Serial.print (Value2);
Serial.print (" ");
Serial.print ("V3:");
Serial.print (Value3);
Serial.print (" ");
Serial.print ("V4:");
Serial.print (Value4);
Serial.print (" ");
Serial.print ("V5:");
Serial.println (Value5);

Value1 = analogRead(P1);
Value2 = analogRead(P2);
Value3 = analogRead(P3);
Value4 = analogRead(P4);
Value5 = analogRead(P5);
MP3player.setVolume(volume, volume);


if(Value1 > 100){
MP3player.playTrack(1);
delay(100);
MP3player.stopTrack();
}

else if (Value2 > 100){
MP3player.playTrack(2);
delay(200);
MP3player.stopTrack();
}

else if (Value3 > 100){
MP3player.playTrack(3);
delay(100);
MP3player.stopTrack();
}

else if (Value4 > 100){
MP3player.playTrack(4);
delay(100);
MP3player.stopTrack();
}

else if (Value5 > 100){
MP3player.playTrack(5);
delay(100);
MP3player.stopTrack();
}
 
else{
}
}

Arduino (Serial Communication with Processing)
char val;
int P1 = 0;
int P2 = 1;
int P3 = 2;
int P4 = 3;
int P5 = 4;
int led = 10;
int valueP1 = 0;
int valueP2 = 0;
int valueP3= 0;
int valueP4= 0;
int valueP5= 0;
int inByte = 0;

void setup() {
pinMode (led, OUTPUT);
Serial.begin (9600);
establishContact();
}

void loop() {
if(Serial.available()>0);{
inByte=Serial.read();
valueP1 = analogRead(P1);
valueP1 = valueP1/4;
delay(10);
valueP2 = analogRead(P2);
valueP2 = valueP2/4;
delay(10);
valueP3 = analogRead(P3);
valueP3 = valueP3/4;
delay(10);
valueP4 = analogRead(P4);
valueP4 = valueP4/4;
delay(10);
valueP5 = analogRead(P5);
valueP5 = valueP5/4;
delay(10); 
}
Serial.write(valueP1);
Serial.write(valueP2);
Serial.write(valueP3);
Serial.write(valueP4);
Serial.write(valueP5);
//data of FSR

if(Serial.available()){
val = Serial.read();
}
if (val == '1'){
digitalWrite(led, HIGH);
}
else{
digitalWrite(led, LOW);
}
delay(10);
}
//LED
void establishContact(){
  while(Serial.available()<=0){
    Serial.print('A');
    delay(300);
  }}

Processing
import processing.serial.*;
Serial myPort;    
int valueP1;
int valueP2;
int valueP3;
int valueP4;
int valueP5;
int[]serialInArray = new int [5];
int serialCount=0;
boolean firstContact = false;

void setup()
{
  size(1980, 1080);
  background(225);
  stroke(160);
  fill(50);
  String portName = Serial.list()[2]; //change the 0 to a 1 or 2 etc. to match your port
  myPort = new Serial(this, portName, 9600);
}

void draw(){
  rect(0,0,1980,1080);
  
if (valueP1 > 100){
  fill(245,238,111);//yellow
  myPort.write('1');        
   println("1"); 
}
else if (valueP2 > 100){
  fill(182,229,157);// green 
  myPort.write('1');        
   println("1"); 
}
else if (valueP3 > 100){
  fill(27,106,188); // blue
  myPort.write('1');         
   println("1"); 
}
else if (valueP5 > 100){
  fill(245,111,111);//red 
  myPort.write('1');        
   println("1"); 
}
else if (valueP4 > 100){
  fill(204,111,245);//purple 
  myPort.write('1');         
   println("1"); 
}
else{
  fill(206,206,206);
}}
    
void serialEvent(Serial port){
     int inByte = port.read();
     if (firstContact == false){
       if(inByte == 'A'){
         port.clear ();
         firstContact = true;
         port.write('A');
       }}
       else{
         serialInArray[serialCount] = inByte;
         print(inByte+ "\t");
         serialCount++;
         delay (8);
         
         if (serialCount==5){
           valueP1 = serialInArray[0];
               valueP2 = serialInArray[1];
                   valueP3 = serialInArray[2];
                       valueP4 = serialInArray[3];
                           valueP5 = serialInArray[4];
           
           port.write('A');
           serialCount = 0;
           println();
         }}}
