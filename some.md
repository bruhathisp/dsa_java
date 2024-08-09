Here is the implementation that combines all your requirements: 

- **X1**: Equipped with an ultrasonic sensor (HC-SR04) to measure distance. If the distance is less than a threshold, it sends a signal to X2 to blink its LED. Both X1 and X2 will report battery voltage and sensor readings to the TTGO module.
- **X2**: Receives commands from X1 and blinks its LED if commanded. Reports its battery voltage to the TTGO module.
- **TTGO**: Collects data from X1 and X2 and forwards it to Blynk.

### X1 Code (BLE Server with Ultrasonic Sensor)

```cpp
#include <BLEDevice.h>
#include <BLEUtils.h>
#include <BLEServer.h>
#include "esp_adc_cal.h"

#define TRIG_PIN 5
#define ECHO_PIN 18
#define LED_PIN 2
#define BATTERY_PIN 35

#define SERVICE_UUID "4fafc201-1fb5-459e-8fcc-c5c9c331914b"
#define CHARACTERISTIC_UUID "beb5483e-36e1-4688-b7f5-ea07361b26a8"

BLEServer *pServer = NULL;
BLECharacteristic *pCharacteristic = NULL;

class MyServerCallbacks: public BLEServerCallbacks {
    void onConnect(BLEServer* pServer) {
      Serial.println("Client connected");
    };

    void onDisconnect(BLEServer* pServer) {
      Serial.println("Client disconnected");
    }
};

void initADC() {
  analogReadResolution(12);
  analogSetAttenuation(ADC_11db);
}

float getBatteryVoltage() {
  int raw = analogRead(BATTERY_PIN);
  return ((float)raw / 4096.0) * 7.4; // Adjust the multiplier based on your voltage divider
}

float readDistance() {
  digitalWrite(TRIG_PIN, LOW);
  delayMicroseconds(2);
  digitalWrite(TRIG_PIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);

  long duration = pulseIn(ECHO_PIN, HIGH);
  float distance = duration * 0.034 / 2;
  return distance;
}

void setup() {
  Serial.begin(115200);
  Serial.println("Starting BLE work!");

  pinMode(TRIG_PIN, OUTPUT);
  pinMode(ECHO_PIN, INPUT);
  pinMode(LED_PIN, OUTPUT);

  BLEDevice::init("X1");
  pServer = BLEDevice::createServer();
  pServer->setCallbacks(new MyServerCallbacks());

  BLEService *pService = pServer->createService(SERVICE_UUID);
  pCharacteristic = pService->createCharacteristic(
                      CHARACTERISTIC_UUID,
                      BLECharacteristic::PROPERTY_READ |
                      BLECharacteristic::PROPERTY_NOTIFY
                    );

  pService->start();
  BLEAdvertising *pAdvertising = pServer->getAdvertising();
  pAdvertising->addServiceUUID(SERVICE_UUID);
  pAdvertising->start();
  Serial.println("BLE setup done.");

  initADC();
}

void loop() {
  float distance = readDistance();
  float batteryVoltage = getBatteryVoltage();

  char dataString[32];
  sprintf(dataString, "D:%.2f,V:%.2f", distance, batteryVoltage);
  pCharacteristic->setValue(dataString);
  pCharacteristic->notify();

  if (distance < 20) { // Threshold distance is 20 cm
    digitalWrite(LED_PIN, HIGH);
    pCharacteristic->setValue("BLINK");
  } else {
    digitalWrite(LED_PIN, LOW);
  }

  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.print(" cm, Battery Voltage: ");
  Serial.println(batteryVoltage);
  delay(1000);
}
```

### X2 Code (BLE Server with LED Control)

```cpp
#include <BLEDevice.h>
#include <BLEUtils.h>
#include <BLEServer.h>
#include "esp_adc_cal.h"

#define LED_PIN 2
#define BATTERY_PIN 35

#define SERVICE_UUID "4fafc201-1fb5-459e-8fcc-c5c9c331914b"
#define CHARACTERISTIC_UUID "beb5483e-36e1-4688-b7f5-ea07361b26a8"

BLEServer *pServer = NULL;
BLECharacteristic *pCharacteristic = NULL;

class MyServerCallbacks: public BLEServerCallbacks {
    void onConnect(BLEServer* pServer) {
      Serial.println("Client connected");
    };

    void onDisconnect(BLEServer* pServer) {
      Serial.println("Client disconnected");
    }
};

void initADC() {
  analogReadResolution(12);
  analogSetAttenuation(ADC_11db);
}

float getBatteryVoltage() {
  int raw = analogRead(BATTERY_PIN);
  return ((float)raw / 4096.0) * 7.4; // Adjust the multiplier based on your voltage divider
}

void setup() {
  Serial.begin(115200);
  Serial.println("Starting BLE work!");

  pinMode(LED_PIN, OUTPUT);

  BLEDevice::init("X2");
  pServer = BLEDevice::createServer();
  pServer->setCallbacks(new MyServerCallbacks());

  BLEService *pService = pServer->createService(SERVICE_UUID);
  pCharacteristic = pService->createCharacteristic(
                      CHARACTERISTIC_UUID,
                      BLECharacteristic::PROPERTY_READ |
                      BLECharacteristic::PROPERTY_NOTIFY |
                      BLECharacteristic::PROPERTY_WRITE
                    );

  pService->start();
  BLEAdvertising *pAdvertising = pServer->getAdvertising();
  pAdvertising->addServiceUUID(SERVICE_UUID);
  pAdvertising->start();
  Serial.println("BLE setup done.");

  initADC();
}

void loop() {
  float batteryVoltage = getBatteryVoltage();
  char voltageString[8];
  dtostrf(batteryVoltage, 4, 2, voltageString);
  pCharacteristic->setValue(voltageString);
  pCharacteristic->notify();

  String receivedValue = pCharacteristic->getValue().c_str();
  if (receivedValue == "BLINK") {
    digitalWrite(LED_PIN, HIGH);
  } else {
    digitalWrite(LED_PIN, LOW);
  }

  Serial.print("Battery Voltage: ");
  Serial.println(voltageString);

  delay(5000); // Send updates every 5 seconds
}
```

### TTGO T-Display Code (BLE Client and Wi-Fi to Blynk)

```cpp
#include <BLEDevice.h>
#include <BLEUtils.h>
#include <BLEClient.h>
#include <WiFi.h>
#include <BlynkSimpleEsp32.h>

#define SERVICE_UUID "4fafc201-1fb5-459e-8fcc-c5c9c331914b"
#define CHARACTERISTIC_UUID "beb5483e-36e1-4688-b7f5-ea07361b26a8"

char auth[] = "Your_Blynk_Auth_Token";
char ssid[] = "Your_SSID";
char pass[] = "Your_PASSWORD";

BLEClient* pClientX1;
BLEClient* pClientX2;
BLERemoteCharacteristic* pRemoteCharacteristicX1;
BLERemoteCharacteristic* pRemoteCharacteristicX2;

void setup() {
  Serial.begin(115200);
  WiFi.begin(ssid, pass);
  Blynk.begin(auth, ssid, pass);

  BLEDevice::init("TTGO_T_Display");

  connectToServer("X1_Address", pClientX1, pRemoteCharacteristicX1);
  connectToServer("X2_Address", pClientX2, pRemoteCharacteristicX2);
}

void loop() {
  if (pClientX1->isConnected() && pClientX2->isConnected()) {
    std::string dataX1 = pRemoteCharacteristicX1->readValue();
    std::string dataX2 = pRemoteCharacteristicX2->readValue();

    Serial.print("X1 Data: ");
    Serial.println(dataX1.c_str());
    Serial.print("X2 Data: ");
    Serial.println(dataX2.c_str());

    Blynk.virtualWrite(V1, dataX1.c_str()); // X1 data to Blynk
    Blynk.virtualWrite(V2, dataX2.c_str()); // X2 data to Blynk
  }

  Blynk.run();
  delay(5000); // Adjust delay as necessary
}

void connectToServer(const char* address, BLEClient*& pClient, BLERemoteCharacteristic*& pRemoteCharacteristic) {
  pClient = BLEDevice::createClient();
  pClient->connect(BLEAddress(address));

  BLERemoteService* pRemoteService = pClient->getService(BLEUUID(SERVICE_UUID));
  if (pRemoteService == nullptr) {
    Serial.print("Failed to find service UUID: ");
    Serial.println(SERVICE_UUID);
    return;
  }

  pRemoteCharacteristic = pRemoteService->getCharacteristic(BLEUUID(CHARACTERISTIC_UUID));
  if (pRemoteCharacteristic == nullptr) {
    Serial.print("Failed to find characteristic UUID: ");
    Serial.println(CHARACTERISTIC_UUID);
    return;
  }
}
```

### Summary

- **X1**: Uses the HC-SR04 ultrasonic sensor to measure distance and reports the distance and battery voltage to TTGO. If the distance is less than a threshold, it triggers X2 to blink its LED.
- **X2**: Receives commands from X1 to blink its LED and reports its battery voltage to TTGO.
- **TTGO**: Acts as the central node, collecting data from X1 and X2 and sending it to Blynk via Wi-Fi.

### Notes
- Replace `"X1_Address"` and `"X2_Address"`
