To see the BLE devices (X1 and X2) using the nRF Connect app, follow these steps:

### Using nRF Connect Mobile App (Android/iOS)

1. **Install nRF Connect App**:
   - Download and install the nRF Connect app from the Google Play Store (for Android) or the Apple App Store (for iOS).

2. **Open nRF Connect App**:
   - Launch the nRF Connect app on your mobile device.

3. **Scan for Devices**:
   - Press the "Scan" button to start scanning for nearby BLE devices.

4. **Locate X1 and X2**:
   - Look for your devices in the list of scanned devices. They should be named "X1" and "X2" (or whatever name you have given them in your code).

5. **Connect to Device**:
   - Tap on the device name (X1 or X2) to connect to it.
   - Once connected, you can explore the services and characteristics offered by the device.

6. **Read/Write Characteristics**:
   - Navigate to the specific service and characteristic you want to interact with.
   - You can read the value, write a new value, or enable notifications for the characteristic.

### Using nRF Connect for Desktop (Windows/Mac/Linux)

1. **Install nRF Connect for Desktop**:
   - Download and install nRF Connect for Desktop from the [Nordic Semiconductor website](https://www.nordicsemi.com/Software-and-Tools/Development-Tools/nRF-Connect-for-desktop).

2. **Install Bluetooth Low Energy App**:
   - Open nRF Connect for Desktop.
   - Install the Bluetooth Low Energy app from the list of available apps.

3. **Open Bluetooth Low Energy App**:
   - Launch the Bluetooth Low Energy app within nRF Connect for Desktop.

4. **Select an Adapter**:
   - Select the Bluetooth adapter you want to use for scanning and interacting with BLE devices.

5. **Scan for Devices**:
   - Click on "Start scan" to begin scanning for nearby BLE devices.

6. **Locate X1 and X2**:
   - Look for your devices in the list of scanned devices. They should be named "X1" and "X2" (or whatever name you have given them in your code).

7. **Connect to Device**:
   - Click on the device name (X1 or X2) to connect to it.
   - Once connected, you can explore the services and characteristics offered by the device.

8. **Read/Write Characteristics**:
   - Navigate to the specific service and characteristic you want to interact with.
   - You can read the value, write a new value, or enable notifications for the characteristic.

### BLE Server Code for X1 (Example)

Here is a detailed BLE server example code for X1:

```cpp
#include <BLEDevice.h>
#include <BLEUtils.h>
#include <BLEServer.h>

// UUIDs for the service and characteristics
#define SERVICE_UUID        "4fafc201-1fb5-459e-8fcc-c5c9c331914b"
#define CHARACTERISTIC_UUID "beb5483e-36e1-4688-b7f5-ea07361b26a8"

BLECharacteristic *pCharacteristic;
bool deviceConnected = false;

class MyServerCallbacks : public BLEServerCallbacks {
  void onConnect(BLEServer* pServer) {
    deviceConnected = true;
  };

  void onDisconnect(BLEServer* pServer) {
    deviceConnected = false;
  }
};

void setup() {
  Serial.begin(115200);
  Serial.println("Starting BLE work!");

  BLEDevice::init("X1");
  BLEServer *pServer = BLEDevice::createServer();
  pServer->setCallbacks(new MyServerCallbacks());

  BLEService *pService = pServer->createService(SERVICE_UUID);

  pCharacteristic = pService->createCharacteristic(
                      CHARACTERISTIC_UUID,
                      BLECharacteristic::PROPERTY_READ |
                      BLECharacteristic::PROPERTY_WRITE |
                      BLECharacteristic::PROPERTY_NOTIFY
                    );

  pCharacteristic->setValue("Hello World from X1");
  pService->start();

  BLEAdvertising *pAdvertising = BLEDevice::getAdvertising();
  pAdvertising->addServiceUUID(SERVICE_UUID);
  pAdvertising->setScanResponse(true);
  pAdvertising->setMinPreferred(0x06);  // functions that help with iPhone connections issue
  pAdvertising->setMinPreferred(0x12);
  BLEDevice::startAdvertising();
  Serial.println("Characteristic defined! Now you can read it in your phone!");
}

void loop() {
  if (deviceConnected) {
    pCharacteristic->setValue("Hello from X1");
    pCharacteristic->notify();
    delay(1000);
  }
}
```

### BLE Client Code for X2 (Example)

Here is a detailed BLE client example code for X2:

```cpp
#include <BLEDevice.h>
#include <BLEUtils.h>
#include <BLEClient.h>

#define SERVICE_UUID        "4fafc201-1fb5-459e-8fcc-c5c9c331914b"
#define CHARACTERISTIC_UUID "beb5483e-36e1-4688-b7f5-ea07361b26a8"

static BLEUUID serviceUUID(SERVICE_UUID);
static BLEUUID charUUID(CHARACTERISTIC_UUID);

static boolean doConnect = false;
static boolean connected = false;
static boolean doScan = false;
static BLERemoteCharacteristic* pRemoteCharacteristic;
static BLEAdvertisedDevice* myDevice;

class MyClientCallback : public BLEClientCallbacks {
  void onConnect(BLEClient* pclient) {
  }

  void onDisconnect(BLEClient* pclient) {
    connected = false;
    Serial.println("Disconnected");
  }
};

bool connectToServer() {
  Serial.print("Forming a connection to ");
  Serial.println(myDevice->getAddress().toString().c_str());

  BLEClient* pClient = BLEDevice::createClient();
  pClient->setClientCallbacks(new MyClientCallback());

  pClient->connect(myDevice);  // if you pass BLEAdvertisedDevice instead of address, it will connect to the address of the advertised device

  BLERemoteService* pRemoteService = pClient->getService(serviceUUID);
  if (pRemoteService == nullptr) {
    Serial.print("Failed to find our service UUID: ");
    Serial.println(serviceUUID.toString().c_str());
    pClient->disconnect();
    return false;
  }
  pRemoteCharacteristic = pRemoteService->getCharacteristic(charUUID);
  if (pRemoteCharacteristic == nullptr) {
    Serial.print("Failed to find our characteristic UUID: ");
    Serial.println(charUUID.toString().c_str());
    pClient->disconnect();
    return false;
  }
  if (pRemoteCharacteristic->canRead()) {
    std::string value = pRemoteCharacteristic->readValue();
    Serial.print("The characteristic value was: ");
    Serial.println(value.c_str());
  }
  connected = true;
  return true;
}

class MyAdvertisedDeviceCallbacks: public BLEAdvertisedDeviceCallbacks {
  void onResult(BLEAdvertisedDevice advertisedDevice) {
    Serial.print("BLE Advertised Device found: ");
    Serial.println(advertisedDevice.toString().c_str());

    if (advertisedDevice.haveServiceUUID() && advertisedDevice.getServiceUUID().equals(serviceUUID)) {
      Serial.print("Found our device!  address: ");
      advertisedDevice.getAddress().toString().c_str();
      BLEDevice::getScan()->stop();
      myDevice = new BLEAdvertisedDevice(advertisedDevice);
      doConnect = true;
      doScan = true;
    }
  }
};

void setup() {
  Serial.begin(115200);
  Serial.println("Starting BLE Client application...");

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
    if (pRemoteCharacteristic->canRead()) {
      std::string value = pRemoteCharacteristic->readValue();
      Serial.print("The characteristic value is: ");
      Serial.println(value.c_str());
    }
    delay(1000);
  } else if (doScan) {
    BLEDevice::getScan()->start(0);
  }
}
```

### Steps to Test with nRF Connect

1. **Upload the Server Code to X1**:
   - Connect X1 to your computer and upload the server code.

2. **Upload the Client Code to X2**:
   - Connect X2 to your computer and upload the client code.

3. **Open nRF Connect on Your Mobile Device**:
   - Open the nRF Connect app.
   - Scan for devices.
   - You should see the device named "X1".

4. **Connect to X1**:
   - Tap on the device named "X1".
   - Explore the services and characteristics offered by X1.

5. **Verify Communication**:
   - Ensure that the characteristic values are being updated and can be read from the client (X2).

By following these steps, you should be able to create a BLE mesh network with your X1 and X2 devices and verify the communication using
