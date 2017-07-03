

/*
 *  Emile Schons 
 *  GPL v.3.0 or higher
 *  Scan wifi and send email if Freifunk is found
 *  
 */



// Include Libraries: 
#include "ESP8266WiFi.h"
#include <ESPMailer.h>
//#include <lmic.h>  // library for Lorawan RFM92/95 chip see  https://github.com/matthijskooijman/arduino-lmic/


// Globale Variable: 
char str[50], strges[10000] = "http://find-wifi.mylnikov.org/#";



void setup() {
  Serial.begin(115200);
  
  // Set WiFi to station mode and disconnect from an AP if it was previously connected
  WiFi.mode(WIFI_STA);
  WiFi.disconnect();
  delay(100);
  pinMode(LED_BUILTIN, OUTPUT); // ledsetup 
  Serial.println("Setup done");
}

void loop() {

   // Email schon vorbereiten um Zeit zu sparen.
   ESPMailer* mail = new ESPMailer(); // Create a new mail
          mail->Host = "fill_smtpserveradress";
          mail->SMTPAuth = true; 
          mail->Username = "fill_username";
          mail->Password = "fill_password"; 
          mail->setFrom("fill_mailadress"); // Set sender, optional with name.
          mail->addAddress("fill_mailadress"); // Add a new recipient, could be addCC or addBCC too.
          mail->isHTML(false); //  Returns, if message is set to html, may set the type, if you call with boolean.
          mail->Subject = "ESP8266 The position of your bike";
          mail->setTimezone(2); // Difference from UTC in hours, xx.5 accepted for XX:30 time difference (e.g. Venezuela: -4.5 will be -0430). If not called, UTC is used in Timestamp.
          mail->setDebugLevel(3); // -1 will be quiet, 0 just print errors (default), 1 messages from Client to Server, 2 Server answers and 3 will output everything

  Serial.println("scan start");
  digitalWrite(LED_BUILTIN, HIGH);

  // WiFi.scanNetworks will return the number of networks found
  int n = WiFi.scanNetworks();
  Serial.println("scan done");
  if (n == 0)
    Serial.println("no networks found");
  else
  {
    Serial.print(n);
    Serial.println(" networks found");
    
    
    for (int i = 0; i < n; ++i)
    {
      //Data to store
      uint8_t *mac = WiFi.BSSID(i); 
      sprintf(str,"%02X:%02X:%02X:%02X:%02X:%02X,%d;",mac[0], mac[1], mac[2], mac[3], mac[4], mac[5], WiFi.RSSI(i));
      strcat(strges, str);

      // Print SSID, BSSID, RSSI for each network found 
      //Serial.printf("%02X:%02X:%02X:%02X:%02X:%02X,%d;",mac[0], mac[1], mac[2], mac[3], mac[4], mac[5], WiFi.RSSI(i) ); // print the MAC address
      //Serial.print(WiFi.SSID(i));
      
      
      if (WiFi.SSID(i) == "Freifunk")
      {
        
          Serial.println("Yeah FREIFUNK ist da!"); 
          
          WiFi.begin("Freifunk");
          
          Serial.print("Connecting");
          
          while (WiFi.status() != WL_CONNECTED) // achtung kann dies in einer endlosschleife enden? 
          {
            delay(1);
            Serial.print(".");
          }
          
          Serial.println();
          Serial.print("Connected, IP address: ");
          Serial.println(WiFi.localIP());

          // Send Mail with the found wifis passt and future ->  https://github.com/ArduinoHannover/ESPMailer    
          mail->Body = strges;
          mail->send(); //  Returns true if sent successfully.
          
          
      }    

    } 
//     // Maschine readable SSID,RSSI 
//     for (int i = 0; i < n; ++i)
//    {
//      uint8_t *mac = WiFi.BSSID(i); 
//      Serial.printf("%02X:%02X:%02X:%02X:%02X:%02X,%d;",mac[0], mac[1], mac[2], mac[3], mac[4], mac[5], WiFi.RSSI(i) ); // print the MAC address
//      delay(10);
//    }

    
  }
  Serial.println("");


  // Go to sleep 2 minutes max would be 4294967295us = 71min 
//  Serial.println("Sleep.... 2");
//  delay(100);
//  Serial.println("Sleep.... 1");
//  delay(100);
  digitalWrite(LED_BUILTIN, LOW);
//  ESP.deepSleep(10000000); // 20000000 = 20 sec //Hardware: Connect RST -> D0
// delay(500);
//  Serial.println("Woked up....");

  
  
  
}
