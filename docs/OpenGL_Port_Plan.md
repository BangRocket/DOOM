# DOOM Cross-Platform OpenGL Port Plan

**Project Goal:** Refactor the Linux DOOM 1.10 codebase to compile and run cross-platform (Linux, Windows, macOS) with a new hardware-accelerated renderer using OpenGL.

---

## Phase 1: Foundational Cross-Platform Setup

*   **Goal:** Get the *existing* game logic running within a cross-platform window using CMake and SDL, with a temporary software framebuffer display.
*   **Tasks:**
    1.  **Build System Migration:** Convert `linuxdoom-1.10/Makefile` to `linuxdoom-1.10/CMakeLists.txt`.
    2.  **Integrate SDL2:** Add SDL2 as a dependency via CMake for windowing, input, OpenGL context creation, and potentially audio/networking.
    3.  **Refactor Platform Interface (`i_*.c` files):** Replace X11/Linux-specific code in `i_video.c`, `i_system.c`, `i_sound.c`, `i_net.c` with SDL2 equivalents.
    4.  **Basic Rendering Stub:** Implement a temporary `I_FinishUpdate` using SDL's 2D functions to copy the original software framebuffer (`screens[0]`) to the SDL window.

---

## Phase 2: OpenGL Renderer Design & Core Implementation

*   **Goal:** Design and implement the core components of the new OpenGL renderer.
*   **Tasks:**
    1.  **Renderer Architecture:** Define the interface between game logic and the OpenGL renderer. Plan resource management (textures, buffers, shaders) and the rendering pipeline.
    2.  **WAD Asset Loading (GPU):** Load textures, flats, and sprites from WAD files into OpenGL textures.
    3.  **Geometry Generation (GPU):** Convert DOOM map data (linedefs, sectors) into vertex and index buffers suitable for OpenGL.
    4.  **Shader Development:** Create initial GLSL vertex and fragment shaders for drawing textured walls, floors, and ceilings.
    5.  **Initial OpenGL Rendering:** Replace the Phase 1 rendering stub with code that uses OpenGL to draw the basic level geometry.

---

## Phase 3: OpenGL Feature Implementation & Refinement

*   **Goal:** Implement remaining DOOM rendering features using OpenGL and optimize.
*   **Tasks (Iterative):**
    1.  Sprite Rendering.
    2.  Sky Rendering.
    3.  Lighting (Sector-based lighting in shaders).
    4.  Transparency and Effects.
    5.  UI/HUD Rendering (Status bar, menus).
    6.  Optimizations (Culling, batching).
    7.  Cross-platform Testing and Bug Fixing.

---

## Diagrammatic Overview

```mermaid
graph TD
    A[Start: Linux DOOM Codebase] --> B{Phase 1: Foundation};
    B --> B1[CMake Build System];
    B --> B2[Integrate SDL2];
    B --> B3[Refactor i_*.c Files];
    B --> B4[Basic SDL Framebuffer Blit];
    B4 --> C{Phase 2: Core OpenGL Renderer};
    C --> C1[Design Renderer Architecture];
    C --> C2[WAD Asset Loading (OpenGL)];
    C --> C3[Geometry Generation (OpenGL)];
    C --> C4[GLSL Shader Development];
    C --> C5[Initial OpenGL Rendering];
    C5 --> D{Phase 3: OpenGL Features & Refinement};
    D --> D1[Sprites];
    D --> D2[Sky];
    D --> D3[Lighting];
    D --> D4[Effects/Transparency];
    D --> D5[UI/HUD];
    D --> D6[Optimizations];
    D --> E[End: Cross-Platform OpenGL DOOM];

    subgraph Phase 1 Tasks
        B1
        B2
        B3
        B4
    end

    subgraph Phase 2 Tasks
        C1
        C2
        C3
        C4
        C5
    end

   subgraph Phase 3 Tasks
        D1
        D2
        D3
        D4
        D5
        D6
    end