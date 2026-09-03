# GEODNET RTK Service Coordinate Systems

**Document status:** Public service reference, updated 2026-09-03

GEODNET provides global and regional coordinate outputs through NTRIP mountpoints. This document records the coordinate frame and coordinate epoch configured by the service. It also distinguishes a frame transformation from a complete national datum realization.

The service outputs Earth-centered Earth-fixed or geodetic coordinates with ellipsoidal height. It does not by itself supply a country's projected mapping grid, vertical datum, geoid model, cadastral transformation, or legal survey procedure. Users doing control, cadastral, legal, or other high-accuracy work should validate the output against the responsible national authority.

> [!TIP]
> For a comprehensive technical reference on national planar map projections (e.g., State Plane, UTM, Lambert-93, Gauss-Krüger) and national hybrid geoid models (e.g., GEOID18, OSGM15, GCG2016, AUSGeoid2020) used in engineering and surveying workflows, see [Planar Coordinate Systems and Geoid Models Reference Guide](PLANAR_COORDINATE_SYSTEMS_AND_GEOID_MODELS.md).

## 1. Mountpoints

| Mountpoint | Configured output |
|:---|:---|
| `AUTO` | Selects the regional output in Sections 2 and 3 from the station's three-character country or territory code, normally ISO 3166-1 alpha-3. Legacy aliases are also accepted. |
| `AUTO_ITRF2020` | ITRF2020 at the observation epoch. |
| `AUTO_ITRF2014` | ITRF2014 at the observation epoch. |
| `AUTO_WGS84` | Legacy WGS84(G2139) output at the midpoint of the observation year. See Section 4. |
| `BRDC` | Broadcast ephemeris data; this is not a coordinate-frame output. |
| `NATRF2022` | North America plate-fixed test service using NATRF2022. Confirm the coordinate epoch in service metadata. |
| `SIRGAS2000(YYYY.5)` | South America test service using SIRGAS2000 at the midpoint of the named year. |

Mountpoint availability and coverage are operational service properties and may differ from the transformation catalogue below.

## 2. AUTO regional outputs

"Configured output" is the name emitted by the current converter when velocity is available. "Status" explains whether the computation is a direct frame-and-epoch transformation or has a known model or authority limitation.

| # | Country or region | Code | Configured output | Basis | Status |
|---:|:---|:---:|:---|:---|:---|
| 1 | United States - CONUS and Alaska | `USA` | `NAD83(2011)(2010.00)` | NAD83(2011), epoch 2010.0 | NGS transformation; regional displacement behavior depends on HTDP and input velocity [R2]. |
| 2 | United States - Hawaii | `USA` | `NAD83(PA11)(2010.00)` | NAD83(PA11), epoch 2010.0 | NGS transformation; selected geographically within the USA route [R2]. |
| 3 | Guam and Northern Mariana Islands | `GUM`, `MNP` | `NAD83(MA11)(2010.00)` | NAD83(MA11), epoch 2010.0 | NGS transformation [R2]. |
| 4 | American Samoa | `ASM` | `NAD83(PA11)(2010.00)` | NAD83(PA11), epoch 2010.0 | NGS plate assignment [R2]. |
| 5 | Puerto Rico and U.S. Virgin Islands | `PRI`, `VIR` | `NAD83(2011)(2010.00)` | NAD83(2011), epoch 2010.0 | Production default during NSRS modernization. CATRF2022 remains a separate experimental transformation [R2, R31]. |
| 6 | Canada | `CAN` | `NAD83(CSRS)v8(2010.00)` | NAD83(CSRS)v8, epoch 2010.0 | EPSG operation 10415 at the common epoch; propagation uses station velocity, not NAD83v80VG [R4, R5]. |
| 7 | Mexico | `MEX` | `ITRF2008(2010.00)` | ITRF2008, epoch 2010.0 | Configured to the RGNA reference frame; validate against current INEGI products [R16]. |
| 8 | Costa Rica | `CRI`, `COS` | `CR-SIRGAS(2014.59)` | ITRF2008/IGb08, epoch 2014.59 | Official reference epoch; distinct from the operational ITRF2014 epoch 2019.24 [R7, R17]. |
| 9 | Panama | `PAN` | `SIRGAS-Panama(2011.60)` | ITRF2008, epoch 2011.6 | National realization listed by SIRGAS [R7]. |
| 10 | Colombia | `COL` | `MAGNA-SIRGAS(2018.00)` | ITRF2014, epoch 2018.0 | Current network basis documented by IGAC [R10]. |
| 11 | Chile | `CHL` | `SIRGAS-Chile(2021.00)` | IGb14/ITRF2014, epoch 2021.0 | National realization listed by SIRGAS; deformation validation remains necessary [R7]. |
| 12 | Jamaica | `JAM` | `JAD2001(2001.00)` | Implemented as ITRF2000, epoch 2001.0 | Exact centimetre-level identity has not been independently verified [R18]. |
| 13 | Europe, except entries below | Listed European codes | `ETRF2000(2010.00)` | ETRF2000, epoch 2010.0 | GEODNET service frame. Countries maintain distinct national ETRS89 realizations [R8]. |
| 14 | France | `FRA` | `ETRF2000(2019.00)` | RGF93 v2b basis, epoch 2019.0 | National realization basis [R9]. |
| 15 | Italy | `ITA` | `ETRF2000(2008.00)` | RDN2008 basis, epoch 2008.0 | National realization basis [R8]. |
| 16 | Iceland | `ISL` | `ISN2016-approx(2016.50)` | IGS14, epoch 2016.5 | Linear approximation; the Icelandic deformation treatment is not implemented [R6]. |
| 17 | Turkey | `TUR` | `ITRF96(2005.00)` | TUREF/ITRF96, epoch 2005.0 | Earthquake deformation requires authority treatment [R19]. |
| 18 | Australia | `AUS` | `GDA2020(2020.00)` | ITRF2014, epoch 2020.0 | Validate plate-motion handling for official work [R20]. |
| 19 | New Zealand | `NZL` | `NZGD2000-approx(2000.00)` | ITRF96, epoch 2000.0 | Linear approximation; LINZ deformation and earthquake patches are not implemented [R11]. |
| 20 | Japan | `JPN` | `JGD2011-approx(2011.39)` | ITRF2008, epoch 2011-05-24 | Approximation to JGD2011. GSI introduced JGD2024 on 2025-04-01 [R14]. |
| 21 | China | `CHN` | `CGCS2000(2000.00)` | ITRF97, epoch 2000.0 | Defining alignment; does not cover Hong Kong or Macao local systems [R21]. |
| 22 | South Korea | `KOR` | `KGD2002(2002.00)` | ITRF2000, epoch 2002.0 | Configured national-frame alignment [R22]. |
| 23 | Taiwan | `TWN` | `ITRF2020(2025.00)` | ITRF2020, epoch 2025.0 | GEODNET configuration. Mainland e-GNSS[2025] changed on 2025-05-02; exact network agreement and offshore-island coverage are not claimed [R23]. |
| 24 | Thailand | `THA` | `ITRF2014(2021.93)` | ITRF2014, epoch 2021.93 | CORS adjustment basis; not a claim about every cadastral product [R24]. |
| 25 | Malaysia | `MYS` | `GDM2020-approx(2020.00)` | ITRF2014, epoch 2020.0 | Linear approximation; JUPEM velocity and post-seismic procedures are not implemented [R13]. |
| 26 | Indonesia | `IDN` | `SRGI2013-approx(2012.00)` | ITRF2008, epoch 2012.0 | Linear approximation; BIG deformation model is not implemented [R12]. |
| 27 | Philippines | `PHL` | `PGD2020(2020.04)` | ITRF2014, epoch 2020.044 | PAGeNet port-specific realization; other ports may use different frames [R25]. |
| 28 | India | `IND` | `ITRF2008(2005.00)` | ITRF2008, epoch 2005.0 | GEODNET configuration; a current SOI specification for this exact epoch was not verified [R26]. |
| 29 | Saudi Arabia | `SAU` | `KSA-GRF17(2017.00)` | ITRF2014, epoch 2017.0 | National-frame alignment [R27]. |
| 30 | United Arab Emirates | `ARE` | `MTRF2000(2004.00)` | ITRF2000, epoch 2004.0 | Dubai-related configuration; not represented as the official frame of every emirate. |
| 31 | Georgia | `GEO` | `ITRF2008(2011.353)` | Georgia Geodetic Datum basis, epoch 2011.353 | Compare velocity treatment with the national specification [R28]. |
| 32 | Egypt | `EGY` | `ITRF2008(2011.811)` | ITRF2008, epoch 2011.811 | Legacy CORS configuration; current ESA operational status was not independently verified. |
| 33 | Nigeria | `NGA` | `NGD2012(2012.00)` | Implemented as ITRF2008, epoch 2012.0 | GEODNET configuration; nationwide legal adoption was not independently verified [R29]. |
| 34 | South Africa | `ZAF` | `ITRF1991(1994.00)` | Hartebeesthoek94 basis, ITRF91 epoch 1994.0 | Output name describes the implemented basis, not the national realization [R30]. |
| 35 | Sri Lanka | `LKA` | `GEODNET-SLCORS-fit(2024.00)` | Empirical local fit | Four-station GEODNET fit; not an official WGS84 realization or SLD99 transformation. |
| 36 | Kenya | `KEN` | `ITRF2014(observation epoch)` | ITRF2014 at observation epoch | GEODNET configuration; current national CORS realization was not independently verified. |
| 37 | Hong Kong | `HKG` | `HK-SatRef-approx(1998.32877)` | ITRF96 at 1998:121 (May 1, 1998) | Linear frame-and-epoch approximation pending SatRef control validation; not HK1980 Grid or HKPD height [R32]. |
| 38 | Macao | `MAC` | `Macao-ITRF2005-approx(2008.37568)` | ITRF2005, epoch 2008.37568 | Linear approximation pending MoSRef control validation; not Macao Grid. Uses the authority's published decimal epoch [R33, R34]. |
| 39 | Remaining three-character codes | Other | `WGS84(G2139)(observation-year.50)` | ITRF2014-equivalent legacy fallback | Global fallback, not the country's official datum. Missing or non-three-character codes are rejected. |

## 3. Country-specific SIRGAS outputs

| Country | Code | Configured output | Published basis |
|:---|:---:|:---|:---|
| Argentina | `ARG` | `POSGAR07(2006.60)` | ITRF2005, epoch 2006.6 [R7] |
| Bolivia | `BOL` | `MARGEN(1995.40)` | SIRGAS95/ITRF94, epoch 1995.4 [R7] |
| Brazil | `BRA` | `SIRGAS2000(2000.40)` | ITRF2000, epoch 2000.4 [R7] |
| Colombia | `COL` | `MAGNA-SIRGAS(2018.00)` | ITRF2014, epoch 2018.0 [R7, R10] |
| Ecuador | `ECU` | `RENAGE(1995.40)` | SIRGAS95/ITRF94, epoch 1995.4 [R7] |
| El Salvador | `SLV` | `SIRGAS-ES2007(2007.80)` | ITRF2005/IGS05, epoch 2007.8 [R7] |
| Dominican Republic | `DOM` | `ITRF2008(2016.40)` | IGb08, epoch 2016.4 [R7] |
| Peru | `PER` | `SIRGAS2000(2000.40)` | ITRF2000, epoch 2000.4 [R7] |
| Uruguay | `URY` | `SIRGAS-ROU98(1995.40)` | SIRGAS95/ITRF94, epoch 1995.4 [R7] |

### Additional regional group assignments

These are service configurations, not verified national-datum assignments. The explicit country routes above take precedence.

- **ITRF2000 at epoch 2000.4:** `ABW`, `BES`, `BLZ`, `CUW`, `FLK`, `GTM`, `GUF`, `GUY`, `HND`, `NIC`, `PRY`, `SUR`, `VEN`.
- **NAD83(2011) at epoch 2010.0:** `AIA`, `ATG`, `BRB`, `BMU`, `VGB`, `CYM`, `CUB`, `DMA`, `GRL`, `GRD`, `GLP`, `HTI`, `MTQ`, `MSR`, `ANT`, `BLM`, `KNA`, `LCA`, `MAF`, `SPM`, `VCT`, `SXM`, `BHS`, `TTO`, `TCA`. `ANT` is a retained legacy code. French territories maintain separate official national references [R9].
- **ETRF2000 at epoch 2010.0:** `AUT`, `BEL`, `BGR`, `HRV`, `CYP`, `CZE`, `DNK`, `EST`, `FIN`, `DEU`, `GRC`, `HUN`, `IRL`, `LVA`, `LTU`, `LUX`, `MLT`, `NLD`, `POL`, `PRT`, `ROU`, `SVK`, `SVN`, `ESP`, `SWE`, `ALB`, `AND`, `ARM`, `BLR`, `BIH`, `FRO`, `GIB`, `IMN`, `XKX`, `LIE`, `MKD`, `MDA`, `MCO`, `MNE`, `NOR`, `RUS`, `SMR`, `SRB`, `CHE`, `UKR`, `GBR`, `VAT`. France and Italy use the epochs listed in Section 2.

## 4. Accuracy and interpretation

Outputs ending in `-approx` omit an authority deformation or earthquake model, or have not yet been validated against the local control network, as identified in the table. Linear propagation using a station velocity cannot reconstruct coseismic jumps or spatially varying deformation. Use the authority's model or transformation service when national-control agreement is required.

The converter applies published Helmert parameters between ITRF realizations [R1]. Regional propagation also depends on the availability and provenance of station velocity. Coordinate uncertainty reported by the upstream positioning solution does not automatically include transformation, velocity, or deformation-model uncertainty.

`AUTO_WGS84` retains WGS84(G2139) for compatibility. NGA introduced WGS84(G2296), aligned with ITRF2020/IGS20, after G2139 [R15]. The service epoch is the midpoint of the observation year, which can differ from the current calendar year for reprocessed data.

The country code controls routing. A country code indicates jurisdiction, not necessarily the applicable legal datum for a particular project, agency, island, or service endpoint.

## 5. References

- **[R1] IERS/IGN**, [ITRF2020 transformation parameters](https://itrf.ign.fr/docs/solutions/itrf2020/Transfo-ITRF2020_TRFs.txt) - Helmert parameters and rates.
- **[R2] NOAA NGS**, [HTDP User Guide v3.6.0](https://www.ngs.noaa.gov/TOOLS/Htdp/HTDP-user-guide.pdf) - NAD83 transformations and displacement models.
- **[R3] NOAA NGS**, [2022 terrestrial reference frames](https://beta.ngs.noaa.gov/NATRF2022/) - Euler parameters for NATRF2022, PATRF2022, CATRF2022 and MATRF2022.
- **[R4] NRCan**, [Canadian reference-system updates](https://webapp.csrs-scrs.nrcan-rncan.gc.ca/geod/tools-outils/nad83-docs.php) - NAD83(CSRS)v8 and NAD83v80VG.
- **[R5] EPSG**, [ITRF2020 to NAD83(CSRS)v8, operation 10415](https://epsg.org/transformation_10415/ITRF2020-to-NAD83-CSRS-v8-1.html).
- **[R6] National Land Survey of Iceland**, [ISNET2016 remeasurement report](https://www.lmi.is/static/files/utgefid_efni/Maelingar/isnet_endurmael_2016_skyrsla.pdf), printed page 36 - IGS14 at epoch 2016.5.
- **[R7] SIRGAS**, [National networks and infrastructure](https://sirgas.ipgh.org/densificaciones-nacionales/).
- **[R8] BKG/EUREF**, [National reference frames and geoid models](https://evrs.bkg.bund.de/results-and-products/national-reference-frames-and-geoid-models) - includes the EUREF overview, version 1.0c, 2025-06-30.
- **[R9] IGN France**, [Official geodetic texts and frames](https://geodesie.ign.fr/textes-et-decrets).
- **[R10] IGAC Colombia**, [MAGNA-SIRGAS network metadata](https://mapas2.igac.gov.co/server/rest/services/geodesia/redgravimetrica/MapServer/layers).
- **[R11] LINZ**, [NZGD2000 deformation model](https://www.linz.govt.nz/guidance/geodetic-system/coordinate-systems-used-new-zealand/geodetic-datums/new-zealand-geodetic-datum-2000-nzgd2000/new-zealand-geodetic-datum-2000-deformation-model).
- **[R12] BIG Indonesia**, [SRGI2013 deformation model](https://srgi.big.go.id/download/info_produk/INFO%20MODEL%20DEFORMASI.pdf).
- **[R13] JUPEM Malaysia**, [GDM2020 propagation procedure](https://www.jupem.gov.my/storage/upload/pekeliling/d9d1d-pkpup-3-2021.pdf).
- **[R14] GSI Japan**, [JGD2024 adoption](https://web1.gsi.go.jp/ENGLISH/page_e30030.html).
- **[R15] NGA**, [WGS84(G2296) terrestrial reference update](https://earth-info.nga.mil/php/download.php?file=WGS+84%28G2296%29.pdf).
- **[R16] INEGI**, [Red Geodesica Nacional Activa](https://www.inegi.org.mx/temas/geodesia_activa/).
- **[R17] Registro Nacional Costa Rica**, [CR-SIRGAS information](https://www.rnpdigital.go.cr/otros_servicios_linea.htm) - cites Executive Decree 40962-MJP.
- **[R18] National Land Agency Jamaica**, [Digital cadastral surveying standards](https://www.nla.gov.jm/sites/default/files/Procedures%20and%20Standards%20for%20Digital%20Cadastral%20Surveying%20in%20Jamaica.pdf).
- **[R19] BKG CRS-EU**, [TUREF record](https://crs.bkg.bund.de/crs/eu-description.php?crs_id=Y1RSX1RVUkVGKyUyRitUUl9UTQ%3D%3D).
- **[R20] Geoscience Australia**, [GDA2020](https://www.ga.gov.au/scientific-topics/positioning-navigation/positioning-australia/geodesy/datums-projections/gda2020).
- **[R21] Shenzhen Municipal Government**, [CGCS2000 description](https://www.sz.gov.cn/hdjl/ywzsk/ghj/zrzyl/content/mpost_9358232.html).
- **[R22] NGII Korea**, [Geodetic surveying overview](https://www.ngii.go.kr/lib/file/%280907%29%20%EA%B5%AD%ED%86%A0%EC%A7%80%EB%A6%AC%EC%A0%95%EB%B3%B4%EC%9B%90%20%EC%B8%A1%EC%A7%80%EC%B8%A1%EB%9F%89%20%EC%97%85%EB%AC%B4%20%EC%86%8C%EA%B0%9C.pdf).
- **[R23] NLSC Taiwan**, [e-GNSS 2025 announcement](https://www.nlsc.gov.tw/NLSC_Content.aspx?n=1742&s=327692&sms=9680) - effective 2025-05-02; Penghu, Kinmen and Matsu retain the previous framework.
- **[R24] JICA**, [Thailand CORS project report](https://openjicareport.jica.go.jp/pdf/12386298_01.pdf).
- **[R25] NAMRIA**, [PAGeNet frame bulletin](https://pagenet.namria.gov.ph/AGN/Bulletins/Bulletin_PAGeNet-2022_05_04.pdf).
- **[R26] Survey of India**, [CORS service](https://surveyofindia.gov.in/pages/continuously-operating-reference-stations-cors-).
- **[R27] GEOSA**, [KSA-GRF17 FAQ](https://www.geosa.gov.sa/ar/Pages/faq.aspx).
- **[R28] NAPR Georgia**, [Coordinate transformation guide](https://geotransform.napr.gov.ge/download_help.php?filename=HELP.pdf).
- **[R29] OSGOF**, [Geocentric datum transition paper](https://osgof.gov.ng/wp-content/uploads/2024/01/Geocentric-Datum.pdf).
- **[R30] South Africa NGI**, [Geodesy and GPS](https://ngi.dlrrd.gov.za/index.php/technical-information/geodesy-and-gps).
- **[R31] NOAA NGS**, [NSRS modernization transition](https://ngs.noaa.gov/datums/newdatums/release.shtml) and [NAD83 frame assignments](https://www.ngs.noaa.gov/TOOLS/spc.shtml) - NAD83(2011) for the Caribbean plate during the transition.
- **[R32] Hong Kong Lands Department**, [SatRef coordinate list](https://www.geodetic.gov.hk/common/data/pdf/SatRef_Coord.pdf) - ITRF96 at epoch 1998:121; HK1980 Grid requires the published local transformation.
- **[R33] Macao mapping authority**, [Geodetic datum explanatory notes, Appendix 1](https://www.dscc.gov.mo/files/geographical_geodetic_control/ENG/Macaucoord_2009_web_EN_v201702.pdf) - ITRF2005 at published epoch 2008.37568. The legacy DSCC website is migrating to DSSCU.
- **[R34] Macao MoSRef / Hong Kong Lands Department**, [MoSRef coordinate list, 2024-12-03](https://www.geodetic.gov.hk/common/data/SatRef/MoSRef%20Coordinate%20List_20241203.pdf) and [Hong Kong-Macao transformation with a check example](https://www.geodetic.gov.hk/common/data/parameter/7P_HK_Macao_2012_en.pdf).

## 6. Real-Time RTCM Coordinate and Geoid Broadcast Services

GEODNET RTK servers host a multi-model global and regional hybrid geoid engine, dynamically interpolating physical elevations and geoid undulations at the rover's location and streaming broadcast solutions directly over standard and proprietary RTCM channels:

### 6.1 Standard Channel: RTCM Type 1029 (Unicode Text Message)
Supported natively by all commercial GNSS rover software (Trimble Access, Leica Captivate, Carlson SurvCE, FieldGenius, Emlid Reach, CHCNAV LandStar) without firmware modification:
* **Geoid & Physical Elevation String:**
  `GEOID:N=-17.111m,H=1667.111m,NOAA GEOID18 (1-arcmin CONUS)`
* **Station Coordinate & Tectonic Velocity String (122 characters):**
  `CRD:F8B3B7422B01,ITRF2020,EP=2026.50,XYZ=4201275.539,172405.660,4779878.169,V=-12.949,+17.753,+10.544,S95=5.0mm,IAR=99.8%`

### 6.2 Proprietary Binary Channel: GEODNET RTCM Type 4054
Uses GEODNET's assigned proprietary RTCM ID **4054** with a standard header (`ver:3`, `subtype:9`) matching the RTKLIB `decode_type4054` specification:
* **Subtype 1: Geoid Undulation & Vertical Datum (14 bytes total over wire):**
  Signed 20-bit millimeter geoid undulation $N$ ($\pm 524.287\text{ m}$ range at $1.0\text{ mm}$ resolution) + 8-bit Vertical Datum ID (NAVD88, CGVD2013, ODN, IGN69, DHHN2016, AHD, NZVD2016, Tokyo Peil, NAP, REDNAP, LN02, EGM2008).
* **Subtype 2: Station Coordinate, Tectonic Velocity & Quality Metadata (57 bytes total over wire):**
  Carries the full 12-character station identifier (96 bits), coordinate system code, observation date (Year, Month, Day), reference epoch, 38-bit ECEF $X, Y, Z$ at $0.1\text{ mm}$ resolution (RTCM 1005 standard), signed 20-bit tectonic plate velocities $V_x, V_y, V_z$ at **$0.001\text{ mm/year}$ ($1\text{ \mu m/year}$)** resolution (bounded to $\pm 524.287\text{ mm/year}$ for Earth plate tectonic motions), 12-bit 95% sigmas ($0.1\text{ mm}$ resolution), 10-bit integer ambiguity resolution fix rate (`iar`, $0.1\%$ resolution), and CRC-24Q parity.

For detailed technical specifications, projection formulas, and model comparisons, refer to [**PLANAR_COORDINATE_SYSTEMS_AND_GEOID_MODELS.md**](PLANAR_COORDINATE_SYSTEMS_AND_GEOID_MODELS.md).

## 7. Feedback and change control

Corrections should include the responsible authority, realization name, coordinate epoch, source URL, publication date or version, and whether a deformation model is required. Send corrections to **Dr. Yudan Yi** at **yudan@geodnet.com**.

Transformation parameters and routing are maintained in the GEODNET coordinate converter. A documentation entry does not establish legal adoption or guaranteed accuracy; the cited authority remains definitive.
