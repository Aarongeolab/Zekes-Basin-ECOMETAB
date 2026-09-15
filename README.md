### Quantifying Seasonal Ecosystem Metabolism of Zeke’s Island Reserve, North Carolina

### Meteorological Data
 
- Wind speed & air temperature: Retrieved from Brunswick County Airport, ~8.2 mi from study site, via NC State Climate Office CRONOS/Cardinal data export system. 
- Air pressure: Retrieved from Wilmington International Airport (ILM), ~20 mi from study site, via Iowa Environmental Mesonet ASOS-METAR data export system. Notably, historical sea level pressure public data for this study site is sparse; however as a physical driver of water quality this parameter exhibits less spatial variability than wind speed/air temp.
## Methods
 
Net ecosystem metabolism (NEM), gross primary production (GPP), and total respiration (Rt) were calculated using the `ecometab` function from the `SWMPr` R package (called from Python via `rpy2`). `ecometab` implements the Odum open-water method (Odum, 1956). It employs the diel pattern of dissolved oxygen, corrected for air-water gas exchange (via wind speed and barometric pressure) and site-specific solar day length (from latitude/longitude/timezone), to partition net O2 flux into daytime production and nighttime respiration values.

- **Latitude / Longitude**: `33.954680, -77.934807`
- **Timezone**: `America/New_York`
- **Units**: `mmol O2/m^2/day`

### 1. Dissolved Oxygen Mass Balance
$$\frac{dC}{dt} = P_g - R_t + D$$

*   $C$: Dissolved oxygen concentration ($mg \cdot L^{-1}$ or $mmol \cdot m^{-3}$)
*   $P_g$: Volumetric hourly gross primary production rate ($mg \cdot L^{-1} \cdot hr^{-1}$)
*   $R_t$: Volumetric hourly ecosystem respiration rate ($mg \cdot L^{-1} \cdot hr^{-1}$)
*   $D$: Volumetric hourly air-water gas exchange (diffusion) flux ($mg \cdot L^{-1} \cdot hr^{-1}$)

### 2. Air-Water Gas Exchange - Atmospheric Diffusion
The gas exchange at each time step is determined by the oxygen deficit gradient and a temperature-corrected volumetric reaeration coefficient (Caffrey, 2004):
$$D = k(C_s - C)$$
$$k = k_{20} \cdot \theta^{(T - 20)}$$

*   $C_s$: Dissolved oxygen concentration at 100 percent saturation calculated from salinity  and water temperature
*   $k$: Volumetric reaeration coefficient ($hr^{-1}$) adjusted for water temperature $T$ ($^\circ\text{C}$)
*   $\theta$: Empirical temperature correction constant (defaults to $1.0241$)

### 3. Daily Ecosystem Respiration ($R_t$)

Since organisms cannot photosynthesize without sunlight ($P_g = 0$), the hourly respiration rate is calculated within study area nighttime windows determined by coordinates (`33.954680, -77.934807`) and `America/New_York` timezone:

$$R_{\text{hourly, night}} = D_{\text{night}} - \left(\frac{dC}{dt}\right)_{\text{night}}$$

The mean nighttime rate ($\overline{R}_{\text{hourly, night}}$) is assumed constant over the 24-hour cycle and scaled by the daily mean depth ($H$) to output total areal consumption:

$$Rt = \left( \overline{R}_{\text{hourly, night}} \times 24 \right) \times H$$

### 4. Gross Primary Production ($P_g$)

Daytime production at each interval corrects the observed change in daytime DO for diffusion and baseline dark respiration:

$$P_{\text{hourly, day}} = \left(\frac{dC}{dt}\right)_{\text{day}} - D_{\text{day}} + \overline{R}_{\text{hourly, night}}$$

Summing across all daylight intervals yields the integrated daily areal production rate:

$$Pg = \left( \sum_{\text{sunrise}}^{\text{sunset}} P_{\text{hourly, day}} \right) \times H$$
### 5. Net Ecosystem Metabolism (NEM)
The daily integrated net metabolic balance is computed directly as:
$$NEM = Pg - Rt$$

*   **Pg**: Gross Primary Production ($mmol \cdot O_2 \cdot m^{-2} \cdot d^{-1}$)
*   **Rt**: Total Ecosystem Respiration ($mmol \cdot O_2 \cdot m^{-2} \cdot d^{-1}$)
*   **NEM**: Net Ecosystem Metabolism ($mmol \cdot O_2 \cdot m^{-2} \cdot d^{-1}$)


## Results & Discussion

### Annual Trophic Balance

Despite significant organic loading and agricultural runoff entering from the lower Cape Fear River, Zeke’s Basin functions near an overall annual trophic balance, with daily $NEM$ values oscillating tightly around $0 \text{ mmol O}_2 \cdot \text{m}^{-2} \cdot \text{d}^{-1}$ (typically ranging within $[-100, +100] \text{ mmol O}_2 \cdot \text{m}^{-2} \cdot \text{d}^{-1}$). In this restricted, poorly flushed system, metabolic conditions rapidly alternate between short-lived periods of net autotrophy ($NEM > 0$) and net heterotrophy ($NEM < 0$). It appears significant production and respiration occur, and offset one another when integrated over space and time. 

### Seasonal Dynamics & Carbon Cycling

Metabolic rates exhibit pronounced seasonality driven by thermal and radiative forcing:

- **Summer**: Highest daily flux magnitudes and variance occur during summer, driven by elevated water temperatures and maximum solar irradiance. Photosynthetic production ($P_g$) peaks near $+800 \text{ mmol O}_2 \cdot \text{m}^{-2} \cdot \text{d}^{-1}$, while total respiration ($R_t$) drops to $-1000 \text{ mmol O}_2 \cdot \text{m}^{-2} \cdot \text{d}^{-1}$.
- **Coupling of Production & Respiration**: High $P_g$ pulses are rapidly offset by proportional increases in respiration ($R_t$). Photosynthetically derived organic matter, alongside incoming riverine carbon inputs, is rapidly consumed or decomposed locally rather than stored within the basin, reinforcing the system's susceptibility to short-term hypoxia during warm, low-wind conditions.

###  Limitations

- **Analysis Window**: The analysis begins on January 22, 2022 rather than January 1, 2022. The full January 1, 2022–January 1, 2023 meteorological record could not be retrieved from the Cardinal export system without exceeding available data export credits. Therefore, the analysis was restricted to the period with reliable overlapping meteorological and water quality observations. Proximity of atmospheric pressure to study site is a limitation given climatological variability of Zeke's Basin. 
- **Study-Year Selection**: 2022 was selected because it provided the most complete in-situ water quality record at Zeke’s Basin within the preceding five years. However, the incomplete record means that the results should be interpreted as representative of the available 2022 observation period rather than a fully continuous annual metabolic budget. 
- **Temporal Gaps**: A considerable data gap occurs during September 2022, coinciding with regional hurricane activity, including Hurricane Ian. Values for this period are currently treated as missing rather than interpolated. This gap limits interpretation of late-summer to early-fall metabolic dynamics and represents an opportunity for future investigation using remote sensing products to characterize conditions during periods without in-situ observations.
- **Diel Model **: Occasional unphysical estimates such as negative $P_g$ values (dipping to $-300 \text{ mmol O}_2 \cdot \text{m}^{-2} \cdot \text{d}^{-1}$) or positive $R_t$ rates are artifacts of in-situ station open-water diel oxygen modeling. In a shallow ($<2\text{ m}$), wind-exposed, tide and flushing restricted water column; these anomalous outputs typically originate from wind driven resuspension and or high atmospheric gas reaeration fluxes.

## Data Citation
 
Water quality and meteorological data used in this analysis were provided by the National Estuarine Research Reserve System (NERRS). Per NERRS/CDMO citation guidance, this dataset is cited as:
 
> NOAA National Estuarine Research Reserve System (NERRS). System-wide Monitoring Program. Data accessed from the NOAA NERRS Centralized Data Management Office website: http://www.nerrsdata.org; accessed 1/16/2025. doi:10.25921/vw8a-8031

* Caffrey, (2004). "Factors controlling net ecosystem metabolism in U.S. estuaries". *Estuaries*, 27(1), 90–101.
* Beck, (2016). SWMPr: "An R Package for Retrieving, Organizing, and Analyzing Environmental Data for Estuaries". *The R Journal*, 8(1), 219–232.
* Odum, (1956). "Primary Production in Flowing Waters". *Limnology and Oceanography*, 1(2), 102–117.
 
## License
 
Code and analysis in this repository are licensed under the MIT License. NOAA/NERRS SWMP data are U.S. federal public data; NERRS site where data were collected to be fully acknowledged in any use. 
 
