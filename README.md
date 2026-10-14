# Espresso Extraction Dynamics Lab

An interactive, in-browser hydrodynamic and extraction kinetics simulator for espresso brewing.

## Overview
This laboratory models the non-linear relationship between mechanical grinding particle size, pump pressure profiles, and water temperature to simulate real-time extraction yield (EY) and sensory profiles.

## Theoretical Modeling
- **Hydraulic Resistance & Fluid Flow:** Formulated via a simplified Darcy's Law:
  $$Q(t) = \frac{P}{R(d, P)} \cdot f(t)$$
  incorporating dynamic puck compaction penalties when pressure exceeds 9 bar.
- **Extraction Kinetics:** Solute dissolution calculated through temperature-dependent Arrhenius rate constants and particle surface area factors.
- **Sensory Balance:** Real-time multi-dimensional mapping across acidity, caramel sweetness, roast bitterness, TDS-driven body, and balance clarity.

## Tech Stack
- Vanilla JavaScript (Zero build dependencies)
- [Chart.js](https://www.chartjs.org/) for dual-axis dynamic curves & radar matrices
- [Tailwind CSS](https://tailwindcss.com/) for UI architecture

## Live Demo
Run directly in your browser via GitHub Pages: [https://leetcook100.github.io/espresso-extraction-dynamics/]
