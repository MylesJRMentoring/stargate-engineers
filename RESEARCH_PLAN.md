# Research Plan: Mechanics Extraction

**Goal:** Use REA to understand the mathematical and logical foundations of existing space-sandbox games to avoid their pitfalls and replicate/improve their strengths.

## Phase 1: Physics & Mass Study (Target: Space Engineers)
**Objective:** Understand how mass is handled when ship shape changes.
- [x] **Task 1.1:** Use REA to inspect `SpaceEngineers.exe` / core DLLs.
- [x] **Task 1.2:** Identify the data structure for voxel/block-to-mass conversion.
- [x] **Task 1.3:** Extract the math for Center of Mass (CoM) updates after block addition/removal.
- [x] **Task 1.4:** Compare SE's CoM calculation vs. UE5 Chaos Physics.
- [x] **TASK COMPLETED:** Baseline research conducted via technical deconstruction. See `RESEARCH_NOTES_PHYSICS.md`.

## Phase 2: Transition/Jump Study (Target: Space Engineers Mods)
**Objective:** Analyze how \"jump\" or \"teleport\" mods handle the transition between coordinates.
- [ ] **Task 2.1:** Use REA to analyze common jump-drive mods.
- [ ] **Task 2.2:** Identify how they mitigate \"stutter\" (e.g., time-dilation, background loading).
- [ ] **Task 2.3:** Document the \"Event Horizon\" logic (detecting when a player/object is in the transition zone).

## Phase 3: Networking & Scale Study (Target: Star Citizen/Elite Dangerous Concepts)
**Objective:** Research how high-fidelity games handle \"system hopping.\"
- [ ] **Task 3.1:** Research \"Seamless Travel\" implementation in modern engines.
- [ ] **Task 3.2:** Plan the UE5 World Partition strategy for \"Stargate\" handover.

## Phase 4: Implementation (The \"Better\" Part)
- [ ] Write C++ `StargateGateActor` for UE5.
- [ ] Implement `GateTransitionManager` to handle background asset streaming.
- [ ] Design the \"Gate Alloy\" material and voxel interaction logic.
