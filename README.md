🏠 ESP32 Smart Home — Offline Wi-Fi Dashboard

A local smart home monitoring and control system built with an ESP32 Dev Module and the Arduino framework. The ESP32 creates its own Wi-Fi network and hosts a responsive web dashboard, so no router, internet connection, cloud account, or external web assets are required.

Project status: Source code is provided for Arduino IDE. Hardware operation and compilation on your specific board should be verified before connecting any loads.

✨ Features

🌡️ Live temperature readings in °C from a DHT11 sensor

💧 Live humidity readings from a DHT11 sensor

🔥 Digital flame detection with a prominent on-screen warning (active LOW)

💡 Independent ON/OFF controls for living room and bedroom LEDs

⚡ Independent relay ON/OFF control, with configurable active-LOW/active-HIGH polarity

📱 Mobile-friendly dark dashboard with animated indicators

🔄 Automatic status refresh approximately every 2 seconds, without reloading the page

📶 Connection status indicator and error handling

📴 Fully self-hosted HTML, CSS, and vanilla JavaScript, embedded in one .ino file

🧰 Hardware

ESP32 Dev Module

DHT11 temperature and humidity sensor

Digital flame sensor module (digital output)

2 LEDs with suitable series resistors

Relay module with a suitable driver and supply

Jumper wires and a suitable power supply

🔌 Wiring

Component

ESP32 pin

Notes

DHT11 DATA

GPIO 4

Use a suitable pull-up if your sensor/module requires one.

Flame Sensor DO

GPIO 23

LOW = flame detected; digital output must be 3.3 V-safe.

Living Room LED

GPIO 18

Use a current-limiting resistor.

Bedroom LED

GPIO 17

Use a current-limiting resistor.

Relay Module IN

GPIO 2

Boot-strapping pin: read safety notes below.

Connect grounds appropriately and power each module according to its specifications. Do not feed a 5 V output directly into an ESP32 GPIO.

🛠️ Software requirements

Install Arduino IDE.

Install ESP32 by Espressif Systems using Arduino IDE's Boards Manager.

Install DHT11 by Dhruba Saha (developed against version 2.1.0) through Library Manager.

The sketch uses WiFi.h and WebServer.h supplied with the Arduino ESP32 core.

This version of the project uses #include <DHT11.h> (Dhruba Saha's library), not Adafruit's DHT.h API.

🚀 Upload and run

Open Smart_Home_AP_Mode_DHT11.ino in Arduino IDE.

Select Tools → Board → ESP32 Dev Module.

Connect your ESP32 using a data-capable USB cable and choose the correct port under Tools → Port.

Click Verify, then Upload.

Open Serial Monitor at 115200 baud.

On a phone or laptop, connect to the Wi-Fi network below:

Setting

Value

Network name (SSID)

Smart_Home

Password

12345678

Dashboard address

http://192.168.4.1

Open http://192.168.4.1 in your browser to use the dashboard.

Your device may report “No internet” on this Wi-Fi network; that is expected. Stay connected to Smart_Home. If the page does not open, make sure mobile data or automatic Wi-Fi switching isn't taking over, and use http:// rather than https://.

🖥️ Dashboard

The web interface includes temperature and humidity cards, a fire-safety alert, individual light controls, and a Power Control switch for the relay. It is served directly from the ESP32, not from GitHub Pages.

<!-- Optional: add an actual project screenshot to the repository and link it here. -->

🔗 Local API

Method

Route

Purpose

GET

/

Display the dashboard

GET

/api/status

Read sensor values and current commanded output states

POST

/api/light1

Set living room LED state

POST

/api/light2

Set bedroom LED state

POST

/api/relay

Set relay state

Control requests accept state=on or state=off as form data and respond with JSON. Example status response (illustrative values only):

{
  "temperature": 25.0,
  "humidity": 60.0,
  "flameDetected": false,
  "light1": false,
  "light2": false,
  "relay": false
}

Invalid sensor readings are represented as unavailable in the dashboard. Light and relay status represents software-commanded states, not independent electrical feedback.

🧪 Safe testing

DHT11: Check that temperature and humidity display and update over time.

Flame input: Test sensor behavior using the manufacturer's safe procedure; verify the warning appears when the digital input becomes LOW. Do not bring an open flame near electronics.

LEDs: Toggle each light separately and observe the corresponding LED.

Relay: Test with no connected electrical load first. Confirm the default OFF state and both commanded states.

⚠️ Electrical and safety notes

GPIO 2 is a boot-strapping pin. The relay module's input circuit may interfere with startup or create an unwanted pulse at boot/reset. Software cannot guarantee a glitch-free signal before initialization. GPIO 25 or 26 may be safer alternatives if you choose to redesign the wiring and update the sketch; this project intentionally retains GPIO 2.

The sketch defaults to static const bool RELAY_ACTIVE_LOW = true;. If the relay module is active HIGH, change it to false and upload again. Confirm polarity without a connected load.

Never drive a relay coil directly from an ESP32 pin; use an appropriate relay module/driver and power supply.

Do not connect mains voltage or high-energy equipment without qualified electrical design and supervision.

A hobby flame sensor and this dashboard are not a certified fire alarm and must not be used as a sole life-safety system. Flame detection does not automatically switch the relay.

The default Wi-Fi password is for local prototyping; change it before use in an untrusted environment.

🧩 Troubleshooting

Problem

Check

DHT does not name a type

Open the DHT11 version of the sketch and install DHT11 by Dhruba Saha.

Sketch won't upload

Check board, USB data cable, port, drivers, and boot mode. Disconnect problematic external circuitry while troubleshooting.

Smart_Home Wi-Fi not visible

Check power and Serial Monitor output at 115200 baud.

Dashboard won't open

Connect to Smart_Home, then enter http://192.168.4.1; disable automatic network switching if needed.

Relay behaves backward

Check RELAY_ACTIVE_LOW, without connecting a load.

Sensor shows --

Check wiring, supply voltage, and sensor connections.

📁 Project files

ESP32-Smart-Home/
├── Smart_Home_AP_Mode_DHT11.ino   # Firmware + embedded web dashboard
└── README.md                     # Project documentation

📄 License

No license has been specified. Add a LICENSE file if you want to define reuse terms.
