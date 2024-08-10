Sure! Below is the complete code for both X1 and X2. Each device will send and receive messages over BLE Mesh, and they will print the received values on the serial monitor.

### **X1 Code**

This code will be flashed onto the X1 device. It sends a multiple of 5 and receives values from X2.

```c
#include <stdio.h>
#include "esp_ble_mesh_defs.h"
#include "esp_ble_mesh_common_api.h"
#include "esp_ble_mesh_networking_api.h"
#include "esp_ble_mesh_provisioning_api.h"
#include "esp_ble_mesh_config_model_api.h"
#include "esp_ble_mesh_generic_model_api.h"
#include "nvs_flash.h"
#include "esp_log.h"

#define TAG "X1"

ESP_BLE_MESH_MODEL_PUB_DEFINE(onoff_pub, 2 + 1, ROLE_NODE);

static void onoff_set(struct bt_mesh_onoff_srv *srv, struct bt_mesh_msg_ctx *ctx,
                      const struct bt_mesh_onoff_set *set,
                      struct bt_mesh_onoff_status *rsp)
{
    int value = 5;  // Send multiple of 5
    ESP_LOGI(TAG, "Sending value: %d", value);

    // Send value over mesh
    esp_ble_mesh_model_publish(srv->model, &value, sizeof(value), ROLE_NODE);
}

static const struct bt_mesh_onoff_srv_handlers onoff_srv_handlers = {
    .set = onoff_set,
};

static struct bt_mesh_onoff_srv onoff_srv = BT_MESH_ONOFF_SRV_INIT(&onoff_srv_handlers);

static void ble_mesh_generic_server_cb(esp_ble_mesh_generic_server_cb_event_t event,
                                       esp_ble_mesh_generic_server_cb_param_t *param)
{
    if (event == ESP_BLE_MESH_GENERIC_SERVER_RECV_SET_MSG_EVT) {
        int received_value = *(int *)param->value.set.onoff.onoff;
        ESP_LOGI(TAG, "Received value from X2: %d", received_value);
    }
}

static esp_ble_mesh_model_t root_models[] = {
    ESP_BLE_MESH_MODEL_GEN_ONOFF_SRV(&onoff_pub, &onoff_srv),
};

static esp_ble_mesh_elem_t elements[] = {
    ESP_BLE_MESH_ELEMENT(0, root_models, ESP_BLE_MESH_MODEL_NONE),
};

static esp_ble_mesh_comp_t composition = {
    .cid = ESP_BLE_MESH_CID_NVAL,
    .elements = elements,
    .element_count = ARRAY_SIZE(elements),
};

static esp_ble_mesh_prov_t prov = {
    .uuid = {0xdd, 0xdd},
};

void app_main(void)
{
    esp_err_t err;

    ESP_LOGI(TAG, "Initializing...");

    err = nvs_flash_init();
    if (err == ESP_ERR_NVS_NO_FREE_PAGES || err == ESP_ERR_NVS_NEW_VERSION_FOUND) {
        ESP_ERROR_CHECK(nvs_flash_erase());
        err = nvs_flash_init();
    }
    ESP_ERROR_CHECK(err);

    err = bluetooth_init();
    if (err) {
        ESP_LOGE(TAG, "Bluetooth init failed (err %d)", err);
        return;
    }

    esp_ble_mesh_register_prov_callback(ble_mesh_prov_callback);
    esp_ble_mesh_register_generic_server_callback(ble_mesh_generic_server_cb);

    err = esp_ble_mesh_init(&prov, &composition);
    if (err) {
        ESP_LOGE(TAG, "BLE Mesh init failed (err %d)", err);
        return;
    }

    err = esp_ble_mesh_node_prov_enable(ESP_BLE_MESH_PROV_ADV | ESP_BLE_MESH_PROV_GATT);
    if (err) {
        ESP_LOGE(TAG, "Failed to enable BLE Mesh node (err %d)", err);
        return;
    }

    ESP_LOGI(TAG, "BLE Mesh Node initialized");
}
```

### **X2 Code**

This code will be flashed onto the X2 device. It sends a multiple of 10 and receives values from X1.

```c
#include <stdio.h>
#include "esp_ble_mesh_defs.h"
#include "esp_ble_mesh_common_api.h"
#include "esp_ble_mesh_networking_api.h"
#include "esp_ble_mesh_provisioning_api.h"
#include "esp_ble_mesh_config_model_api.h"
#include "esp_ble_mesh_generic_model_api.h"
#include "nvs_flash.h"
#include "esp_log.h"

#define TAG "X2"

ESP_BLE_MESH_MODEL_PUB_DEFINE(onoff_pub, 2 + 1, ROLE_NODE);

static void onoff_set(struct bt_mesh_onoff_srv *srv, struct bt_mesh_msg_ctx *ctx,
                      const struct bt_mesh_onoff_set *set,
                      struct bt_mesh_onoff_status *rsp)
{
    int value = 10;  // Send multiple of 10
    ESP_LOGI(TAG, "Sending value: %d", value);

    // Send value over mesh
    esp_ble_mesh_model_publish(srv->model, &value, sizeof(value), ROLE_NODE);
}

static const struct bt_mesh_onoff_srv_handlers onoff_srv_handlers = {
    .set = onoff_set,
};

static struct bt_mesh_onoff_srv onoff_srv = BT_MESH_ONOFF_SRV_INIT(&onoff_srv_handlers);

static void ble_mesh_generic_server_cb(esp_ble_mesh_generic_server_cb_event_t event,
                                       esp_ble_mesh_generic_server_cb_param_t *param)
{
    if (event == ESP_BLE_MESH_GENERIC_SERVER_RECV_SET_MSG_EVT) {
        int received_value = *(int *)param->value.set.onoff.onoff;
        ESP_LOGI(TAG, "Received value from X1: %d", received_value);
    }
}

static esp_ble_mesh_model_t root_models[] = {
    ESP_BLE_MESH_MODEL_GEN_ONOFF_SRV(&onoff_pub, &onoff_srv),
};

static esp_ble_mesh_elem_t elements[] = {
    ESP_BLE_MESH_ELEMENT(0, root_models, ESP_BLE_MESH_MODEL_NONE),
};

static esp_ble_mesh_comp_t composition = {
    .cid = ESP_BLE_MESH_CID_NVAL,
    .elements = elements,
    .element_count = ARRAY_SIZE(elements),
};

static esp_ble_mesh_prov_t prov = {
    .uuid = {0xdd, 0xdd},
};

void app_main(void)
{
    esp_err_t err;

    ESP_LOGI(TAG, "Initializing...");

    err = nvs_flash_init();
    if (err == ESP_ERR_NVS_NO_FREE_PAGES || err == ESP_ERR_NVS_NEW_VERSION_FOUND) {
        ESP_ERROR_CHECK(nvs_flash_erase());
        err = nvs_flash_init();
    }
    ESP_ERROR_CHECK(err);

    err = bluetooth_init();
    if (err) {
        ESP_LOGE(TAG, "Bluetooth init failed (err %d)", err);
        return;
    }

    esp_ble_mesh_register_prov_callback(ble_mesh_prov_callback);
    esp_ble_mesh_register_generic_server_callback(ble_mesh_generic_server_cb);

    err = esp_ble_mesh_init(&prov, &composition);
    if (err) {
        ESP_LOGE(TAG, "BLE Mesh init failed (err %d)", err);
        return;
    }

    err = esp_ble_mesh_node_prov_enable(ESP_BLE_MESH_PROV_ADV | ESP_BLE_MESH_PROV_GATT);
    if (err) {
        ESP_LOGE(TAG, "Failed to enable BLE Mesh node (err %d)", err);
        return;
    }

    ESP_LOGI(TAG, "BLE Mesh Node initialized");
}
```

### **Explanation:**

- **OnOff Set Function**: 
  - In both codes (`onoff_set` function), X1 sends a multiple of 5, and X2 sends a multiple of 10. These values are published to the mesh network.
  
- **Receiving Messages**:
  - The `ble_mesh_generic_server_cb` function is used in both codes to receive and print the messages from the other device.

- **Mesh Configuration**:
  - The provisioning, element composition, and model initialization are done similarly in both codes. This ensures that both X1 and X2 are part of the same BLE Mesh network and can communicate with each other.

### **Steps to Integrate and Test**:

1. **Provisioning**:
   - Use a mesh provisioner (e.g., nRF Mesh app) to provision both X1 and X2 into the same mesh network.

2. **Binding Application Keys**:
   - Bind the same application key to the Generic OnOff Server model on both X1 and X2.

3. **Flashing and Testing**:
   - Flash the respective code onto X1 and X2.
   - Open the serial monitor for both devices.
   - You should see X1 sending a multiple of 5, and X2 sending a multiple of 10. Each device should also print the value received from the other device.

This setup should allow both X1 and X2 to communicate within the BLE Mesh network, sending and receiving the specified multiples and printing them on the serial monitor.
