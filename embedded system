#include <WiFiNINA.h>
#include <ArduinoHttpClient.h>
// WiFi credentials (Consider using secure storage in a real application)
char ssid[] = "Your_SSID";  // Your network SSID (name)
char pass[] = "Your_Password";  // Your network password
// Sensor and threshold settings
const int lightSensorPin = A4;  // Analog pin for the light sensor
int threshold = 500;  // Set the sunlight threshold level
// IFTTT Webhook details
char serverAddress[] = "maker.ifttt.com";  // IFTTT Webhook Server
String eventName = "Light_trigger";  // Your IFTTT event name
// Secure your IFTTT key
const String IFTTT_Key = "Your_IFTTT_Key";  // Your IFTTT key
// WiFi and HTTP client objects
WiFiClient wifi;
HttpClient client = HttpClient(wifi, serverAddress, 80);
void setup() {
  Serial.begin(9600);  // Start the serial communication
  connectToWiFi();  // Connect to the WiFi network
}
void loop() {
  int lightLevel = analogRead(lightSensorPin);  // Read the light sensor value
  Serial.print("Light Level: ");
  Serial.println(lightLevel);  // Print the light level to the serial monitor
  if (lightLevel > threshold) {  // If light level exceeds the threshold
    triggerIFTTTEvent();  // Trigger the IFTTT event
    delay(10000);  // Wait 10 seconds before checking again
  } else {  // If light level is below the threshold
    Serial.println("Light level is below threshold, no action taken.");
  }
  delay(1000);  // Delay between readings
}
void connectToWiFi() {
  Serial.println("Connecting to WiFi...");
  while (WiFi.begin(ssid, pass) != WL_CONNECTED) {
    Serial.print(".");
    delay(1000);  // Wait a second before trying again
  }
  Serial.println("\nConnected to WiFi");
  Serial.print("IP Address: ");
  Serial.println(WiFi.localIP());  // Print the IP address
}
void triggerIFTTTEvent() {
  String url = "/trigger/" + eventName + "/with/key/" + IFTTT_Key;
  Serial.println("Sending request to IFTTT...");
  client.get(url);  // Send the HTTP GET request
  int statusCode = client.responseStatusCode();  // Get the status code of the response
  String response = client.responseBody();  // Get the body of the response
  Serial.print("Status code: ");
  Serial.println(statusCode);  // Print the status code
  if (statusCode == 200) {
    Serial.println("Event triggered successfully!");
  } else {
    Serial.println("Failed to trigger event.");
  }
  Serial.print("Response: ");
  Serial.println(response);  // Print the response body
} 
