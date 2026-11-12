# Research Note: Space Engineers Physics (VRAGE 2.0) Analysis

**Subject:** Physics, Mass, and Center of Mass (CoM) implementation.
**Source:** Developer Blogs, Wiki, and Community Technical Discussions.

## 1. The Mass Model
*   **Grid-Based Mass:** SE uses a "Grid" system. Every physical object (ship, station) is a collection of interconnected "blocks."
*   **Cumulative Mass:** Total mass is the sum of all constituent blocks.
*   **Inert vs. Artificial Mass:**
    *   **Inert Mass:** The standard's resistance to acceleration.
    *   **Artificial Mass:** A specific game mechanic where "Artificial Mass Blocks" interact with Gravity Generators.
    *   *Pitfall to avoid:* In SE, gravity pulls "Artificial Mass," not the ship's actual mass, which can feel unintuitive. In **Stargate Engineers**, we will ensure gravity acts on the *total mass* of the grid for consistency.

## 2. Center of Mass (CoM) Dynamics
*   **Calculated CoM:** The CoM is the mathematical balance point of the grid.
*   **The "Anti-Flip" Hack:** SE uses a dynamic CoM adjustment for wheeled vehicles to prevent tipping during high-speed turns. This is an "intentional sacrifice of realism for gameplay."
*   **The Challenge for UE5:** In UE5, we will use **Chaos Physics**. We should aim for a CoM that is physically accurate but use **Substepping** (running physics at a higher frequency than the frame rate) to ensure stability during rapid construction/destruction.

## 3. Voxel vs. Grid Distinction
*   **Static Voxels:** Terrain (planets/asteroids) is "static" voxel data. It does not move or react to gravity.
*   **Dynamic Grids:** Ships (grids) are the only things that move via physics.
*   **The "Better" Approach:** In **Stargate Engineers**, we want to allow for *deformable* or *movable* terrain (e.g., massive asteroid collisions), which will require a more advanced voxel-to-physics integration than SE's decoupled model.

## 4. Summary of Implementation Lessons
*   **Do not** decouple gravity from actual mass (use total mass).
*   **Do not** use "hacks" for CoM; instead, use UE5's high-frequency physics substeps.
*   **Do** leverage the distinction between static terrain and dynamic grids to manage performance.
