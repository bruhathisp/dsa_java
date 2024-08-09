Let's integrate the radar functionality on X1 and ensure that both X1 and X2 have LED indicators that blink when the measured distance is below a specified threshold. Both X1 and X2 will report their battery voltage and the radar reading (in X1's case) to the TTGO module via BLE.

### X1 Code (BLE Server with Radar and Battery)

X1 will have a radar sensor and a battery level monitor. If the radar detects an object closer than the threshold, it will blink an LED and send the distance and battery information to the TTGO module.

```cpp
#include <BLEDevice.h>
#include <BLEUtils.h>
#include <BLEServer.h>
#include "esp_adc_cal.h"

#define SERVICE_UUID        "4fafc201-1fb5-459e-8fcc-c5c9c331914b"
#define CHARACTERISTIC_UUID "beb5483e-36e1-4688-b7f5-ea07361b26a8"
#define RADAR_PIN           34
#define LED_PIN             2
#define BATTERY_PIN         35
#define DISTANCE_THRESHOLD  2000  // Threshold value for blinking LED

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

int getSimulatedDistance() {
  return random(1000, 5000);  // Simulating a random distance value
}

void setup() {
  Serial.begin(115200);
  Serial.println("Starting BLE work!");

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
  int distance = getSimulatedDistance();
  float batteryVoltage = getBatteryVoltage();

  // Check if distance is below the threshold
  if (distance < DISTANCE_THRESHOLD) {
    digitalWrite(LED_PIN, HIGH);  // Turn on LED
  } else {
    digitalWrite(LED_PIN, LOW);   // Turn off LED
  }

  // Prepare the data to send over BLE
  char data[50];
  snprintf(data, sizeof(data), "Distance:%d;Battery:%.2f", distance, batteryVoltage);
  pCharacteristic->setValue(data);
  pCharacteristic->notify();

  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.print(" mm, Battery Voltage: ");
  Serial.println(batteryVoltage);

  delay(5000); // Send updates every 5 seconds
}
```

### X2 Code (BLE Server with Battery Monitoring and LED Indicator)

X2 will monitor its battery level and blink an LED if the received radar distance from X1 is below a specified threshold. It will also send its battery information to the TTGO module via BLE.

```cpp
#include <BLEDevice.h>
#include <BLEUtils.h>
#include <BLEServer.h>
#include "esp_adc_cal.h"

#define SERVICE_UUID        "4fafc201-1fb5-459e-8fcc-c5c9c331914b"
#define CHARACTERISTIC_UUID "beb5483e-36e1-4688-b7f5-ea07361b26a8"
#define LED_PIN             2
#define BATTERY_PIN         35
#define DISTANCE_THRESHOLD  2000  // Threshold value for blinking LED

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
  float batteryVoltage = getBatteryVoltage();

  // Prepare the data to send over BLE
  char data[30];
  snprintf(data, sizeof(data), "Battery:%.2f", batteryVoltage);
  pCharacteristic->setValue(data);
  pCharacteristic->notify();

  Serial.print("Battery Voltage: ");
  Serial.println(batteryVoltage);

  delay(5000); // Send updates every 5 seconds
}
```
Below is the TTGO T-Display code for connecting to both X1 and X2 via BLE, receiving battery voltage and radar readings, and then sending the data to Blynk via Wi-Fi.

### TTGO T-Display Code (BLE Client and Wi-Fi to Blynk)

```cpp
#include <BLEDevice.h>
#include <BLEUtils.h>
#include <BLEClient.h>
#include <WiFi.h>
#include <BlynkSimpleEsp32.h>

#define SERVICE_UUID        "4fafc201-1fb5-459e-8fcc-c5c9c331914b"
#define CHARACTERISTIC_UUID "beb5483e-36e1-4688-b7f5-ea07361b26a8"

char auth[] = "Your_Blynk_Auth_Token";
char ssid[] = "Your_SSID";
char pass[] = "Your_PASSWORD";

BLEClient* pClient;
BLERemoteCharacteristic* pRemoteCharacteristicX1;
BLERemoteCharacteristic* pRemoteCharacteristicX2;
boolean doConnectX1 = false;
boolean doConnectX2 = false;
boolean connectedX1 = false;
boolean connectedX2 = false;

void setup() {
  Serial.begin(115200);
  WiFi.begin(ssid, pass);
  Blynk.begin(auth, ssid, pass);

  BLEDevice::init("TTGO_T_Display");

  BLEScan* pBLEScan = BLEDevice::getScan();
  pBLEScan->setAdvertisedDeviceCallbacks(new MyAdvertisedDeviceCallbacks());
  pBLEScan->setInterval(1349);
  pBLEScan->setWindow(449);
  pBLEScan->setActiveScan(true);
  pBLEScan->start(30, false);
}

void loop() {
  if (doConnectX1) {
    if (connectToServer(X1_Address, pRemoteCharacteristicX1)) {
      connectedX1 = true;
    }
    doConnectX1 = false;
  }

  if (doConnectX2) {
    if (connectToServer(X2_Address, pRemoteCharacteristicX2)) {
      connectedX2 = true;
    }
    doConnectX2 = false;
  }

  if (connectedX1) {
    std::string value = pRemoteCharacteristicX1->readValue();
    Serial.print("X1 Data: ");
    Serial.println(value.c_str());
    Blynk.virtualWrite(V1, value.c_str()); // Send X1 data to Blynk
  }

  if (connectedX2) {
    std::string value = pRemoteCharacteristicX2->readValue();
    Serial.print("X2 Data: ");
    Serial.println(value.c_str());
    Blynk.virtualWrite(V2, value.c_str()); // Send X2 data to Blynk
  }

  Blynk.run();
  delay(5000);
}

class MyAdvertisedDeviceCallbacks: public BLEAdvertisedDeviceCallbacks {
  void onResult(BLEAdvertisedDevice advertisedDevice) {
    if (advertisedDevice.haveServiceUUID() && advertisedDevice.getServiceUUID().equals(BLEUUID(SERVICE_UUID))) {
      if (advertisedDevice.getAddress().equals(BLEAddress(X1_Address))) {
        BLEDevice::getScan()->stop();
        doConnectX1 = true;
      }
      if (advertisedDevice.getAddress().equals(BLEAddress(X2_Address))) {
        BLEDevice::getScan()->stop();
        doConnectX2 = true;
      }
    }
  }
};

bool connectToServer(BLEAddress address, BLERemoteCharacteristic*& pRemoteCharacteristic) {
  pClient = BLEDevice::createClient();
  pClient->connect(address);

  BLERemoteService* pRemoteService = pClient->getService(BLEUUID(SERVICE_UUID));
  if (pRemoteService == nullptr) {
    Serial.print("Failed to find our service UUID: ");
    Serial.println(SERVICE_UUID);
    pClient->disconnect();
    return false;
  }

  pRemoteCharacteristic = pRemoteService->getCharacteristic(BLEUUID(CHARACTERISTIC_UUID));
  if (pRemoteCharacteristic == nullptr) {
    Serial.print("Failed to find our characteristic UUID: ");
    Serial.println(CHARACTERISTIC_UUID);
    pClient->disconnect();
    return false;
  }

  return true;
}
```

### Code for ESP32 to Substitute TTGO

If you want to run the same code on an ESP32 instead of the TTGO T-Display, the code remains the same, except you might want to change the Wi-Fi connection credentials and the Blynk virtual pins to suit your setup.

```cpp
#include <BLEDevice.h>
#include <BLEUtils.h>
#include <BLEClient.h>
#include <WiFi.h>
#include <BlynkSimpleEsp32.h>

// Same as above, but you may want to adjust Wi-Fi credentials or Blynk pins.

void setup() {
  Serial.begin(115200);
  WiFi.begin(ssid, pass);
  Blynk.begin(auth, ssid, pass);

  // Initialize BLE, scan, and connect to X1 and X2 as above
}

void loop() {
  // Same loop code as above
}
```

### Key Points to Implement:
- Replace `X1_Address` and `X2_Address` with the actual BLE addresses of X1 and X2.
- Ensure the correct Blynk virtual pins are configured for V1 and V2.
- Adjust the Blynk auth token, SSID, and password accordingly.

This code will allow the TTGO T-Display or an ESP32 to form a BLE mesh with X1 and X2, gather data, and send it to Blynk via Wi-Fi.
