# Project-1
Internet-Based Temperature Monitoring System
#define BLYNK_TEMPLATE_ID "TMPL6yV917LqZ"
#define BLYNK_TEMPLATE_NAME "Project 1"
#define BLYNK_FIRMWARE_VERSION "0.1.0"
#define BLYNK_PRINT Serial
#define APP_DEBUG
#define USE_NODE_MCU_BOARD
#include "BlynkEdgent.h"
#include "DHTesp.h"
#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>

// OLED setup
#define SCREEN_WIDTH 128 
#define SCREEN_HEIGHT 64 
#define OLED_RESET     -1 
#define SCREEN_ADDRESS 0x3C 
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);

// DHT setup
DHTesp dht;
float temperature, humidity;
unsigned long timeShowOled = millis();

// Blynk
BlynkTimer timer;
WidgetLED LEDCONNECT(V0);
#define NHIETDO V1
#define DOAM V2

// Buzzer setup (Đã đổi sang D5)
#define BUZZER_PIN 14  // D5 - GPIO14
bool buzzerState = false;

// Fan (quạt) setup
#define FAN_PIN 12     // D6 - GPIO12
bool fanState = false;

// Forward declarations
void updateBlynk();
void showOled(float t, float h);
void controlOutputs();

// Setup
void setup() {
  Serial.begin(115200);
  delay(100);

  pinMode(BUZZER_PIN, OUTPUT);
  digitalWrite(BUZZER_PIN, LOW);

  pinMode(FAN_PIN, OUTPUT);
  digitalWrite(FAN_PIN, LOW);

  BlynkEdgent.begin();

  if(!display.begin(SSD1306_SWITCHCAPVCC, SCREEN_ADDRESS)) {
    Serial.println(F("SSD1306 allocation failed"));
    for(;;); 
  }
  display.display();
  delay(2000);
  
  dht.setup(16, DHTesp::DHT11); // D0 -> DHT11

  timer.setInterval(1000L, updateBlynk);
}

// Main loop
void loop() {
  BlynkEdgent.run();
  timer.run();

  if (millis() - timeShowOled > dht.getMinimumSamplingPeriod()) {
    float t = dht.getTemperature();
    float h = dht.getHumidity();
    if (dht.getStatusString() == "OK") {
      temperature = t;
      humidity = h;
      showOled(temperature, humidity);
    }
    timeShowOled = millis();
  }

  controlOutputs();
}

// Update Blynk
void updateBlynk() {
  if (LEDCONNECT.getValue()) LEDCONNECT.off();
  else LEDCONNECT.on();

  Blynk.virtualWrite(NHIETDO, temperature);
  Blynk.virtualWrite(DOAM, humidity);
}

// Show OLED
void showOled(float t, float h) {
  display.clearDisplay();

  // Vẽ nhiệt độ & độ ẩm
  display.setTextSize(2);
  display.setTextColor(SSD1306_WHITE);
  display.setCursor(0, 0);
  display.print("T: ");
  display.print(t, 1);
  display.print((char)247);
  display.println("C");
  display.print("H: ");
  display.print(h, 0);
  display.println("%");

  // Chuyển sang text size nhỏ để hiển thị trạng thái
  display.setTextSize(1);
  display.setCursor(0, 40);
  display.print("Buzzer: ");
  display.println(buzzerState ? "ON" : "OFF");
  display.setCursor(0, 50);
  display.print("Fan   : ");
  display.println(fanState    ? "ON" : "OFF");

  display.display();
}

// Blynk write handlers
BLYNK_WRITE(V3) { // Buzzer control
  int pinValue = param.asInt();
  buzzerState = (pinValue == 1);
}

BLYNK_WRITE(V4) { // Fan (quạt) control
  int pinValue = param.asInt();
  fanState = (pinValue == 1);
}

// Control buzzer and fan hardware
void controlOutputs() {
  digitalWrite(BUZZER_PIN, buzzerState ? HIGH : LOW);
  digitalWrite(FAN_PIN, fanState ? HIGH : LOW);
}
