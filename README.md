# SistemEmbedded
### Disusun Oleh
- Sari Wahyuningsih 4.31.20.0.23

# Daftar Laporan JobSheet Sistem Embedded

- [JobSheet 1 - Dasar Pemrograman ESP32 Untuk Pemrosesan Data Input/Output Analog dan Digital](https://github.com/sariwhyu/JobSheet1)
- [JobSheet 1.1 - Jaringan Sensor Nirkabel Menggunakan ESP-NOW](https://github.com/sariwhyu/JobSheet1.1)
- [JobSheet 2 - Protokol Komunikasi dan Sensor](https://github.com/sariwhyu/JobSheet2)
- [JobSheet 3 - Topologi Jaringan Lokal dan WiFi](https://github.com/sariwhyu/JobSheet3)
- [Jobsheet4.1 - Cayenne(MQTT)+Sensor+Button+Website Monitoring](https://github.com/sariwhyu/TugasNo1)
- [Jobsheet4.2 - Adafruit.IO+Sensor+LED+Suara](https://github.com/sariwhyu/TugasNo2)
- [Jobsheet4.3 - ThingSpeak+Sensor](https://github.com/sariwhyu/TugasNo3)
- [Jobsheet4.4 - ESP Now+IOT](https://github.com/sariwhyu/TugasNo4)


# Jobsheet4.2 - Adafruit.IO+Sensor+LED+Suara

## Coding

### Adafruit.IO

```bash
  #include <WiFi.h>

#include "Adafruit_MQTT.h"

#include "Adafruit_MQTT_Client.h"

#define WLAN_SSID       "kos putri"

#define WLAN_PASS       "123456788"

#define AIO_SERVER      "io.adafruit.com"

#define AIO_SERVERPORT  1883                  

#define AIO_USERNAME    "rindangoktaviani"

#define AIO_KEY         "aio_Rusa02DaEppjxLMpsN3oxsBKQ1IM"

int output=2;

WiFiClient client;     // Create an ESP8266 WiFiClient class to connect to the MQTT server.

Adafruit_MQTT_Client mqtt(&client, AIO_SERVER, AIO_SERVERPORT, AIO_USERNAME, AIO_KEY);        // Setup the MQTT client class by passing in the WiFi client and MQTT server and login details.

Adafruit_MQTT_Subscribe lampu1 = Adafruit_MQTT_Subscribe(&mqtt, AIO_USERNAME "/feeds/lampu1");

void MQTT_connect();

void setup() {

  Serial.begin(115200);

  delay(10);

pinMode(2,OUTPUT);

 // Connect to WiFi access point.

  Serial.println(); Serial.println();

  Serial.print("Connecting to ");

  Serial.println(WLAN_SSID);

 

  WiFi.begin(WLAN_SSID, WLAN_PASS);

  while (WiFi.status() != WL_CONNECTED) {

    delay(500);

    Serial.print(".");

  }

  Serial.println();

 Serial.println("WiFi connected");

  Serial.println("IP address: "); Serial.println(WiFi.localIP());

  mqtt.subscribe(&lampu1);

}

uint32_t x=0;

void loop() {

   MQTT_connect();

Adafruit_MQTT_Subscribe *subscription;

  while ((subscription = mqtt.readSubscription(5000))) {

    if (subscription == &lampu1) {

      Serial.print(F("Got: "));

      Serial.println((char *)lampu1.lastread);

       if (!strcmp((char*) lampu1.lastread, "ON"))

      {

        digitalWrite(2, HIGH);

      }

      else

      {

        digitalWrite(2, LOW);

      }

    }

  }

 

 

}

void MQTT_connect() {

  int8_t ret;

  // Stop if already connected.

  if (mqtt.connected()) {

    return;

  }

 Serial.print("Connecting to MQTT... ");

uint8_t retries = 3;

  while ((ret = mqtt.connect()) != 0) { // connect will return 0 for connected

       Serial.println(mqtt.connectErrorString(ret));

       Serial.println("Retrying MQTT connection in 5 seconds...");

       mqtt.disconnect();

       delay(5000);  // wait 5 seconds

       retries--;

       if (retries == 0) {

         // basically die and wait for WDT to reset me

         while (1);

       }

  }

  Serial.println("MQTT Connected!");

}

```

## Hasil

![App Screenshot](https://github.com/sariwhyu/TugasNo2/blob/main/WhatsApp%20Image%202023-01-03%20at%2018.55.30.jpeg)

![App Screenshot](https://github.com/sariwhyu/TugasNo2/blob/main/WhatsApp%20Image%202023-01-03%20at%2018.56.04.jpeg)

![App Screenshot](https://github.com/sariwhyu/TugasNo2/blob/main/WhatsApp%20Image%202023-01-03%20at%2021.14.48.jpeg)
