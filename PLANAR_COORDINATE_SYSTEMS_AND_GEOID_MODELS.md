# Planar Coordinate Systems and Geoid Models Reference Guide
**GEODNET RTK Service Technical Reference Document**  
*Document Version: 1.0 — September 2026*

---

## 1. Introduction

High-precision GNSS positioning operates natively in a 3D Earth-Centered, Earth-Fixed (ECEF) Cartesian coordinate system $(X, Y, Z)$ or geodetic ellipsoidal coordinates $(\text{Latitude }\phi, \text{Longitude }\lambda, \text{Ellipsoidal Height }h)$ referenced to a mathematical ellipsoid (such as GRS80 or WGS84). 

However, practical engineering, cadastral surveying, construction, infrastructure development, and GIS applications require:
1. **Planar (Projected) Coordinates $(E, N)$ or $(X, Y)$:** Flattening the curved earth onto a two-dimensional plane using conformal map projections that minimize scale and angular distortion.
2. **Orthometric Heights $(H)$ or Normal Heights $(H_N)$:** Elevations referenced to physical gravity equipotential surfaces (Mean Sea Level / Geoid) where water flows according to gravitational potential, derived via **Geoid Models**.

This document provides a comprehensive technical reference of the planar coordinate systems and geoid models commonly used across major countries and regions served by the GEODNET RTK Network.

---

## 2. Fundamental Geodetic Principles

### 2.1 The Vertical Relationship: Ellipsoid, Geoid, and Topography

GNSS receivers determine the **ellipsoidal height ($h$)**, which is the geometric distance along the ellipsoidal normal from the reference ellipsoid to the GNSS antenna phase center. 

Physical heights (elevations above mean sea level), known as **orthometric heights ($H$)**, are measured along the plumb line from the **geoid** (the equipotential surface of the Earth's gravity field that closely coincides with undisturbed mean sea level).

$$\boxed{H = h - N}$$

Where:
* $h$ = **Ellipsoidal Height** (pure geometric height above the mathematical reference ellipsoid, measured by GNSS).
* $H$ = **Orthometric Height** (physical elevation above Mean Sea Level / Geoid).
* $N$ = **Geoid Undulation / Geoid Height** (separation distance between the reference ellipsoid and the geoid at the given latitude and longitude, interpolated from a geoid grid model).

```
                            [ GNSS Antenna ]
                                   |
                                   |
          Ellipsoidal Height (h)   |
          ======================== |  (Mathematical Reference Ellipsoid: GRS80 / WGS84)
                                   |         /
              Geoid Height (N)     |        /
              -------------------- | ------+------ Geoid (Equipotential gravity surface / MSL)
                                   |      /
          Orthometric Height (H)   |     /
          ======================== | ---+--------- Ground Physical Surface / Topography
```

> [!NOTE]
> In countries using **Normal Heights** (e.g., Germany, France, and parts of continental Europe), the reference surface is the **quasi-geoid**, and the separation distance is referred to as **height anomaly ($\zeta$)**:
> $$H_N = h - \zeta$$

---

### 2.2 Map Projections Used in Engineering & Surveying

To preserve angles and shapes locally, high-precision surveying strictly utilizes **conformal map projections**:

1. **Transverse Mercator (TM):**
   * Projects the ellipsoid onto a cylinder tangent or secant along a chosen **central meridian** (North-South orientation).
   * Distortion increases east and west of the central meridian. Ideal for regions extending predominantly North-South or divided into narrow longitudinal strips ($3^\circ$ or $6^\circ$ wide).
   * *Examples:* UTM (global), British National Grid, Australia MGA, Japan XIX Zones, Gauss-Krüger.

2. **Lambert Conformal Conic (LCC):**
   * Projects the ellipsoid onto a cone secant at two **standard parallels** (East-West orientation).
   * Distortion increases north and south of the standard parallels. Ideal for mid-latitude countries extending predominantly East-West.
   * *Examples:* US State Plane (East-West states), France Lambert-93, India Survey of India grids.

3. **Oblique Mercator / Hotine Oblique Mercator (HOM):**
   * Projects onto a cylinder oriented along an arbitrary oblique geodesic.
   * Ideal for long, narrow geographic features running at an angle.
   * *Examples:* Switzerland LV95, Malaysia Rectified Skew Orthomorphic (RSO), Alaska Zone 1.

4. **Stereographic Projection:**
   * Double projection onto a plane tangent at a central point.
   * *Examples:* Netherlands Rijksdriehoeksmeting (RD New), Poland 1965 Zone 5.

---

## 3. Commonly Used Planar Coordinate Systems by Country / Region

| Country / Region | Geodetic Datum | Official Planar System / Projection | Projection Family | Key Parameters & Zone Specifications |
| :--- | :--- | :--- | :---: | :--- |
| **Global** | WGS84 / ITRF | **UTM (Universal Transverse Mercator)** | Transverse Mercator | 60 zones of $6^\circ$ longitude width (Zones 1 to 60); $k_0 = 0.9996$; False Easting $= 500,000\text{ m}$; False Northing $= 0\text{ m}$ (NH) / $10,000,000\text{ m}$ (SH). |
| **United States** | NAD83(2011) | **State Plane Coordinate System (SPCS 83)** | LCC & TM | ~125 zones across 50 states:<br>• **LCC:** East-West oriented states (e.g., TX, CA, WA, PA, CO, NC)<br>• **TM:** North-South oriented states (e.g., IL, IN, FL, GA, AZ, MS)<br>• **Oblique Mercator:** Alaska Zone 1 (Panhandle) |
| **Canada** | NAD83(CSRS) | **MTM (Modified Transverse Mercator) & UTM** | Transverse Mercator | • **MTM:** $3^\circ$ narrow zones with $k_0 = 0.9999$ for reduced grid distortion in provincial cadastral surveys (e.g., Ontario, Quebec).<br>• **UTM:** Used for federal and regional mapping. |
| **United Kingdom** | ETRS89 / OSGB36 | **British National Grid (BNG)** | Transverse Mercator | Single projection centered at $2^\circ\text{W}$, $49^\circ\text{N}$; $k_0 = 0.9996012717$; False Easting $= 400,000\text{ m}$, False Northing $= -100,000\text{ m}$. |
| **France** | RGF93 (ETRS89) | **Lambert-93 & Coniques Conformes (CC 9 Zones)** | Lambert Conformal Conic | • **Lambert-93:** National grid, standard parallels $44^\circ\text{N}$ & $49^\circ\text{N}$; $E_0 = 700,000\text{ m}, N_0 = 6,600,000\text{ m}$.<br>• **CC 9 Zones (CC42 to CC50):** $1^\circ$ latitudinal strips designed to restrict linear scale error to $< 10\text{ cm/km}$ for legal surveying. |
| **Germany** | ETRS89 | **ETRS89 / UTM (Zones 32N & 33N)** | Transverse Mercator | Replaced historic Gauss-Krüger (DHDN) grids. Zone 32N (Central meridian $9^\circ\text{E}$) and Zone 33N (Central meridian $15^\circ\text{E}$) with 8-digit or 6-digit easting formats. |
| **Australia** | GDA2020 | **Map Grid of Australia 2020 (MGA2020)** | Transverse Mercator | UTM Zones 49 to 56 ($6^\circ$ strips); central meridians $111^\circ\text{E}$ to $153^\circ\text{E}$; $k_0 = 0.9996$; False Easting $= 500,000\text{ m}$. |
| **New Zealand** | NZGD2000 | **New Zealand Transverse Mercator 2000 (NZTM2000)** | Transverse Mercator | Single national grid centered at $173^\circ\text{E}$, $0^\circ\text{S}$; $k_0 = 0.9996$; False Easting $= 1,600,000\text{ m}$, False Northing $= 10,000,000\text{ m}$. |
| **Japan** | JGD2011 | **Japan Plane Rectangular Coordinate System** | Transverse Mercator | **19 zones** (Zones I through XIX) spanning Hokkaido to Okinawa; $k_0 = 0.9999$; scale distortion kept within $\pm 1/10,000$. False Easting $= 0\text{ m}$, False Northing $= 0\text{ m}$. |
| **China** | CGCS2000 | **Gauss-Krüger Projections** | Transverse Mercator | • **$6^\circ$ strips:** Zones 13 to 23 ($k_0 = 1.0$) for small-scale mapping.<br>• **$3^\circ$ strips:** Zones 25 to 45 ($k_0 = 1.0$) for municipal engineering, cadastre, and construction. |
| **South Korea** | KGD2002 | **Korea Unified Coordinate System** | Transverse Mercator | 4 zones (West, Central, East, East-Sea); central meridians $125^\circ, 127^\circ, 129^\circ, 131^\circ\text{E}$; $k_0 = 1.0$; False Easting $= 200,000\text{ m}$, False Northing $= 500,000\text{ m}$ / $600,000\text{ m}$. |
| **Switzerland** | LV95 (ETRS89) | **Swiss Grid (CH1903+ / LV95)** | Oblique Mercator | Swiss oblique cylindrical projection (Rosenmund); fundamental point at Bern ($46^\circ 57' 08.66''\text{N}, 7^\circ 26' 22.50''\text{E}$); False Easting $= 2,600,000\text{ m}$, False Northing $= 1,200,000\text{ m}$. |
| **Netherlands** | ETRS89 | **Rijksdriehoeksmeting (RD New)** | Oblique Stereographic | Double stereographic projection with origin at Amersfoort ($52^\circ 09' 22.178''\text{N}, 5^\circ 23' 15.500''\text{E}$); scale factor $k_0 = 0.9999079$; $E_0 = 155,000\text{ m}, N_0 = 463,000\text{ m}$. |
| **Costa Rica** | CR-SIRGAS | **CRTM05 (Costa Rica Transverse Mercator 2005)** | Transverse Mercator | Single national TM projection centered at $84^\circ\text{W}$, $0^\circ\text{N}$; scale factor $k_0 = 0.9999$; False Easting $= 500,000\text{ m}$, False Northing $= 0\text{ m}$. |
| **Panama** | SIRGAS-Panama | **UTM Zones 17N & 18N** | Transverse Mercator | Standard UTM projection across Western (Zone 17N) and Eastern (Zone 18N) Panama. |
| **Colombia** | MAGNA-SIRGAS | **Origen Nacional (CTM12)** | Transverse Mercator | Single unified national projection established by IGAC in 2020 (Res. 471 & 529), superseding 5 legacy regional origins. Centered at $73^\circ\text{W}$, $4^\circ\text{N}$; $k_0 = 0.9992$; $E_0 = 5,000,000\text{ m}, N_0 = 2,000,000\text{ m}$. |
| **Mexico** | ITRF2008 | **UTM Zones 11N to 16N** | Transverse Mercator | Standard UTM $6^\circ$ zones across the Mexican republic. |
| **Saudi Arabia** | KSA-GRF17 | **Ain el Abd / UTM Zones 36N to 39N** | Transverse Mercator | Standard UTM zones spanning Western, Central, and Eastern provinces. |
| **Turkey** | ITRF96 (TUREF) | **TUREF / TM $3^\circ$ Belts & UTM $6^\circ$** | Transverse Mercator | • **$3^\circ$ belts:** Zones 27, 30, 33, 36, 39, 42, 45 ($k_0 = 1.0$) for large-scale cadastral surveys.<br>• **$6^\circ$ UTM:** Zones 35N, 36N, 37N, 38N ($k_0 = 0.9996$). |
| **Malaysia** | MGRF2020 | **Cassini-Soldner & RSO (Rectified Skew Orthomorphic)** | Cassini / Oblique Mercator | State-level Cassini-Soldner grids for cadastre; Malayan RSO for regional and engineering mapping. |

---

## 4. Commonly Used Geoid Models

### 4.1 Global Geoid Models & Accuracy Specifications

Global geopotential models are derived from satellite gravimetry (CHAMP, GRACE, GOCE), satellite altimetry, and global terrestrial gravity databases. They provide continuous, seamless vertical reference surfaces worldwide.

#### Global Geoid Accuracy Comparison

| Global Model | Publishing Agency & Year | Spatial Resolution | Max Degree & Order ($n_{\max}$) | Global Absolute Accuracy (RMS $1\sigma$) | Relative Accuracy (over $10\text{--}50\text{ km}$) | Terrain Sensitivity (Lowland vs Mountainous) | Memory / Storage Profile |
| :--- | :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **EGM2008** *(2.5′ Grid)* | NGA / 2008 | $2.5' \times 2.5'$ ($\sim 4.5\text{ km}$) | $n,m = 2159$ | **$\pm 5\text{ to }15\text{ cm}$** | **$1\text{ to }3\text{ cm}$** ($< 1\text{ ppm}$) | • Lowland: $\pm 5\text{ to }10\text{ cm}$<br>• Mountains: $\pm 15\text{ to }30\text{ cm}$ | $\sim 149\text{ MB}$ (`.gtx`) |
| **EGM2008** *(1.0′ Grid)* | NGA / 2008 | $1.0' \times 1.0'$ ($\sim 1.8\text{ km}$) | $n,m = 2159$ | **$\pm 5\text{ to }12\text{ cm}$** | **$1\text{ to }2\text{ cm}$** | • Lowland: $\pm 5\text{ to }8\text{ cm}$<br>• Mountains: $\pm 12\text{ to }25\text{ cm}$ | $\sim 800\text{ MB}$ (`.tif`) |
| **NGA EGM96** *(15′ Grid)* | NIMA / NASA 1996 | $15' \times 15'$ ($\sim 28\text{ km}$) | $n,m = 360$ | **$\pm 25\text{ to }50\text{ cm}$** | **$5\text{ to }15\text{ cm}$** | • Lowland: $\pm 20\text{ to }35\text{ cm}$<br>• Mountains: $\pm 50\text{ to }100\text{ cm}$ | $\sim 4.15\text{ MB}$ (`.bin`) |
| **XGM2020** | TU Munich / ICGEM 2020 | $2.0' \times 2.0'$ ($\sim 3.7\text{ km}$) | $n,m = 5399$ | **$\pm 4\text{ to }10\text{ cm}$** | **$< 1.5\text{ cm}$** | Incorporates full GOCE satellite gradiometry | Scientific format |
| **EIGEN-6C4** | GFZ / GRGS 2014 | $2.5' \times 2.5'$ ($\sim 4.5\text{ km}$) | $n,m = 2190$ | **$\pm 5\text{ to }12\text{ cm}$** | **$1\text{ to }3\text{ cm}$** | Optimal oceanic altimetry & continent blend | Scientific format |

#### Key Factors Governing Global Geoid Accuracy

1. **Omission Error (High-Frequency Truncation):**  
   Spherical harmonic expansions truncate at degree $n_{\max}$. The shortest spatial half-wavelength resolved is:
   $$\frac{\lambda}{2} \approx \frac{20,000\text{ km}}{n_{\max}}$$
   * For **EGM96** ($n_{\max} = 360$), structures narrower than $\sim 55\text{ km}$ cannot be resolved, contributing $20\text{--}40\text{ cm}$ omission error in rugged topography.
   * For **EGM2008** ($n_{\max} = 2160$), the spatial resolution is $\sim 9.3\text{ km}$, reducing global omission error to under $5\text{--}10\text{ cm}$.

2. **Commission Error (Data Noise & Gravimetry Gaps):**  
   Errors propagating from surface gravity anomaly noise, airborne survey biases, and sparse gravity coverage in remote desert, jungle, or oceanic trench regions. In North America and Europe, terrestrial gravity density is dense ($< 5\text{ km}$ spacing), yielding top-tier EGM2008 performance ($\sim 5\text{ cm}$); in parts of central Africa or Antarctica, accuracy softens to $\sim 20\text{--}40\text{ cm}$.

3. **Global Equipotential Datum Offset ($W_0$ vs National Tidal MSL):**  
   Global models reference an idealized global geopotential value $W_0 = 62,636,856.0\text{ m}^2/\text{s}^2$ (IAG standard). Local vertical datums (such as NAVD88 in the US or ODN in the UK) are referenced to specific historic tide-gauge benchmarks, which differ from global $W_0$ by a permanent datum bias:
   * **In the United States:** NAVD88 sits approximately **$30\text{ to }50\text{ cm}$** above global $W_0$, creating an apparent constant offset if using pure EGM2008 without hybrid warping.
   * **Solution:** This is why **hybrid geoid models** (e.g. NOAA GEOID18) are utilized for centimeter-level surveying, warping the gravimetric geoid to match the legal leveled benchmarks.

---

### 4.2 National & Regional Hybrid Geoid Models

Hybrid geoid models are gravimetric geoid solutions that have been mathematically warped and fitted to thousands of high-accuracy GNSS/leveling benchmarks across a country. They provide direct centimeter-level conversion between ellipsoidal heights and the official national vertical datum.

| Country / Region | Geoid Model | Official Vertical Datum | Typical Accuracy | Memory / Storage Profile | Common Grid File Formats | Authority |
| :--- | :--- | :--- | :---: | :---: | :---: | :--- |
| **United States** | **GEOID18** <br>*(older: GEOID12B, GEOID09)* | **NAVD88** (North American Vertical Datum 1988) | $1.0\text{ to }2.0\text{ cm}$ | $\sim 34.3\text{ MB}$ | `.bin`, `.ggf`, `.gtx` | **NOAA NGS** |
| **Canada** | **CGG2013a** <br>*(legacy: HTv2.0)* | **CGVD2013** (gravimetric) / **CGVD28** (leveled) | $1.0\text{ to }2.0\text{ cm}$ | $\sim 25\text{ to }30\text{ MB}$ | `.bin`, `.byn`, `.gtx` | **NRCan CGS** |
| **United Kingdom** | **OSGM15** | **ODN** (Ordnance Datum Newlyn) | $< 1.0\text{ cm}$ | $\sim 1.5\text{ to }2.7\text{ MB}$ | `.txt`, `.ggf`, `.gtx` | **Ordnance Survey** |
| **Germany** | **GCG2016** *(German Combined Geoid)* | **DHHN2016** (Normalhöhen) | $0.5\text{ to }1.5\text{ cm}$ | $\sim 3.0\text{ MB}$ | `.gri`, `.asc`, `.gtx` | **BKG / AdV** |
| **France** | **RAF20** <br>*(older: RAF18, RAC09)* | **IGN69** (Mainland) & **IGN78** (Corsica) | $1.0\text{ cm}$ | $\sim 340\text{ KB to }640\text{ KB}$ | `.mnt`, `.gtx`, `.gri` | **IGN France** |
| **Australia** | **AUSGeoid2020** <br>*(older: AUSGeoid09)* | **AHD** (Australian Height Datum) | $2.0\text{ to }3.0\text{ cm}$ | $\sim 15.0\text{ MB}$ | `.gsb`, `.gtx`, `.txt` | **Geoscience Australia / ICSM** |
| **New Zealand** | **NZGeoid2016** | **NZVD2016** (New Zealand Vertical Datum 2016) | $1.0\text{ to }2.0\text{ cm}$ | $\sim 3.5\text{ MB}$ | `.gtx`, `.byn`, `.asc` | **LINZ** |
| **Japan** | **GSIGEO2011** | **Japanese Vertical Datum (Tokyo Peil - T.P.)** | $1.0\text{ to }2.0\text{ cm}$ | $\sim 6.0\text{ MB}$ | `.asc`, `.bin`, `.gtx` | **GSI** |
| **China** | **CQG2000 / CNGeo2000** | **1985 National Height Datum** (Yellow Sea, Qingdao) | $2.0\text{ to }5.0\text{ cm}$ | $\sim 20.0\text{ MB}$ | `.grd`, `.txt` | **National Geomatics Center of China** |
| **South Korea** | **KNGeoid18** <br>*(older: KNGeoid14)* | **Incheon Mean Sea Level** | $2.0\text{ cm}$ | $\sim 1.2\text{ MB}$ | `.grd`, `.bin` | **NGII** |
| **Switzerland** | **CHGeo2004** | **LN02** (Landeshöhennetz 1902) | $1.0\text{ to }2.0\text{ cm}$ | $\sim 400\text{ KB}$ | `.bin`, `.grd`, `.gtx` | **Swisstopo** |
| **Netherlands** | **NLGEO2018** | **NAP** (Normaal Amsterdams Peil) | $< 1.0\text{ cm}$ | $\sim 200\text{ KB}$ | `.rdnap`, `.gtx` | **Kadaster / NSGI** |
| **Spain** | **EGM08-REDNAP** | **REDNAP** (Alicante Datum) | $1.5\text{ to }2.5\text{ cm}$ | $\sim 3.0\text{ MB}$ | `.grd`, `.gtx` | **IGN Spain** |
| **Nordic Region** | **NKG2015** | National Normal Height systems (RH2000, N2000, DVR90) | $1.0\text{ to }2.0\text{ cm}$ | $\sim 8.5\text{ MB}$ | `.gri`, `.gtx` | **Nordic Geodetic Commission (NKG)** |
| **Mexico** | **GGM10 / GGM20** | **NAVD88 / Red Geodésica Vertical** | $3.0\text{ to }5.0\text{ cm}$ | $\sim 12.0\text{ MB}$ | `.bin`, `.asc` | **INEGI** |
| **Colombia** | **GEOCOL2004** | **Buenaventura Vertical Datum** | $5.0\text{ cm}$ | $\sim 2.5\text{ MB}$ | `.bin`, `.txt` | **IGAC** |
| **Brazil** | **MAPGEO2015** | **Imbituba Vertical Datum** | $5.0\text{ to }10.0\text{ cm}$ | $\sim 18.0\text{ MB}$ | `.txt`, `.grd` | **IBGE** |
| **Europe (Unified)** | **EGG2015 / EVRF2019** | **EVRF2019** (Normaal Amsterdams Peil) | $1.5\text{ to }3.0\text{ cm}$ | $\sim 45.0\text{ MB}$ | `.gri`, `.gtx` | **EUREF / BKG** |

---

## 5. Practical Guidelines for RTK Rover & Survey Workflow Integration

To achieve survey-grade accuracy in both horizontal coordinates and physical elevations using GEODNET RTK services:

```
+-----------------------------------------------------------------------------------+
| 1. Connect to GEODNET Mountpoint (e.g., AUTO, AUTO_ITRF2020)                      |
|    -> Receiver receives centimeter-accurate (Lat, Lon, Ellipsoidal Height h)      |
+-----------------------------------------------------------------------------------+
                                         |
                                         v
+-----------------------------------------------------------------------------------+
| 2. Apply Horizontal Map Projection in Rover Software / CAD                        |
|    -> Transforms (Lat, Lon) into planar (Easting, Northing)                       |
|    -> e.g., State Plane Zone, UTM Zone 32N, Lambert-93, MGA2020                   |
+-----------------------------------------------------------------------------------+
                                         |
                                         v
+-----------------------------------------------------------------------------------+
| 3. Apply Local Hybrid Geoid Model Grid in Rover Software                          |
|    -> Interpolates Geoid Undulation (N) at current rover position                 |
|    -> Computes physical orthometric elevation: H = h - N                          |
|    -> e.g., GEOID18 (USA), OSGM15 (UK), GCG2016 (DE), AUSGeoid2020 (AU)           |
+-----------------------------------------------------------------------------------+
```

### Common Geoid File Formats by Equipment Manufacturer

* **Trimble:** `.ggf` (Geoid Grid File)
* **Leica Geosystems:** `.gem` / `.csc` (Coordinate System File)
* **Topcon / Sokkia:** `.clg` / `.bin`
* **Carlson SurvCE / SurvPC:** `.gsf`
* **Emlid Reach / Field Software:** `.gtx` / `.tif` (GeoTIFF / NOAA VDatum format)
* **GIS & Open Source (PROJ / QGIS / GDAL):** `.gtx`, `.tif` (Cloud-Optimized GeoTIFF - COG via PROJ-data)

---

---

## 6. Server-Side RTK Service Geoid Hosting & Real-Time RTCM Broadcast

In traditional RTK operations, rovers are burdened with storing dozens of massive vendor-proprietary geoid files (`.ggf`, `.gem`, `.clg`) and selecting them manually in field controller software.

In the **GEODNET RTK Service architecture**, the server hosts the complete library of precise global and national hybrid geoid models, automatically evaluates the geoid undulation $N$ for the rover's exact coordinates, and broadcasts the solution down to the rover in real time.

```
+----------------------------------------------------------------------------------------------------+
| 1. Rover connects to GEODNET NTRIP Caster & transmits NMEA GGA (Latitude, Longitude, Height h)     |
+----------------------------------------------------------------------------------------------------+
                                                  |
                                                  v
+----------------------------------------------------------------------------------------------------+
| 2. GEODNET RTK Server evaluates High-Resolution Geoid Multi-Grid Pool (geoid.c / rtcm_geoid.c)     |
|    • Multi-Grid Pool: 14 loaded models (NOAA GEOID18, OSGM15, RAF18, GCG2016, EGM2008, etc.)       |
|    • Interpolates Geoid Undulation N with sub-millimeter bilinear precision                        |
|    • Computes Physical Elevation: H = h - N                                                        |
+----------------------------------------------------------------------------------------------------+
                                                  |
                                                  v
+----------------------------------------------------------------------------------------------------+
| 3. RTCM Real-Time Broadcast to Rover                                                               |
|    • Channel A: RTCM Type 1029 (Unicode Text Message)                                              |
|      -> Standard RTCM3 string displayed/logged by all survey controllers (Trimble, Leica, Carlson) |
|      -> Payload: "GEOID:N=-17.111m,H=1667.111m,NOAA GEOID18 (1-arcmin CONUS)"                     |
|                                                                                                    |
|    • Channel B: GEODNET Proprietary RTCM Message (Type 4054)                                       |
|      -> Ultra-compact 14-byte binary frame (12-bit station, 20-bit N in mm, 8-bit datum code)      |
|      -> Direct hardware consumption for auto-leveling, autonomous machines, and GEODNET SDK        |
+----------------------------------------------------------------------------------------------------+
```

### 6.1 RTCM Message Formats

#### A. RTCM Type 1029 (Unicode Text String)
* **Standard:** RTCM 10403.3 Section 3.5.15.
* **Compatibility:** Supported by virtually all field controllers (Trimble Access, Leica Captivate, SurvCE, FieldGenius, Emlid Reach, CHCNAV LandStar).
* **Payload Structure:**
  * DF002: Message Number = 1029 (12 bits)
  * DF137: Reference Station ID (12 bits)
  * DF041 / DF042: Epoch time (MJD 16 bits, Seconds of Day 17 bits)
  * DF138: Number of Characters (7 bits)
  * DF139: Number of UTF-8 Code Units (8 bits)
  * DF140: UTF-8 Text String: `"GEOID:N=+43.802m,H=36.198m,IGN France RAF (France/Corsica)"`
  * CRC-24Q Parity: 24 bits

#### B. GEODNET Proprietary RTCM Message (Type 4054)
* **Message ID:** Type 4054 (Official GEODNET Assigned Proprietary Message).
* **Frame Header (matches RTKLIB `decode_type4054`):**
  * Message Number: 4054 (12 bits)
  * Version: `ver` (3 bits, default `0`)
  * Message Subtype: `subtype` (9 bits)

##### Subtype 1: Geoid Undulation & Vertical Datum (14 bytes total)
* **Subtype ID:** `1`
* **Station ID:** 12 bits
* **Undulation ($N$) in millimeters:** signed 20 bits (range $\pm 524.287\text{ m}$, resolution $1.0\text{ mm}$)
* **Vertical Datum ID:** 8 bits (e.g. 1=NAVD88, 2=CGVD2013, 3=ODN, 4=IGN69, 5=DHHN2016, 6=AHD, 7=NZVD2016, 8=Tokyo Peil, 9=NAP, 10=REDNAP, 11=LN02, 12=RH2000, 13=DVR90, 50=EGM2008, 51=EGM96)
* **CRC-24Q:** 24 bits

##### Subtype 2: ITRF2020 Coordinate Info, Velocity & Quality Metadata (57 bytes total)
* **Subtype ID:** `2`
* **Station ID:** 12 bits ($0\text{--}4095$)
* **Station Identifier:** 12 ASCII characters (96 bits, e.g. `"F8B3B7422B01"`)
* **Coordinate System ID:** 8 bits (1=ITRF2020, 2=ITRF2014, 3=NAD83(2011), 4=ETRF2000, 5=WGS84(G2139))
* **Observation Date:** 21 bits (Year 12 bits, Month 4 bits, Day 5 bits)
* **Reference Epoch:** 24 bits (decimal year $\times 1000$, e.g. $2026.500 \to 2026500$)
* **Cartesian ECEF Coordinates ($X, Y, Z$):** $3 \times 38 = 114\text{ bits}$ (signed 38 bits each at $0.1\text{ mm}$ resolution, following RTCM 1005 standard)
* **Tectonic Plate Velocities ($V_x, V_y, V_z$):** $3 \times 20 = 60\text{ bits}$ (signed 20 bits each at **$0.001\text{ mm/yr}$ ($1\text{ \mu m/yr}$)** precision, magnitude range $\pm 524.287\text{ mm/yr}$ specifically bounding Earth plate tectonic motions)
* **95% Confidence Sigmas ($\sigma_{X95}, \sigma_{Y95}, \sigma_{Z95}$):** $3 \times 12 = 36\text{ bits}$ (unsigned 12 bits each at $0.1\text{ mm}$ resolution)
* **Ambiguity Resolution Fix Rate (`iar`):** 10 bits ($0\text{ to }1000$ at $0.1\%$ resolution, $0.0\text{ to }100.0\%$)
* **Align Padding:** 3 bits (total 51 bytes payload)
* **CRC-24Q:** 24 bits

Also emitted via standard **RTCM 1029 Text Message** (121 characters):
`CRD:F8B3B7422B01,ITRF2020,EP=2026.50,XYZ=4201275.539,172405.660,4779878.169,V=-12.949,+17.753,+10.544,S95=5.0mm,IAR=99.8%`

---

## 7. References and Further Reading

1. **NOAA National Geodetic Survey (NGS):** *The State Plane Coordinate System of 1983*, NOAA Manual NOS NGS 5.
2. **National Geospatial-Intelligence Agency (NGA):** *The Development of the Joint Defense Mapping Agency/NASA Earth Gravitational Model 1996 (EGM96)* & *Earth Gravitational Model 2008 (EGM2008)*.
3. **EUREF (European Reference Frame):** *Specifications for ETRS89 and EVRF2019*, International Association of Geodesy (IAG).
4. **Intergovernmental Committee on Surveying and Mapping (ICSM):** *Geocentric Datum of Australia 2020 Technical Manual*.
5. **Instituto Geográfico Agustín Codazzi (IGAC):** *Resolución 471 de 2020 y 529 de 2020 - Proyección Cartográfica Única para Colombia (CTM12)*.
6. **Land Information New Zealand (LINZ):** *New Zealand Geodetic Datum 2000 (NZGD2000) and NZVD2016 Specifications*.
7. **Radio Technical Commission for Maritime Services (RTCM):** *RTCM Standard 10403.3 for Differential GNSS Services - Version 3*.
