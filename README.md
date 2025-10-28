---
TITLE: Tunnel-Transient-Heat-Transfer
AUTHOR: Priyanka Rajeev Hichkad
---

## Overview :

Tunnel Transient Heat Transfer is a simulation project dedicated to understanding and visualizing how heat dissipates from the hot rock around a newly excavated, ventilated mine tunnel. This is directly applicable to real-world mining operations where safe, efficient, and cost-effective cooling is essential.

***

## What is Mine Ventilation ?

Mine ventilation is the process of supplying fresh air to underground mine workings in order to remove heat, hazardous gases, and dust. It ensures:
- **Safety**: Removes harmful gases and regulates oxygen.
- **Comfort**: Prevents dangerous heat buildup for workers.
- **Operational Efficiency**: Enables productive, sustained mining even at great depths.
- **Energy Optimization**: Supports planning for cooling and ventilation energy use.

As tunnels get deeper, high temperatures and poor air quality increase risks for personnel and equipment, making efficient ventilation and cooling strategy a key design consideration.

***

## Problem Statement :

When a tunnel is created underground, the surrounding rock is much hotter than the ventilating air. As cooler air flows in, it absorbs heat from the rock wall, and the temperature near the wall declines with time and with distance into the rock. Predicting this cooling process, especially for varying air speeds and rock properties, is critical for:
- Worker safety and comfort.
- Proper timing of tunnel infrastructure installation.
- Optimizing ventilation and energy usage.

***

## Parameters Used :

| Parameter   | Description                                | Value/Unit           |
|-------------|--------------------------------------------|----------------------|
| `rho`       | Rock density                               | 2500 kg/m³           |
| `cp`        | Rock specific heat                         | 880 J/kg/K           |
| `k`         | Rock thermal conductivity                  | 2.5 W/m/K            |
| `h`         | Tunnel wall heat transfer coefficient      | 15 W/m²/K            |
| `T_air`     | Tunnel air temperature                     | 15.0 °C              |
| `T0`        | Initial rock temperature                   | 30.0 °C              |
| `T_outer`   | Far field rock temperature                 | 30.0 °C              |
| `r1`        | Tunnel inner (wall) radius                 | 2.0 m                |
| `r2`        | Outer simulated radius                     | 10.0 m               |
| `t_sim_min` | Total simulation time                      | 2880 min (2 days)    |
| `nr`        | Number of radial mesh points               | 100                  |
| `nt`        | Number of time steps                       | 400                  |
| `dr`        | Radial step size                           | 0.0808 m             |
| `dt`        | Time step                                  | 432 s                |
| `alpha`     | Thermal diffusivity (k/(rho*cp))           | ~1.136e-6 m²/s       |
| `air_velocities` | Simulated air velocities              | [11][12] m/s        |
| `nu`        | Air kinematic viscosity (convection calc)  | 1.5e-5 m²/s          |
| `k_air`     | Air conductivity (convection calc)         | 0.026 W/m/K          |
| `D`         | Tunnel diameter (convection calc)          | 4.0 m                |

***

## Simulation Approach :

### 1. **Conduction Simulation -**

- **Why**: Models the case where the tunnel wall temperature is forced to that of the air instantly (idealized or very strong cooling).
- **How**: Solves the 1D transient heat equation in cylindrical coordinates, using the finite difference method.
- **Result**: Produces a smooth temperature gradient as heat diffuses steadily from the hot rock to the cooler air.

> *The temperature profile due to conduction forms a smooth gradient as heat slowly diffuses from the hotter rock towards the tunnel air by direct molecular contact.*

***

### 2. **Convection Simulation (with Changing Air Velocity) -**

- **Why**: In real mines, cooling depends on airflow speed; high velocity cools the wall much faster than still air.
- **How**: The wall cooling is dynamically computed at each timestep using a Nusselt number formula that accounts for airflow velocity:
  ```
  Nu = (0.35 * Re) / (1 + 1.592 * ((15.217 * Re^0.2 - 1) / Re^0.125))
  ```
  (where `Re` is the Reynolds number based on current air velocity, tunnel diameter, and air viscosity).
- **Result**: The profile near the wall steepens rapidly for higher velocities—heat is carried away by the air much more efficiently.

> *The temperature profile due to convection changes more rapidly near the wall because moving air carries heat away quickly, steepening the temperature gradient.*

***

## Results

### Conduction (Fixed Wall Temperature) -

![Conduction Temperature Profile](https://github.com/PriyankaHichkad/Tunnel-Transient-Heat-Transfer/blob/main/Tunnel%20Conduction%20Temperature%20Profile.png)  
The temperature profile obtained from the conduction simulation shows a smooth and gradual decline in temperature from the tunnel wall into the surrounding rock. This is characteristic of heat conducting slowly outward by molecular diffusion, resulting in a gentle thermal gradient that changes steadily with time and distance.

***

### Convection (Variable Wall Cooling with Air Velocity) -

![Convection Temperature Profile](https://github.com/PriyankaHichkad/Tunnel-Transient-Heat-Transfer/blob/main/Tunnel%20Convection%20Temperature%20Profile.png)  
The convection simulation reveals that when air velocity is increased, the tunnel wall cools much more rapidly, leading to a steeper temperature gradient near the face. The effect of forced ventilation is prominent: rock temperature near the tunnel drops sharply compared to the conduction-only case, and cooling penetrates further into the rock for the same simulation time.

***

*These visual results confirm the greater efficiency of convective cooling via ventilation compared to pure conductive cooling through rock, highlighting the importance of airflow management in safe and energy-efficient mine tunnel design.*

***

## Key Learnings :

- Physical modeling and Python simulation can robustly predict mine tunnel cooling for both basic and advanced cases.
- Ventilation velocity is a critical control variable for rapid wall cooling and mine climate safety.
- The choice of boundary (instant cooling vs. velocity-dependent) drastically affects predictions—realistic boundaries are important for mine design.
- Careful step selection for grid and time values, and regular code debugging and visualization, are essential for stable, reliable simulations.

***

## Challenges and Limitations :

- **Numerical Stability**: Choice of timestep and grid size affects simulation accuracy and stability.
- **Parameter Uncertainty**: Real rock and air properties (thermal, physical) can be variable and uncertain.
- **Geometry**: Real tunnels can be non-cylindrical or have complex boundaries, requiring more advanced models.
- **Boundary Effects**: Actual air-rock contact resistance and airflow patterns may differ from simple theoretical assumptions.

***

## Contributing :

Thank you for exploring this project!  
If you find an issue, have suggestions or code improvements, or want to contribute your own approaches, feel free to open an issue or submit a pull request. Collaboration and feedback are welcome!

***

*Mining Engineering, IIT BHU, Varanasi*  
*Contact: priyanka.rhichkad.min23@itbhu.ac.in*

***
