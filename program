#include <Wire.h>
#include <LiquidCrystal_I2C.h>

// Initialize the LCD with I2C address 0x27 (common address for many LCDs) and 16 columns x 2 rows
LiquidCrystal_I2C lcd(0x27, 16, 2);

const int trigPin = 9;
const int echoPin = 10;
const int relayPin = 8;

const int highLevelThreshold = 20;  // Distance at which the tank is considered full
const int lowLevelThreshold = 10;   // Distance at which the tank is considered empty

void setup() {
  Serial.begin(9600);

  // Initialize the LCD
  lcd.init();             // Initialize the LCD
  lcd.backlight();       // Turn on the backlight

  // Set up the pins
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(relayPin, OUTPUT);

  digitalWrite(relayPin, LOW);

  // Display initial message
  lcd.setCursor(0, 0);
  lcd.print("Water Tank Control");
  lcd.setCursor(0, 1);
  lcd.print("Initializing...");
  delay(2000);
}

void loop() {
  long duration, distance;

  // Measure the distance using the ultrasonic sensor
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  duration = pulseIn(echoPin, HIGH);
  distance = (duration / 2) * 0.0344;

  // Print the distance to the Serial Monitor
  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.println(" cm");

  // Update the LCD display
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Distance: ");
  lcd.print(distance);
  lcd.print(" cm");

  // Control the relay based on the distance
  if (distance > highLevelThreshold) {
    // Turn off the pump if the tank is full
    digitalWrite(relayPin, LOW);
    lcd.setCursor(0, 1);
    lcd.print("Pump: ON ");
    Serial.println("Tank is full. Pump ON.");
  } 
  else if (distance < lowLevelThreshold) {
    // Turn on the pump if the tank is empty
    digitalWrite(relayPin, HIGH);
    lcd.setCursor(0, 1);
    lcd.print("Pump: OFF  ");
    Serial.println("Tank is empty. Pump OFF.");
  } 
  else {
    // Do nothing if the water level is within the range
    digitalWrite(relayPin, LOW);
    lcd.setCursor(0, 1);
    lcd.print("Pump: OFF ");
    Serial.println("Tank is at a normal level.");
  }

  // Wait before taking the next measurement
  delay(1000);
}
