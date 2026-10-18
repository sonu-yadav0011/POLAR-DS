# POLAR-DS

### Mission-Aware AI Decision Support for Antarctic Research Vessels

**Smart India Hackathon 2026 — SIH26059**  
**Organization:** Ministry of Earth Sciences (MoES)  
**Department:** National Centre for Polar and Ocean Research (NCPOR)  
**Team:** Orion  
**Primary demonstration scenario:** **Cape Town → Bharati, Antarctica**

> **POLAR-DS** is a proposed AI/ML-enabled decision-support platform that combines Antarctic satellite, oceanographic, meteorological, iceberg, vessel and mission data to forecast changing conditions, quantify route-specific risk, generate safe and fuel-aware route alternatives, and continuously adapt the mission as new information arrives.
>
> The system is designed to **support—not replace—qualified navigators, captains and existing certified navigation workflows**.

---

## Table of Contents

- [1. Problem](#1-problem)
- [2. Why POLAR-DS](#2-why-polar-ds)
- [3. Core Innovation](#3-core-innovation)
- [4. Solution Overview](#4-solution-overview)
- [5. Technical Architecture](#5-technical-architecture)
- [6. MissionOps](#6-missionops)
- [7. Prediction & Intelligence](#7-prediction--intelligence)
- [8. Risk & Route Optimization](#8-risk--route-optimization)
- [9. Continuous Re-Planning](#9-continuous-re-planning)
- [10. Data Sources](#10-data-sources)
- [11. Validation Strategy](#11-validation-strategy)
- [12. Feasibility & Known Limitations](#12-feasibility--known-limitations)
- [13. Safety & Human-in-the-Loop](#13-safety--human-in-the-loop)
- [14. Expected Impact](#14-expected-impact)
- [15. Planned Product Modules](#15-planned-product-modules)
- [16. Proposed Project Structure](#16-proposed-project-structure)
- [17. Development Roadmap](#17-development-roadmap)
- [18. Suggested Visuals for the Repository](#18-suggested-visuals-for-the-repository)
- [19. Project Status](#19-project-status)
- [20. Responsible Use & Disclaimer](#20-responsible-use--disclaimer)
- [21. References](#21-references)

---

# 1. Problem

Antarctic research-vessel navigation is difficult because the environment is dynamic, observations are heterogeneous, and voyage decisions are tightly connected to the mission being executed.

### Real operational problems identified in the project research

- **Ice can turn a normal voyage into a prolonged operation.** During India's 2018–19 expedition, MV *Vasiliy Golovnin* took about **23 days instead of about 7 days** to negotiate roughly 50 km of heavy sea ice.
- **Heavy ice can consume time and fuel.** The same case involved substantial ice resistance, heavy fuel consumption, and a later decision to conserve fuel and wait for assistance.
- **Ice severity is more than “ice/no ice.”** MV *Explorer* was damaged after entering Antarctic ice that was assessed incorrectly; the case demonstrates why ice type and severity matter to vessel decisions.
- **Ice can disrupt the mission itself.** Heavy ice has prevented access to planned operational locations, while shelf-ice dynamics can affect logistics and cargo operations.
- **A route cannot be treated as permanently valid.** Environmental conditions evolve during the voyage, so route suitability needs to be reassessed as new observations and forecasts become available.

### The core problem

> **A route planned before departure may not remain the safest, most efficient or mission-feasible route when the vessel reaches it.**

---

# 2. Why POLAR-DS

Existing polar systems and research already provide important capabilities such as:

- satellite-based sea-ice observation,
- Antarctic sea-ice forecasting,
- iceberg tracking,
- environmental route optimization, and
- operational navigation expertise.

POLAR-DS is therefore **not positioned as another standalone navigation map** and does not claim to invent each component independently.

Instead, the project focuses on the **decision layer that connects prediction, uncertainty, vessel capability, mission constraints and route planning into one continuously updated mission workflow**.

### Intended value

**Observe → Predict → Assess → Optimize → Adapt → Coordinate**

---

# 3. Core Innovation

## 3.1 Mission-Aware Navigation

**Route around the mission, not just A → B.**

The captain or authorized mission planner creates a mission containing:

- Vessel
- Origin and destination
- Stops
- Waypoints
- Holding points
- Scientific activities
- Deadlines / arrival windows
- Mission priorities

Route generation then considers those constraints instead of optimizing only geographic distance.

## 3.2 Uncertainty-Aware Risk

**Show what may happen—and how certain it is.**

The system combines predicted sea-ice and iceberg conditions with:

- forecast ranges,
- model/source confidence,
- iceberg uncertainty corridors, and
- route-specific risk probability.

## 3.3 Continuous Adaptive Re-Planning

**The route evolves with the environment.**

When new information arrives:

`New Observation → Update State → Recalculate Risk → Re-evaluate Route → Generate Alternatives`

## 3.4 Shared Mission Digital Twin

**One mission. One shared operational picture.**

Authorized teams in India and Antarctica share the same evolving mission record, including:

- route,
- forecast,
- risk,
- ETA,
- tasks,
- alerts,
- comments/observations, and
- route/decision history.

The captain remains the final route-approval authority.

---

# 4. Solution Overview

POLAR-DS is designed as a **mission-level decision-support system** rather than a collection of independent ML models.

```text
┌────────────────────────────────────────────────────────────────────┐
│                         POLAR-DS                                  │
│       Mission-Aware Antarctic Decision Support                    │
└────────────────────────────────────────────────────────────────────┘
                              │
                              ▼
                 ┌────────────────────────┐
                 │ 1. CREATE MISSION      │
                 │ Vessel • Stops • Tasks │
                 │ Waypoints • Deadlines  │
                 └────────────┬───────────┘
                              ▼
                 ┌────────────────────────┐
                 │ 2. FUSE DATA           │
                 │ Satellite • Ocean      │
                 │ Weather • Icebergs     │
                 │ Vessel • Mission       │
                 └────────────┬───────────┘
                              ▼
                 ┌────────────────────────┐
                 │ 3. PREDICT             │
                 │ Sea Ice • Icebergs     │
                 │ Weather/Environment    │
                 │ + Uncertainty          │
                 └────────────┬───────────┘
                              ▼
                 ┌────────────────────────┐
                 │ 4. ASSESS RISK         │
                 │ Environment + Vessel   │
                 │ + Mission Constraints  │
                 └────────────┬───────────┘
                              ▼
                 ┌────────────────────────┐
                 │ 5. OPTIMIZE ROUTES     │
                 │ Safety • Fuel • Time    │
                 │ Mission • Robustness    │
                 └────────────┬───────────┘
                              ▼
                 ┌────────────────────────┐
                 │ 6. HUMAN DECISION       │
                 │ Captain reviews &       │
                 │ approves/changes route  │
                 └────────────┬───────────┘
                              ▼
                 ┌────────────────────────┐
                 │ 7. MISSIONOPS          │
                 │ India ↔ Antarctica      │
                 │ Shared mission state   │
                 └────────────┬───────────┘
                              │
                              └──────────────┐
                                             ▼
                                   NEW OBSERVATION
                                             │
                                             └──→ Re-plan
```

> **The important design idea:** the routing engine does not only ask *“Is this cell safe now?”*; it asks *“What conditions and risk are expected here when the vessel reaches this location?”*

---

# 5. Technical Architecture

## 5.1 Multi-Source Data Layer

POLAR-DS is designed to ingest a combination of:

### Satellite / sea ice

- Sentinel-1 SAR
- AMSR2
- Copernicus Antarctic sea-ice products
- NSIDC historical sea-ice records

### Ocean

- Ocean currents
- Wave conditions
- Sea-surface temperature
- Ocean/sea-ice state variables

### Weather / atmosphere

- ERA5 historical/reanalysis variables
- Forecast fields where available

### Icebergs

- SAR detections
- Existing iceberg products
- Historical iceberg tracks

### Terrain

- GEBCO bathymetry

### Context

- Vessel profile
- Historical voyage information
- Mission plan and constraints

---

## 5.2 Data Fusion & Quality Control

Different sources may have different:

- spatial resolutions,
- temporal sampling,
- projections,
- formats,
- coverage, and
- data latency.

POLAR-DS therefore uses a common space-time processing layer:

```text
Different Sources
       ↓
Coordinate Alignment
       ↓
Time Synchronization
       ↓
Spatial Resampling
       ↓
Missing-Data Handling
       ↓
Quality / Freshness Flags
       ↓
COMMON ANTARCTIC GRID + TIME AXIS
       ↓
ML-READY ENVIRONMENTAL DATASET
```

### Observation quality metadata

Each observation or derived field should carry metadata such as:

`Age • Coverage • Resolution • Source • Confidence`

This metadata is later used to support uncertainty estimation and decision-making.

---

## 5.3 Sea-Ice Forecasting

### Proposed approach

**Physics-aware spatio-temporal forecasting**.

Conceptually:

```text
Historical Ice
+ Recent Observations
+ Weather
+ Ocean Currents
+ Previous Ice Motion
         ↓
Spatio-Temporal ML
+ Physical Constraints / Baselines
         ↓
Future Sea-Ice Concentration
+ Ice Edge
+ Uncertainty
```

### Model-selection strategy

The project should not hard-code one model before validation.

Proposed benchmark path:

- **Baseline:** Persistence
- **ML candidates:** U-Net / ConvLSTM / temporal-attention models
- **Advanced candidate:** Transformer, only if it provides measurable improvement

The selected model will be based on historical hindcast performance, compute requirements and forecast calibration—not model popularity.

### Forecast horizons

The system is designed to support multiple horizons such as:

`6 h • 12 h • 24 h • 48 h • 72 h • longer horizons where validated`

The exact supported horizon will depend on validation results and data availability.

---

## 5.4 Iceberg Intelligence

### Detect → Track → Predict

```text
SAR / Existing Tracks
        ↓
Iceberg Detection
        ↓
Multi-Time-Step Tracking
        ↓
Ocean Current + Wind + Waves
        ↓
Physics-Based Drift
        +
ML Residual Correction
        ↓
Trajectory Ensemble
        ↓
Uncertainty Corridor
```

### Planned outputs

- Current position
- Estimated velocity
- Predicted trajectory
- Prediction confidence
- Uncertainty corridor
- Route encounter probability

A core principle is:

> **No detection does not mean no iceberg.**

Sparse observations are treated explicitly as uncertainty rather than as proof of absence.

---

## 5.5 Environmental Risk Engine

The environmental state is converted into a **time-dependent, vessel-specific risk field**.

### Environmental inputs

- Sea-ice concentration
- Ice type / thickness where available
- Ice drift / convergence
- Icebergs
- Wind
- Waves
- Ocean currents
- Bathymetry
- Forecast uncertainty

### Vessel inputs

- Speed
- Draft
- Fuel state/model
- Ice capability
- Operating limits

### Mission inputs

- Destination
- Stops
- Waypoints
- Holding points
- Scientific tasks
- Deadlines
- Mission priorities

### Conceptual risk field

```text
Environment Forecast
        +
Vessel Capability
        +
Mission Constraints
        +
Forecast Uncertainty
        ↓
DYNAMIC MISSION RISK FIELD
```

The platform can use an applicable vessel/ice-operability framework for ice-related operational constraints rather than inventing a new maritime safety standard.

---

# 6. MissionOps

## One mission. One evolving record.

MissionOps is the collaboration and mission-management layer of POLAR-DS.

### Mission creation

The captain/authorized planner can define:

- Mission name and type
- Vessel
- Origin
- Destination
- Stops
- Waypoints
- Holding points
- Scientific activities
- Schedule/deadlines
- Mission priorities
- Route preferences

### Mission state

The system stores:

```text
MISSION
├── Vessel
├── Objectives
├── Route plan
├── Current recommended route
├── Approved route
├── Waypoints / stops
├── Holding points
├── Tasks / scientific activities
├── ETA / schedule
├── Forecasts
├── Risk state
├── Alerts
├── Comments / observations
├── Route versions
└── Decision history
```

### India ↔ Antarctica collaboration

```text
                 SHARED MISSION
                       │
          ┌────────────┴────────────┐
          │                         │
     ANTARCTICA                    INDIA
  Captain / Navigator       Operations / Scientists
          │                         │
          └────────────┬────────────┘
                       ▼
               Shared Mission State
```

### Permissions

A role-based design is planned so that different users can have different capabilities.

| Role | Typical capabilities |
|---|---|
| Captain / Master | Create mission, review, approve, modify, override route recommendations |
| Navigator | Analyze conditions, generate route options, provide recommendations |
| Operations Team | Mission planning, updates, comments, status monitoring |
| Scientist | Scientific activities, waypoints, mission notes |
| Ice / Weather Analyst | Environmental annotations, alerts, expert observations |
| Administrator | Users, permissions, mission administration |

### Route versioning

Every major route decision should be auditable:

```text
Mission v1.0
   ↓
Route v1.1
   ↓
New observation / risk change
   ↓
Route v1.2
   ↓
Captain approval
```

The goal is not only collaboration, but also **traceable decision history**.

---

# 7. Prediction & Intelligence

POLAR-DS separates the prediction problem into complementary components.

## Sea-ice intelligence

**Input:** historical ice + current observations + weather + ocean + motion

**Output:** future SIC / edge / uncertainty

## Iceberg intelligence

**Input:** SAR / tracks + environmental forcing

**Output:** trajectories + uncertainty corridor + encounter probability

## Environmental state

Predictions are combined into a common, time-dependent environmental representation.

---

# 8. Risk & Route Optimization

## 8.1 Route objectives

The route engine is intended to optimize several competing objectives:

- **Safety**
- **Fuel efficiency**
- **Travel time**
- **Mission completion**
- **Forecast uncertainty**
- **Robustness**

### Conceptual objective

A route cost can be represented as:

```text
Route Cost =
    Safety/Risk
  + Fuel
  + Time
  + Iceberg Exposure
  + Forecast Uncertainty
  + Mission Penalties
```

The exact weights should remain configurable and should be validated rather than fixed arbitrarily.

## 8.2 Multiple route alternatives

POLAR-DS should return alternatives rather than a single unexplained route:

- **Safety-first**
- **Balanced**
- **Fuel-efficient**
- **Time-efficient**
- **Robust**

This lets the navigator understand the trade-off instead of treating one route as universally optimal.

## 8.3 Robust route testing

Candidate routes should be tested under multiple plausible forecast scenarios:

```text
Forecast Scenario 1 ─┐
Forecast Scenario 2 ─┤
Forecast Scenario 3 ─┼→ Candidate Route → Feasibility / Risk
Forecast Scenario 4 ─┤
Forecast Scenario N ─┘
```

A robust route is one that remains feasible and acceptable across a broad range of plausible conditions, not merely the route that is optimal under one forecast.

---

# 9. Continuous Re-Planning

The environment changes after departure. POLAR-DS therefore treats route planning as a loop.

```text
NEW SATELLITE / WEATHER / OCEAN / VESSEL OBSERVATION
                     ↓
                UPDATE STATE
                     ↓
               UPDATE FORECAST
                     ↓
                  UPDATE RISK
                     ↓
             RE-EVALUATE ROUTE
                     ↓
          NEW ALTERNATIVE ROUTES
                     ↓
                HUMAN REVIEW
                     ↓
             CAPTAIN APPROVAL
                     ↓
                  MISSION
                     │
                     └────────────→ NEW OBSERVATION
```

### Alert examples

- Route enters deteriorating ice conditions
- Predicted iceberg corridor approaches route
- Mission waypoint may become inaccessible
- ETA moves outside the mission window
- Fuel margin becomes insufficient for continued difficult-ice operation

The system should use **meaningful event-driven alerts**, rather than excessive notifications.

---

# 10. Data Sources

The initial design prioritizes open and accessible scientific datasets.

| Source | Planned use |
|---|---|
| **Sentinel-1** | SAR observations; sea-ice/iceberg information |
| **AMSR2** | Large-area sea-ice concentration information |
| **Copernicus Marine** | Ocean currents, waves, ocean/sea-ice variables |
| **ERA5** | Historical weather and atmospheric variables |
| **NSIDC** | Historical sea-ice datasets |
| **USNIC** | Antarctic iceberg tracking information |
| **GEBCO** | Bathymetry |
| **Copernicus Antarctic Sea-Ice Products** | Integrated/high-resolution sea-ice observations |
| **Historical voyage / mission data** | Route replay and validation where available |
| **Vessel data** | Speed, draft, ice capability, fuel/performance model |

### Data-access principle

> Use the most recent synchronized information available for each source; do not assume that every dataset is continuously live.

### Example data lineage

```text
Raw Data
  ↓
Validated / Quality Flagged
  ↓
Common Space-Time Grid
  ↓
Derived Features
  ↓
Forecast Models
  ↓
Risk Field
  ↓
Route Optimization
  ↓
Mission Decision
```

---

# 11. Validation Strategy

A major design principle of POLAR-DS is:

> **Prove the system on historical Antarctic conditions before considering operational deployment.**

## 11.1 Sea-ice validation

Compare predicted and observed sea-ice conditions using:

- MAE
- RMSE
- Ice-edge error
- spatial agreement metrics where appropriate

## 11.2 Iceberg validation

Compare predicted and observed trajectories using:

- position error,
- trajectory deviation, and
- uncertainty/coverage calibration.

## 11.3 Route validation

Compare candidate routes against baselines using:

- route risk exposure,
- distance,
- modelled fuel,
- travel time,
- ETA deviation, and
- mission-constraint compliance.

## 11.4 Adaptive-response validation

Measure:

- observation → risk-update time,
- risk-update → new-route time,
- ability to detect route deterioration, and
- mission impact after a re-planning event.

## 11.5 Historical replay

The system should support a replay mode:

```text
Historical Mission
       ↓
Reconstruct what was known at time T
       ↓
Run POLAR-DS
       ↓
Generate prediction / risk / route
       ↓
Compare with later observations and actual mission data
```

This makes it possible to evaluate the concept without requiring live-vessel access in the initial research phase.

---

# 12. Feasibility & Known Limitations

POLAR-DS is intentionally designed around known limitations rather than assuming perfect data or perfect AI.

| Challenge | Planned mitigation |
|---|---|
| Different spatial/temporal resolutions | Common spatio-temporal grid |
| Missing / delayed observations | Quality and freshness flags; robust data handling |
| Forecast uncertainty | Ensemble/range reporting and calibration |
| Sparse iceberg observations | SAR + historical tracks + physics + ML + uncertainty corridor |
| Limited public vessel-performance data | Configurable vessel digital twin; replace assumptions with operator data when available |
| Connectivity interruptions | Local cache / edge-capable workflow + periodic synchronization |
| AI recommendation errors | Human approval + explanations + confidence + alternatives |
| Rapid environmental changes | Continuous monitoring + dynamic re-planning |
| Real-world deployment risk | Historical replay → simulation → expert review → controlled pilot |

### Important limitations

- **Not every Antarctic location will have the same observation quality.**
- **No iceberg dataset should be interpreted as a complete inventory of every iceberg.**
- **Forecast uncertainty increases with prediction horizon and depends on the environmental regime and model.**
- **Fuel/performance estimates depend on vessel-specific information and operating conditions.**
- **A prototype result is not equivalent to certified maritime navigation performance.**

---

# 13. Safety & Human-in-the-Loop

POLAR-DS is conceived as a **decision-support layer**, not an autonomous navigation system.

### AI responsibilities

- Forecast
- Assess
- Compare
- Explain
- Recommend

### Human responsibilities

- Review
- Approve
- Modify
- Reject
- Override

Every route recommendation should expose:

- **Why this route?**
- **What data supports it?**
- **How fresh is the data?**
- **How uncertain is the forecast?**
- **What alternatives exist?**
- **What changes if the route is rejected?**

This separation is central to the project's safety and adoption strategy.

---

# 14. Expected Impact

POLAR-DS is intended to convert changing Antarctic observations into measurable mission-level outcomes.

## Mission continuity

Support earlier identification of conditions that may affect:

- route feasibility,
- ETA,
- planned stops,
- scientific activities, and
- station operations.

**Measure:** ETA error, delay, mission objective completion.

## Fuel & time efficiency

Compare routes using the trade-off between:

**Safety ↔ Fuel ↔ Time ↔ Mission requirements**

**Measure:** modelled fuel, distance, transit time and difficult-ice exposure.

## Safety & decision quality

Move from *“ice detected”* to *“route-specific risk understood.”*

**Measure:** risk exposure, sea-ice forecast error, iceberg trajectory error, calibration and false alerts.

## Operational coordination

Keep the vessel, scientists, operations staff and India ground teams synchronized through a shared mission state.

**Measure:** update latency, synchronization, decision history and alert response time.

### Success principle

> **POLAR-DS should not promise a fixed percentage improvement before validation. Its performance should be demonstrated through historical replay, simulation and baseline comparison.**

---

# 15. Planned Product Modules

```text
POLAR-DS
│
├── MissionOps
│   ├── Mission creation
│   ├── Stops / waypoints / holding points
│   ├── Scientific tasks
│   ├── Deadlines
│   ├── Collaboration
│   └── Route / decision history
│
├── Data Hub
│   ├── Satellite
│   ├── Sea ice
│   ├── Weather
│   ├── Ocean
│   ├── Icebergs
│   ├── Bathymetry
│   └── Vessel data
│
├── Data Fusion
│   ├── Spatial alignment
│   ├── Time synchronization
│   ├── Resampling
│   ├── Missing data
│   └── Quality / freshness
│
├── Prediction Engine
│   ├── Sea-ice forecasting
│   ├── Iceberg detection
│   ├── Iceberg tracking
│   ├── Iceberg drift prediction
│   └── Uncertainty estimation
│
├── Risk Engine
│   ├── Ice risk
│   ├── Iceberg risk
│   ├── Weather / ocean risk
│   ├── Vessel impact
│   └── Mission impact
│
├── Route Engine
│   ├── Safety-first
│   ├── Balanced
│   ├── Fuel-efficient
│   ├── Time-efficient
│   └── Robust route
│
├── Re-planning Engine
│   ├── New-data ingestion
│   ├── Risk update
│   ├── Route re-evaluation
│   └── Alternative generation
│
└── Decision Support UI
    ├── Map
    ├── Forecasts
    ├── Risk
    ├── Route comparison
    ├── Alerts
    ├── Explainability
    └── Mission collaboration
```

---

# 16. Proposed Project Structure

A modular repository structure is recommended so each major subsystem can be developed and validated independently.

```text
polar-ds/
│
├── README.md
├── LICENSE
├── .gitignore
├── .env.example
├── pyproject.toml
├── requirements.txt
├── docker-compose.yml
│
├── docs/
│   ├── architecture.md
│   ├── data-sources.md
│   ├── model-design.md
│   ├── validation.md
│   └── operations.md
│
├── data/
│   ├── raw/
│   ├── interim/
│   ├── processed/
│   └── examples/
│
├── notebooks/
│   ├── 01_data_exploration.ipynb
│   ├── 02_sea_ice_baseline.ipynb
│   ├── 03_sea_ice_forecast.ipynb
│   ├── 04_iceberg_tracking.ipynb
│   ├── 05_route_optimization.ipynb
│   └── 06_historical_replay.ipynb
│
├── src/
│   └── polar_ds/
│       ├── config/
│       ├── data/
│       ├── fusion/
│       ├── forecasting/
│       ├── iceberg/
│       ├── risk/
│       ├── routing/
│       ├── mission/
│       ├── validation/
│       └── utils/
│
├── api/
│   └── main.py
│
├── frontend/
│   └── README.md
│
├── tests/
│   ├── unit/
│   ├── integration/
│   └── validation/
│
├── assets/
│   ├── architecture/
│   ├── screenshots/
│   └── figures/
│
└── scripts/
    ├── download_data.py
    ├── preprocess_data.py
    ├── train_models.py
    ├── run_forecast.py
    └── run_replay.py
```

> **Recommendation:** keep raw scientific data outside Git when files are large; store download instructions, metadata and reproducible scripts in the repository.

---

# 17. Development Roadmap

## Phase 1 — Data foundation

- Identify final dataset versions
- Build download/ingestion scripts
- Establish common spatial/temporal representation
- Add quality/freshness metadata
- Create reproducible preprocessing pipeline

## Phase 2 — Baselines

- Persistence sea-ice baseline
- Simple physical iceberg drift baseline
- Baseline route optimizer
- Baseline fuel/ETA model

## Phase 3 — Predictive intelligence

- Benchmark sea-ice ML architectures
- Build iceberg detection/tracking pipeline
- Add physics + ML drift correction
- Calibrate predictive uncertainty

## Phase 4 — Risk and routing

- Build dynamic risk field
- Add vessel constraints
- Add mission constraints
- Implement multi-objective routing
- Add robust-route testing

## Phase 5 — MissionOps

- Mission creation
- Waypoints/stops/holding points
- Mission timeline
- Shared mission state
- Role-based collaboration
- Route and decision history

## Phase 6 — Continuous operations

- New-observation ingestion
- Automatic risk refresh
- Route re-evaluation
- Alternative generation
- Human approval workflow

## Phase 7 — Validation and demonstration

- Historical Cape Town → Bharati replay
- Forecast-vs-observation evaluation
- Iceberg trajectory validation
- Baseline route comparison
- Re-planning scenarios
- Expert/domain review

---

# 18. Suggested Visuals for the Repository

A professional README should include a few strong visuals instead of long blocks of text.

## Recommended image 1 — One-page system overview

**Place after the Solution Overview section.**

Suggested content:

`Data → Forecast → Risk → Mission → Route → Human Decision → Re-plan`

## Recommended image 2 — Technical architecture

**Place after Technical Architecture.**

Show:

`Data Sources → Fusion → Prediction → Risk → Routing → MissionOps`

## Recommended image 3 — MissionOps concept

Show a Cape Town → Bharati mission with:

- vessel,
- waypoints,
- scientific stop,
- holding point,
- deadline,
- route alternatives, and
- India ↔ Antarctica synchronization.

## Recommended image 4 — Real historical validation

**Strongly recommended.**

Use a real-data figure such as:

```text
Observed Sea-Ice     Forecast Sea-Ice      Forecast Error
     [map]                 [map]               [map]
```

Label it clearly as **historical hindcast / validation output**.

## Recommended image 5 — Route comparison

Show multiple candidate routes on the same Antarctic map with a small comparison table:

| Route | Risk | Fuel | ETA | Robustness |
|---|---:|---:|---:|---|
| Safety | Low | Higher | Longer | High |
| Balanced | Low–Med | Medium | Medium | High |
| Fuel | Med | Lower | Medium | Medium |

Do not use fabricated metrics; replace the example values with results from validated runs.

## Recommended image 6 — Re-planning timeline

```text
Observation → Forecast update → Risk change → Re-plan → Captain approval
```

## Repository media recommendation

Store diagrams/screenshots under:

```text
assets/
├── architecture/
├── screenshots/
├── figures/
└── demo/
```

Use descriptive file names such as:

- `polar-ds-system-architecture.png`
- `missionops-workflow.png`
- `sea-ice-hindcast-example.png`
- `route-comparison.png`
- `dynamic-rerouting-demo.png`

---

# 19. Project Status

> **Current status: Solution design and research phase**

The architecture, problem framing, feature set, validation strategy and initial dataset plan are being defined before full implementation.

### Current design decisions

- Demonstration corridor: **Cape Town → Bharati**
- Open scientific data is the primary prototype foundation
- MissionOps is a first-class module
- Prediction includes uncertainty rather than only point forecasts
- Route optimization is multi-objective
- Re-planning is part of the core loop
- Human approval remains central
- Historical replay is a primary validation method
- No unsupported performance percentages are claimed before testing

---

# 20. Responsible Use & Disclaimer

POLAR-DS is a research and development project intended to demonstrate an **AI-assisted decision-support workflow for Antarctic research-vessel operations**.

It is **not** presented as a replacement for:

- certified maritime navigation systems,
- official nautical charts/advisories,
- vessel operating procedures,
- Polar Code requirements,
- professional Ice Navigator / bridge-team judgment, or
- operator/NCPOR-approved procedures.

Any future operational deployment would require appropriate:

1. scientific validation,
2. vessel-specific testing,
3. domain-expert review,
4. cybersecurity and reliability assessment,
5. integration testing, and
6. regulatory/operational approval as applicable.

---

# 21. References

## Research & Existing Systems

| Research / System | Relevance |
|---|---|
| **SOIPS v1.0 — Southern Ocean Ice Prediction System** | Antarctic sea-ice forecasting |
| **BAS PolarRoute** | Polar vessel route optimization |
| **BAS Digital Twins of Polar Regions** | Polar environmental intelligence and routing |
| **Copernicus Antarctic Sea-Ice Product** | High-resolution Antarctic sea-ice observations |
| **USNIC Antarctic Iceberg Tracking** | Antarctic iceberg tracking |
| **NCPOR Annual Reports** | Indian Antarctic expedition operational evidence |

## Datasets & Official Sources

| Source | Primary use in POLAR-DS |
|---|---|
| **Copernicus Marine** | Ocean currents, waves and sea-ice/ocean variables |
| **ERA5 — Copernicus Climate Data Store** | Weather and atmospheric variables |
| **GEBCO** | Bathymetry |
| **NSIDC** | Historical sea-ice datasets |
| **NCPOR** | Indian Antarctic operations and advisories |
| **USNIC** | Antarctic iceberg tracking |

## Key Links

### Research / Systems

- **SOIPS v1.0:** https://gmd.copernicus.org/articles/17/6867/2024/
- **BAS PolarRoute:** https://github.com/bas-logist/PolarRoute
- **BAS Digital Twins of Polar Regions:** https://www.bas.ac.uk/project/digital-twins-of-the-polar-regions/
- **Copernicus Antarctic Sea-Ice Product:** https://data.marine.copernicus.eu/product/SEAICE_ANT_PHY_AUTO_L3_NRT_011_012/description
- **USNIC Antarctic Iceberg Tracking:** https://usicecenter.gov/Catalog/AntarcIceberg
- **NCPOR:** https://ncpor.res.in/

### Datasets

- **Copernicus Marine:** https://data.marine.copernicus.eu/
- **ERA5:** https://cds.climate.copernicus.eu/datasets/reanalysis-era5-single-levels
- **GEBCO:** https://www.gebco.net/data-products-gridded-bathymetry-data/gebco2026-grid
- **NSIDC:** https://nsidc.org/

---

## Evidence Base for the Problem Definition

The project research reviewed documented Antarctic maritime incidents including:

- MV *Explorer* (2007)
- MV *Vasiliy Golovnin* (2018–19)
- Indian Barrier cargo incident (2012)
- MV *Ivan Papanin* (2018)

The research emphasizes that ice conditions can affect vessel safety, mission time, fuel requirements, logistics and route feasibility, and that route evaluation should continue as conditions evolve.

The project team has also incorporated operational insights from conversations with people connected to Antarctic field and expedition operations. These conversations are treated as **field input for problem framing**, not as a substitute for formal operational validation.

---

## Core Project Statement

> **POLAR-DS transforms changing Antarctic observations into mission-aware, uncertainty-aware and continuously updated decision support—helping human navigators compare risk, fuel, time and mission impact while keeping ship and shore teams synchronized.**

---

### Built for research. Designed for real constraints. Validated before operational use.

