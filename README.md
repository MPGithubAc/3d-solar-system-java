# Real-Time Solar System Renderer

A real-time, interactive 3D solar system rendered from scratch in Java using **JOGL** (Java bindings for OpenGL 4) and hand-written **GLSL** shaders. The scene features the Sun, all eight planets, two orbiting moons, visible orbital trajectories, and a free-fly camera you can use to explore the system at 60 FPS.

## Overview

This project implements a small real-time rendering engine on top of the OpenGL 4 programmable pipeline. Every celestial body is a procedurally generated, UV-mapped sphere drawn with a custom vertex/fragment shader pair, positioned and animated each frame through model-view-projection matrix math. A UVN ("free-look") camera lets the viewer fly through the scene, and toggleable overlays draw the world axes and each body's orbital path.

## Features

- **Procedural geometry** — spheres for the Sun, eight planets, and two moons are generated in code (vertices, normals, and texture coordinates), plus a pentagonal prism as an additional orbiting body.
- **Textured rendering** — each body is wrapped in its own surface texture sampled in the fragment shader.
- **Custom GLSL shaders** — inline GLSL 4.30 vertex and fragment shaders handle the MVP transform and switch rendering mode between textured surfaces and solid-color line geometry (axes and orbit paths).
- **Orbital animation** — bodies are advanced along their orbits over time and rendered with visible trajectory rings.
- **Free-fly UVN camera** — translate and rotate freely through the scene with keyboard controls.
- **Real-time loop** — driven by JOGL's `FPSAnimator` at a target 60 FPS.
- **Toggleable debug overlays** — world axes and orbit trajectories can be shown or hidden on the fly.

## Controls

| Key | Action |
| --- | --- |
| `W` / `S` | Move forward / backward |
| `A` / `D` | Strafe left / right |
| `Q` / `E` | Move up / down |
| Arrow keys | Pan (left/right) and pitch (up/down) the camera |
| `Space` | Toggle orbit trajectories |
| `X` | Toggle world axes |

## Tech Stack

- **Language:** Java (JDK 21)
- **Graphics API:** OpenGL 4 via [JOGL](https://jogamp.org/) (`jogl-all`, `gluegen-rt`)
- **Math:** [JOML](https://github.com/JOML-CI/JOML) for matrices and vectors
- **Shaders:** GLSL `#version 430`
- **Windowing/IO:** Java AWT/Swing (`GLCanvas`, `JFrame`, `KeyListener`)

## Project Structure

```
src/jogl_shader_course/
├── Main.java                     # Main app: GL setup, render loop, shaders, camera input
├── Camera.java                   # UVN free-fly camera (move/strafe/pan/pitch)
├── Sphere.java                   # Procedural UV sphere geometry
├── PentagonalPrism.java          # Procedural prism geometry
├── OrbitTrajectory.java          # Orbit path / trajectory ring geometry
├── Axes.java                     # World-axis overlay
└── IModel.java                   # Shared model interface
textures/                         # Planet, moon, and sun surface textures
lib/                              # JOGL / GlueGen / JOML jars
```

## Building & Running

**Requirements:** JDK 21 and the JOGL/JOML jars in `lib/`.

> **Note on platform:** the native libraries bundled in `lib/` (`*-natives-windows-amd64.jar`) target **Windows (x64)**. To run on macOS or Linux, replace them with the matching JOGL/GlueGen native jars for your platform from [jogamp.org](https://jogamp.org/).

Compile:

```bash
javac --add-exports jogl.all/com.jogamp.opengl.util=ALL-UNNAMED \
      --add-exports jogl.all/com.jogamp.opengl.util.texture=ALL-UNNAMED \
      -cp "lib/*" -d bin src/jogl_shader_course/*.java
```

Run:

```bash
java --add-exports jogl.all/com.jogamp.opengl.util=ALL-UNNAMED \
     --add-exports jogl.all/com.jogamp.opengl.util.texture=ALL-UNNAMED \
     -cp "bin:lib/*" jogl_shader_course.CompGraphicsProjectDos
```

> On Windows, use `;` instead of `:` as the classpath separator: `-cp "bin;lib/*"`.

## What I Learned

Building this meant working directly with the OpenGL programmable pipeline rather than a game engine: writing and compiling GLSL shaders, managing vertex array/buffer objects, packing uniform data into native buffers, and implementing the full model-view-projection transform chain by hand. The camera and orbital motion required a working understanding of 3D linear algebra — coordinate frames, rotations, and perspective projection — applied in real time.
