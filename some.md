To create a detailed BLE mesh network where X1 and X2 measure distance, blink an LED if the distance is below a threshold, and report the battery voltage to TTGO, here’s how you can structure the code for each device. The TTGO module will collect the data from X1 and X2 and then send it to Blynk or any other cloud service.

### X1 Code (With Radar and LED)
```cpp
#include <BLEDevice.h>
#include <BLEUtils.h>
#include <BLEServer.h>
#include "esp_adc_cal.h"

#define SERVICE_UUID "4fafc201-1fb5-459e-8fcc-c5c9c331914b"
#define CHARACTERISTIC_UUID "beb5483e-36e1-4688-b7f5-ea07361b26a8"

#define RADAR_PIN 34
#define LED_PIN D10
#define BATTERY_PIN 35
#define THRESHOLD 2000  // Example threshold for radar distance

BLEServer *pServer = NULL;
BLECharacteristic *pCharacteristic = NULL;

bool batteryStatus = false;
float voltage = 0.0;
float distance = 0.0;

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
  return ((float)raw / 4096.0) * 7.4; // Adjust based on voltage divider
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
  // Radar simulation
  distance = analogRead(RADAR_PIN);  // Simulated radar distance reading
  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.println(" mm");

  if (distance < THRESHOLD) {
    digitalWrite(LED_PIN, HIGH);
    pCharacteristic->setValue("1");  // Indicating LED is ON
  } else {
    digitalWrite(LED_PIN, LOW);
    pCharacteristic->setValue("0");  // Indicating LED is OFF
  }
  pCharacteristic->notify();

  // Read and print battery voltage
  voltage = getBatteryVoltage();
  Serial.print("Battery Voltage: ");
  Serial.println(voltage);

  delay(1000); // Adjust delay as per requirement
}
```

### X2 Code (LED and BLE Only)
```cpp
#include <BLEDevice.h>
#include <BLEUtils.h>
#include <BLEClient.h>

#define SERVICE_UUID "4fafc201-1fb5-459e-8fcc-c5c9c331914b"
#define CHARACTERISTIC_UUID "beb5483e-36e1-4688-b7f5-ea07361b26a8"

#define LED_PIN D10
#define BATTERY_PIN 35

bool batteryStatus = false;
float voltage = 0.0;
bool connected = false;

BLEClient *pClient;
BLERemoteCharacteristic *pRemoteCharacteristic;

class MyClientCallback: public BLEClientCallbacks {
  void onConnect(BLEClient* pclient) {
    Serial.println("Connected to server");
    connected = true;
  }

  void onDisconnect(BLEClient* pclient) {
    Serial.println("Disconnected from server");
    connected = false;
  }
};

void initADC() {
  analogReadResolution(12);
  analogSetAttenuation(ADC_11db);
}

float getBatteryVoltage() {
  int raw = analogRead(BATTERY_PIN);
  return ((float)raw / 4096.0) * 7.4; // Adjust based on voltage divider
}

void setup() {
  Serial.begin(115200);
  Serial.println("Starting BLE client...");

  pinMode(LED_PIN, OUTPUT);

  BLEDevice::init("");
  pClient = BLEDevice::createClient();
  pClient->setClientCallbacks(new MyClientCallback());

  BLEScan* pBLEScan = BLEDevice::getScan();
  pBLEScan->setAdvertisedDeviceCallbacks(new MyAdvertisedDeviceCallbacks());
  pBLEScan->setInterval(1349);
  pBLEScan->setWindow(449);
  pBLEScan->setActiveScan(true);
  pBLEScan->start(5, false);

  initADC();
}

void loop() {
  if (connected && pRemoteCharacteristic) {
    std::string value = pRemoteCharacteristic->readValue();
    Serial.print("Received value: ");
    Serial.println(value.c_str());

    if (value == "1") {
      digitalWrite(LED_PIN, HIGH);
    } else {
      digitalWrite(LED_PIN, LOW);
    }

    // Read and print battery voltage
    voltage = getBatteryVoltage();
    Serial.print("Battery Voltage: ");
    Serial.println(voltage);
  }

  delay(1000); // Adjust delay as per requirement
}
```

### TTGO Code (Receives Data from X1 and X2)
```cpp
#include <BLEDevice.h>
#include <BLEUtils.h>
#include <BLEClient.h>
#include <TFT_eSPI.h>
#include <BlynkSimpleEsp32.h>

#define SERVICE_UUID "4fafc201-1fb5-459e-8fcc-c5c9c331914b"
#define CHARACTERISTIC_UUID "beb5483e-36e1-4688-b7f5-ea07361b26a8"

TFT_eSPI tft = TFT_eSPI();

BLEClient *pClientX1, *pClientX2;
BLERemoteCharacteristic *pRemoteCharacteristicX1, *pRemoteCharacteristicX2;

class MyClientCallback: public BLEClientCallbacks {
  void onConnect(BLEClient* pclient) {
    Serial.println("Connected to server");
  }

  void onDisconnect(BLEClient* pclient) {
    Serial.println("Disconnected from server");
  }
};

void setup() {
  Serial.begin(115200);
  Serial.println("Starting TTGO...");

  tft.init();
  tft.setRotation(1);
  tft.fillScreen(TFT_BLACK);
  tft.setTextColor(TFT_WHITE, TFT_BLACK);

  BLEDevice::init("");

  pClientX1 = BLEDevice::createClient();
  pClientX1->setClientCallbacks(new MyClientCallback());

  pClientX2 = BLEDevice::createClient();
  pClientX2->setClientCallbacks(new MyClientCallback());

  Blynk.begin("YourAuthToken", "YourSSID", "YourPassword");
}

void loop() {
  // Connect to X1 and X2, read their data
  if (pClientX1->isConnected() && pClientX2->isConnected()) {
    std::string valueX1 = pRemoteCharacteristicX1->readValue();
    std::string valueX2 = pRemoteCharacteristicX2->readValue();

    // Display data on TTGO screen
    tft.fillScreen(TFT_BLACK);
    tft.setCursor(0, 0);
    tft.printf("X1: %s\n", valueX1.c_str());
    tft.printf("X2: %s\n", valueX2.c_str());

    // Send data to Blynk
    Blynk.virtualWrite(V1, valueX1.c_str());
    Blynk.virtualWrite(V2, valueX2.c_str());
  } else {
    tft.fillScreen(TFT_RED);
    tft.setCursor(0, 0);
    tft.printf("Disconnected...");
  }

  delay(2000); // Adjust delay as per requirement
}
```

### Explanation:
1. **X1**: Simulates radar data, checks distance, blinks LED if below the
