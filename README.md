### Quantifying Seasonal Ecosystem Metabolism of Zeke’s Island Reserve, North Carolina
 
Zeke's Basin sits within the Zeke's Island component of the North Carolina National Estuarine Research Reserve system, in the lower Cape Fear River Estuary adjacent to Kure Beach, NC. Closure of the New Inlet caused water throughout Zeke's Island Reserve to no longer exchange directly with the Atlantic Ocean, extending residence time, reducing mixing, and increasing sedimentation. Zeke's Basin water quality is now driven almost entirely by the lower Cape Fear River, which carries agricultural, industrial, and wastewater discharge inputs (NCNERR Site Profile). As a result, Zeke’s Basin provides a unique case study for elucidating river-driven changes on estuarine water quality within a restricted system. Quantifying biogeochemical processes, in this case net ecosystem metabolism (NEM), as a result of the difference between gross primary productivity (GPP) and total respiration (Rt) reveals the broader influence of water quality variability on seasonal estuarine ecosystem function.
 
Site specific characteristics:
 
- Restricted flushing: cut off from tidal ocean exchange, the Basin is more vulnerable to nutrient accumulation and eutrophication than sites with an open inlet.
- Shallow, wind exposed water column of less than 2m depth causes resuspension of sediment and subsequent turbid water, limiting light and thereby primary production.
- Elevated hypoxia risk: shallow, poorly flushed systems with high organic loading are predisposed to dissolved oxygen saturation below 30%, which GPP/Rt/NEM metabolism estimates track over time.

Continuous in-situ water quality and nutrient data were collected at Zeke's Basin and retrieved via the NCNERR System Wide Monitoring Program (SWMP), using the `SWMPr` R package.

### Meteorological Data
 
- Wind speed & air temperature: Retrieved from Brunswick County Airport, ~8.2 mi from study site, via NC State Climate Office CRONOS/Cardinal data export system.
- Air pressure: Retrieved from Wilmington International Airport (ILM), ~20 mi from study site, via Iowa Environmental Mesonet ASOS-AWOS-METAR data export system. Notably, historical sea level pressure data is sparse; however has less spatial variability than wind speed/air temp.
## Methods
 
Net ecosystem metabolism (NEM), gross primary production (Pg), and total respiration (Rt) were calculated using the `ecometab` function from the `SWMPr` R package (called from Python via `rpy2`). `ecometab` implements the Odum open-water method. It employs the diel pattern of dissolved oxygen, corrected for air-water gas exchange (via wind speed and barometric pressure) and site-specific solar day length (from latitude/longitude/timezone), to partition net O2 flux into daytime production and nighttime respiration values. Equations listed below.

### 1. Dissolved Oxygen Mass Balance
$$\frac{dC}{dt} = P_g - R_t + D$$

*   $C$: Dissolved oxygen concentration ($mg \cdot L^{-1}$ or $mmol \cdot m^{-3}$)
*   $P_g$: Volumetric hourly gross primary production rate ($mg \cdot L^{-1} \cdot hr^{-1}$)
*   $R_t$: Volumetric hourly ecosystem respiration rate ($mg \cdot L^{-1} \cdot hr^{-1}$)
*   $D$: Volumetric hourly air-water gas exchange (diffusion) flux ($mg \cdot L^{-1} \cdot hr^{-1}$)

### 2. Air-Water Gas Exchange (Diffusion)
The gas exchange at each time step is determined by the oxygen deficit gradient and a temperature-corrected volumetric reaeration coefficient:
$$D = k(C_s - C)$$
$$k = k_{20} \cdot \theta^{(T - 20)}$$

*   $C_s$: Dissolved oxygen concentration at $100\%$ saturation calculated dynamically from salinity ($sal$) and water temperature ($temp$)
*   $k$: Volumetric reaeration coefficient ($hr^{-1}$) adjusted for water temperature $T$ ($^\circ\text{C}$)
*   $\theta$: Empirical temperature correction constant (defaults to $1.0241$)

### 3. Daily Ecosystem Respiration ($R_t$)
Since photosynthesis ceases during dark hours ($P_g = 0$), the hourly respiration rate is calculated strictly within site-specific astronomical night windows determined by coordinates (`lat` / `long`):
$$R_{\text{hourly, night}} = D_{\text{night}} - \left(\frac{dC}{dt}\right)_{\text{night}}$$

The mean nighttime rate ($\overline{R}_{\text{hourly, night}}$) is assumed constant over the 24-hour cycle and scaled by the daily mean depth ($H$) to output total areal consumption:
$$Rt = \left( \overline{R}_{\text{hourly, night}} \times 24 \right) \times H$$

### 4. Gross Primary Production ($P_g$)
Daytime production at each interval corrects the observed change in daytime DO for diffusion and baseline dark respiration:
$$P_{\text{hourly, day}} = \left(\frac{dC}{dt}\right)_{\text{day}} - D_{\text{day}} + \overline{R}_{\text{hourly, night}}$$

Summing across all daylight intervals yields the integrated daily areal production rate:
$$Pg = \left( \sum_{\text{sunrise}}^{\text{sunset}} P_{\text{hourly, day}} \right) \times H$$

### 5. Net Ecosystem Metabolism (NEM)
The ultimate daily integrated net metabolic balance is computed directly as:
$$NEM = Pg - Rt$$

*   **Pg**: Gross Primary Production ($mmol \cdot O_2 \cdot m^{-2} \cdot d^{-1}$)
*   **Rt**: Total Ecosystem Respiration ($mmol \cdot O_2 \cdot m^{-2} \cdot d^{-1}$)
*   **NEM**: Net Ecosystem Metabolism ($mmol \cdot O_2 \cdot m^{-2} \cdot d^{-1}$) 
 
- **Latitude / Longitude**: `33.954680, -77.934807`
- **Timezone**: `America/New_York`
- **Units**: `mmol O2/m^2/day`

### Data Availability & Limitations
 
- The analysis window begins 1/22/2022 rather than 1/1/2022. Pulling the full 1/1/2022–1/1/2023 range from the Cardinal export system would have exceeded available data export credits, so the record was trimmed to the period with reliable overlapping meteorological and water quality data.
- 2022 as the study year given it has the most complete in-situ water quality data at Zeke's Basin in previous 5 years. Note considerable gap in September due to hurricane, and room for investigation regarding interpolating w/ remote sensing products.
- Values for Sep 2022 period are currently missing (possibly due to Hurricane Ian) rather than interpolated.
## Data Citation
 
Water quality and meteorological data used in this analysis were provided by the National Estuarine Research Reserve System (NERRS). Per NERRS/CDMO citation guidance, this dataset is cited as:
 
> NOAA National Estuarine Research Reserve System (NERRS). System-wide Monitoring Program. Data accessed from the NOAA NERRS Centralized Data Management Office website: http://www.nerrsdata.org; accessed 1/16/2025. doi:10.25921/vw8a-8031
 
## License
 
Code and analysis in this repository are licensed under the MIT License. NOAA/NERRS SWMP data are U.S. federal public data; NERRS site where data were collected to be fully acknowledged in any use.
 
