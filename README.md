# Adaptive Digital Twin Learning Environment — Unreal Engine 5.3

This package documents the reproducible Unreal Engine implementation specification for the environment-aware digital-twin learning study.

## Package contents

- `Unreal_Blueprint_Specification.docx` — complete technical specification.
- `membership_functions.csv` — Mamdani membership-function parameters.
- `fuzzy_rules.csv` — 12-rule IF–THEN controller table.
- `scenario_matrix.csv` — 12 controlled weather × time scenarios.
- `experiment_log.xls` — run-level logging schema.
- `controller_config.json` — machine-readable controller configuration.
- `UE5_Project_Skeleton/` — Unreal Engine 5.3 project descriptor and folder structure.

## Unreal Engine implementation

The project uses standard Unreal Engine 5.3 Blueprint concepts: Actor Blueprints, variables, functions, Data Tables/Structs, timers, HTTP/JSON handling, Light Components, Cine Camera Actor, and file/data logging.

The binary `.uasset` files are not included because those assets must be created/saved by Unreal Engine Editor itself. The supplied node-level specification is intended to be implemented directly in UE 5.3.

## Controller sequence

1. Initialize system.
2. Acquire weather data or load controlled scenario data.
3. Convert weather condition to controller index.
4. Read local hour.
5. Fuzzify weather and time.
6. Evaluate 12 Mamdani rules.
7. Aggregate rule consequents.
8. Apply centroid defuzzification.
9. Set Key, Fill, and Back light source intensity.
10. Render/capture the virtual camera view.
11. Log inputs, memberships, rule activations, outputs, and metadata.

## Experiment structure

3 weather states × 4 time states = 12 scenarios.
15 runs per scenario = 180 run-level observations.
Each run produces a correlated Key/Fill/Back triplet = 540 light-output values.

The three outputs from a run must remain linked by `Run_ID` during statistical analysis.

## Camera settings

- ISO: 400
- Shutter: 1/48 s
- Aperture: f/4.0

## Weather synchronization

OpenWeather is queried every 5 minutes. If the request fails or the latest valid observation is older than the specified 10-minute freshness threshold, the controller falls back to local computer time and a clear-weather state.

## Data source flag

Each experimental run should record one of:
- `live`
- `simulated`
- `replayed`

This distinguishes real observations from controlled scenarios and replayed states.

## Important implementation note

The manuscript specifies several controller parameters as provisional. The package preserves those values as the documented controller specification and does not silently replace them with other values. Any production release should verify the values against the actual project configuration before claiming exact archival reproducibility.
