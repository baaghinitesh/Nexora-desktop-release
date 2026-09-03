# Interactive Pen Tablet & Screen Annotation

[← Back to Main Documentation](../README.md)

Nexora transforms your touchscreen smartphone into a wireless drawing tablet, mirroring annotations live onto a transparent fullscreen Windows Ghost Window overlay.

---

## Live Drawing Architecture

```mermaid
flowchart TD
    A[Mobile Drawing Canvas / Stylus Touch] -->|Vector Path Data| B[WebSocket Realtime Stream :8080]
    B --> C[Desktop Background Server]
    C -->|IPC WebContents Send| D[Transparent Ghost Window Overlay]
    D -->|HTML5 2D Canvas Render| E[Live Desktop Annotation on Screen]
```

---

## Key Features

### 1. Transparent Ghost Window Overlay
- Nexora creates a borderless, always-on-top, click-through transparent Windows desktop window.
- Drawing strokes made on your mobile device appear instantaneously across your active PC workspace without obscuring open applications.

### 2. Stylus & Touch Controls
- **Pressure Simulation**: Simulated stroke width modulation based on finger velocity and stylus pressure.
- **Palette & Brushes**: Customizable brush widths, eraser mode, undo/redo stack, and vibrant color presets.
- **Grid Modes**: Switch between blank, lined note, and isometric grid backgrounds on mobile for precise sketching.

### 3. Document Export
- Export your mobile sketches and marked-up annotations directly into high-resolution **PNG** images or formatted **PDF** documents with one tap.

---

[← Back to Main Documentation](../README.md)
