# temporary-segmentation
Model input files and results to accompany "Geodetic imaging of strain partitioning between the megathrust and crustal faults in Cascadia" (Elston and Loveless, submitted to JGR-Solid Earth on September 24, 2026).

All model results here were estimated using celeri (https://github.com/brendanjmeade/celeri). Input files can be manipulated using celeri_ui (https://github.com/brendanjmeade/celeri_ui) and outputs can be visualized using fennil (https://github.com/brendanjmeade/fennil). 

# Model information
model_info.ods is a spreadsheet including information on each model’s ID in the supporting information of Elston and Loveless (submitted to JGR September 24, 2026) and broad details of model block geometry. For the Eel River Basin, Puget Sound, and British Columbia regions, specific faults are listed. Which strand used in the models focused on the Puget Sound region are given for the Seattle and Southern Whidbey Island faults. For all other columns, values of 0 mean no or not included in the model while values of 1 mean yes or included in the model.

More specific information related to model segment geometry and block motion results are included in the runs/ directory.
# Input directory
inputs/ contains model input files used to run all models presented in Elston and Loveless (submitted to JGR September 24, 2026). For each file type, we provide additional information on select variables that are commonly modified.
## Block files
Block/ contains \*block.csv files that include block information. 

interior_lon & interior_lat: The coordinates for designated interior points that are arbitrarily position within each block. This position does not impact the block motion calculations. 

euler_lon, euler_lat, rotation_rate: The coordinates and rate for an a prior Euler pole and rotation rate. For all models here, only the Juan de Fuca plate is prescribed an Euler pole and rotation rate.

rotation_flag: A value of 1 indicates a priori block motion is prescribed. 

Strain_rate_flag: A value of 1 indicates that internal block strain will be estimated in the model. 

Complete header: other1, other2, other3, other4, other5, other6, name, interior_lon, interior_lat, euler_lon, euler_lon_sig, euler_lat, euler_lat_sig, rotation_rate, rotation_rate_sig, rotation_flag, apriori_flag, strain_rate, strain_rate_sig, strain_rate_flag
## General configuration files
Config/ contains the general model configuration files. We modified the segment, block and mesh configuration files as needed in command line. For the specific files used in each model see the config.json file in the results folder. To run celeri models on your device, you must modify the file paths in the casc_config.json.
## Cascadia Subduction Zone mesh and configuration files
Mesh/ contains the pertinent Cascadia subduction interface mesh (mccrory2012_30km.msh; taken from Elston et al., 2025) and configuration files (casc_mesh*.json). For casc_mesh*.json files, the number following nm indicates the number of eigen modes used in the inversion while the number following ml indicates the Matern length (in m) used in the inversion. 
## Segment files
Segment/ contains \*segment.csv files that include block boundary segment information.
name: Where faults align with Cascadia Region Earthquake Science Center Community Fault Model (CFM) faults, the names match the CFM database.
Lon\* & lat\*: The coordinates of the segment endpoints. 

Complete header: Name, lon1, lat1, lon2, lat2, dip, res, other3, other6, rake, rakeSig, rakeTog, other7, other8, other9, locking_depth, locking_depth_sig, locking_depth_flag, dip_sig, dip_flag, ss_rate, ss_rate_sig, ss_rate_flag, ds_rate, ds_rate_sig, ds_rate_flag, ts_rate, ts_rate_sig, ts_rate_flag, resolution_override, resolution_other, mesh_file_index, mesh_flag, patch_slip_file, patch_slip_flag, ss_rate_bound_flag, ss_rate_bound_min, ss_rate_bound_max, ds_rate_bound_flag, ds_rate_bound_min, ds_rate_bound_max, ts_rate_bound_flag, ts_rate_bound_min, ts_rate_bound_max
## GNSS station files
Station/ contains the file with GNSS horizontal surface velocities that constrain the inversions (.csv). Vertical velocities are not a constraint in the inversions, and the values included in the files here have no physical meaning.

Casc_panga_offshore_sta_data.csv includes horizontal surface velocities from Saux et al. (2022), PANGA, and DeSanto et al. (2025). For Elston and Loveless (in review), this is the standard station file used to constrain the inversions.

Casc_panga_low_uncert_offshore_sta_data.csv includes horizontal surface velocities from Saux et al. (2022), PANGA, and DeSanto et al. (2025) with the estimated errors for DeSanto et al. (2025) stations divided by 10.

Casc_panga_sta_data.csv includes horizontal surface velocities only from Saux et al. (2022) and PANGA.

Complete header: Lon, lat, corr, other1, name, east_vel, north_vel, east_sig, north_sig, flag, up_vel, up_sig, east_adjust, north_adjust, up_adjust
# Output directory
Runs/ contains all model results with the folder name (number) corresponding to that in model_info.ods.
## Individual model results
0000000XX/ contains results for individual models.
### General model run information
0000000XX.log contains information for the model run, such as any file substitution that was applied in the command line and the percentage of variation that was captured for the specific model configuration. 

Config.json is the complete configuration file for the model (combination of the broader and mesh-specific files).

Mcmc_trace.zarr/ contains additional information on the 1,800 Monte Carlo draws that can be used to extract statistics such as the 95% confidence width and standard deviation. For more information on how to extract information from the directory, visit the notebooks/ directory in celeri (https://github.com/brendanjmeade/celeri).

### Block motion results
Model_block.csv includes model estimated block motion in addition to input model information.

euler_lon & euler_lat: The model estimated Euler pole coordinates.

rotation_rate: The model estimated block rotation rate.

Complete header: other1, other2, other3, other4, other5, other6, name, interior_lon, interior_lat, euler_lon, euler_lon_sig, euler_lat, euler_lat_sig, rotation_rate, rotation_rate_sig, rotation_flag, apriori_flag, strain_rate, strain_rate_sig, strain_rate_flag, area_steradians, areaplate_carree, block_label, euler_lon_err, euler_lat_err, euler_rate, euler_rate_err
### Slip deficit rate & coupling estimates on the Cascadia Subduction Zone mesh
Model_meshes.csv includes slip deficit rates and coupling estimates on the TDE that comprise the Cascadia Subduction Zone interface. 

Lon\*, lat\* & dep\*: The coordinates for the TDE’s vertices. 

\*_slip_rate: The elastic strike/dip slip rate in the TDE strike/dip direction. This is the mean value from the 1,800 draws. 

\*_slip_rate_kinematic: The kinematic strike/dip slip rate in the TDE strike/dip direction. This is the mean value from the 1,800 draws. 

\*_slip_coupling: The ratio of the elastic to kinematic strike/dip slip rates. This is the mean value from the 1,800 draws.

Complete header: lon1, lat1, dep1, lon2, lat2, dep2, lon3, lat3, dep3, mesh_idx, strike_slip_rate, dip_slip_rate, strike_slip_rate_kinematic, dip_slip_rate_kinematic, strike_slip_coupling, dip_slip_coupling
### Slip deficit rates on individual segments
Model_segment.csv includes slip deficit rates and additional information on segments defined within the model. 

model_\*_slip_rate: The model estimated strike/dip/tensile slip rate. This is the mean value from the 1,800 draws.

model_\*_slip_rate_uncertainty:  The model estimated strike/dip/tensile slip rate uncertainty from the 1,800 draws.

Complete header: name, lon1, lat1, lon2, lat2, dip, res, other3, other6, rake, rakeSig, rakeTog, other7, other8, other9, locking_depth, locking_depth_sig, locking_depth_flag, dip_sig, dip_flag, ss_rate, ss_rate_sig, ss_rate_flag, ds_rate, ds_rate_sig, ds_rate_flag, ts_rate, ts_rate_sig, ts_rate_flag, resolution_override, resolution_other, mesh_file_index, mesh_flag, patch_slip_file, patch_slip_flag, ss_rate_bound_flag, ss_rate_bound_min, ss_rate_bound_max, ds_rate_bound_flag, ds_rate_bound_min, ds_rate_bound_max, ts_rate_bound_flag, ts_rate_bound_min, ts_rate_bound_max, ss_reg_flag, ds_reg_flag, ts_reg_flag, x1, y1, z1, x2, y2, z2, length, azimuth, mid_lon_plate_carree, mid_lat_plat_carree, mid_lon, mid_lat, mid_x, mid_y, mid_z, centroid_x, centroid_y, centroid_z, centroid_lon, centroid_lat, west_labels, east_labels, model_strike_slip_rate, model_dip_slip_rate, model_tensile_slip_rate, model_strike_slip_rate_uncertainty, model_dip_slip_rate_uncertainty, model_tensile_slip_rate_uncertainty
### Horizontal velocity predictions at GNSS station locations
Model_station.csv includes model estimated surface velocities at location of GNSS stations that are used to constrain the inversions. 

model_\*_vel: The model estimated velocity in the east/north/up direction. 

model_\*\_vel = model_\*\_vel_rotation – model_\*\_elastic_segment – model_\*\_vel_tde + model_\*_vel_strain_rate

Complete header: lon, lat, corr, other1, name, east_vel, north_vel, east_sig, north_sig, flag, up_vel, up_sig, east_adjust, north_adjust, up_adjust, depth, x, y, z, block_label, model_east_vel, model_north_vel, model_east_vel_residual, model_north_vel_residual, model_east_vel_rotation, model_north_vel_rotation, model_east_elastic_segment, model_north_elastic_segment, model_east_vel_tde, model_north_vel_tde, model_up_vel_tde, model_east_vel_block_strain_rate, model_north_vel_block_strain_rate, model_east_vel_mogi, model_north_vel_mogi, model_up_vel, model_up_vel_residual, model_up_vel_rotation, model_up_elastic_segment, model_up_vel_block_strain_rate, model_up_vel_mogi

#### References
DeSanto, J. B., Schmidt, D. A., Zumberge, M., Sasagawa, G., & Chadwell, C. D. (2025). Near full locking on the shallow megathrust of the central Cascadia subduction zone revealed by GNSS-Acoustic. Earth and Planetary Science Letters, 665, 119463. https://doi.org/10.1016/j.epsl.2025.119463

Elston, H. M., Loveless, J. P., & Delph, J. R. (2025). Influence of Subduction Interface Geometry on Surface Displacements and Slip Processes in Cascadia. Earth and Space Science, 12(10), e2025EA004623. https://doi.org/10.1029/2025EA004623

Saux, J. P., Molitors Bergman, E. G., Evans, E. L., & Loveless, J. P. (2022). The Role of Slow Slip Events in the Cascadia Subduction Zone Earthquake Cycle. Journal of Geophysical Research: Solid Earth, 127(2), e2021JB022425. https://doi.org/10.1029/2021JB022425

