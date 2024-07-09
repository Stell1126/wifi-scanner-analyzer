# wifi-scanner-analyzer

This project utilizes the M5StickC Plus microcontroller to create a compact and efficient WiFi Scanner Analyzer. The device scans for available WiFi networks, providing detailed information such as SSID, signal strength, channel, and security type.

#include <M5Unified.h>
#include <WiFi.h>

void setup() {
  auto cfg = M5.config();
  M5.begin(cfg);

  M5.Lcd.setRotation(3);  // Adjust screen orientation if needed
  M5.Lcd.fillScreen(BLACK);
  M5.Lcd.setTextSize(1);
  M5.Lcd.setTextColor(WHITE, BLACK);

  WiFi.mode(WIFI_STA);
  WiFi.disconnect();
  delay(100);
}

void loop() {
  M5.Lcd.setCursor(0, 0);
  M5.Lcd.println("Scanning Wi-Fi networks...");
  
  int n = WiFi.scanNetworks();
  M5.Lcd.fillScreen(BLACK);
  M5.Lcd.setCursor(0, 0);
  
  if (n == 0) {
    M5.Lcd.println("No networks found");
  } else {
    M5.Lcd.printf("%d networks found\n", n);
    for (int i = 0; i < n && i < 8; ++i) {
      String ssid = WiFi.SSID(i);
      int rssi = WiFi.RSSI(i);
      String security = (WiFi.encryptionType(i) == WIFI_AUTH_OPEN) ? "Open" : "Secured";
      
      M5.Lcd.printf("%d: %s\n", i + 1, ssid.c_str());
      M5.Lcd.printf("   Strength: %d dBm\n", rssi);
      M5.Lcd.printf("   Security: %s\n\n", security.c_str());
    }
  }
  
  delay(10000);  
}








