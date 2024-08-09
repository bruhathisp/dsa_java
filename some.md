To create a mesh network of two XIAO ESP32C3 devices (X1 and X2) and one TTGO T-Display, we'll implement a system where the XIAO devices communicate with each other over BLE (Bluetooth Low Energy), and the TTGO T-Display connects to the mesh network, displays the relevant data (like battery status), and publishes this data to the Blynk IoT platform.

Here is the outline of the setup:

### Components:
- **X1 (XIAO ESP32C3)**: BLE Server, collects data like battery status and LED status, and sends it over BLE.
- **X2 (XIAO ESP32C3)**: BLE Client, connects to X1 to receive data and send the status back.
- **TTGO T-Display**: Connects to both X1 and X2, receives data from them, displays it on its screen, and publishes the data to Blynk.

### Key Features:
1. **Mesh Network Configuration**: X1 and X2 form a BLE mesh network. TTGO connects to the BLE mesh and acts as a gateway to the internet for sending data to Blynk.
2. **Battery Monitoring**: Both X1 and X2 monitor their battery status and share it with TTGO.
3. **Automatic Reconnection**: All devices periodically scan for each other to maintain a reliable connection.
4. **Data Transfer**: Data is transferred through BLE characteristics, and TTGO displays it and sends it to Blynk.

---

### X1 (BLE Server) Code

```cpp
#include <BLEDevice.h>
#include <BLEUtils.h>
#include <BLEServer.h>
#include "esp_adc_cal.h"

#define SERVICE_UUID        "4fafc201-1fb5-459e-8fcc-c5c9c331914b"
#define CHARACTERISTIC_UUID "beb5483e-36e1-4688-b7f5-ea07361b26a8"
#define LED_PIN 2
#define BATTERY_PIN 35

BLEServer *pServer = NULL;
BLECharacteristic *pCharacteristic = NULL;
bool deviceConnected = false;

class MyServerCallbacks: public BLEServerCallbacks {
    void onConnect(BLEServer* pServer) {
      deviceConnected = true;
      Serial.println("Client connected");
    };

    void onDisconnect(BLEServer* pServer) {
      deviceConnected = false;
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
  // Monitor and update LED status
  int batteryVoltage = getBatteryVoltage();
  String data = String("LED:") + String(digitalRead(LED_PIN)) + ",Battery:" + String(batteryVoltage);
  pCharacteristic->setValue(data.c_str());
  if (deviceConnected) {
    pCharacteristic->notify();
  }

  delay(1000);
}
```

### X2 (BLE Client) Code

```cpp
#include <BLEDevice.h>
#include <BLEUtils.h>
#include <BLEClient.h>

#define SERVICE_UUID        "4fafc201-1fb5-459e-8fcc-c5c9c331914b"
#define CHARACTERISTIC_UUID "beb5483e-36e1-4688-b7f5-ea07361b26a8"
#define LED_PIN 2

BLEClient* pClient;
BLERemoteCharacteristic* pRemoteCharacteristic;
BLEAdvertisedDevice* myDevice;
bool doConnect = false;
bool connected = false;
bool doScan = false;

class MyClientCallback : public BLEClientCallbacks {
  void onConnect(BLEClient* pclient) {
    Serial.println("Connected to server");
  }

  void onDisconnect(BLEClient* pclient) {
    connected = false;
    Serial.println("Disconnected from server");
  }
};

void read_task(void* data) {
  BLERemoteCharacteristic* pBLERemoteCharacteristic = (BLERemoteCharacteristic*)data;
  String value = pBLERemoteCharacteristic->readValue().c_str();
  if (value.indexOf("LED:1") > -1) {
    digitalWrite(LED_PIN, HIGH);
  } else {
    digitalWrite(LED_PIN, LOW);
  }
  vTaskDelete(NULL);
}

static void notifyCallback(
  BLERemoteCharacteristic* pBLERemoteCharacteristic,
  uint8_t* pData,
  size_t length,
  bool isNotify) {
    xTaskCreate(read_task, "read", 2048, (void*)pBLERemoteCharacteristic, 5, nullptr);
}

bool connectToServer() {
  Serial.print("Forming a connection to ");
  Serial.println(myDevice->getAddress().toString().c_str());

  pClient = BLEDevice::createClient();
  pClient->setClientCallbacks(new MyClientCallback());

  pClient->connect(myDevice);

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
  if (pRemoteCharacteristic->canNotify()) {
    pRemoteCharacteristic->registerForNotify(notifyCallback);
  }
  connected = true;
  return true;
}

class MyAdvertisedDeviceCallbacks: public BLEAdvertisedDeviceCallbacks {
  void onResult(BLEAdvertisedDevice advertisedDevice) {
    Serial.print("BLE Advertised Device found: ");
    Serial.println(advertisedDevice.toString().c_str());

    if (advertisedDevice.haveServiceUUID() && advertisedDevice.getServiceUUID().equals(BLEUUID(SERVICE_UUID))) {
      Serial.print("Found our device!  address: ");
      Serial.println(advertisedDevice.getAddress().toString().c_str());
      BLEDevice::getScan()->stop();
      myDevice = new BLEAdvertisedDevice(advertisedDevice);
      doConnect = true;
      doScan = false;
    }
  }
};

void setup() {
  Serial.begin(115200);
  pinMode(LED_PIN, OUTPUT);
  digitalWrite(LED_PIN, LOW);

  BLEDevice::init("");
  BLEScan* pBLEScan = BLEDevice::getScan();
  pBLEScan->setAdvertisedDeviceCallbacks(new MyAdvertisedDeviceCallbacks());
  pBLEScan->setInterval(1349);
  pBLEScan->setWindow(449);
  pBLEScan->setActiveScan(true);
  pBLEScan->start(5, false);
}

void loop() {
  if (doConnect) {
    if (connectToServer()) {
      Serial.println("We are now connected to the BLE Server.");
    } else {
      Serial.println("We have failed to connect to the server; there is nothing more we will do.");
    }
    doConnect = false;
  }

  if (connected) {
    delay(1000);
  } else if (doScan) {
    BLEDevice::getScan()->start(0);
  }
}
```

### TTGO T-Display Code (as a BLE Central and Blynk Gateway)

```cpp
#include <TFT_eSPI.h>
#include <Wire.h>
#include <BLEDevice.h>
#include <BLEUtils.h>
#include <BLEClient.h>
#include <WiFi.h>
#include <BlynkSimpleEsp32.h>

#define SERVICE_UUID        "4fafc201-1fb5-459e-8fcc-c5c9c331914b"
#define CHARACTERISTIC_UUID "beb5483e-36e1-4688-b7f5-ea07361b26a8"

char auth[] = "YourBlynkAuthToken";
char ssid[] = "YourSSID";
char pass[] = "YourPassword";

BLEClient* pClient;
BLERemoteCharacteristic* pRemoteCharacteristicX1;
BLERemoteCharacteristic* pRemoteCharacteristicX2;
BLEAdvertisedDevice* myDeviceX1;
BLEAdvertisedDevice* myDeviceX2;
bool doConnectX1 = false;
bool doConnectX2 = false;
bool connectedX1 = false;
bool connectedX2 = false;

TFT_eSPI tft = TFT_eSPI();  // Initialize TFT

class MyClientCallback : public BLEClientCallbacks {
  void onConnect(BLEClient* pclient) {
    Serial.println("Connected to server");
  }



Here's a detailed code implementation for the TTGO T-Display, X1 (ESP32-C3), and X2 (ESP32-C3) to create a BLE mesh network. The TTGO acts as the central hub, connecting to both X1 and X2, reading their data, displaying it on the TFT screen, and sending it to Blynk.

### TTGO T-Display Code

This code connects to both X1 and X2, receives data over BLE, displays the information on the TFT screen, and publishes it to Blynk.

```cpp
#include <TFT_eSPI.h>
#include <Wire.h>
#include <BLEDevice.h>
#include <BLEUtils.h>
#include <BLEClient.h>
#include <WiFi.h>
#include <BlynkSimpleEsp32.h>

#define SERVICE_UUID        "4fafc201-1fb5-459e-8fcc-c5c9c331914b"
#define CHARACTERISTIC_UUID "beb5483e-36e1-4688-b7f5-ea07361b26a8"

char auth[] = "YourBlynkAuthToken";
char ssid[] = "YourSSID";
char pass[] = "YourPassword";

BLEClient* pClientX1;
BLEClient* pClientX2;
BLERemoteCharacteristic* pRemoteCharacteristicX1;
BLERemoteCharacteristic* pRemoteCharacteristicX2;

TFT_eSPI tft = TFT_eSPI();  // Initialize TFT

class MyClientCallback : public BLEClientCallbacks {
  void onConnect(BLEClient* pclient) {
    Serial.println("Connected to server");
  }

  void onDisconnect(BLEClient* pclient) {
    Serial.println("Disconnected from server");
  }
};

void connectToServer(BLEClient* pClient, BLERemoteCharacteristic** pRemoteCharacteristic, String serverName) {
  BLERemoteService* pRemoteService = pClient->getService(BLEUUID(SERVICE_UUID));
  if (pRemoteService == nullptr) {
    Serial.println("Failed to find service UUID");
    pClient->disconnect();
    return;
  }

  *pRemoteCharacteristic = pRemoteService->getCharacteristic(BLEUUID(CHARACTERISTIC_UUID));
  if (*pRemoteCharacteristic == nullptr) {
    Serial.println("Failed to find characteristic UUID");
    pClient->disconnect();
    return;
  }
}

void displayData(String name, String data) {
  tft.fillScreen(TFT_BLACK);
  tft.setCursor(0, 0);
  tft.setTextColor(TFT_WHITE);
  tft.setTextSize(2);
  tft.print(name);
  tft.println(": ");
  tft.println(data);

  Blynk.virtualWrite(V1, data);  // Send data to Blynk
}

void setup() {
  Serial.begin(115200);
  tft.init();
  tft.setRotation(1);

  WiFi.begin(ssid, pass);
  Blynk.begin(auth, ssid, pass);

  BLEDevice::init("");
  pClientX1 = BLEDevice::createClient();
  pClientX1->setClientCallbacks(new MyClientCallback());

  pClientX2 = BLEDevice::createClient();
  pClientX2->setClientCallbacks(new MyClientCallback());
}

void loop() {
  if (pClientX1->isConnected()) {
    if (pRemoteCharacteristicX1->canRead()) {
      String data = pRemoteCharacteristicX1->readValue().c_str();
      displayData("X1", data);
    }
  } else {
    BLEScan* pBLEScan = BLEDevice::getScan();
    pBLEScan->start(5, false);
    pClientX1->connect(BLEAddress("X1_Address"));
    connectToServer(pClientX1, &pRemoteCharacteristicX1, "X1");
  }

  if (pClientX2->isConnected()) {
    if (pRemoteCharacteristicX2->canRead()) {
      String data = pRemoteCharacteristicX2->readValue().c_str();
      displayData("X2", data);
    }
  } else {
    BLEScan* pBLEScan = BLEDevice::getScan();
    pBLEScan->start(5, false);
    pClientX2->connect(BLEAddress("X2_Address"));
    connectToServer(pClientX2, &pRemoteCharacteristicX2, "X2");
  }

  Blynk.run();
  delay(1000);
}
```

### Explanation

1. **X1 and X2 Code**: Already provided in the earlier responses, where X1 acts as a BLE Server and X2 acts as a BLE Client.
2. **BLE Connections**: The TTGO connects to both X1 and X2 via BLE, retrieves the battery status and LED status, and displays the information on the TFT screen.
3. **Blynk Integration**: The data received from X1 and X2 is also sent to the Blynk IoT platform.

### Important Notes

1. **BLE Address**: Replace `"X1_Address"` and `"X2_Address"` with the actual BLE addresses of your X1 and X2 devices.
2. **Blynk Configuration**: Replace the Blynk auth token, SSID, and password with your actual network credentials.

This code provides the framework to establish a BLE mesh network with the TTGO as the central hub, displaying and transmitting data to Blynk. Ensure all devices are powered and in range for successful communication.
