# Pro Signal Processor (CSV Smoothing Filter Set)

A robust, browser-based tool designed to visualize, filter, and clean noisy time-series data.

Originally developed to process high-noise voltage and current signals measured near electrical arcs, this tool is completely client-side (privacy-focused) and runs without any external dependencies.

🔗 **[Live Demo](https://lightningphil.github.io/CSV-Smoothing-Filter/)**

![App Screenshot](https://via.placeholder.com/800x400.png?text=Add+Your+Screenshot+Here)
*(Replace the link above with an actual screenshot of your tool)*

## 🚀 Key Features

*   **Zero Dependencies:** A single HTML file containing all logic, styling, and visualization engine.
*   **Privacy First:** All processing happens in your browser. No data is uploaded to any server.
*   **Pipeline Architecture:** Stack multiple filters in series (e.g., remove DC offset &rarr; Notch Filter &rarr; Savitzky-Golay).
*   **Real-time Visualization:**
    *   **Split View:** Simultaneously view the signal and its first derivative.
    *   **Domain Switching:** Instantly toggle between Time Domain and Frequency Domain (FFT).
*   **Interactive Charts:** Custom-built canvas engine supporting zoom, pan, and logarithmic scaling.
*   **Export:** Download the processed data and calculated derivatives as a new CSV file.

## 🎨 UI & Tech Stack

This application was built with a focus on performance and simplicity. It does **not** use heavy frameworks like React, Vue, or charting libraries like Chart.js.

*   **Core:** Vanilla JavaScript (ES6+).
*   **Rendering:** A custom-written **HTML5 Canvas** engine ("SplitChart") handles the rendering. This ensures high frame rates even with large datasets, allowing for specific engineering visualization needs (like synchronous scrubbing of signal and derivative) that generic libraries often struggle with.
*   **Styling:** Native CSS utilizing **CSS Variables** for consistent theming and **Flexbox** for a responsive, dashboard-style layout.
*   **Math:** Custom implementations of:
    *   Fast Fourier Transform (FFT)
    *   Gauss-Jordan Matrix Inversion (for polynomial fitting)
    *   IIR Filter coefficient generation (Biquad)

## 📉 Why Visualize in Different Domains?

### Time vs. Frequency Domain
*   **Time Domain:** Essential for seeing the shape of pulses, rise times, and peak amplitudes.
*   **Frequency Domain (FFT):** Essential for identifying *what* is causing the noise. If you see a large spike at 50Hz or 60Hz, you know you have mains hum interference. If you see high-frequency "grass" at the right side of the graph, you know you need a Low Pass filter.

### The Importance of the Derivative View ($dy/dx$)
In signal processing—especially with arcs or control systems—we often need to know how fast a signal is changing (e.g., $di/dt$ for inductance calculations).

**The Problem:** Differentiation amplifies noise. A signal that looks "smooth enough" to the eye might have a derivative that looks like pure chaos.
**The Solution:** This tool includes a dedicated "Rate of Change" panel. By tuning your filters while watching the Derivative panel, you can ensure your smoothing is aggressive enough to yield a usable derivative, but gentle enough not to distort the physical reality of the signal.

## 🎛 Filter Guide

### 1. Savitzky-Golay (SG)
*   **What it is:** A digital filter that fits a polynomial to a set of data points (a window).
*   **Best for:** Preserving the **height and width of peaks** while removing noise. Unlike a standard average, it doesn't "flatten" sharp features as much.
*   **Variants:** The tool offers x2, x3, and x4 variants, which run the filter multiple times for a smoother result.

### 2. Rolling Average
*   **What it is:** Takes the arithmetic mean of neighbors within a window.
*   **Best for:** Heavy noise reduction where peak precision is less critical.
*   **Warning:** It acts as a crude low-pass filter and will flatten sharp peaks and slow down rise times.

### 3. IIR Frequency Filters (Biquad)
*   **Low Pass:** Removes high-frequency noise (jitter/static).
*   **High Pass:** Removes low-frequency drift or DC offsets.
*   **Notch:** Surgically removes a specific frequency band. (Use this to kill 50Hz/60Hz mains hum).

### 4. Utilities
*   **DC Offset:** Shifts the entire signal up or down.
*   **Gain:** Multiplies the signal amplitude (useful for calibration corrections).

## 💾 Installation & Offline Use

Because the application is a single HTML file, it is completely portable.

1.  Go to the [GitHub Repository](https://github.com/LightningPhil/CSV-Smoothing-Filter).
2.  Download the `index.html` file (or the zip of the repo).
3.  Open the file in Chrome, Firefox, Edge, or Safari.

**No internet connection is required to run the tool once downloaded.**

## 📄 CSV Input Format

The tool expects a standard CSV file with a header row.

*   **Column 1:** Time (Optional). If numeric, it is used for the X-axis and sampling rate calculation.
*   **Column 2+:** Data. You can select which column to process via the dropdown menu.

**Example:**
```csv
Time, Voltage_kV, Current_kA
0.000, 12.5, 0.1
0.001, 12.6, 0.2
0.002, 12.4, 0.15
```

## ⚖️ License

MIT License. Feel free to use, modify, and distribute this software for personal or commercial use.

Copyright (c) 2025 Philip Leichauer
