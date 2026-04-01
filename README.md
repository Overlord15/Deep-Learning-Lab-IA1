# Deep Learning Lab IA1
```arduino
// ============================================================
//  Ultrasonic Sensor (3-pin: VCC, GND, SIG) + LED Blink
//  Single-pin sensor — TRIG and ECHO share the same SIG pin
// ============================================================

// --- PIN CONFIGURATION ---
#define SIG_PIN    9    // SIG → UNO Digital Pin 9 (change if needed)
#define LED_PIN    13   // LED → UNO Digital Pin 13

// --- DETECTION THRESHOLD ---
#define MAX_DISTANCE_CM  30  // Trigger if object closer than this (cm)

// --- BLINK TIMING (5 blinks/sec = 100ms ON + 100ms OFF) ---
#define BLINK_ON_TIME   100  // ms LED stays ON
#define BLINK_OFF_TIME  100  // ms LED stays OFF

void setup() {
  Serial.begin(9600);
  pinMode(LED_PIN, OUTPUT);
  digitalWrite(LED_PIN, LOW);
}

void loop() {
  // --- STEP 1: Send pulse via SIG pin (set as OUTPUT) ---
  pinMode(SIG_PIN, OUTPUT);
  digitalWrite(SIG_PIN, LOW);
  delayMicroseconds(2);
  digitalWrite(SIG_PIN, HIGH);
  delayMicroseconds(10);         // 10µs trigger pulse
  digitalWrite(SIG_PIN, LOW);

  // --- STEP 2: Listen for echo on same SIG pin (set as INPUT) ---
  pinMode(SIG_PIN, INPUT);
  long duration = pulseIn(SIG_PIN, HIGH, 30000);  // timeout in µs (change for range cap)

  // --- STEP 3: Convert to cm ---
  float distance_cm = (duration * 0.0343) / 2.0;

  // --- STEP 4: Debug (optional, remove if not needed) ---
  Serial.print("Distance: ");
  Serial.print(distance_cm);
  Serial.println(" cm");

  // --- STEP 5: Blink or idle ---
  if (duration > 0 && distance_cm <= MAX_DISTANCE_CM) {
    digitalWrite(LED_PIN, HIGH);
    delay(BLINK_ON_TIME);
    digitalWrite(LED_PIN, LOW);
    delay(BLINK_OFF_TIME);
  } else {
    digitalWrite(LED_PIN, LOW);
    delay(50);  // idle poll delay (change if needed)
  }
}
```
