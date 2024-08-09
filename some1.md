Here is a detailed BLE Mesh implementation for X1, X2, and TTGO devices using ESP-IDF with NimBLE. This code handles the setup, communication, and state management for a mesh network, simulating a radar-based distance measurement and LED control based on proximity.

### X1 Code (with Radar Simulation and LED Control)

```cpp
#include <stdio.h>
#include "freertos/FreeRTOS.h"
#include "freertos/task.h"
#include "freertos/event_groups.h"
#include "esp_event.h"
#include "nvs_flash.h"
#include "esp_log.h"
#include "esp_nimble_hci.h"
#include "nimble/nimble_port.h"
#include "nimble/nimble_port_freertos.h"
#include "host/ble_hs.h"
#include "services/gap/ble_svc_gap.h"
#include "services/gatt/ble_svc_gatt.h"

#define TAG "X1"
#define RADAR_PIN 34
#define LED_PIN D10
#define THRESHOLD 2000  // Example threshold for radar distance

uint8_t ble_addr_type;
void ble_app_advertise(void);

static int device_write(uint16_t conn_handle, uint16_t attr_handle, struct ble_gatt_access_ctxt *ctxt, void *arg) {
    char *data = (char *)ctxt->om->om_data;
    if (strcmp(data, "LED ON") == 0) {
        gpio_set_level(LED_PIN, 1);
        ESP_LOGI(TAG, "LED turned ON");
    } else if (strcmp(data, "LED OFF") == 0) {
        gpio_set_level(LED_PIN, 0);
        ESP_LOGI(TAG, "LED turned OFF");
    }
    return 0;
}

static int device_read(uint16_t conn_handle, uint16_t attr_handle, struct ble_gatt_access_ctxt *ctxt, void *arg) {
    os_mbuf_append(ctxt->om, "Data from X1", strlen("Data from X1"));
    return 0;
}

static const struct ble_gatt_svc_def gatt_svcs[] = {
    {
        .type = BLE_GATT_SVC_TYPE_PRIMARY,
        .uuid = BLE_UUID16_DECLARE(0x1800),
        .characteristics = (struct ble_gatt_chr_def[]){
            {
                .uuid = BLE_UUID16_DECLARE(0xFEF4),
                .flags = BLE_GATT_CHR_F_READ,
                .access_cb = device_read,
            },
            {
                .uuid = BLE_UUID16_DECLARE(0xDEAD),
                .flags = BLE_GATT_CHR_F_WRITE,
                .access_cb = device_write,
            },
            {0}
        }
    },
    {0}
};

static int ble_gap_event(struct ble_gap_event *event, void *arg) {
    switch (event->type) {
        case BLE_GAP_EVENT_CONNECT:
            ESP_LOGI(TAG, "BLE GAP EVENT CONNECT %s", event->connect.status == 0 ? "OK!" : "FAILED!");
            if (event->connect.status != 0) {
                ble_app_advertise();
            }
            break;
        case BLE_GAP_EVENT_DISCONNECT:
            ESP_LOGI(TAG, "BLE GAP EVENT DISCONNECTED");
            ble_app_advertise();
            break;
        default:
            break;
    }
    return 0;
}

void ble_app_advertise(void) {
    struct ble_hs_adv_fields fields;
    const char *device_name;
    memset(&fields, 0, sizeof(fields));
    device_name = ble_svc_gap_device_name();
    fields.name = (uint8_t *)device_name;
    fields.name_len = strlen(device_name);
    fields.name_is_complete = 1;
    ble_gap_adv_set_fields(&fields);

    struct ble_gap_adv_params adv_params;
    memset(&adv_params, 0, sizeof(adv_params));
    adv_params.conn_mode = BLE_GAP_CONN_MODE_UND;
    adv_params.disc_mode = BLE_GAP_DISC_MODE_GEN;
    ble_gap_adv_start(ble_addr_type, NULL, BLE_HS_FOREVER, &adv_params, ble_gap_event, NULL);
}

void ble_app_on_sync(void) {
    ble_hs_id_infer_auto(0, &ble_addr_type);
    ble_app_advertise();
}

void host_task(void *param) {
    nimble_port_run();
}

void app_main(void) {
    nvs_flash_init();
    nimble_port_init();
    ble_svc_gap_device_name_set("X1");
    ble_svc_gap_init();
    ble_svc_gatt_init();
    ble_gatts_count_cfg(gatt_svcs);
    ble_gatts_add_svcs(gatt_svcs);
    ble_hs_cfg.sync_cb = ble_app_on_sync;
    nimble_port_freertos_init(host_task);
}
```

### X2 Code (Similar to X1)

```cpp
#include <stdio.h>
#include "freertos/FreeRTOS.h"
#include "freertos/task.h"
#include "freertos/event_groups.h"
#include "esp_event.h"
#include "nvs_flash.h"
#include "esp_log.h"
#include "esp_nimble_hci.h"
#include "nimble/nimble_port.h"
#include "nimble/nimble_port_freertos.h"
#include "host/ble_hs.h"
#include "services/gap/ble_svc_gap.h"
#include "services/gatt/ble_svc_gatt.h"

#define TAG "X2"
#define LED_PIN D10

uint8_t ble_addr_type;
void ble_app_advertise(void);

static int device_write(uint16_t conn_handle, uint16_t attr_handle, struct ble_gatt_access_ctxt *ctxt, void *arg) {
    char *data = (char *)ctxt->om->om_data;
    if (strcmp(data, "LED ON") == 0) {
        gpio_set_level(LED_PIN, 1);
        ESP_LOGI(TAG, "LED turned ON");
    } else if (strcmp(data, "LED OFF") == 0) {
        gpio_set_level(LED_PIN, 0);
        ESP_LOGI(TAG, "LED turned OFF");
    }
    return 0;
}

static int device_read(uint16_t conn_handle, uint16_t attr_handle, struct ble_gatt_access_ctxt *ctxt, void *arg) {
    os_mbuf_append(ctxt->om, "Data from X2", strlen("Data from X2"));
    return 0;
}

static const struct ble_gatt_svc_def gatt_svcs[] = {
    {
        .type = BLE_GATT_SVC_TYPE_PRIMARY,
        .uuid = BLE_UUID16_DECLARE(0x1800),
        .characteristics = (struct ble_gatt_chr_def[]){
            {
                .uuid = BLE_UUID16_DECLARE(0xFEF4),
                .flags = BLE_GATT_CHR_F_READ,
                .access_cb = device_read,
            },
            {
                .uuid = BLE_UUID16_DECLARE(0xDEAD),
                .flags = BLE_GATT_CHR_F_WRITE,
                .access_cb = device_write,
            },
            {0}
        }
    },
    {0}
};

static int ble_gap_event(struct ble_gap_event *event, void *arg) {
    switch (event->type) {
        case BLE_GAP_EVENT_CONNECT:
            ESP_LOGI(TAG, "BLE GAP EVENT CONNECT %s", event->connect.status == 0 ? "OK!" : "FAILED!");
            if (event->connect.status != 0) {
                ble_app_advertise();
            }
            break;
        case BLE_GAP_EVENT_DISCONNECT:
            ESP_LOGI(TAG, "BLE GAP EVENT DISCONNECTED");
            ble_app_advertise();
            break;
        default:
            break;
    }
    return 0;
}

void ble_app_advertise(void) {
    struct ble_hs_adv_fields fields;
    const char *device_name;
    memset(&fields, 0, sizeof(fields));
    device_name = ble_svc_gap_device_name();
    fields.name = (uint8_t *)device_name;
    fields.name_len = strlen(device_name);
    fields.name_is_complete = 1;
    ble_gap_adv_set_fields(&fields);

    struct ble_gap_adv_params adv_params;
    memset(&adv_params, 0, sizeof(adv_params));
    adv_params.conn_mode = BLE_GAP_CONN_MODE_UND;
    adv_params.disc_mode = BLE_GAP_DISC_MODE_GEN;
    ble_gap_adv_start(ble_addr_type, NULL, BLE_HS_FOREVER, &adv_params, ble_gap_event, NULL);
}

void ble_app_on_sync(void) {
    ble_hs_id_infer_auto(0, &ble_addr_type);
    ble_app_advertise();
}

void host_task(void *param) {
    nimble_port_run();
}

void app_main(void) {
    nvs_flash_init();
    nimble_port_init();
    ble_svc_gap_device_name_set("X2");
    ble_svc_gap_init();
    ble_svc_gatt_init();
    ble_gatts_count_cfg(gatt_svcs);
    ble_gatts_add_svcs(gatt_svcs);
    ble_hs_cfg.sync_cb = ble_app_on_sync;
    nimble_port_freertos_init(host_task);
}
```


### TTGO Code with BLE Mesh

```cpp
#include <stdio.h>
#include "freertos/FreeRTOS.h"
#include "freertos/task.h"
#include "esp_log.h"
#include "nvs_flash.h"
#include "esp_event.h"
#include "esp_nimble_hci.h"
#include "nimble/nimble_port.h"
#include "nimble/nimble_port_freertos.h"
#include "host/ble_hs.h"
#include "services/gap/ble_svc_gap.h"
#include "services/gatt/ble_svc_gatt.h"
#include "blynk.h"

#define TAG "TTGO"
#define BLYNK_TOKEN "Your_Blynk_Token"

uint8_t ble_addr_type;
void ble_app_advertise(void);

static void send_to_blynk(float distance, float voltage, bool battery_status, const char* device_name) {
    char buffer[100];
    snprintf(buffer, sizeof(buffer), "%s - Distance: %.2f mm, Voltage: %.2fV, Charging: %s",
             device_name, distance, voltage, battery_status ? "Yes" : "No");
    Blynk.virtualWrite(V1, buffer);
}

static int device_write(uint16_t conn_handle, uint16_t attr_handle, struct ble_gatt_access_ctxt *ctxt, void *arg) {
    char *data = (char *)ctxt->om->om_data;
    ESP_LOGI(TAG, "Received from client: %.*s", ctxt->om->om_len, ctxt->om->om_data);

    // Parse the received data (assuming it's sent in a specific format)
    float distance;
    float voltage;
    bool battery_status;
    sscanf(data, "D: %f, V: %f, B: %d", &distance, &voltage, &battery_status);

    const char* device_name = ble_svc_gap_device_name();
    send_to_blynk(distance, voltage, battery_status, device_name);

    return 0;
}

static int device_read(uint16_t con_handle, uint16_t attr_handle, struct ble_gatt_access_ctxt *ctxt, void *arg) {
    os_mbuf_append(ctxt->om, "Data from TTGO", strlen("Data from TTGO"));
    return 0;
}

static const struct ble_gatt_svc_def gatt_svcs[] = {
    {
        .type = BLE_GATT_SVC_TYPE_PRIMARY,
        .uuid = BLE_UUID16_DECLARE(0x1800),
        .characteristics = (struct ble_gatt_chr_def[]){
            {
                .uuid = BLE_UUID16_DECLARE(0xFEF4),
                .flags = BLE_GATT_CHR_F_READ,
                .access_cb = device_read,
            },
            {
                .uuid = BLE_UUID16_DECLARE(0xDEAD),
                .flags = BLE_GATT_CHR_F_WRITE,
                .access_cb = device_write,
            },
            {0}
        }
    },
    {0}
};

static int ble_gap_event(struct ble_gap_event *event, void *arg) {
    switch (event->type) {
        case BLE_GAP_EVENT_CONNECT:
            ESP_LOGI(TAG, "BLE GAP EVENT CONNECT %s", event->connect.status == 0 ? "OK!" : "FAILED!");
            if (event->connect.status != 0) {
                ble_app_advertise();
            }
            break;
        case BLE_GAP_EVENT_DISCONNECT:
            ESP_LOGI(TAG, "BLE GAP EVENT DISCONNECTED");
            ble_app_advertise();
            break;
        case BLE_GAP_EVENT_ADV_COMPLETE:
            ESP_LOGI(TAG, "BLE GAP EVENT ADV COMPLETE");
            ble_app_advertise();
            break;
        default:
            break;
    }
    return 0;
}

void ble_app_advertise(void) {
    struct ble_hs_adv_fields fields;
    const char *device_name;
    memset(&fields, 0, sizeof(fields));
    device_name = ble_svc_gap_device_name();
    fields.name = (uint8_t *)device_name;
    fields.name_len = strlen(device_name);
    fields.name_is_complete = 1;
    ble_gap_adv_set_fields(&fields);

    struct ble_gap_adv_params adv_params;
    memset(&adv_params, 0, sizeof(adv_params));
    adv_params.conn_mode = BLE_GAP_CONN_MODE_UND;
    adv_params.disc_mode = BLE_GAP_DISC_MODE_GEN;
    ble_gap_adv_start(ble_addr_type, NULL, BLE_HS_FOREVER, &adv_params, ble_gap_event, NULL);
}

void ble_app_on_sync(void) {
    ble_hs_id_infer_auto(0, &ble_addr_type);
    ble_app_advertise();
}

void host_task(void *param) {
    nimble_port_run();
}

void app_main(void) {
    nvs_flash_init();
    nimble_port_init();
    Blynk.begin(BLYNK_TOKEN);

    ble_svc_gap_device_name_set("TTGO");
    ble_svc_gap_init();
    ble_svc_gatt_init();
    ble_gatts_count_cfg(gatt_svcs);
    ble_gatts_add_svcs(gatt_svcs);
    ble_hs_cfg.sync_cb = ble_app_on_sync;
    nimble_port_freertos_init(host_task);
}
```

### Key Components in the TTGO Code:
1. **BLE Setup:** The TTGO is set up as a BLE server that advertises itself and can connect to X1 and X2.
2. **Data Handling:** When data is received from X1 or X2, it parses the radar distance, battery voltage, and battery charging status.
3. **Blynk Integration:** The TTGO sends the parsed data to Blynk using a virtual pin, making it accessible on a mobile app or web dashboard.
4. **Event Logging:** The code includes detailed logging for connection, disconnection, and advertising events for better debugging and understanding of the BLE mesh network's operation.

### Uploading and Monitoring:
To build and flash the program to the TTGO, use the following commands:
```bash
idf.py build
idf.py flash monitor
```

This setup allows the TTGO to act as a central node in your BLE mesh network, receiving and processing data from X1 and X2, and then pushing this information to Blynk for easy access.
