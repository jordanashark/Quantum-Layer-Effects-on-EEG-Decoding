# Quantum-Layer-Effects-on-EEG-Decoding

**Question:** Does adding a quantum layer to an EEG motor imagery decoder actually help, or does it just look like it helps?

**Three hypotheses:**

1. **It’s real.** The quantum-hybrid model beats the standard model by more than a meaningful margin (say 1.5 points of accuracy) and by more than the random run-to-run wobble you get from just changing the random seed.
2. **The quantum part is the reason.** The advantage disappears if we swap the quantum layer for something equivalent but not quantum. Specifically, it should go away if we remove entanglement, and it should go away if we use an ordinary layer with the same number of parameters. If a classical computer running an exact simulation of the circuit gets the same boost, then the benefit is real but doesn’t need quantum hardware.
3. **It’s consistent.** The advantage shows up across different datasets and for most individual subjects, not just a lucky few.
