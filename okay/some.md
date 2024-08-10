```C
#include <stdio.h>
#include <string.h>
#include "freertos/FreeRTOS.h"
#include "freertos/task.h"
#include "esp_log.h"
#include "esp_ble_mesh_provisioning_api.h"
#include "esp_ble_mesh_networking_api.h"
#include "esp_ble_mesh_local_data_operation_api.h"
#include "esp_random.h"

#define GROUP_ADDR 0xC000  // Group address
#define PUB_INTERVAL 5000  // Publish interval in milliseconds

static const char *TAG = "X1";

// Function to simulate radar distance
float generate_random_distance() {
    return (float)(esp_random() % 1000) / 100.0; // Generate random distance between 0 and 10 meters
}

// Callback function for BLE Mesh provisioning events
void provisioning_event_handler(esp_ble_mesh_prov_cb_event_t event, esp_ble_mesh_prov_cb_param_t *param) {
    ESP_LOGI(TAG, "Provisioning event: %d", event);
}

// Callback function for BLE Mesh model events
void model_event_handler(esp_ble_mesh_model_cb_event_t event, esp_ble_mesh_model_cb_param_t *param) {
    ESP_LOGI(TAG, "Model event: %d", event);
}

void app_main(void) {
    esp_ble_mesh_model_t model = {
        .model_id = ESP_BLE_MESH_MODEL_ID_GEN_ONOFF_CLI,
        .op = NULL,
        .keys = {ESP_BLE_MESH_KEY_UNUSED},  // Use correct initialization
        .groups = {GROUP_ADDR},             // Corrected initialization
        .pub = NULL,
        .user_data = NULL,
    };

    // Register the provisioning event callback
    esp_ble_mesh_register_prov_callback(provisioning_event_handler);

    // Register the model event callback
    esp_ble_mesh_register_custom_model_callback(model_event_handler);

    while (1) {
        float distance = generate_random_distance();
        ESP_LOGI(TAG, "Publishing distance: %.2f meters", distance);

        // Publish the distance to the group address
        esp_ble_mesh_msg_ctx_t ctx = {
            .net_idx = 0,  // Default network index
            .app_idx = 0,  // Default application index
            .addr = GROUP_ADDR,
            .send_rel = false,
            .send_ttl = ESP_BLE_MESH_TTL_DEFAULT,
        };

        uint8_t msg_data[4];
        memcpy(msg_data, &distance, sizeof(distance));

        esp_err_t err = esp_ble_mesh_model_publish(&model, ESP_BLE_MESH_MODEL_OP_GEN_ONOFF_SET, sizeof(msg_data), msg_data, 0);
        if (err) {
            ESP_LOGE(TAG, "Publish failed: %d", err);
        } else {
            ESP_LOGI(TAG, "Distance published successfully.");
        }

        vTaskDelay(PUB_INTERVAL / portTICK_PERIOD_MS);
    }
}



```



```C

#include <stdio.h>
#include <string.h>
#include "freertos/FreeRTOS.h"
#include "freertos/task.h"
#include "esp_log.h"
#include "esp_ble_mesh_provisioning_api.h"
#include "esp_ble_mesh_networking_api.h"
#include "esp_ble_mesh_local_data_operation_api.h"

#define GROUP_ADDR 0xC000  // Group address

static const char *TAG = "X2";

// Callback function for BLE Mesh provisioning events
void provisioning_event_handler(esp_ble_mesh_prov_cb_event_t event, esp_ble_mesh_prov_cb_param_t *param) {
    ESP_LOGI(TAG, "Provisioning event: %d", event);
}

// Callback function for BLE Mesh model events
void model_event_handler(esp_ble_mesh_model_cb_event_t event, esp_ble_mesh_model_cb_param_t *param) {
    ESP_LOGI(TAG, "Model event: %d", event);

    // Check if the event is related to receiving a message
    if (event == ESP_BLE_MESH_MODEL_OPERATION_EVT) {
        if (param->model_operation.opcode == ESP_BLE_MESH_MODEL_OP_GEN_ONOFF_SET) {
            float received_distance;
            memcpy(&received_distance, param->model_operation.msg, sizeof(received_distance));
            ESP_LOGI(TAG, "Received distance: %.2f meters", received_distance);
        }
    }
}

void app_main(void) {
    // Register the provisioning event callback
    esp_ble_mesh_register_prov_callback(provisioning_event_handler);

    // Register the model event callback
    esp_ble_mesh_register_custom_model_callback(model_event_handler);

    ESP_LOGI(TAG, "X2 is ready to receive distance data...");
}


```
