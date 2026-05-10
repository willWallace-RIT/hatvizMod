### hatviz

This is a small P5js sketch that lets you construct patches of aperiodic hat tiles, and displays them along with their associated metatile and supertile outlines. You can download the patches as PNG images or SVG vector illustrations. The mathematical structure of the tile and its tilings is described in the paper "An aperiodic monotile" (Smith, Myers, Kaplan, and Goodman-Strauss, 2023). For more information, including a link to the paper, see [my web page](https://cs.uwaterloo.ca/~csk/hat/) about this project.

The sketch is built using the [P5js](https://p5js.org/) library.  You can run it by pointing your browser at `app.html`.  That file references an online copy of the file.  You can also run it with a local copy of the library; download `P5.min.js` from the [P5js download page](https://p5js.org/download/), put it in the same directory as `hat.js` and `app.html`, and modify `app.html` to reference it instead of the online version.

progress:

on a private repo until initial release but i have made strides with the number of iterations with local storage.

working on refactoring that into a libraryy and the second phase...
 update news

 # Hat System – Object Pool + Compressed State Persistence

## Overview

This fork modifies the original “hats” JS system to support efficient state persistence and reuse of hat objects for shader-based rendering pipelines.

The goal is to convert a dynamic runtime system into a stable, reusable dataset that can be iterated at higher resolution and later consumed by GPU shaders (texture arrays, uniforms, or baked simulation fields).

---

## Key Changes

### 1. Recycled Object Pooling System

Hat instances are no longer discarded after use. Instead, they are reused through an object pool.

- Hats are reused instead of recreated
- State is updated in-place on pooled objects
- Object identity is preserved across iterations
- Reduces garbage collection and allocation overhead

This is important for high-resolution iteration or simulation baking, where object churn becomes a performance bottleneck.

---

### 2. Compressed Local Storage Persistence

Hat state is serialized and stored in `localStorage` using a compressed string format.

Stored data includes only:
- Minimal transform / geometric state
- Shader-relevant parameters
- Pool reference identifiers (not full object graphs)

The intent is to maximize how many simulation states can be stored per session within browser storage limits.

---

## Data Flow

Runtime objects are reduced to a compact representation before storage:

- Runtime Object → Minimal State → Compressed String → localStorage  
- localStorage → Decompressed State → Rehydrated Pool Objects → Runtime

---

## Target Use Case

This system is intended to produce datasets for GPU-driven rendering:

- Texture array generation
- Precomputed simulation states
- High-resolution iterative baking
- Deterministic replay of system behavior

---

## Shader Intent

Each “hat” becomes a data sample in a structured field rather than a fully dynamic object.

Output is intended for:
- Texture buffers (RGBA / packed floats)
- Shader attribute arrays
- Lookup tables for procedural rendering

---

## Ongoing Refactor

Work in progress:
- Cleaner object pool API
- Separation of serialization and runtime logic
- More efficient compression strategy
- Export format suitable for shader pipelines

---

## Design Philosophy

This fork reframes the system from a runtime simulation into a data generator for GPU consumption.

The emphasis is on structured, repeatable state generation rather than per-frame object management.

NEWS: 
currently my offline/private version does squeeze out another iteration on javascript but a little lost in
in the sauce reworking the export data during a displacement event. 

refactoring should help get things working again.
