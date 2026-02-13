# Circular Reveal Image Application Documentation

## Overview
This application is an interactive visualization tool that allows users to reveal specific linguistic or geographic regions on a map using a circular animation effect. It combines SVG for interaction detection and HTML5 Canvas for high-performance image masking and rendering.

## File Information
*   **Main File**: `newcharacters.html`
*   **Dependencies**: Requires an `img/` directory containing specific PNG/GIF assets (e.g., `ara_full.png`, `arm_full.png`, `hranice.gif`).

## Architecture

### 1. HTML Structure
The DOM is organized into three main layers (z-index ordered from bottom to top):
1.  **Canvas Layers (`z-index: 10-120`)**: Multiple `<canvas>` elements, each dedicated to a specific script/region (e.g., Arabic, Latin, Cyrillic).
2.  **SVG Overlay (`z-index: 300`)**: An SVG containing transparent `<polygon>` elements that define the clickable hotspots for each region.
3.  **Control Interface (`z-index: 1000`)**: A fixed navigation bar at the top containing control links (ALL, RANDOM, FREEZE, CLEAR, etc.).

### 2. State Management
The application uses parallel arrays to manage the state of 14 distinct regions (indices 0-13).
*   **`isRevealing[]`**: Boolean, true if the region is currently animating open.
*   **`isHiding[]`**: Boolean, true if the region is currently animating closed.
*   **`isHidden[]`**: Boolean, true if the region is fully invisible.
*   **`isFrozen[]`**: Boolean, true if the animation is paused.
*   **`revealRadius[]`**: Current radius of the revealed circle for each region.
*   **`clickX[]` / `clickY[]`**: The center coordinates of the reveal animation.

### 3. Key Functions

#### `paint(region)`
The core animation loop.
*   **Logic**: Clears the specific canvas, creates a circular clipping path based on `revealRadius`, and draws the associated image.
*   **Animation**: Uses `requestAnimationFrame` to recursively call itself while expanding or contracting the radius.

#### `clickedRegion(event, index, getclick)`
Handles user interaction with specific regions.
*   **Parameters**:
    *   `event`: The DOM event.
    *   `index`: The ID of the region (0-13).
    *   `getclick`: Boolean. If true, uses the mouse coordinates as the center of the reveal. If false (triggered via menu), uses a predefined center.
*   **Behavior**: Toggles the state between revealing, hiding, or freezing based on current state.

#### `randomRegions(event, randomizeStarts)`
Automates the display of regions.
*   **Behavior**: Iterates through all regions and toggles their visibility. Can optionally randomize the starting point of the reveal.

#### `freezeAllRegions(event)`
Pauses all currently animating regions in their current state.

#### `resetRegions(event)`
Resets all regions to the hidden state and clears the canvas.

#### `calculateStarts()`
Recalculates the default starting coordinates for animations based on the current window size, ensuring the reveal effect stays centered on the correct map location during resizing.

### 4. Configuration
*   **`speed`**: Controls the speed of the reveal animation (default toggles between 100 and 700).
*   **`images`**: Array of `Image` objects loaded from the `img/` directory.

## Usage
1.  **Interactive Map**: Click on any region on the map (defined by blue outlines in the SVG) to reveal the underlying image for that script.
2.  **Menu Controls**:
    *   **ALL**: Toggles all regions.
    *   **RANDOM**: Toggles regions with random reveal centers.
    *   **FREEZE**: Pauses animations.
    *   **CLEAR**: Hides everything.
    *   **Script Links (e.g., ARMENIAN, GREEK)**: Toggles specific regions programmatically.

## CSS Classes
*   `.active`: Applied to links when a region is animating (blinking yellow).
*   `.frozen`: Applied when a region is paused (solid yellow).
*   `.shown`: Applied when a region is fully revealed (green).
*   `.hidden`: Applied when a region is fully hidden (white).

## Region Index Mapping
| Index | Region |
|-------|--------|
| 0 | ARABIC |
| 1 | ARMENIAN |
| 2 | CHINESE |
| 3 | ETHIOPIAN |
| 4 | GEORGIAN |
| 5 | GREEK |
| 6 | HEBREW |
| 7 | JAPANESE |
| 8 | KOREAN |
| 9 | LATIN |
| 10 | CYRILLIC |
| 11 | BRAHMIC |
| 12 | BORDERS |
| 13 | COLOURS |