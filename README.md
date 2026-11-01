# Reflets: a digital twin of the Paris Fire Brigade

Team project (PSC, École polytechnique, 2025–2026) with Ferran Roset Pastor, Baptiste Duperray, Gaspard Lasorne and Ylan Marx, in partnership with the Brigade de sapeurs-pompiers de Paris (BSPP).

> The code and data are private (they rely on real BSPP operational data). This page describes the project; the code is available on request.

## The problem

The BSPP handles hundreds of thousands of interventions a year from dozens of fire stations. Where each vehicle (fire engine, ambulance...) is stationed drives response times. Reflets answers two questions:

- **What happens** if we change the fleet allocation? A simulator replays a full year of real interventions, minute by minute, with the vehicle that would actually be sent.
- **Where should the vehicles be?** An optimiser searches for the allocation that minimises response time or another operational metric.

## How it works

- **Simulation**: chronological replay of the interventions. Each one is assigned the nearest available suitable vehicle, with geographic fallback when the local station is out of vehicles. Vehicles are tracked through their busy and unavailable periods.
- **Travel times**: after benchmarking several approaches (MLP, random forest, k-nearest neighbours, precomputed road matrices), the simulator uses precomputed travel-time matrices (Valhalla on OpenStreetMap) corrected by time of day, with a nearest-neighbour fallback for unseen locations.
- **Optimisation**: simulated annealing, genetic algorithm and multi-start local search over vehicle-to-station allocations. A GPU version evaluates thousands of candidate allocations in parallel with PyTorch, which made full-year optimisations practical.
- **Metrics**: mean response time, 95th/99th percentile for ambulances, share of interventions reached in under 8 minutes, load balance between stations, hours with no vehicle available.
- **Interface**: a Streamlit web app to run simulations, launch optimisations and compare allocations on a map.

## Results

Replayed over a full year, the optimised allocation reduces the mean response time by about 1.2 % while moving only about 12 % of the fleet. Other runs target tail response times or load balance between stations instead.

## My contributions

- **Travel-time models**: benchmark of the travel-time methods with Baptiste Duperray (MLP, random forest, k-nearest neighbours, precomputed matrices) and quantification of their prediction uncertainty, which led to the precomputed-matrix approach used in the simulator.
- **Optimisation**: benchmark and implementation of the allocation algorithms, and the experiments showing the gains above.
- **Interface**: the Streamlit web app.

## Stack

Python, pandas, NumPy, scikit-learn, PyTorch, SciPy, Valhalla / OpenStreetMap, Streamlit.
