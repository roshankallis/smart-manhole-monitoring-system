# smart-manhole-monitoring-system
to monitor the over flow toxic gas leak from a drainage 
#define BLYNK_TEMPLATE_ID "YOUR_TEMPLATE_ID"
#define BLYNK_TEMPLATE_NAME "Manhole Monitoring"
#define BLYNK_AUTH_TOKEN "YOUR_AUTH_TOKEN"

#define BLYNK_PRINT Serial

#include <ESP8266WiFi.h>
#include <BlynkSimpleEsp8266.h>

// WiFi
char ssid[] = "YOUR_WIFI_NAME";
char pass[] = "YOUR_WIFI_PASSWORD";

// ---------------- PIN DEFINITIONS ----------------

// Tilt sensor
#define TILT_PIN D5

// Float switch
#define FLOAT_PIN D6

// MQ-2 analog output
#define MQ2_PIN A0

// Buzzer
#define BUZZER_PIN D7

// LEDs
#define RED_LED D1
#define GREEN_LED D2

// ---------------- VARIABLES ----------------

int gasValue = 0;
int tiltStatus = 0;
int waterStatus = 0;

BlynkTimer timer;

// ---------------- SENSOR FUNCTION ----------------

void readSensors()
{
  // Read tilt sensor
  tiltStatus = digitalRead(TILT_PIN);

  // Read float switch
  waterStatus = digitalRead(FLOAT_PIN);

  // Read MQ2
  gasValue = analogRead(MQ2_PIN);

  // Print values
  Serial.println("-------------------------");

  Serial.print("MQ2 Gas Value: ");
  Serial.println(gasValue);

  Serial.print("Tilt Status: ");
  Serial.println(tiltStatus);

  Serial.print("Water Status: ");
  Serial.println(waterStatus);

  // Send data to Blynk

  Blynk.virtualWrite(V0, gasValue);
  Blynk.virtualWrite(V1, tiltStatus);
  Blynk.virtualWrite(V2, waterStatus);

  // ---------------- SAFETY CHECK ----------------

  bool gasDanger = gasValue > 400;
  bool tiltDanger = tiltStatus == HIGH;
  bool waterDanger = waterStatus == HIGH;

  if (gasDanger || tiltDanger || waterDanger)
  {
    digitalWrite(RED_LED, HIGH);
    digitalWrite(GREEN_LED, LOW);
    digitalWrite(BUZZER_PIN, HIGH);

    Serial.println("!!! WARNING !!!");

    if (gasDanger)
      Serial.println("Gas Level HIGH");

    if (tiltDanger)
      Serial.println("Manhole Cover Tilted");

    if (waterDanger)
      Serial.println("Water Level HIGH");

    // Blynk warning
    Blynk.virtualWrite(V3, "WARNING");
  }
  else
  {
    digitalWrite(RED_LED, LOW);
    digitalWrite(GREEN_LED, HIGH);
    digitalWrite(BUZZER_PIN, LOW);

    Serial.println("SYSTEM SAFE");

    Blynk.virtualWrite(V3, "SAFE");
  }
}

// ---------------- SETUP ----------------

void setup()
{
  Serial.begin(115200);

  pinMode(TILT_PIN, INPUT_PULLUP);
  pinMode(FLOAT_PIN, INPUT_PULLUP);

  pinMode(BUZZER_PIN, OUTPUT);
  pinMode(RED_LED, OUTPUT);
  pinMode(GREEN_LED, OUTPUT);

  digitalWrite(BUZZER_PIN, LOW);
  digitalWrite(RED_LED, LOW);
  digitalWrite(GREEN_LED, HIGH);

  Serial.println();
  Serial.println("Manhole Monitoring System");

  // Connect Blynk
  Blynk.begin(BLYNK_AUTH_TOKEN, ssid, pass);

  // Read sensors every 1 second
  timer.setInterval(1000L, readSensors);
}

// ---------------- LOOP ----------------

void loop()
{
  Blynk.run();
  timer.run();
}
