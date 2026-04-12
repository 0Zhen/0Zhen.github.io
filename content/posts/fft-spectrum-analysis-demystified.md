---
title: "FFT Spectrum Analysis Demystified"
date: 2026-03-23T09:01:40Z
description: "Window Functions · Zero-Padding · Cooley-Tukey · THD"
canonicalURL: "https://medium.com/@chrislee8613/fft-spectrum-analysis-demystified-354d6e86067f"
cover:
  image: "https://cdn-images-1.medium.com/max/800/1*ZQ66C5L_VOuQLmv68i9vgA.png"
  relative: false
---
*Window Functions · Zero-Padding · Cooley-Tukey · THD*

A practical deep-dive for engineers and signal processing beginners

## Introduction

Every time you capture a waveform with an oscilloscope and export it as a CSV, you hold a time-domain snapshot of a signal. The frequency content — which harmonics exist, how large they are, what the Total Harmonic Distortion is — remains invisible until you apply a **Fast Fourier Transform (FFT)**.

This article walks through every stage of the FFT pipeline, from raw CSV data to a fully annotated Excel output. We explain why each step is necessary, show how Cooley-Tukey makes FFT fast enough for real-time analysis, and cover the crucial role of window functions — especially Flat-top for accurate harmonic amplitude measurement.

## Step 1 — DC Removal

Oscilloscope signals often sit on a non-zero DC baseline. Without removing it, the FFT produces a massive spike at 0 Hz that overwhelms all other frequency components. The fix: subtract the mean of the entire signal from every sample.

> dc_removed[i] = signal[i] — mean(signal)

![Figure 1 — Left: raw signal with DC offset (mean = 0.60 V). Right: after subtracting the mean, the waveform is centered on zero.](https://cdn-images-1.medium.com/max/800/1*ZQ66C5L_VOuQLmv68i9vgA.png)
*Figure 1 — Left: raw signal with DC offset (mean = 0.60 V). Right: after subtracting the mean, the waveform is centered on zero.*

## Step 2 — Zero-Padding

Cooley-Tukey FFT runs optimally when N is a power of 2. Oscilloscope captures rarely produce exactly 2ⁿ samples. Zero-padding appends zeros to the end of the signal until the total length reaches the next power of 2. For 10,000 samples, that is 16,384.

![Figure 2 — Left: 100 original samples. Right: 28 zeros appended (orange) to reach 128 = ²⁷. Zero-padding speeds up computation and smooths the spectrum, but does not increase true resolution.](https://cdn-images-1.medium.com/max/800/1*ij_kZuCyoFiEYhEuob6zmg.png)
*Figure 2 — Left: 100 original samples. Right: 28 zeros appended (orange) to reach 128 = ²⁷. Zero-padding speeds up computation and smooths the spectrum, but does not increase true resolution.*

## Step 3 — Window Functions

## What is Spectral Leakage?

FFT assumes the captured signal repeats infinitely. When the signal end does not connect smoothly back to its start, the discontinuity at the boundary introduces artificial frequency components that spread across the entire spectrum — called spectral leakage.

## How Window Functions Help

A window function tapers the signal smoothly to zero at both ends, so head and tail always connect at zero, eliminating the discontinuity.

![Figure 3 — Four common windows. Rectangular does nothing (leakage-prone). Hann and Hamming taper smoothly to zero. Flat-top has a wide, extremely flat top — trading frequency resolution for superior amplitude accuracy.](https://cdn-images-1.medium.com/max/800/1*YVYBQM-UVFhn3OHkgk2dJA.png)
*Figure 3 — Four common windows. Rectangular does nothing (leakage-prone). Hann and Hamming taper smoothly to zero. Flat-top has a wide, extremely flat top — trading frequency resolution for superior amplitude accuracy.*

![Figure 4 — Spectral leakage for a 5.7 Hz sine (between two FFT bins). Red dashed = true frequency. Rectangular shows heavy skirts; Flat-top keeps the peak close to true amplitude even without perfect bin alignment.](https://cdn-images-1.medium.com/max/800/1*H6F3IXtStoXhdir3tTE8oA.png)
*Figure 4 — Spectral leakage for a 5.7 Hz sine (between two FFT bins). Red dashed = true frequency. Rectangular shows heavy skirts; Flat-top keeps the peak close to true amplitude even without perfect bin alignment.*

## Window Comparison

![](https://cdn-images-1.medium.com/max/800/1*oPFtqzzcQbQ36_uW0oLp1w.png)

## Flat-top Window Formula

> w(i) = 0.21558–0.41663*cos(θ) + 0.27726*cos(2θ) — 0.08358*cos(3θ) + 0.00695*cos(4θ)

where θ = 2π·i / (N−1). The alternating signs produce a shape that dips slightly below zero at the edges, creating the unusually flat passband.

## Step 4 — Cooley-Tukey FFT

A direct DFT requires O(N²) operations. For N = 16,384, that is ~268 million multiplications. Cooley-Tukey reduces this to O(N log₂N) — about 229,376 operations — a **~1,000× speedup**.

The algorithm splits an N-point DFT into two N/2-point DFTs recursively until reaching trivial 1-point transforms, then combines results bottom-up using butterfly operations.

![Figure 5 — Butterfly diagram for N=8. Three stages (log₂8 = 3) produce the full spectrum from bit-reversed inputs. Each stage combines pairs using complex twiddle factors.](https://cdn-images-1.medium.com/max/800/1*9Ait8r_IMXSJZgWxS9fSmg.png)
*Figure 5 — Butterfly diagram for N=8. Three stages (log₂8 = 3) produce the full spectrum from bit-reversed inputs. Each stage combines pairs using complex twiddle factors.*

• Phase 1 — Bit-reversal permutation: reorder inputs so recursive halving corresponds to simple index interleaving

- Phase 2 — Butterfly passes: log₂(N) stages, each combining adjacent pairs with twiddle factors e^(−j2πk/N)

## Step 5 — Amplitude Compensation (ACF)

Applying a window reduces the overall signal energy. Without correction, every amplitude in the spectrum appears smaller than reality. The Amplitude Correction Factor compensates:

> amplitude = |FFT(i)| x (2 / N) x ACF

• |FFT(i)| — magnitude of the complex FFT output

• (2 / N) — normalizes FFT scaling and combines positive + negative frequency halves

- ACF — window compensation factor: Flat-top = 4.638, Hann = 2.0, Hamming = 1.852

![Figure 6 — Light bars = before ACF, dark bars = after. Red dashed = true amplitude (1.0 V). After ACF, all windows recover the correct peak.](https://cdn-images-1.medium.com/max/800/1*M58isS-2UtWy8DjAdzgYRg.png)
*Figure 6 — Light bars = before ACF, dark bars = after. Red dashed = true amplitude (1.0 V). After ACF, all windows recover the correct peak.*

## Step 6 — Harmonic Extraction & THD

## Finding the Fundamental

After computing all amplitudes, we scan the spectrum (excluding DC) for the largest peak. That is the fundamental f₁. In our oscilloscope example: 228.9 Hz.

## Extracting Harmonics

Harmonics are integer multiples of the fundamental: 2f₁, 3f₁, … 10f₁. For each, compute the nearest FFT bin:

> bin_index = round(h * f_fundamental / freq_resolution)

## Total Harmonic Distortion

> THD = sqrt(A2² + A3² + … + A10²) / A1

![Figure 7 — Left: harmonic amplitudes on log scale. Fundamental (blue) at 228.9 Hz dominates; all harmonics (red) are at least 40 dB smaller. Right: energy ratio pie with THD = 1.37% — a very clean signal.](https://cdn-images-1.medium.com/max/800/1*IE4dOLoZ_IF8V395usyaRg.png)
*Figure 7 — Left: harmonic amplitudes on log scale. Fundamental (blue) at 228.9 Hz dominates; all harmonics (red) are at least 40 dB smaller. Right: energy ratio pie with THD = 1.37% — a very clean signal.*

## Summary

The complete FFT pipeline for oscilloscope harmonic analysis:

• Remove DC offset by subtracting the signal mean

• Zero-pad to the next power of 2 for FFT efficiency

• Apply a window function — use Flat-top when amplitude accuracy matters, Hann for general analysis

• Run Cooley-Tukey FFT: O(N log N) vs O(N²) for direct DFT

• Apply Amplitude Correction Factor (ACF) to recover true amplitudes

• Extract fundamental and 10 harmonics; compute THD

**Rule of thumb:**if you need to distinguish two close frequencies, use Hann. If you need to measure exactly how large a harmonic is, use Flat-top.
