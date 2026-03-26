# Heat Transfer Project MCEN 3022 Spring 2026
## Investigating Energy Use For Heating and Cooling Through Outdoor Wall Heat Transfer
## Bridget Jewell, Chiara Pesce

## Python Files: 
### **Heat_Transfer_Temp_Temp_Irr.ipynb**: Fits two-term fourier series to irradiance and temperature data for a specified month
**Functionalities:**
- Requires CSV file with temperature and GHI data from https://nsrdb.nlr.gov/data-viewer averaged over 60 minutes with at least a months worth of data in the same folder as **Heat_Transfer_Temp_Temp_Irr.ipynb** script
- *OUT_DIR* is the directory that output figures will be saved to, this is a pathname
- *N_HARM* is the number of harmonic parameters to fit in the fourier series
- *T_PER* is the length of a period in seconds, the default is 1 day (86400s)
- *MONTH* is the month the code will fit GHI and temperature functionsto
**Figure Outputs:**
|Filename|Description    |                                                |
|--------|---------------|------------------------------------------------|
|01_forcing_functions.png|Irradiance and temperature vs time in the period|
**File Outputs:**
|Filename|Description|                                                                          |
|--------|-----------|--------------------------------------------------------------------------|
|july_params.json    |Coefficients for fourier series, needed in **Heat_Transfer_Project.ipynb**|

### **Heat_Transfer_Project.ipynb**: Main script -- solves the ODE and PDE for heat transfer through a multi-layered wall subject to two boundary conditions
**Functionalities:**
- Array LAYERS is for wall materials and properties, the first material in the list is the closest to the interior and the last material is closest to the exterior. The list runs from x = 0 to x = L
- String *INDOOR_BC_MODE* has two options: *fixed_Ts* and *comfort_band*. The *fixed_Ts* setting will run the model with a constant internal surface temperature set by *T_INDOOR_SURFACE_C*. The comfort_band setting will run the model with internal convection, and allow temperature to modulate until the room temperature reaches an upper or lower bound specified by *T_comfort_lo* and *T_comfort_hi*
- Requires a .json file from **Heat_Transfer_Temp_Temp_Irr.ipynb** for irradiance and temperature functions, this has to be in the same folder as the **Heat_Transfer_Project.ipynb** script
- *OUT_DIR* is the directory that output figures will be saved to, this is a pathname
- *L_c* is the height of the wall (used for *comfort_band*)
- *h_outdoor* is the outdoor forced convection coefficient
- *H_room* is the length of air in the room perpendicular to the wall (used for *comfort_band*)
- *t_end* is the amount of time the simpulation runs in seconds, so *t_end* = 24 * 3600 would be 24hr, *dt* is the resolution of the simulation in seconds, so *dt* = 60 will evaluate every minute
**Figure Outputs**
|Filename                   |Description                                                                                            |
|---------------------------|-------------------------------------------------------------------------------------------------------|
|01_forcing_functions.png   |Irradiance and temperature fourier functions from **Heat_Transfer_Temp_Temp_Irr.ipynb**                |
|02_temperature_profiles.png|Temperature profiles for *n_prof* points in time                                                       |
|03_surface_temperatures.png|Interior surface, midpoint and exterior surface temperatures vs time                                   |
|03b_room_hvac_h.png        |Used in *comfort_band*, displays room temperature, q_hvac and the indoor natural convection coefficient|
|03b_wall_flux_fixedTs.png  |Used in *comfort_band*, displays indoor natural convection coefficient, nu, and rho for the air        |
|04_temperature_field.png   |Heatmap of temperature vs time and location in wall (x)                                                |
|05_heat_flux.png           |Interior and exterior surface flux from conduction vs time                                             |
|06_thermal_load.png        |Thermal load and cumulative energy demand vs time                                                      |
|07_summary_figure.png      |Displays four summary graphs from above                                                                |
