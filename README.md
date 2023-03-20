# The Effect of Housing Features on Sale Price in Ames, IA

## Problem Statement
Using a robust sample of approximately 80 features relating to 2051 housing sales in Ames, IA, we will attempt to predict which features contribute most to sale price.

## Data Dictionary

| Feature | Type | Description |
|----|------|--------|
| ms_zoning | object | type of zoning (residential, commerical, etc.) |
| lot_frontage | float64 | frontage (feet)
| lot_area | int64 | lot area (sq ft) 
| street | object | street paving material
| lot_shape | categorical mapped to int64 | irregularity of lot shape 
| land_contour | object | type of gradation of land (level, sloped, etc)
| utilities | object | availability of utilities |
| lot_config | object | arrangement of lot adjacent to streets and/or other houses
| land_slope | categorical mapped to int64 | extent of slope of land |
| neighborhood | object | neighborhood name |
| condition_1  | object | adjacent features, positive and negative
| condition_2 | object | second adjacent feature ('Norm' if none)
| bldg_type | object | house type (single family, condo, etc)
| house_style | object | number and shape of stories | 
 | overall_qual | int64 | overall house quality, rated 1-10 | 
 | overall_cond | int64 | overall house condition, rated 1-10 | 
 | year_built | int64 | year of initial construction | 
 | year_remod/add | int64 | year of remodel or addition (default to year built if none) |
 | roof_style | object | style and shape of roof |
 | roof_matl | object | roof material |
 | exterior_1st | object | materials of outside of house |
 | mas_vnr_type | object | type of masonry, if any |
 | mas_vnr_area | float64 | square footage of masonry work |
 | exter_qual | int64 | exterior quality, rated 1-10 | 
 | exter_cond | int64 | exterior condition, rated 1-10 | 
 | foundation | object | foundation material |
 | bsmt_qual | int64 | basement quality, rated 1-10 | 
 | bsmt_cond | int64 | basement condition, rated 1-10 | 
 | bsmt_exposure | ordinal - int64 | ranked exposure of basement | 
 | bsmtfin_type_1 | ordinal - int64 | ranked extent of finish in basement # 1 | 
 | bsmtfin_sf_1 | float64 | finished area in basement # 1 (square feet) |
 | bsmtfin_type_2 | ordinal - int64 | ranked extent of finish in basement # 2 | 
 | bsmtfin_sf_2 | float64 | finished area in basement # 2 (square feet) |
 | bsmt_unf_sf | float64 | unfinished basement area (square feet) |
 | total_bsmt_sf | float64 | total basement area (square feet) |
 | heating | object | type of heating system |
 | heating_qc | ordinal - int64 | heating quality |
 | central_air | object | whether house has air conditioning |
 | electrical | object | type of electrical system |
 | 1st_flr_sf | int64 | first floor living area (sq ft) | 
 | 2nd_flr_sf | int64 | second floor living area (sq ft) | 
 | low_qual_fin_sf | int64 | below-grade finished living area (sq ft) | 
 | gr_liv_area | int64 | gross living area (sq ft) | 
 | bsmt_full_bath | float64 | full baths in basement |
 | bsmt_half_bath | float64 | half baths in basement |
 |  full_bath | int64 | total full baths | 
 | half_bath | int64 | total half baths | 
 | bedroom_abvgr | int64 | bedrooms above ground | 
 | kitchen_abvgr | int64 | kitchens above ground | 
 | kitchen_qual | ordinal - int64 | ranked kitchen quality |
 | totrms_abvgrd | int64 | total rooms above ground | 
 | functional | ordinal - int64 | ranked extent of necessary upgrades for full functionality |
 | fireplaces | int64 | number of fireplaces | 
 | fireplace_qu | int64 | quality of fireplace, rated 1-10 |
 | garage_type | object | type of garage |
 | garage_yr_blt | float64 | year garage was built
 | garage_finish | ordinal - int64 | ranked extent of garage finish | 
 | garage_cars | float64 | garage capacity (cars) |
 | garage_area  | float64 | garage capacity (sq ft) |
 | garage_qual | int64 | garage quality, rated 1-10 | 
 | garage_cond | ordinal - int64 | ranked condition of garage | 
 | paved_drive | object | extent of paving on driveway (full, none or partial) |
 | wood_deck_sf | int64 | deck area (sq ft) | 
 | open_porch_sf | int64 | open porch area (sq ft) | 
 | enclosed_porch | int64 | enclosed porch area (sq ft) |  
 | 3ssn_porch | int64 | 3-season porch area (sq ft) | 
 | screen_porch | int64 | screened porch area (sq ft) | 
 | pool_area | int64 | pool area (sq ft) | 
 | pool_qc | ordinal - int64 | ranked pool quality | 
 | fence | object | quality of fence privacy (GdPrv or MnPrv) and wood (GdWo,MnWo) |            
 | misc_feature | object | miscellaneous features (incl elevator, 2nd garage, shed over 100 sq ft, and tennis court) |
 | misc_val | int64 | value of miscellaneous feature (USD) 
 | mo_sold | int64 | month of sale (1-12) | 
 | yr_sold | int64 | year of sale (2007-2010) | 
 | sale_type |  object | WD (Warranty Deed - Conventional), CWD (Cash), VWD (VA loan), New, COD (by Court), Con (15% down), ConLw (low down and low interest), ConLI (low interest), ConLD (low down), Other  
 | saleprice | int64 | sale price - USD |
 | pid_1 | int64 | map region (first digit of property ID) | 
 | pid_2 | int64 | map sub-region (digits 2-3 of property ID) |
 
 
## Process

After reviewing and cleaning the data, we one-hot encode all categorical variables, and scale all variables.  This results in a feature space of 185 features.

In order to reduce multicollinearity, we remove the features with greatest variance inflation factor one at a time until the remaining features all have VIF of 5 or lower.  This produces some surprising results, such as removing overall condition and total living area.  These catch-all variables, while highly correlated with sale price, are also highly correlated with numerous other features as well.  

For this data, lasso models perform better than ordinary linear regression or ridge.  Elastic Net produces comparable results to lasso, and only when the l1 ratio approaches 1, i.e. when the Elastic Net model has effectively turned into a lasso model.

Finally, we extract the features with the coefficients of highest magnitude (positive or negative).  

##  Results

The most significant variables in which a homeowner can practically change the value of her home are adding extra rooms (approx $12k per extra room), adding an extra bathroom (approx $8400 per extra bath) and adding an extra fireplace (approx $7600 per fireplace).

Other significant, but less practical, effects on sale price include exterior quality ($11k per move in quality by 1 point on a scale of 10) and change in neighborhood ($10.8k for Northridge Heights, $9300 for Stone Brook, and $7500 for Northridge).
