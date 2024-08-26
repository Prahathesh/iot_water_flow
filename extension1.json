#include <WiFi.h>
#include <HTTPClient.h>

// Wi-Fi credentials
const char* ssid = "your_SSID";
const char* password = "your_PASSWORD";

// HTTP server settings
const char* serverName = "http://your-server.com/post-data";  // Replace with your server URL

// Simulated flow sensor (button) settings
const int flowSensorPin = 15;
volatile int pulseCount = 0;
float flowRate = 0.0;
unsigned long previousMillis = 0;
const long interval = 10000;  // Interval to send data (10 seconds)

// Interrupt service routine for the button (simulating flow sensor)
void IRAM_ATTR pulseCounter() {
  pulseCount++;
}

void setup() {
  Serial.begin(115200);

  // Initialize the button pin
  pinMode(flowSensorPin, INPUT_PULLUP);
  attachInterrupt(digitalPinToInterrupt(flowSensorPin), pulseCounter, FALLING);

  // Connect to Wi-Fi
  connectToWiFi();
}

void loop() {
  unsigned long currentMillis = millis();

  if (currentMillis - previousMillis >= interval) {
    previousMillis = currentMillis;

    // Calculate flow rate
    calculateFlowRate();

    // Send data to the server
    sendToServer(flowRate);

    // Reset pulse count
    pulseCount = 0;
  }
}

void calculateFlowRate() {
  // Simulated flow rate calculation based on pulse count
  flowRate = (pulseCount / 7.5);  // Example calculation: pulses per minute to liters per minute
  Serial.print("Flow Rate: ");
  Serial.print(flowRate);
  Serial.println(" L/min");
}

void sendToServer(float flowRate) {
  if (WiFi.status() == WL_CONNECTED) {
    HTTPClient http;

    // Construct the URL with the flow rate data
    String serverPath = serverName + "?flowRate=" + String(flowRate);

    // Send HTTP GET request
    http.begin(serverPath);
    int httpResponseCode = http.GET();

    if (httpResponseCode > 0) {
      String response = http.getString();
      Serial.println(httpResponseCode);
      Serial.println(response);
    } else {
      Serial.print("Error on sending GET: ");
      Serial.println(httpResponseCode);
    }

    // Close connection
    http.end();
  } else {
    Serial.println("Error in WiFi connection");
  }
}

void connectToWiFi() {
  Serial.print("Connecting to Wi-Fi...");
  WiFi.begin(ssid, password);

  while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    Serial.print(".");
  }

  Serial.println("");
  Serial.println("Connected to Wi-Fi");
  Serial.print("IP Address: ");
  Serial.println(WiFi.localIP());
}
