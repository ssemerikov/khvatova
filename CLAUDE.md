# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**khvatova** — a single-page web application implementing artificial neuron models for person recognition by features (Ukrainian: "Математична модель штучного нейрону для розпізнавання людини за її ознаками").

The project data and model parameters are tracked in a Google Spreadsheet:
https://docs.google.com/spreadsheets/d/1qDnroTtck3cY1-7W-VF9UGvZiqokbezl3jiqzFZzod4/edit?usp=sharing

The file `26_02_26.ods` is a local copy of the spreadsheet containing training data and model parameters.

## Running the Application

Open `index.html` directly in a browser — no build step, server, or dependencies required. All logic is vanilla JS in a single HTML file.

## Architecture

The entire application lives in `index.html`. Key sections (by JS comment blocks):

### Data Model
- `features[]` — 5 binary features encoded as {-1, +1}: "Білий фон", "Жовта кофта", "Дуже світле волосся", "Окуляри", "Коротке волосся"
- `DATA[]` — 6 training persons; target: Хватова=+1, all others=-1
- Feature inputs: +1 = "Так" (yes), -1 = "Ні" (no)

### Two Competing Models
1. **Perceptron (5→1)** — hardcoded weights `w[]` on `features[]` + `BIAS_W=-3`; `S = Σwᵢxᵢ + bias`, result = sign(S)
2. **Hebb Network (5→3→2→1)** — trained at page load via `trainHebb(seed)` using perceptron learning rule (error-correction), `sign()` activation throughout. `findGoodSeed()` searches seeds 1..10000 until 100% training accuracy is achieved.

### PRNG
`mulberry32(seed)` — deterministic seeded PRNG used to initialize Hebb network weights reproducibly.

### UI Flow
`update()` is called on page load and on any feature toggle change. It:
1. Runs both models on current input
2. Updates result display elements
3. Re-renders both SVG network visualizations
4. Rebuilds perceptron and Hebb detail tables

`buildComparisonTable()` runs once on init to render the training-set accuracy table.

## The `iris/` Directory

Contains the UCI Iris dataset (`iris.data`, `bezdekIris.data`, `iris.names`, `Index`) — present for potential future use or reference, not yet integrated into the application.
