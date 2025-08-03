# 🛠️ KiCad PCB Design Guide

This guide walks through the practical process of designing a printed circuit board (PCB) in **KiCad**, assuming you already have a schematic and know how to place components. It also includes a solid set of general-purpose PCB design rules for most 2-layer boards.

---

## 📁 Project Setup

1. **Create a Project**
   - Open KiCad → `File` → `New Project`.
   - Save in a dedicated directory.

2. **Prepare Schematic**
   - Open the **Schematic Editor**.
   - Ensure all components have footprints:
     - `Tools` → `Assign Footprints`.
   - Run **ERC**: `Inspect` → `Electrical Rules Checker`.

3. **Push to PCB**
   - Use the **"Update PCB from Schematic"** button (or press `F8`).

---

## 📐 PCB Layout Steps

1. **Define Board Outline**
   - In **PCB Editor**, draw the board shape on the `Edge.Cuts` layer.

2. **Import Components**
   - Auto-imported via `Update PCB from Schematic`. If not, use the same tool again.

3. **Arrange Components**
   - `M` to move, `R` to rotate, `F` to flip.
   - Optimize for routing: group logically, minimize trace length.

4. **Set Design Rules**
   - `File` → `Board Setup` → `Design Rules`.
   - Use the general rules listed below.

---

## 🛣️ Routing

1. **Routing Tracks**
   - Use `Route Tracks` tool (`X`) to draw traces.
   - Press `V` to change layers and insert vias.

2. **Add Zones**
   - For GND or power fills: `Shift+Z` → assign the net (e.g., GND).

3. **Run DRC**
   - `Inspect` → `Design Rules Checker` to catch clearance, width, or unconnected net issues.

---

## 🔍 Final Checks

1. **3D View**
   - `Alt+3` to inspect board in 3D.

2. **Silkscreen**
   - Hide layers to verify silkscreen is readable and not on pads.

3. **Finishing Touches**
   - Add mounting holes, text (`T`), or a logo (`Bitmap to Component Converter`).

---

## 📤 Export for Fabrication

1. **Generate Gerbers**
   - `File` → `Plot` → Select layers:
     - `F.Cu`, `B.Cu`, `F.SilkS`, `B.SilkS`, `F.Mask`, `B.Mask`, `Edge.Cuts`.

2. **Generate Drill Files**
   - Click `Generate Drill Files` after plotting.

3. **Verify**
   - Open in GerbView or [https://gerber-viewer.easyeda.com/](https://gerber-viewer.easyeda.com/).

4. **Send to Manufacturer**
   - Zip all Gerber and drill files and upload to your PCB manufacturer.

---

## 📏 General Design Rules

### 🛠️ Design Rule Settings

| Rule                      | Value                     | Notes |
|--------------------------|---------------------------|-------|
| **Clearance**            | `0.2 mm` (8 mil)          | Spacing between traces, pads, etc. |
| **Track width (min)**    | `0.2 mm` (8 mil)          | Use `0.25 mm` for safer margin |
| **Via drill/pad**        | `0.4 mm` / `0.8 mm`       | Standard signal vias |
| **Pad-to-edge clearance**| `0.5 mm`                  | Prevent copper at board edge |
| **Silkscreen to pad**    | `0.15 mm`                 | Avoid ink on solder pads |
| **Annular ring**         | ≥ `0.15 mm`               | Copper ring around drilled holes |
| **Copper to edge**       | `0.3 mm`                  | Maintain gap from board edge |

### 🧵 Track Width Guidelines

| Type            | Width        | Notes |
|-----------------|--------------|-------|
| Signal          | `0.2–0.25 mm`| Low-current traces |
| Power           | `0.5–1 mm`   | Based on current draw |
| High current    | ≥ `1 mm`     | For >1 A |
| GND/VCC zones   | ≥ `0.25 mm`  | Zone clearance/width |

### 🕳️ Via Sizes

| Via Type     | Drill | Diameter | Use case |
|--------------|-------|----------|----------|
| Standard via | 0.4 mm| 0.8 mm   | General use |
| Small via    | 0.3 mm| 0.6 mm   | Tight spaces (check fab limits) |

---

## 🏭 Manufacturer Reference Specs

| Manufacturer | Min Track Width | Min Clearance | Via Drill | Link |
|--------------|------------------|----------------|-----------|------|
| **JLCPCB**   | 0.13 mm (5 mil) | 0.13 mm        | 0.2 mm    | [jlcpcb.com/capabilities](https://jlcpcb.com/capabilities/Capabilities) |
| **PCBWay**   | 0.15 mm (6 mil) | 0.15 mm        | 0.3 mm    | [pcbway.com/capabilities](https://www.pcbway.com/capabilities/) |
| **OSH Park** | 0.15 mm (6 mil) | 0.15 mm        | 0.3 mm    | [docs.oshpark.com](https://docs.oshpark.com/) |

---

## ✅ Extra Tips

- Use `Ctrl+Shift` while routing to force 45° angles.
- Label important nets (GND, VCC, etc.) for easier layout.
- Use **Highlight Net** in PCB editor to visually follow traces.
- Always run DRC + 3D check before exporting.

---

> **Note:** Always check your PCB manufacturer’s **capabilities page** to adjust these rules if needed.

