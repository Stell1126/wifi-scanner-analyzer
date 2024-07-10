# wifi-scanner-analyzer

This project utilizes the M5StickC Plus microcontroller to create a compact and efficient WiFi Scanner Analyzer. The device scans for available WiFi networks, providing detailed information such as SSID, signal strength, channel, and security type.

#include <M5Unified.h>
#include <WiFi.h>

void setup() {
  // Initialize M5Stack device
  auto cfg = M5.config();
  M5.begin(cfg);

  // Set up the display
  M5.Lcd.setRotation(3);  // Adjust screen orientation for better viewing
  M5.Lcd.fillScreen(BLACK);
  M5.Lcd.setTextSize(1);
  M5.Lcd.setTextColor(WHITE, BLACK);

  // Initialize WiFi in station mode and disconnect from any previous networks
  WiFi.mode(WIFI_STA);
  WiFi.disconnect();
  delay(100);  // Short delay to ensure WiFi is fully initialized
}

void loop() {
  // Display scanning message
  M5.Lcd.setCursor(0, 0);
  M5.Lcd.println("Scanning Wi-Fi networks...");
  
  // Perform WiFi scan
  int n = WiFi.scanNetworks();
  
  // Clear screen and reset cursor for results
  M5.Lcd.fillScreen(BLACK);
  M5.Lcd.setCursor(0, 0);
  
  if (n == 0) {
    M5.Lcd.println("No networks found");
  } else {
    M5.Lcd.printf("%d networks found\n", n);
    
    // Display details of up to 8 networks
    for (int i = 0; i < n && i < 8; ++i) {
      String ssid = WiFi.SSID(i);
      int rssi = WiFi.RSSI(i);
      String security = (WiFi.encryptionType(i) == WIFI_AUTH_OPEN) ? "Open" : "Secured";
      
      // Print network details
      M5.Lcd.printf("%d: %s\n", i + 1, ssid.c_str());
      M5.Lcd.printf("   Strength: %d dBm\n", rssi);
      M5.Lcd.printf("   Security: %s\n\n", security.c_str());
    }
  }
  
  // Wait for 10 seconds before next scan
  delay(10000);
}








