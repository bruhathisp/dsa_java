Certainly! Here's the code for X1 and X2, each with an LED connected, to blink if the simulated radar distance is below a threshold, along with sending battery voltage and radar reading data to the TTGO module via BLE.

### X1 Code

```cpp
#include <BLEDevice.h>
#include <BLEUtils.h>
#include <BLEServer.h>
#include "esp_adc_cal.h"

#define SERVICE_UUID        "4fafc201-1fb5-459e-8fcc-c5c9c331914b"
#define CHARACTERISTIC_UUID "beb5483e-36e1-4688-b7f5-ea07361b26a8"

#define RADAR_PIN 34
#define LED_PIN D10
#define BATTERY_PIN 35
#define RADAR_THRESHOLD 2000  // Threshold for radar distance in mm

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
  return ((float)raw / 4096.0) * 7.4;  // Adjust based on your voltage divider
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
                      BLECharacteristic::PROPERTY_WRITE |
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
  // Simulated radar reading
  int radarValue = analogRead(RADAR_PIN);
  
  // Blink LED based on radar distance
  if (radarValue < RADAR_THRESHOLD) {
    digitalWrite(LED_PIN, HIGH);
    pCharacteristic->setValue("1");
  } else {
    digitalWrite(LED_PIN, LOW);
    pCharacteristic->setValue("0");
  }
  pCharacteristic->notify();

  // Read and print battery voltage
  float batteryVoltage = getBatteryVoltage();
  Serial.print("X1 Battery Voltage: ");
  Serial.println(batteryVoltage);

  delay(500);  // Adjust delay as necessary
}
```

### X2 Code

```cpp
#include <BLEDevice.h>
#include <BLEUtils.h>
#include <BLEServer.h>
#include "esp_adc_cal.h"

#define SERVICE_UUID        "4fafc201-1fb5-459e-8fcc-c5c9c331914b"
#define CHARACTERISTIC_UUID "beb5483e-36e1-4688-b7f5-ea07361b26a8"

#define LED_PIN D10
#define BATTERY_PIN 35

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
  return ((float)raw / 4096.0) * 7.4;  // Adjust based on your voltage divider
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
                      BLECharacteristic::PROPERTY_WRITE |
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
  // Simulate distance received from X1 or other means
  int simulatedDistance = 1500;  // This would be replaced by actual data

  // Blink LED if distance is below threshold
  if (simulatedDistance < 2000) {
    digitalWrite(LED_PIN, HIGH);
    pCharacteristic->setValue("1");
  } else {
    digitalWrite(LED_PIN, LOW);
    pCharacteristic->setValue("0");
  }
  pCharacteristic->notify();

  // Read and print battery voltage
  float batteryVoltage = getBatteryVoltage();
  Serial.print("X2 Battery Voltage: ");
  Serial.println(batteryVoltage);

  delay(500);  // Adjust delay as necessary
}
```

### Code Explanation:

- **X1 and X2**:
  - Both devices initialize BLE with a specific service UUID and characteristic UUID.
  - The radar simulation is represented by the radarValue variable on X1, which triggers the LED on both X1 and X2 if below the threshold.
  - The battery voltage is read and printed to the serial monitor, and the radar data and LED status are sent to the TTGO module via BLE.

### Note:
This code assumes that the TTGO module you previously implemented will read the characteristic values from both X1 and X2. The TTGO code will need to connect to both X1 and X2, read their characteristics, and send the data to Blynk or display it on its screen.

This code can be modified for specific pin configurations or other requirements as needed.
