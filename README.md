output
#define BLYNK_TEMPLATE_ID "TMPL3wpFP3MEc"
#define BLYNK_TEMPLATE_NAME "medicine reminder"
#define BLYNK_AUTH_TOKEN "zBneVJLkNr4pI6J3mKTwEQEGImWETtuP"

#define BLYNK_PRINT Serial
#include <WiFi.h>
#include <BlynkSimpleEsp32.h>

const int soundSensorPin = A0;
const int motionSensorPin = 2;

char auth[] = BLYNK_AUTH_TOKEN;
char ssid[] = "Redmi Note 10S";
char pass[] = "12345678";

BlynkTimer timer;
int soundFlag = 0;
int motionFlag = 0;

void sendSoundSensor() {
  int soundValue = analogRead(soundSensorPin);
  Serial.print("Sound sensor value: ");
  Serial.println(soundValue);

  int soundThreshold = 500;  // Adjust as needed

  if (soundValue > soundThreshold && soundFlag == 0) {
    Serial.println("Sound detected!");
    
    soundFlag = 1;
  } else if (soundValue <= soundThreshold) {
    Serial.println("No sound detected");
    soundFlag = 0;
  }
}

void sendMotionSensor() {
  int motionValue = digitalRead(motionSensorPin);

  if (motionValue == HIGH && motionFlag == 0) {
    Serial.println("Motion detected!");
    Blynk.logEvent("motion_alert", "Motion Detected");
    motionFlag = 1;
    Blynk.logEvent("medicine_reminder");
  } else if (motionValue == LOW) {
    Serial.println("No motion detected");
    motionFlag = 0;
  }
}

void setup() {
  pinMode(soundSensorPin, INPUT);
  pinMode(motionSensorPin, INPUT);

  Serial.begin(115200);
  Blynk.begin(auth, ssid, pass);

  timer.setInterval(5000L, sendSoundSensor);  // Adjust the interval as needed
  timer.setInterval(5000L, sendMotionSensor); // Adjust the interval as needed
}

void loop() {
  Blynk.run();
  timer.run();
}

picture:

![WhatsApp Image 2023-11-23 at 22 21 28_5ade2039](https://github.com/sourav22as/output/assets/121729561/5909103e-ac24-4be5-8bf4-6c14141cbb6e)

video:


https://github.com/sourav22as/output/assets/121729561/a275a4e0-0e47-4b81-8ea6-c5f1e6aa196b

