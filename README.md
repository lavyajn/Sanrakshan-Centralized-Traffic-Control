# 🚆 Sanrakshan CTC: Real-Time Railway Interlocking Engine

Sanrakshan (Centralized Traffic Control) is a high-performance, multithreaded railway traffic control simulator. Built with a C++ backend and a dynamic Vanilla JavaScript frontend, it simulates complex railway networks, autonomous train routing, and advanced safety interlocking systems via real-time WebSockets.

---

## ✨ Key Features

- **TCAS / Kavach (Anti-Collision System):** Prevents catastrophic train collisions using OS-level Non-Blocking Mutual Exclusion (`try_lock`). Trains autonomously yield execution time and halt if a track segment is occupied.
- **Wildlife TSR (Temporary Speed Restrictions):** Dispatchers can flag tracks with wildlife hazards (🐘). The C++ physics engine dynamically throttles train speed, and the UI uses raw geometric pixel-snatching to perfectly freeze the animation mid-track.
- **Zero-Latency Telemetry HUD:** Click on any moving train to view real-time WebSocket telemetry, including exact track location, speed parameters, and safety statuses.
- **Global E-Stop:** An instantaneous system-wide halt utilizing C++ `std::atomic` flags to safely pause all active multithreaded train engines without introducing race conditions.
- **Deadlock Demonstration Mode:** Includes a purpose-built feature to visually demonstrate the **Dining Philosophers Problem** and **Circular Wait** conditions by intentionally forcing a system deadlock on a triangular route.

---

## 🛠️ Tech Stack

### Backend
- C++ (Standard 17)
- Multithreading (`std::thread`, `std::mutex`, `std::unique_lock`, `std::atomic`)
- [Crow](https://crowcpp.org/) — C++ Microframework for HTTP & WebSockets
- JSON (`nlohmann/json`)

### Frontend
- Vanilla JavaScript (ES6)
- HTML5 / CSS3 (CSS Variables, Keyframe Animations, Dynamic DOM Manipulation)
- WebSocket API for bi-directional data streaming

---

## 📋 Prerequisites

Ensure your system has the following installed before compiling:

- A C++17 compatible compiler (GCC, Clang, or MSVC)
- CMake (Version 3.15 or higher)
- Make (Linux/macOS)
- A modern web browser (Chrome, Edge, Firefox)

---

## 🚀 Installation & Setup

### 1. Clone the repository
```bash
git clone https://github.com/yourusername/sanrakshan-ctc.git
cd sanrakshan-ctc
```

### 2. Build the C++ Backend Server
Navigate to the backend build directory, generate the makefiles, and compile the engine:
```bash
cd backend/build
cmake ..
make
```

### 3. Run the Server
Start the simulation engine and WebSocket broadcaster:
```bash
./sanrakshan_server
```
> The server will typically run on `localhost:8080`. Check your terminal output for confirmation.

### 4. Launch the SCADA Frontend
Navigate to the `frontend` folder and open `index.html` directly in your web browser.

> Because it uses standard WebSockets to communicate with `localhost`, no separate web server (Nginx/Apache) is required for the frontend.

---

## 🎮 How to Use & Demo

| Action | How To |
|--------|--------|
| **Auto-Build Yard** | Click `[ AUTO-BUILD YARD ]` on the bottom left to instantly generate the default track topology |
| **Dispatch a Train** | Click any station (e.g., `KALYAN`) to open the Central Dispatch modal. Enter a destination (e.g., `CST`), select priority, and authorize launch |
| **Trigger Kavach** | Dispatch a Slow `Freight` train, then immediately dispatch a Fast `Express` train on the same route — the Express will autonomously brake and flash red |
| **Trigger Wildlife TSR** | Left-click any track segment to toggle the Neon Elephant 🐘. Trains will freeze until the hazard is cleared |
| **Sabotage Track** | Right-click any track to simulate a physical fault. The system will spark and incoming trains will attempt to reroute |

---

## 🧠 System Internals: OS Concepts Demonstrated

This project is built to demonstrate core Operating System and Concurrency concepts:

1. **Thread Independence** — Every dispatched train is spawned as an independent `std::thread`, calculating its own physics and pathing.
2. **Resource Allocation** — Track segments are shared resources protected by `std::mutex`.
3. **Deadlock Avoidance** — Trains utilize `std::unique_lock` with `std::defer_lock` and `try_lock()` to probe tracks. Instead of a blocking `lock()` that could cause a Circular Wait, threads safely yield execution if a resource is unavailable.
4. **Atomic Operations** — Global state variables (like Emergency Stop) use `std::atomic` to prevent data races across dozens of threads without the overhead of heavy locking mechanisms.

---

🤝 The Team

1.Aaryamaan Rai
2.Lavya Jain

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).
