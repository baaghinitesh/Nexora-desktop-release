# Interactive Pen Tablet & Live Screen Annotation

[← Back to Main Documentation](../README.md)

Nexora transforms your touchscreen smartphone into a wireless drawing tablet, mirroring annotations live onto a transparent fullscreen **Windows Ghost Window overlay** across your entire desktop workspace.

---

## 🎨 Ghost Window Live Annotation Architecture

```mermaid
flowchart TD
    subgraph MobileDevice ["📱 Mobile Companion"]
        A["Touchscreen Canvas / Stylus Touch"] --> B["Stroke Vector & Pressure Normalizer"]
        B -->|WebSocket Stream :8080| C["WebSocket Client"]
    end

    subgraph DesktopHost ["💻 Windows Desktop Host"]
        C --> D["Desktop Protocol Dispatcher"]
        D -->|Electron IPC: 'draw-stroke'| E["Transparent Ghost Window Overlay"]
        E -->|HTML5 Canvas 2D ctx.bezierCurveTo()| F["Real-Time Hardware Accelerated Screen Render"]
    end

    subgraph ExportPipeline ["📁 PC Document Export"]
        D -->|Export Command| G["PDFKit / Canvas PNG Encoder"]
        G --> H["C:\\Users\\...\\Downloads\\Nexora Drawings"]
    end
```

---

## 🌟 Key Features & Workings

### 1. Transparent Ghost Window Overlay
- **Non-Obtrusive Live Mirroring**: Nexora creates a borderless, always-on-top, transparent desktop window covering your entire monitor.
- **Draw on Top of Any App**: Annotate PowerPoint presentations, mark up PDF blueprints, highlight code in VS Code, or explain diagrams during Microsoft Teams, Zoom, or Google Meet screen shares.

### 2. Stylus & Touch Pressure Engine
- **Pressure Simulation**: Stroke thickness modulates based on stylus hardware pressure levels and touch finger velocity.
- **Tools & Brushes**:
  - **Pen**: Solid vector drawing with sharp precision.
  - **Highlighter**: Semi-transparent highlighter stroke that enhances readability without obscuring underlying text.
  - **Eraser**: Precision path-based erasing to quickly clean up annotations.
  - **Multi-Level Undo & Redo**: Unlimited stroke undo/redo stack.

### 3. Multi-Color Palette & Background Canvas Grids
- **Color Selection**: Rapid-access palette featuring vibrant primary and neon colors with custom opacity sliders.
- **Mobile Background Grids**: Switch between **Blank**, **Lined Notebook**, and **Isometric Grid** templates on mobile to guide sketches while keeping the desktop overlay clean.

### 4. Direct PC Document Export
- Tap **Export** on your mobile screen to instantly compile annotations into:
  - **High-Resolution PNG** image.
  - **Multi-Page Vector PDF** document.
- Exported files automatically land in `C:\Users\<User>\Downloads\Nexora Drawings` with timestamp metadata.

---

[← Back to Main Documentation](../README.md)
