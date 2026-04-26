# 🎮 F/A-18 COCKPIT SIMULATOR - NEJ-10

A fully interactive, browser-based fighter jet cockpit simulator inspired by modern military aircraft like the F/A-18 Super Hornet.

**Live Demo**: Open `index.html` in your browser

---

## 🎯 Features Overview

### ✈️ **Realistic Cockpit Layout**
- **HUD Overlay** (Top): Speed, Altitude, Heading, G-Force with threat warnings
- **Center Displays**: 
  - Left MFD: Radar system with SEARCH/TRACK/LOCK modes
  - Right MFD: Engine systems, weapons status, navigation
- **Left Panel**: Engine controls, throttle slider with visual feedback
- **Right Panel**: Weapons system, countermeasures, RWR display
- **Bottom Panel**: Real-time warning indicators

### 🔥 **Engine Start Sequence** (Realistic)
1. Battery → ON
2. APU → START (2 seconds)
3. Engine Crank → START (3 seconds)
4. Engine Idle Stabilizes (3 seconds)
5. Avionics → ONLINE (2 seconds)

Each phase enables the next, with visual and audio feedback.

### 📡 **Radar System**
- **Three Modes**: SEARCH (slow scan), TRACK (multiple targets), LOCK (single target)
- **Dynamic Targets**: Simulated enemy and friendly aircraft with realistic movement
- **Range Control**: 10-200 NM adjustable
- **Threat Levels**: High (red), Medium (orange), Low (green)
- **Target Information**: Altitude, speed, heading display

### 🔊 **Advanced Audio System** (Web Audio API)
- **RWR Sounds**:
  - SEARCH: Slow periodic beep (0.8s on/off)
  - LOCK: "BeBO" pattern at 400Hz (critical threat)
  - INCOMING: Fast urgent beeping (300ms on/off at 350Hz)
- **Engine Audio**: Dynamic based on throttle percentage
- **Afterburner**: Additional high-frequency content above 85% throttle
- **Priority System**: Critical warnings override lower-priority sounds

### 🚨 **Warning System**
- **STALL**: Speed too low with throttle < 50%
- **PULL UP**: Altitude < 2000 ft with negative vertical speed
- **OVER-G**: G-force exceeds 95% of limit (9G max)
- **ENGINE FAILURE**: Simulated engine issues at high throttle
- **EJECT**: Critical emergency

Visual warnings flash in HUD and trigger distinctive audio patterns.

### 🎯 **Weapons System**
- **Master Arm**: SAFE/ARM toggle
- **Weapon Select**: GUN (500 rounds), AIM-9 Sidewinder (2x), AIM-120 AMRAAM (4x)
- **Target Lock Requirement**: Missiles need locked target before firing
- **Real-Time Ammo Display**: Visual indication of remaining weapons

### 🛡️ **Countermeasures**
- **Chaff**: 120 rounds (decoy for radar)
- **Flare**: 60 rounds (decoy for infrared)
- **Manual/Auto Mode**: Deploy manually or automatic on threats
- **Realistic Depletion**: Limited quantities with visual counters

### 🧠 **Avionics & Navigation**
- **Autopilot**: Altitude hold, heading hold modes
- **Navigation Display**: Real-time heading, altitude, speed, distance
- **Waypoint System**: Navigation to pre-set waypoints
- **System Status**: Continuous monitoring of all aircraft systems

### 🛩️ **Flight Dynamics** (Simplified Physics)
- **Speed Control**: Throttle affects acceleration (max 1190 knots)
- **Altitude**: Controlled by pitch angle and throttle
- **G-Force Calculation**: Real-time calculation from pitch/roll
- **Vertical Speed**: Tracked for climb/descent rate
- **Automatic Warnings**: Triggers on dangerous flight conditions

### 🎨 **Visual Style**
- Dark cockpit theme with realistic panel layout
- Neon green (#0f0) and amber (#fa0) text
- Glow effects for active indicators
- Realistic gauge visualization
- Professional monospace fonts

---

## 🎮 How to Use

### Initial Startup
1. **Open** `index.html` in a modern web browser
2. **Click BATTERY** button to enable power
3. **Click APU** button (requires battery)
4. **Click ENGINE CRANK** to start startup sequence
5. Watch the sequence complete automatically
6. Engines will reach idle RPM when ready

### Basic Flight
- **Throttle Slider** (left panel): Adjust engine power (0-100%)
- **Watch Gauges**:
  - RPM gauge shows engine speed
  - Temperature gauges show engine heat
  - Fuel gauge shows remaining fuel
- **Monitor HUD** (top): Speed, altitude, heading, G-force

### Weapons Engagement
1. **Master Arm**: Switch from SAFE to ARM
2. **Radar Mode**: Cycle through SEARCH → TRACK → LOCK
3. **Lock Target**: Click a target on radar display
4. **Select Weapon**: Choose GUN, Sidewinder, or AMRAAM
5. **Fire**: Click FIRE button (requires lock for missiles)

### Defensive Measures
1. **Monitor RWR** (right panel): Watch for incoming threats
2. **Deploy Chaff/Flare**: As needed against threats
3. **Set CM Mode**: Manual or Auto deployment
4. **Perform Maneuvers**: Use pitch/roll to evade

### Keyboard Controls
```
FLIGHT CONTROL:
  W/S       - Pitch Up/Down
  A/D       - Roll Left/Right  
  Q/E       - Turn Left/Right
  ↑/↓       - Throttle +/-

QUICK ACTIONS:
  B         - Battery toggle
  M         - Master Arm
  F         - Fire
  C         - Chaff
  L         - Flare
  R         - Radar Mode Cycle
  ESC       - Emergency Eject
```

---

## 🔧 Technical Architecture

### JavaScript Modules
1. **audio-engine.js** - Web Audio API integration, priority-based sound system
2. **flight-model.js** - Physics simulation, flight dynamics, warning logic
3. **avionics.js** - Radar, RWR, weapons, navigation systems
4. **systems.js** - Engine startup, power management, thermal simulation
5. **ui-controller.js** - Button handlers, display updates, user feedback
6. **graphics.js** - Radar and RWR rendering using Canvas API
7. **main.js** - Main simulation loop, system orchestration

### HTML/CSS
- **Semantic HTML5** structure for accessibility
- **CSS Grid & Flexbox** for responsive layout
- **Custom properties** for consistent theming
- **Canvas elements** for dynamic graphics

### Audio Priority System
```
Priority Order (highest to lowest):
5. EJECT/CRITICAL     - Emergency alert
4. INCOMING           - Missile warning
3. LOCK               - Target lock warning
2. SEARCH             - Radar scan
1. ENGINE             - Background engine sound
```

---

## 🎯 Game States & Flow

```
INITIALIZATION
    ↓
POWER OFF (all systems disabled)
    ↓
BATTERY ON
    ├─ APU available
    ├─ Avionics unavailable
    └─ Engines unavailable
    ↓
APU START + ENGINE CRANK
    ├─ Engines cranking (3 sec)
    ├─ Reaching idle (3 sec)
    └─ Temperature rising
    ↓
ENGINES RUNNING
    ├─ Throttle now controllable
    ├─ Flight possible
    ├─ Avionics coming online
    └─ Flight model active
    ↓
FLIGHT OPERATIONS
    ├─ Radar active (if avionics on)
    ├─ RWR monitoring threats
    ├─ Weapons armed (if master arm on)
    ├─ Countermeasures available
    └─ Real-time system monitoring
    ↓
COMBAT/EMERGENCY
    ├─ Threat warnings activate
    ├─ Evasive maneuvers required
    ├─ Countermeasures deployed
    └─ Weapons engaged
    ↓
EJECT/SHUTDOWN
```

---

## 🎓 Learning Objectives

This simulator teaches:
- **Aviation Concepts**: Engine startup, flight dynamics, G-forces
- **Avionics Operation**: Radar modes, weapon systems, navigation
- **Real-Time System Management**: Multiple gauges, priorities, warnings
- **Audio Engineering**: Web Audio API, spatial audio, priority mixing
- **Game Architecture**: Modular systems, state management, main loop

---

## 🚀 Expansion Ideas

### Short-term
- [ ] ILS (Instrument Landing System) display
- [ ] Fuel transfer management
- [ ] Landing gear simulation
- [ ] Formation flying with AI wingmen
- [ ] Weather effects (turbulence, icing)

### Medium-term
- [ ] 3D cockpit view using Three.js
- [ ] Multiplayer combat (WebSocket)
- [ ] Campaign missions
- [ ] Damage modeling
- [ ] Realistic aerodynamic effects

### Long-term
- [ ] Full 6-DOF flight model
- [ ] Complex engine simulation
- [ ] Electronic warfare simulation
- [ ] VR/Motion controller support
- [ ] Voice commands (Web Speech API)

---

## 🐛 Debugging

Access debug objects in browser console:
```javascript
window.DEBUG.flight          // FlightModel state
window.DEBUG.systems         // SystemsManager state
window.DEBUG.avionics        // AvionicsSystem state
window.DEBUG.audio           // AudioEngine controls
window.DEBUG.ui              // UIController state
```

Check browser console for detailed logs:
```
[SIMULATOR] Starting F/A-18 Cockpit Simulator
[STARTUP] APU started
[STARTUP] Engine cranking
[STARTUP] Engines reaching idle
[STARTUP] Avionics online
[STARTUP] Startup sequence complete!
```

---

## 📋 Browser Requirements

- **Chrome/Edge**: Full support (recommended)
- **Firefox**: Full support
- **Safari**: Full support (iOS 14+)
- **IE**: Not supported

Required APIs:
- Web Audio API (AudioContext)
- Canvas 2D API
- ES6 JavaScript
- CSS Grid & Flexbox

---

## 🎵 Audio Notes

The audio engine uses:
- **Oscillators**: Sine waves for tones (600Hz search, 400Hz lock, 350Hz incoming)
- **Gain nodes**: For volume control and priority mixing
- **Filters**: Low-pass for engine, high-pass for afterburner
- **Envelopes**: ADSR-style amplitude shaping
- **No external audio files**: All generated procedurally

---

## 📝 License

This project is provided as-is for educational purposes.

---

## 🙏 Credits

Inspired by real F/A-18 Super Hornet cockpit designs and flight simulator software. Built with vanilla JavaScript, HTML5, and Web Audio API.

---

## 📞 Support

For issues, questions, or suggestions:
1. Check the console for error messages
2. Verify browser compatibility
3. Clear browser cache and reload
4. Check GitHub issues/discussions

Enjoy your flight! ✈️🚀

