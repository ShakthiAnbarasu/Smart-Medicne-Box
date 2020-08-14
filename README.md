# Smart-Medicne-Box
#include <Wire.h>
#include <LiquidCrystal.h>
#include "RTClib.h"

DateTime now;

char daysOfTheWeek[7][12] = {"Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"};

RTC_DS3231 rtc;

LiquidCrystal lcd(7, 6, 5, 4, 3, 2); // (rs, e, d4, d5, d6, d7)

int button1=8;
int button2=9;
int button3=10;
int button4=A0;


int buzz_led=13;

int buttonVal1,buttonVal2,buttonVal3,buttonVal4;

int buzz8amHH = 8;          //    HH - hours         ##Set these for reminder 
time in 24hr Format
int buzz8amMM = 00;          //    MM - Minute

int buzz8amSS = 30;          //    SS - Seconds

int buzz2pmHH = 14;          //    HH - hours

int buzz2pmMM = 00;          //    MM - Minute
int buzz2pmSS = 00;          //    SS - Seconds

int buzz8pmHH = 20;          //    HH - hours

int buzz8pmMM = 00;          //    MM - Minute
int buzz8pmSS = 00;          //    SS - Seconds
void firstScreen(void);

void showDate(void);
void showTime(void);
void showDay(void);
void secondScreen(void);
void at8am(void);

void at8pm(void);
void at2pm(void);

void setup ()

{


Serial.begin(9600);
pinMode(button1,INPUT);
pinMode(button2,INPUT);
pinMode(button3,INPUT);
pinMode(button4,INPUT);
pinMode(buzz_led,OUTPUT);
digitalWrite(button1,HIGH);
digitalWrite(button2,HIGH);
digitalWrite(button3,HIGH);
lcd.begin(16,2);

//delay(2000);
if (! rtc.begin())

{

Serial.println("Couldn't find RTC Module");
while (1);

}

if (rtc.lostPower())

{

Serial.println("RTC lost power, lets set the time!");
rtc.adjust(DateTime(F(_DATE), F(TIME_)));

}

rtc.adjust(DateTime(F(_DATE), F(TIME_)));

}

void loop ()


{

now = rtc.now();
firstScreen();
showDate();
showDay();
showTime();
secondScreen();

buttonVal1=digitalRead(button1);
buttonVal2=digitalRead(button2);
buttonVal3=digitalRead(button3);
buttonVal4=digitalRead(button4);
if(buttonVal1==0)

{

lcd.clear();
lcd.setCursor(0,0);
lcd.print("mode  1");
lcd.setCursor(0,1);
lcd.print("once a day");
delay(1000);

at8am();

}

if(buttonVal2==0)

{

lcd.clear();
lcd.setCursor(0,0);
lcd.print("mode                               2");


lcd.setCursor(0,1);
lcd.print("twice a day");
at8am();

at8pm();

}

if(buttonVal3==0)

{

lcd.clear();
lcd.setCursor(0,0);
lcd.print("mode  3");
lcd.setCursor(0,1);
lcd.print("thrice a day");
at8am();

at2pm();

at8pm();

}

}

void firstScreen()

{

lcd.clear();
lcd.setCursor(0,0);


lcd.print("  welcome to  ");
lcd.setCursor(0,1);
lcd.print("pill reminder");
delay(2000);

}

void secondScreen()

{

lcd.clear();
lcd.setCursor(0,0);

lcd.print("enter mode 1,2,3");
lcd.setCursor(0,1);
lcd.print("for reminder");
delay(2000);

}

void showDate()

{

lcd.clear();
lcd.setCursor(0,0);
lcd.print(now.day());

lcd.print('/');

lcd.print(now.month());

lcd.print('/');

lcd.print(now.year());


}

void showDay()

{

lcd.setCursor(11,0);
lcd.print(daysOfTheWeek[now.dayOfTheWeek()]);

}

void showTime()

{

lcd.setCursor(0,1);
lcd.print("Time:");

lcd.print(now.hour());

lcd.print(':');
lcd.print(now.minute());
lcd.print(':');
lcd.print(now.second());
lcd.print("                 ");
delay(5000);

}

void startBuzz()

{

if(buttonVal4==1)

{

digitalWrite(buzz_led,HIGH);
delay(5000);

}


if(buttonVal4==0)

{

digitalWrite(buzz_led,LOW);
delay(5000);

}

}

void at8am() {                      // function to start buzzing at 8am
DateTime now = rtc.now();

if (int(now.hour()) >= buzz8amHH) {

if (int(now.minute()) >= buzz8amMM) {
if (int(now.second())== buzz8amSS) {

/////////////////////////////////////////////////////

startBuzz();

/////////////////////////////////////////////////////

}

}

}

}

void at2pm() {                          // function to start buzzing at 2pm
DateTime now = rtc.now();

if (int(now.hour()) >= buzz2pmHH) {

if (int(now.minute()) >= buzz2pmMM) {
if (int(now.second()) > buzz2pmSS) {


///////////////////////////////////////////////////
startBuzz();

//////////////////////////////////////////////////

}

}

}

}

void at8pm() {                           // function to start buzzing at 8pm
DateTime now = rtc.now();

if (int(now.hour()) >= buzz8pmHH) {

if (int(now.minute()) >= buzz8pmMM) {
if (int(now.second()) > buzz8pmSS) {

/////////////////////////////////////////////////////
startBuzz();

/////////////////////////////////////////////////////

}

}

}

}
