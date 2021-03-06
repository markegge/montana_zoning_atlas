# Montana Zoning Atlas

February, 2022

**Contact**: Mark Egge (mark at eateggs dot com)

_Mark Egge is a Bozeman-based advocate for affordable housing and for sustainable and safe transportation._

This repository contains the code used to produce the data layers behind the [Montana Zoning Atlas](). This code combines zoning layers and cadastral parcel data to evaluate where affordable multifamily housing is permitted and prohibited in Whitefish, Helena, Kalispell, Missoula, Bozeman, and Billings, MT. Data processing by Mark Egge. Zoning code research and by the [Frontier Institute](https://frontierinstitute.org).

## Data Sources

Parcel Data downloaded from Montana Cadastral: https://msl.mt.gov/geoinfo/msdi/cadastral/

Zoning Data Sources:

* Billings: https://billingsgis.com/arcgis_public/rest/services/ArcOnline_Public/COB_Zoning/MapServer/4
* Bozeman: https://gisweb.bozeman.net/arcgis/rest/services/Open_Data/Zoning/FeatureServer/0
* Helena: https://helenamontanamaps.org/arcgisadp/rest/services/LCSimple/MapServer/42
* Kalispell: https://maps.ci.kalispell.mt.us/server/rest/services/PlanningDept/Zoning/FeatureServer/0
* Missoula: https://services.arcgis.com/HfwHS0BxZBQ1E5DY/arcgis/rest/services/Zoning_and_Land_Use_mso/FeatureServer/0/
* Whitefish: https://maps.cityofwhitefish.org/server/rest/services/kiosk_zones/MapServer/0

Data layers accessed and downloaded January 15, 2022

## Data Processing

For each city:
* Overlying zoning is joined to each parcel based on the zoning polygon where the parcel's center is located
* Data is filtered to just those parcels with residential zoning and a cadastral property type not in this list:

```
 PropType NOT IN (
    '',
    'VR - Vacant Land Rural', 
    'CA - Centrally Assessed', 
    'VU - Vacant Land Urban',
    'FARM_U - Farmstead - Urban', 
    'NVS - Non-Valued with Specials', 
    'RV_PARK - RV Park', 
    'GRAVEL - Gravel Pit', 
    'GOLF - Golf Course',
    'EP_PART - Partial Exempt Property', 
    'CN - Centrally Assessed Non-Valued Property', 
    'NV - Non-Valued Property', 'FARM_R - Farmstead - Rural', 
    'VAC_U - Vacant Land - Urban', 
    'EP - Exempt Property', 'VAC_R - Vacant Land - Rural')
```

* Lot Size (square feet) is calculated
* For each parcel, a minimum bounding box is calcualted. The width of the minimum bounding box is joined back to the parcel as its width
* Appropriate logic is applied to determine the number of units the parcel's zoning, lot size, and lot width allows

Data processing steps are detailed in `data_prep.ipynb`

The rules used to assign zoning categories for each city are contained in their respective notebooks.


### Zoning Category Color Scheme

* Multifamily: #01a1a0
* Penalized Multifmaily: #80cdc1
* Single Family: #ea60b9
* De Facto Single Family: #eaa0b9