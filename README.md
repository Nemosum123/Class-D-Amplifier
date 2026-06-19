# Class-D-Amplifier
A complete, fully functional Class-D audio amplifier built completely from scratch using discrete op-amps, comparators, and bipolar junction transistors (BJTs). This project bridges the gap between theoretical feedback systems and practical hardware design by ditching pre-packaged amplifier ICs in favor of a raw, component-level build.

Instead of amplifying an analog audio signal directly (which causes high thermal losses), this amplifier converts the smooth analog audio wave into a high-frequency digital square wave using Pulse Width Modulation (PWM), achieving high efficiency and massive voltage gain.

# Key Features

Custom Audio Frontend: Active-RC Multiple Feedback (MFB) Bandpass Filters tuned to specific audio bands (156.25 Hz and 625 Hz) mixed perfectly via a Summing Amplifier.

Differential PWM Modulation: Uses a custom triangle wave ramp generator to slice the analog audio into exact digital PWM pulses.

Discrete Power Stage: An H-Bridge constructed from 2N2222 and 2N2907 transistors driven into hard saturation and cutoff for high-efficiency switching.

Robust Hardware Logic: Integrated dead-time logic to prevent shoot-through, and CD4069 hex inverter buffers to overcome transistor gate capacitance.

Verified via Simulation & Hardware: Fully modeled in LTSpice and physically verified on a breadboard using an oscilloscope.

# System Architecture & Signal Chain

This repository is broken down into the 6 core stages of the amplifier's signal chain:

1. # The Audio Frontend (Active Filters & Mixer)

MFB Bandpass Filters: Filters the incoming raw audio to isolate specific frequency bands. Built using capacitors and resistors to meticulously tune the Center Frequency, Q-Factor and Passband Gain. 

Summing Amplifier: An inverting mixer combining the filtered audio channels back into a single analog path at a 1:1 ratio.

2. # The Ramp Generator (The Heartbeat)

Built with an MCP6004 Op-Amp RC Integrator and an LM339 Schmitt Trigger.

Generates a continuous 5 kHz triangle wave spanning perfectly between 2.0V and 3.0V, providing the fundamental reference signal (or "ruler") for the modulation stage.

3. # The PWM Modulator (The Translator)

Single-to-Differential Converter: Splits the single-ended analog audio into a balanced pair of mirror-image signals ("push" and "pull").

Comparator Array: Measures the differential audio waves against the high-speed triangle wave. Outputs a HIGH (5V) signal when the audio is taller than the triangle wave, creating a rapid-fire series of digital square waves whose pulse widths expand and contract with the audio.

4. # Gate Logic & Buffering

Dead-Time Control: Cross-coupled NAND logic and 1 nF capacitors establish a non-overlap delay window to prevent shoot-through (where top and bottom transistors conduct simultaneously and short the power supply).

Current Buffering: CD4069 Hex Inverters wired in parallel to pump high base current (up to 20–50 mA) into the H-bridge, overcoming the heavy input capacitance of the transistors.

5. # The H-Bridge Output Stage

Operates bipolar junction transistors strictly in Saturation Mode (Fully ON, acting as a low-resistance closed switch) and Cutoff Mode (Fully OFF).

Carefully designed to avoid the linear region to prevent thermal burnout (Base Starvation).

Pushes the high-power digital signal across a 28Ω speaker load (20Ω series padding + 8Ω speaker coil). The speaker's physical inductance acts as an intrinsic low-pass filter, ignoring the 5 kHz switching speed and vibrating only to the original analog audio frequencies.

# Hardware Used:

# Op-Amps: MCP6004 (Single-supply rail-to-rail op-amps for integrators, filters, and mixers)

# Comparators: LM339 (Open-collector comparators for Schmitt triggers and PWM generation)

# Logic/Buffers: CD4069 Hex Inverters & NAND Gates

Transistors: 2N2222 (NPN) and 2N2907 (PNP) Bipolar Junction Transistors

Passives: Various precise resistors, tuning potentiometers (for Vcm bias), and capacitors.​
