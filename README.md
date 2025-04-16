# GEODNET RTK Service Coordinate System
GEODNET supports both the widely adopted global Geodetic Coordinate Systems (for example, WGS84, ITRF2014, and ITRF2020) and regional Geodetic Coordinate Systems for mapping purposes (for example, NAD83 and ETRS89). GEODNET uses different mountpoints to support various Geodetic Coordinate Systems (as shown in Table 1). GEODNET can customize mountpoint with a special Geodetic Coordinate System based on user’s requests.  

Table 1. Mountpoints of GEODNET RTK service  
|#|Mountpoint|Details|
|:---:|:---:|:---:|
|1|AUTO|Regional Geodetic Coordinate System (RGCS)|
|2|AUTO_ITRF2020|ITRF2020 Geodetic Coordinate System at epoch 2015.0|
|3|AUTO_ITRF2014|ITRF2014 Geodetic Coordinate System with current epoch|
|4|AUTO_WGS84<sup>1</sup>|WGS84(G2139) in the middle of the current year (for example, 2025.5 for year 2025, 2024.0 for year 2024)|

The Geodetic Coordinate System of the AUTO will change based on the regions to match the regional Geodetic Coordinate Systems, the currently adopted regional Geodetic Coordinate Systems are listed as below (Table 2). if there is any information which is not accurate, please feedback to GEODNET team. 

Table 2. Regional Geodetic Coordinate System
|#|Geodetic Coordinate System Name|Epoch #|Effective regions|
|:---:|:---:|:---:|:---:|
|1|NAD83(2011)|	2010.0|	USA and North America|
|2|	NAD83(PA11)|	2010.0|	USA Hawii|
|3|	NAD83(MA11)|	2010.0|	GUM|
|4|	NAD83(CSRS)|	2010.0|	CAN|
|5|	ETRS89(ETRF2010)(2010.0)|	2010.0|	Europe|
|6|	GDA2020(2020.0)|	2020.0|	AUS|
|7|	NZGD2000(2000.0)|	2000.0|	NZL|
|8|	TUREF(2005.0)=ITRF96(2005.0)|	2005.0|	TUR|
|9|	ITRF2008(2005.0)=ITRF2008(2005.0)|	2005.0|	IND|
|11|	ITRF2008(2011.811)|	2011.811|	EGY|
|12|	NGD2012(2012.0)=ITRF2008(2012.0)|	2012.0|	NGA|
|13|	PGD2020=ITRF2014(2020.044)|	2020.044|	PHL|
|14|	ITRF2014(2010)|	2010.0|	MEX|
|15|	ITRF2014 current epoch|	Current|	KEN|
|16|	CGCS2000(2000.0)=ITRF97(2000.0)<sup>2</sup>|	2000.0|	CHN|
|17|	JGD2011(2011.3945)=ITRF2008(2011.3945)|	2011.3945|	JPN|
|18|	IGRS2013(2012.0)=ITRF2008(2012.0)|	2012.0|	IDN|
|19|	ITRF1991(1994.0)|	1994.0|	ZAF|
|20|	WGS84(G730)(1994.0)|	1994.0|	LKA|
|21|	ITRF2020(2023.0)|	2023.0|	TWN|
|22|	ITRF2014(2010)|	2010.0|	THA|
|23|	KGD2002(2002.0)=ITRF2000(2002.0)|	2002.0|	KOR|
|24|	MGRF2020(2020.0)=ITRF2020(2020.0)|	2020.0|	MYS|
|25|	MTRF2000(2004.0)=ITRF2000(2004.0)|	2004.0|	ARE|
|26|	SIRGAS2000(2000.4)=ITRF2000(2000.4)	|2000.4|	South America|
|27|	WGS84(G2139)(20xx.5)<sup>1</sup>|	20xx.5|	Other regions|

<sup>1</sup> The WGS84 is used in AUTO_WGS84, since WGS84 is a global dynamic coordinate system, where the coordinate of the station will change due to the global plate tectonics movement. The WGS84(G2139)(20xx.50) with the middle of current year is adapted to best match the current GPS real-time orbit based on NGS recommendations.  
<sup>2</sup> GEODNET does not provide RTK service in this region now, can broadcast partnerships’ RTK/VRS service in this region based on user requests.

