# Food-Quality-Monitoring
Circuit Description

Components Used
1.	Arduino Board.
2.	Gas sensors
3.	DHT11-Temperature Sensor
4.	IR sensor
5.	ESP-Wifi Module
6.	LCD-Display
7.	A box- Acts as a food storage facility.

Working
1.	Plug the circuit to an external power supply. This kickstarts the circuit.
2.	Once the power is supplied to the circuit, the user will get a prompt on the LCD displaying “Reading the Data”.
3.	The user can place the food items in the container and close it.
4.	The food quality parameters, Temperature, Humidity, Air Quality, and Menthane amounts, are displayed on the LCD. These values are further transmitted to the mobile phone using a Wi-Fi module and the open-source tool JuiceSSH application.
5.	After a certain iteration of reading the parameters, the Arduino board displays whether the food quality is “Good” or “Bad.”


Software to transmit the data to mobile devices
To transmit the data to mobile devices we will be using the ESP wifi module and open-source tools Network Analyzer and JuiceSSH app.
1.	Connect the ESP module to the mobile phone using personal hotspot.
2.	Now open the Network Analyzer app and identify the IP address ESP by using LAN scan functionality.
3.	Once we have identified the IP of the ESP module, open the JuiceSSH app.
4.	In the JuiceSSH app, click on manage connection- add a new connection. Here paste the IP address and choose the mode of communication as Telnet and port as 8080.
5.	Once the connection has been established the reading of the temperature, humidity, air quality, and methane will be displayed on the mobile screen.



Pin Description:
1.	The ESP Wifi Module is connected to the pins 8 and 9.
2.	The Humidity Sensor is connected to the pin 7.
3.	The Gas sensors are connected to the pin 6.

Predefined Parameters
Temperature:
•	We are reading the temperature using the function DHT.read11(DHT11_PIN) from pin 7.

if(temp > 40) {
    digitalWrite(13,1);
    wdata = String("***ALERT*** High Temperature\r\n");
    mySerial.println(wdata);
    delay(700);
}
When the temperature of the storage facility exceeds 40 degrees Celsius the text “High Temperature” is displayed on the LCD.
Humidity:
•	The humidity is measured in percentage using the function DHT.read11(DHT11_PIN) from pin 7.

if(hmd > 92) {
    digitalWrite(13,1);
    wdata = String("***ALERT*** High Humidity\r\n");
    mySerial.println(wdata);
    delay(700);
}
When the humidity of the storage facility exceeds 92% the text “High Humidity” is displayed on the LCD.

Methane:
•	We are digitally reading the methane value using the digital read function from digitalRead().
if(digitalRead(mth) == 0)
{           
    digitalWrite(13, 1);        
    wdata = String("Status : ABNORMAL\r\n");
    mySerial.println(wdata);
    delay(700);  
}



Code
#include <dht.h>
#include <SoftwareSerial.h>
#define DHT11_PIN 7
#define mth 6
SoftwareSerial mySerial(9,8); //  RX, TX
dht DHT;
void setup()
{
  Serial.begin(9600);
  mySerial.begin(115200);
  pinMode(13,OUTPUT);
  pinMode(mth,INPUT);
  digitalWrite(13,1);
  delay(700);
  digitalWrite(13,0);
  delay(700);
  digitalWrite(13,1);
  delay(700);
  digitalWrite(13,0);
  delay(700);
}

void loop()
{
  while(1)
  {
      digitalWrite(13,0);
      int chk = DHT.read11(DHT11_PIN);

      int temp = DHT.temperature;
      int hmd = DHT.humidity;

          stemp = String(temp);
          
          shmd = String(hmd);
      
    
      wdata =  String("Temperature:"+stemp+" DegC,  Humidity:"+shmd+" Percentage \r\n"); // concatenating two strings
      Serial.println(wdata);  
      mySerial.println(wdata);   
  
    if(digitalRead(mth) == 0)
    {           
        digitalWrite(13,1);        
        wdata =  String("Status : ABNORMAL\r\n"); // concatenating two strings
                 
        mySerial.println(wdata);             
        delay(700);  
    }
      
    if(temp > 40)
    {           
        digitalWrite(13,1);        
        wdata =  String("***ALERT*** High Temperature\r\n"); // concatenating two strings
                 
        mySerial.println(wdata);             
        delay(700);  
    }
    
    if(hmd > 92)
    {           
        digitalWrite(13,1);        
        wdata =  String("***ALERT*** High Humidity\r\n"); // concatenating two strings
        
        mySerial.println(wdata);             
        delay(700);  
    }
        
      delay(1600);        
    }
}

