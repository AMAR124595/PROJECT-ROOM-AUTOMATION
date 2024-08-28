 # PROJECT-ROOM-AUTOMATION
This project involves creating a room automation system that uses an LCD display and keypad to manage lighting and fans in various rooms. The system allows users to control the on/off status of lights and fans for each room through a simple keypad interface
# AIM
To interface a keypad and I2C LCD with an Arduino to develop a user-friendly room automation system. This system will efficiently control lighting and fans across multiple rooms and collect data on user interactions
# Components
1. I2C LCD
2. 4x4 matrix keypad
# Connetion
![Screenshot (8)](https://github.com/user-attachments/assets/8b99e517-7f01-48a0-8d1f-e149a8bc8eb5)
* PIN 6 ----> R1
* PIN 7 ----> R2
* PIN 8 ----> R3
* PIN 9 ----> R4
* PIN 10 ----> C1
* PIN 11 ----> C2
* PIN 12 ----> C3
* PIN 13 ----> C4
* PIN A4 ----> SDA
* PIN A5 ----> SCL
# Procedure
1. Open Arduino IDE Create New Project
2. Interface  4x4 matrix keypad  With Arduino And Print numaric data on the serial Moniter
    * Library ---> https://github.com/Chris--A/Keypad.git
3. Interface Arduino With I2C 16x2 LCD display and display the value on the LCD.
    * Library ---> https://github.com/fdebrabander/Arduino-LiquidCrystal-I2C-library.git
4. Interface Both I2C LCD and 4x4 matrix keypad with Arduino , Display to the  I2C LCD
# Code 
```
#include <Keypad.h>
#include <LiquidCrystal_I2C.h>

LiquidCrystal_I2C lcd(0x27, 16, 2);
/***********************************************************************************/
const byte ROWS = 4; //four rows
const byte COLS = 4; //four columns

char keys[ROWS][COLS] = 
{
  {'1','2','3','A'},
  {'4','5','6','B'},
  {'7','8','9','C'},
  {'*','0','#','D'}
};

byte rowPins[ROWS] = {6, 7, 8, 9}; //connect to the row pinouts of the keypad
byte colPins[COLS] = {10, 11, 12, 13}; //connect to the column pinouts of the keypad
Keypad keypad = Keypad( makeKeymap(keys), rowPins, colPins, ROWS, COLS );
/***********************************************************************************/
int password = 1234;
int Column = 0;
int s=0,r=0,c=0;
/***********************************************************************************/
void room_menu();
void rooms_menu(int);
void roomc_menu(int);
/***********************************************************************************/
void setup() 
{
    lcd.backlight();
    lcd.begin();
    lcd.clear();
    lcd.setCursor(1,0);
    lcd.print("ENTER PASSWORD");    
    Serial.begin(9600);
    lcd.setCursor(0, 1);
    pinMode(13,OUTPUT);

}
/***********************************************************************************/
void loop() 
{
   char key = keypad.getKey();
   if (key)
    {
      Serial.print(key);
        if(key<='9' && key>='0')
        {
          lcd.print(key);
          delay(300);
          lcd.setCursor(c, 1);
          lcd.print("*");
          s=key;
          s=s-48;
          r=r*10+s;
          c++;
        }
        if(key=='#')
        {
                if(password==r)
                { 
                  lcd.clear();
                  lcd.print("ACCESS GRANTED");
                  delay(500);
                  room_menu();
                  lcd.clear();
                  lcd.setCursor(1,0);
                  lcd.print("ENTER PASSWORD");  
                  lcd.setCursor(0, 1);
                  r=0;c=0;
                }
                else
                {
                  lcd.clear();
                  lcd.print("ACCESS DENIED"); 
                  delay(2000);
                  lcd.clear();
                  lcd.setCursor(1,0);
                  lcd.print("ENTER PASSWORD");  
                  lcd.setCursor(0, 1);
                  r=0;c=0;
                }
        }
        
    }
}
/***********************************************************************************/
void room_menu()
{
  lcd.clear();
  lcd.print("  Room Menu");  
  lcd.setCursor(0, 1);
  lcd.print("1)Room1 2)Room2");  
  while(1)
  {
    char key = keypad.getKey();
    if (key == '1')
      {
          rooms_menu(1);
          lcd.clear();
          lcd.print("  Room Menu");  
          lcd.setCursor(0, 1);
          lcd.print("1)Room1 2)Room2"); 
      }
    else if (key == '2')
      {
        rooms_menu(2);
        lcd.clear();
        lcd.print("  Room Menu");  
        lcd.setCursor(0, 1);
        lcd.print("1)Room1 2)Room2"); 
      }
    else if (key == 'C')
      {
        return;
      }
  }
}
/***********************************************************************************/
void rooms_menu(int n)
{
  if(n==1)
  {
      lcd.clear();
      lcd.print("  Room-1 Menu");  
      lcd.setCursor(0, 1);
      lcd.print("1)Light1 2)Fan1");  
      while(1)
      {
        char key = keypad.getKey();
        if (key == '1')
          {
            roomc_menu(1);
            lcd.clear();
            lcd.print("  Room-1 Menu");  
            lcd.setCursor(0, 1);
            lcd.print("1)Light1 2)Fan1");
          }
        else if (key == '2')
          {
            roomc_menu(2);
            lcd.clear();
            lcd.print("  Room-1 Menu");  
            lcd.setCursor(0, 1);
            lcd.print("1)Light1 2)Fan1");
            
          }
        else if (key == 'C')
          {
            return;
          }
      }
  }
  else if(n==2)
  {
      lcd.clear();
      lcd.print("  Room-2 Menu");  
      lcd.setCursor(0, 1);
      lcd.print("1)Light2 2)Fan2");  
      while(1)
      {
        char key = keypad.getKey();
        if (key == '1')
          {
            roomc_menu(1);
            lcd.clear();
            lcd.print("  Room-2 Menu");  
            lcd.setCursor(0, 1);
            lcd.print("1)Light2 2)Fan2");
          }
        else if (key == '2')
          {
            roomc_menu(2);
            lcd.clear();
            lcd.print("  Room-2 Menu");  
            lcd.setCursor(0, 1);
            lcd.print("1)Light2 2)Fan2");
          }
        else if (key == 'C')
          {
            return;
          }
      }
  }
  
}
/***********************************************************************************/
void roomc_menu(int x)
{
  if(x==1)
  {
    lcd.clear();
    lcd.print("  Light Menu");  
    lcd.setCursor(0, 1);
    lcd.print("1)ON 2)OFF");  
    while(1)
    {
      char key = keypad.getKey();
        if (key == '1')
          {
           digitalWrite(13,HIGH);
          }
        else if (key == '2')
          {
           digitalWrite(13,LOW); 
          }
        else if (key == 'C')
          {
            digitalWrite(13,LOW);
            return;
          }
    }
  }
  if(x==2)
  {
    lcd.clear();
    lcd.print("  FAN Menu");  
    lcd.setCursor(0, 1);
    lcd.print("1)ON 2)OFF");  
    while(1)
    {
      char key = keypad.getKey();
        if (key == '1')
          {
           digitalWrite(13,HIGH);
          }
        else if (key == '2')
          {
           digitalWrite(13,LOW); 
          }
        else if (key == 'C')
          {
            digitalWrite(13,LOW);
            return;
          }
    }
  }
      
}
```
# Output
![1](https://github.com/user-attachments/assets/60b2baa3-96c4-43ee-b2b7-61147e557169)
![Screenshot_20240828_143452_Gallery](https://github.com/user-attachments/assets/c0d266f1-9b4c-4952-8504-068619de0e1e)
# Reference
## librarys & Code preference
https://lastminuteengineers.com/i2c-lcd-arduino-tutorial/
