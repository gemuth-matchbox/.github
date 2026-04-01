<div align="center">
  <i>A zero-state, ultra-compact hardware ecosystem built in Go, TinyGo and Kotlin.</i>
  <br><br>
  
  [![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
  [![Tech Stack](https://img.shields.io/badge/Tech-Go_%7C_TinyGo_%7C_Kotlin-black.svg)]()
  [![Architecture](https://img.shields.io/badge/Architecture-Thin_Client-success.svg)]()
</div>

---

## 💡 The Vision
**Matchbox** is an open-source engineering project demonstrating how to build a highly responsive, pocket-sized hardware gadget using a **Thin Client** architecture. 

The physical device is dumb by design (Zero-State). It acts solely as a physical interface, while the entire logic, state management, and data persistence are handled by a robust Go backend. 

---

## 🕯️ The Architecture (The Matchbox Metaphor)

The system is split into four distinct repositories. The architecture follows the physical lifecycle of lighting a match:

### 1. [🔥 `spark-firmware`](https://github.com/gemuth-matchbox/spark-firmware) (The Match Head)
The physical origin of the event. Written in **TinyGo** and running on an ultra-small ESP32-C3 chip, the *Spark* reads hardware interrupts (button presses). It drives the I2C OLED display and handles the dual-mode network stack (WLAN or BLE).

### 2. [🪢 `tether-app`](https://github.com/gemuth-matchbox/tether-app) (The Bridge - *Optional*)
The connection that holds it together. A native Android background service written in **Kotlin**. **This component is only required when the hardware operates in BLE mode.** It acts as an invisible BLE-to-HTTPS proxy, allowing the *Spark* to communicate with the internet while keeping hardware battery consumption near zero. In WLAN mode, this app is completely bypassed.

### 3. [⚡ `striker-backend`](https://github.com/gemuth-matchbox/striker-backend) (The Striking Surface)
Where the friction happens. A high-performance server written in **Go**. The *Striker* catches the raw events from the *Spark*, processes the business logic against a SQLite database, and returns the calculated UI state (the "Flame").

### 4. [📦 `box-hardware`](https://github.com/gemuth-matchbox/box-hardware) (The Shell)
The physical container. This repository contains all CAD files (.step/.stl), circuit diagrams, and assembly instructions for the SLS/SLA printed Matchbox enclosure.

---

## 🔄 Data Flow: How a Match is Struck

The data flow adapts dynamically based on the active connection mode selected on the hardware:

1. **User presses a button** on the `box-hardware`.
2. `spark-firmware` registers the interrupt.
3. **The Signal Routing:**
   * **🌐 Direct Mode (WLAN):** The `spark` sends an HTTPS request *directly* to the `striker-backend` via the smartphone's WiFi hotspot.
   * **🔋 Tethered Mode (BLE):** The `spark` broadcasts a low-energy BLE payload. The `tether-app` (running in the background on the user's phone) catches it and forwards it via HTTPS to the backend.
4. `striker-backend` receives the request, updates the database, and responds with the new display state (JSON).
5. The `spark` parses the JSON and updates the OLED screen instantly.

*(Note: The system heavily utilizes **Optimistic UI** updates on the hardware side to mask network latency, ensuring a premium, responsive feel regardless of the connection mode).*

---

## 🛠️ Get Involved
We welcome contributions across the entire stack! Whether you're a Go backend wizard, a TinyGo embedded enthusiast, or a 3D-printing hardware hacker, there's a place for you here.

Check out the individual repositories above to get started with the specific components.

<div align="center">
  Created by <a href="https://gemuth.com">Gemuth</a><br>
  Contact: <a href="mailto:wohlgemuth.dev@gmail.com">wohlgemuth.dev@gmail.com</a>
</div>
