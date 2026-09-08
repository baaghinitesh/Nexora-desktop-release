# Interactive Pen Tablet & Live Screen Annotation

[← Back to Main Documentation](../README.md)

Nexora transforms your touchscreen smartphone into a wireless drawing tablet, mirroring annotations live onto a transparent fullscreen **Windows Ghost Window overlay** across your entire desktop workspace.

---

## Ghost Window Live Annotation Architecture

```
+--------------------------------------------------+     +--------------------------------------------+
|           MOBILE COMPANION (Android)             |     |        WINDOWS DESKTOP HOST                |
|                                                  |     |                                            |
|  Touchscreen Canvas / Stylus Touch               |     |  Desktop Protocol Dispatcher               |
|           |                                      |     |           |                                |
|           v                                      |     |           v                                |
|  Stroke Vector & Pressure Normalizer             |     |  Transparent Ghost Window Overlay          |
|           |                                      |     |           |                                |
|           | WebSocket Stream :8080               |     |           v                                |
|           +------------------------------------ >+-----+>  HTML5 Canvas — bezierCurveTo()           |
|                                                  |     |  Hardware-Accelerated Screen Render        |
+--------------------------------------------------+     +--------------------------------------------+
                                                                      |
                                                                      | Export Command
                                                                      v
                                                         +--------------------------------------------+
                                                         |         PC DOCUMENT EXPORT                 |
                                                         |                                            |
                                                         |  PDFKit / Canvas PNG Encoder               |
                                                         |           |                                |
                                                         |           v                                |
                                                         |  C:\Users\...\Downloads\Nexora Drawings    |
                                                         +--------------------------------------------+
```

**Data flow summary**

```
Phone Touchscreen
       |
       |  touch delta + pressure value
       v
Stroke Vector Normalizer
       |
       |  WebSocket payload  -->  ws://[PC-IP]:8080
       v
Desktop Protocol Dispatcher
       |
       +---> Ghost Window Overlay  (transparent, always-on-top, fullscreen)
       |           |
       |           v
       |     HTML5 Canvas renders stroke in real time
       |
       +---> Export Pipeline (on request)
                   |
                   v
             PNG / PDF  -->  Downloads\Nexora Drawings\
```

---

## Key Features & How They Work

### 1. Transparent Ghost Window Overlay

Nexora creates a borderless, always-on-top, fully transparent desktop window that covers your entire monitor. Your drawing strokes appear on top of whatever is currently on screen — without closing, minimising, or switching any open application.

**What you can annotate over:**
- PowerPoint and Google Slides presentations
- PDF documents and blueprints
- Code in VS Code, IntelliJ, or any editor
- Browser windows and dashboards
- Live screen shares on Zoom, Microsoft Teams, or Google Meet

### 2. Stylus & Touch Pressure Engine

Stroke thickness adjusts in real time based on hardware stylus pressure levels and touch finger velocity.

**Available tools:**

| Tool | Behaviour |
| :--- | :--- |
| **Pen** | Solid vector stroke with sharp precision edges |
| **Highlighter** | Semi-transparent stroke that enhances text without obscuring it |
| **Eraser** | Path-based precision erasing — removes only what you target |
| **Undo / Redo** | Unlimited stroke-level undo and redo stack |

### 3. Multi-Color Palette & Canvas Grids

- **Color palette** — rapid-access primary and neon colors with custom opacity slider
- **Mobile canvas grids** — switch between Blank, Lined Notebook, and Isometric Grid backgrounds on the phone screen to guide sketches, while the desktop overlay remains clean

### 4. Direct PC Document Export

Tap **Export** on the mobile screen to instantly save your annotations:

```
Mobile [Export tap]
        |
        v
Desktop encodes canvas
        |
        +---> High-Resolution PNG  -->  C:\Users\<User>\Downloads\Nexora Drawings\annotation_<timestamp>.png
        |
        +---> Multi-Page Vector PDF  -->  C:\Users\<User>\Downloads\Nexora Drawings\annotation_<timestamp>.pdf
```

Files are saved automatically with timestamp metadata — no manual file naming required.

---

[← Back to Main Documentation](../README.md)
