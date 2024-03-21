// WiFiNINA_Generic - Version: Latest 
#include <WiFiNINA_Generic.h>
#include <DHT22.h>
#include "ThingSpeak.h"

#define pinDATA 2 // 

DHT22 dht22(pinDATA);

char ssid[] = SECRET_SSID;   // Network SSID 
char pass[] = SECRET_PASS;   // Network Password
WiFiClient  client;

unsigned long myChannelNumber = strtoul(SECRET_CH_ID, NULL, 10); // Convert string to unsigned long
const char * myWriteAPIKey = SECRET_WRITE_APIKEY;

String myStatus = "";

void setup() {
  
  Serial.begin(115200);      // Initialize serial 
  ThingSpeak.begin(client);  // Initialize ThingSpeak 
}

void loop() {

  float t = dht22.getTemperature();
  float h = dht22.getHumidity();

  // Connect or reconnect to WiFi
  if(WiFi.status() != WL_CONNECTED){
    Serial.print("Attempting to connect to SSID: ");
    Serial.println(SECRET_SSID);
    while(WiFi.status() != WL_CONNECTED){
      WiFi.begin(ssid, pass);  // Connect to WPA/WPA2 network. Change this line if using open or WEP network
      Serial.print(".");
      delay(5000);     
    } 
    Serial.println("\nConnected.");

  Serial.print("h=");Serial.print(h,1);Serial.print("\t");
  Serial.print("t=");Serial.println(t,1);
  delay(2000); //Collecting period should be : >1.7 second
  }

  // set the fields with the values
  ThingSpeak.setField(1, t);
  ThingSpeak.setField(2, h);


  // figure out the status message
  if(t > h){
    myStatus = String("field1 is greater than field2"); 
  }
  else if(t < h){
    myStatus = String("field1 is less than field2");
  }
  else{
    myStatus = String("field1 equals field2");
  }
  
  // set the status
  ThingSpeak.setStatus(myStatus);
  
  // write to the ThingSpeak channel 
  int x = ThingSpeak.writeFields(myChannelNumber, myWriteAPIKey);
  if(x == 200){
    Serial.println("Channel update successful.");
  }
  else{
    Serial.println("Problem updating channel. HTTP error code " + String(x));
  }
  

  delay(60000); // Wait 60 seconds to update the channel again
}
