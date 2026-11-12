# Research Note: Jump and Transition Mechanics Analysis

**Subject:** Logic and technical implementation of long-distance jumps/teleports in space-sandbox games (Target: *Space Engineers* and general industry standards).
**Source:** SE Wiki, Dev Blogs, UE5 Documentation, and community technical discussions.

## 1. The "Jump Drive" Implementation (Space Engineers)
*   **Mechanism:** Instantaneous teleportation (teleport, not travel).
*   **Technical Constraints:**
    *   **Mass-Distance Scaling:** The total mass of the grid (including cargo) directly restricts jump distance. 
    *   **Energy Cost:** Highly dependent on distance.
    *   **Pathing Constraints:** Jumps cannot be made if the line-of-sight is blocked by a planetary body/asteroid or if inside a gravity well.
*   **The Pitfall:** SE's "jump" is essentially a hard coordinate overwrite. This often leads to "positional glitches" if the player/ship collides with a voxel object exactly at the destination.

## 2. The "Stutter" Problem (Mitigation Strategies)
In many mods and even base implementations, jumping causes a visual or logical "hitch" (stutter).
*   **Causes:** 
    *   **Asset Loading:** The engine hasn't loaded the assets/textures/voxels for the new coordinate.
    *   **Physics Re-calculation:** The physics engine (like Chaos or VRAGE) has to suddenly re-calculate the CoM, gravity, and collisions for a completely new position in a single frame.
    *   **Network Latency:** In multiplayer, the client moves, then the server validates, then the client "snaps" to the true position.

## 3. The "Stargate Engineers" Engineering Solution (UE5)
Instead of a "hard jump" (coordinate overwrite), we will implement a **Seamless Transition Model**.

**A. Transition Map (The "Wormhole" Intermediate):**
*   Based on UE5's `SeamlessTravel` documentation, we will not jump from System A $\rightarrow$ System B.
*   We will use a **Transition Map** (a stylized, non-Euclidean wormhole tunnel).
*   **Flow:** System A $\rightarrow$ Transition Map (Wormhole) $\rightarrow$ System B.
*   This allows the engine to *asynchronously load* System B while the player is visually moving through the wormhole, eliminating the "hitch."

**B. Handling Actors (The "Carry-Through" Logic):**
*   In UE5, we must use `GetSeamlessTravelActorList` to ensure the player's ship (the "Actor") is carried from the old level to the transition level, and then to the final destination. 
*   This prevents the ship from being "destroyed" or "reset" during the jump.

**C. Mathematical "Horizon" (The Event Horizon):**
*   We will implement a "detection zone" (a trigger volume) around the Stargate. 
*   **Logic:** Once an object/ship enters the Volume $\rightarrow$ Trigger `ServerTravel` to the Transition Map.

## 4. Summary of Lessons for Implementation
*   **Don't jump; travel through an intermediate layer.** (Prevents asset loading stutters).
*   **Use UE5's `SeamlessTravel` API.** (Protects ship data and player state).
*   **Asynchronous Loading is key.** (The wormhole is a visual "buffer" for the engine's background loading work).
