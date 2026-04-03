
# arc-reactor-wallpaper

# ⚛️ Arc Reactor Live Wallpaper

A highly detailed, interactive, 3D live wallpaper inspired by the Iron Man Mark-II Arc Reactor. Built using standard web technologies and the **Three.js** library, it features a spinning 3D reactor, dynamic lighting, particle effects, a cyberpunk-style HUD, a live digital clock, and real-world geolocation tracking.

## ✨ Features

*   **3D Rendered Core:** Built with Three.js, featuring segmented glowing rings, orbit rings, a downward triangle core, and an outer bezel.
*   **Dynamic Particles:** Includes floating orbital dust and fast-moving core sparks using WebGL particle systems.
*   **Interactive Parallax:** The 3D camera subtly shifts based on your mouse movements.
*   **Sci-Fi HUD:** An overlay featuring glowing text, animated scanning bars, and tech-inspired brackets.
*   **Live Clock:** Real-time digital clock with blinking colons, seconds, AM/PM, day, and date.
*   **Live Geolocation:** Automatically detects your coordinates and uses reverse-geocoding to display your current City and Country.

## 🚀 How to Run

Because this project is contained entirely within a single file, setup is incredibly simple:

1. Clone or download this repository.
2. Ensure all the code is saved in an `index.html` file.
3. Simply double-click the `index.html` file to open it in any modern web browser (Chrome, Firefox, Edge, Safari).
4. *Note: Your browser will prompt you for Location permissions to display your city on the HUD. If denied, it will simply display "LOCATION DENIED" or "EARTH".*

---

## 🧠 How the Code Works (Code Explanation)

This project consists of three main layers combined into one file: **HTML Structure**, **CSS Styling (HUD)**, and **JavaScript (Three.js 3D logic + APIs)**.

### 1. The HTML Structure
The HTML is very minimal. It only contains:
*   A `<canvas id="c">` element: This is the blank canvas where Three.js will draw the 3D graphics.
*   A `<div id="hud">` wrapper: This contains the 2D overlay elements like the Stark Industries text, the corner brackets, and the clock panel.

### 2. The CSS (The HUD & Styling)
The CSS is responsible for the 2D overlay that sits *on top* of the 3D canvas.
*   **Absolute Positioning:** Elements like the clock (`#clock-panel`) and text corners (`.hud-corner`) are pinned to the edges of the screen using `position: absolute` or `fixed`.
*   **Glow Effects:** The `text-shadow` property is heavily used on the clock to give it a multi-layered neon cyan glow.
*   **Animations:** `keyframes` are used to create the blinking colons (`colonBlink`) in the clock, the fading in of the HUD, and the scanning status bar (`#sbar`) that moves across the bottom of the screen.
*   **Pointer Events:** `pointer-events: none;` is applied to the HUD so that your mouse can interact with the 3D canvas *underneath* the text.

### 3. JavaScript & Three.js (The 3D Engine)
This is where the heavy lifting happens. The script is broken down into several key components:

#### A. Scene Setup & Lighting
*   **Renderer & Camera:** Initializes the WebGL canvas, sets up a `PerspectiveCamera`, and enables soft shadows and filmic tone mapping for realistic lighting.
*   **Lights:** Uses an `AmbientLight` for base visibility, a central `PointLight` to make the core actually emit light, a `DirectionalLight` for shape definition, and `SpotLights` for dramatic cinematic highlights.

#### B. The Reactor Geometry (The Group)
The reactor is built by assembling basic mathematical 3D shapes (primitives) and applying `MeshStandardMaterial` to them (either dark metals or glowing emissive colors).
*   **Outer & Inner Bezels:** Created using `TorusGeometry` (donut shapes).
*   **Bolts:** Created using `CylinderGeometry` and arranged in a circle using `Math.sin` and `Math.cos`.
*   **Glow Segments:** 12 individual glowing boxes arranged in a circle, with backing plates.
*   **Downward Triangle:** Custom calculated vectors (`Vector2`) to create a glowing triangle mesh inside the reactor.
*   **Orbit Rings & Core:** Rings tilted at angles (`rotation.x`) and a central `SphereGeometry`.

#### C. Particle Systems
Instead of creating individual 3D objects for thousands of particles (which would lag your computer), the code uses a `BufferGeometry` and `PointsMaterial`.
*   **Orbital Dust:** Generates 1,800 points of dust floating around the outside.
*   **Sparks:** Generates 260 points of light flying out of the center core with velocity (`vx, vy, vz`) and a "life" cycle so they constantly respawn.

#### D. The Animation Loop (`animate()`)
The `requestAnimationFrame` creates an infinite loop running at 60 FPS.
*   **`t += 0.016`**: A time variable advances every frame. This `t` value is plugged into `Math.sin()` functions to create smooth, oscillating animations (like the "breathing" core or pulsating lights).
*   **Rotations:** Every group (`reactor`, `segGroup`, `notchGroup`) has its `.rotation.z` continuously updated based on time, spinning them at different speeds and directions.
*   **Mouse Lerp:** Calculates the distance between the mouse and the camera, slowly moving the camera towards the mouse for a smooth parallax effect.

#### E. APIs (Clock & Location)
*   **`tickClock()`:** Uses JavaScript's native `Date()` object to grab the current hour, minute, second, day, and month, formats them, and injects them into the HTML elements every 1,000 milliseconds (1 second).
*   **`fetchLocation()`:** Uses the browser's `navigator.geolocation` API to get your Latitude and Longitude. It then makes a `fetch()` request to the free **OpenStreetMap/Nominatim API** to convert those coordinates into a readable City and Country name.

---

## 🛠️ Customization Guide
Want to tweak the reactor? Here are some easy values to change in the JavaScript section:
*   **Colors:** Find `const C_BLUE = new THREE.Color(0x00ccff);` and change the hex code to change the primary glow color (e.g., `0xff0000` for red/Sith mode).
*   **Speed:** Look inside the `animate()` function. Change multipliers like `reactor.rotation.z = t * 0.18;` to `t * 0.50` to make it spin much faster.
*   **Particle Count:** Change `const PDUST=1800;` or `const SPARKS=260;` (Be careful, setting this too high might lag older devices).
