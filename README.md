Data Sources
Water Quality

Continuous water quality data (temperature, salinity, dissolved oxygen, depth) were collected at the Zekes Basin water quality station and retrieved via the NCNERR System Wide Monitoring Program (SWMP), using the SWMPr R package.

Zeke's Basin sits within the Zeke's Island component of the North Carolina National Estuarine Research Reserve system, in the lower Cape Fear River Estuary adjacent to Kure Beach, NC. Since the last natural oceanic inlet was closed via the rock jetty, water in the Basin no longer exchanges directly with the Atlantic Ocean, extending residence time and reducing mixing significantly. Zekes Basin water quality is now driven almost entirely by the lower Cape Fear River Estuary, which carries agricultural, industrial, and wastewater discharge inputs (NCNERR Site Profile, Ch. 5). This makes the Basin a useful case study for detecting river driven water quality change before it reaches the Atlantic ocean, and broader impacts of hard structures in estuaries on biogeochemical processes.

Several site-specific characteristics:

Restricted flushing: cut off from tidal ocean exchange, the Basin is more vulnerable to nutrient accumulation and eutrophication than sites with an open inlet. 
Shallow, wind-exposed water column of less than 2m depth causes resuspension of sediment and subsequent turbid water, limiting light and thereby primary production.
Elevated hypoxia risk: shallow, poorly flushed systems with high organic loading are predisposed to low dissolved-oxygen events, which GPP/Rt/NEM metabolism estimates are well suited to detect and track over time.

Meteorological Data
Wind speed & air temperature: Retrieved from Brunswick County Airport, ~8.2 mi from study site via  NC State Climate Office CRONOS/Cardinal data export system.
Air pressure: Retrieved from Wilmington International Airport (ILM), ~20 mi from study site via Iowa Environmental Mesonet ASOS-AWOS-METAR data export system. Notably, sea level pressure data considerably sparse; however with less spatial variability than wsp/air temp
Methods

Net ecosystem metabolism (NEM), gross primary production (Pg), and total respiration (Rt) were calculated using the ecometab function from the SWMPr R package (called from Python via rpy2). ecometab implements the Odum open-water method. Employs diel pattern of dissolved oxygen, corrected for air-water gas exchange (via wind speed and barometric pressure) and site-specific solar day length (from latitude/longitude/timezone), to partition net O2 flux into daytime production and nighttime respiration values. Site coordinates, timezone, and metabolism units below:

Latitude / Longitude: 33.954680, -77.934807
Timezone: America/New_York
Units: mmol O2/m²/day
Data Availability & Limitations
The analysis window begins 1/22/2022 rather than 1/1/2022. Pulling the full 1/1/2022–1/1/2023 range from the Cardinal export system would have exceeded available data export credits, so the record was trimmed to the period with reliable overlapping meteorological and water quality data.
2022 as the study year given it has the most complete in-situ water quality data at Zekes Basin in previous 5 years. Note considerable gap in September due to hurricane, and room for investigation regarding interpolating w/ remotely sensed products.
Values for Sep 2022 period are currently missing rather than interpolated.
Data Citation

Water quality and meteorological data used in this analysis were provided by the National Estuarine Research Reserve System (NERRS). Per NERRS/CDMO citation guidance, this dataset is cited as:

NOAA National Estuarine Research Reserve System (NERRS). System-wide Monitoring Program. Data accessed from the NOAA NERRS Centralized Data Management Office website: http://www.nerrsdata.org; accessed 7/16/2025. doi:10.25921/vw8a-8031


License

Code and analysis in this repository are licensed under the MIT License. Underlying NOAA/NERRS SWMP data are U.S. federal public data; NERRS site where data were collected to be fully acknowledged in any use.
