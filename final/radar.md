``` C
#include "freertos/FreeRTOS.h"
#include "freertos/task.h"
#include "esp_system.h"
#include "esp_log.h"
#include "driver/uart.h"
#include "string.h"
#include "driver/gpio.h"

static const int RX_BUF_SIZE = 4096;

#define TXD_PIN (GPIO_NUM_21)  // TX is GPIO 21
#define RXD_PIN (GPIO_NUM_20)  // RX is GPIO 20

static const char *TAG = "UART_DEBUG";

void init(void)
{
    ESP_LOGI(TAG, "Initializing UART...");

    const uart_config_t uart_config = {
        .baud_rate = 115200,  // Changed to 115200
        .data_bits = UART_DATA_8_BITS,
        .parity    = UART_PARITY_DISABLE,
        .stop_bits = UART_STOP_BITS_1,
        .flow_ctrl = UART_HW_FLOWCTRL_DISABLE,
        .source_clk = UART_SCLK_DEFAULT,
    };

    ESP_LOGI(TAG, "Installing UART driver...");
    esp_err_t ret = uart_driver_install(UART_NUM_1, RX_BUF_SIZE, RX_BUF_SIZE, 0, NULL, 0);
    if (ret != ESP_OK) {
        ESP_LOGE(TAG, "Failed to install UART driver: %s", esp_err_to_name(ret));
    }

    ESP_LOGI(TAG, "Configuring UART parameters...");
    ret = uart_param_config(UART_NUM_1, &uart_config);
    if (ret != ESP_OK) {
        ESP_LOGE(TAG, "Failed to configure UART: %s", esp_err_to_name(ret));
    }

    ESP_LOGI(TAG, "Setting UART pins...");
    ret = uart_set_pin(UART_NUM_1, TXD_PIN, RXD_PIN, UART_PIN_NO_CHANGE, UART_PIN_NO_CHANGE);
    if (ret != ESP_OK) {
        ESP_LOGE(TAG, "Failed to set UART pins: %s", esp_err_to_name(ret));
    }

    ESP_LOGI(TAG, "UART initialization complete.");
}

int sendData(const char* logName, const char* data)
{
    ESP_LOGI(logName, "Sending data: %s", data);
    const int len = strlen(data);
    const int txBytes = uart_write_bytes(UART_NUM_1, data, len);
    if (txBytes > 0) {
        ESP_LOGI(logName, "Successfully wrote %d bytes", txBytes);
    } else {
        ESP_LOGE(logName, "Failed to write data");
    }
    return txBytes;
}

static void tx_task(void *arg)
{
    static const char *TX_TASK_TAG = "TX_TASK";
    esp_log_level_set(TX_TASK_TAG, ESP_LOG_INFO);
    ESP_LOGI(TX_TASK_TAG, "TX task started");
    
    while (1) {
        ESP_LOGI(TX_TASK_TAG, "Sending 'Hello world' message...");
        sendData(TX_TASK_TAG, "Hello world");
        vTaskDelay(2000 / portTICK_PERIOD_MS);
    }
}

static void rx_task(void *arg)
{
    static const char *RX_TASK_TAG = "RX_TASK";
    esp_log_level_set(RX_TASK_TAG, ESP_LOG_INFO);
    ESP_LOGI(RX_TASK_TAG, "RX task started");

    uint8_t* data = (uint8_t*) malloc(RX_BUF_SIZE + 1);
    if (data == NULL) {
        ESP_LOGE(RX_TASK_TAG, "Failed to allocate memory for RX buffer");
        vTaskDelete(NULL);
    }

    while (1) {
        ESP_LOGI(RX_TASK_TAG, "Waiting for data...");
        const int rxBytes = uart_read_bytes(UART_NUM_1, data, RX_BUF_SIZE, 1000 / portTICK_PERIOD_MS);
        if (rxBytes > 0) {
            data[rxBytes] = 0;  // Null-terminate the received data
            ESP_LOGI(RX_TASK_TAG, "Read %d bytes: '%s'", rxBytes, data);
            ESP_LOG_BUFFER_HEXDUMP(RX_TASK_TAG, data, rxBytes, ESP_LOG_INFO);
        } else {
            ESP_LOGW(RX_TASK_TAG, "No data received in the last second");
        }
    }
    free(data);
}

void app_main(void)
{
    ESP_LOGI(TAG, "Starting app_main...");
    init();
    ESP_LOGI(TAG, "Creating RX and TX tasks...");
    xTaskCreate(rx_task, "uart_rx_task", 1024 * 4, NULL, configMAX_PRIORITIES - 1, NULL);
    xTaskCreate(tx_task, "uart_tx_task", 1024 * 4, NULL, configMAX_PRIORITIES - 2, NULL);
    ESP_LOGI(TAG, "Tasks created, main application setup complete.");
}
```
