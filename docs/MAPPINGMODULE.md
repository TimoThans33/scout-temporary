# 7. Mapping module
> current version : robot-mapping-module_0.1.1-dev-210727-b5775e1

## 7.1 Config
```
mapping_plan: /home/pvadmin/mapping_plans/floorplan_3x3.json
mapping_settings: /home/pvadmin/mapping_plans/mapping_settings.yaml
mapping_files_output_path: /home/pvadmin/AMF
load_mapping_graph_from_file: false
conditions_retries: 20
payload_detection: false
peers:
  - id: navigation
    ip: 0.0.0.0
    port: 4010
    scope: PARTITION
  - id: qb_api
    ip: 0.0.0.0
    port: 6011
    scope: PARTITION
  - id: qb_ds
    ip: 0.0.0.0
    port: 6013
    scope: PARTITION
  - id: qb_storage
    ip: 0.0.0.0
    port: 6015
    scope: PARTITION
  - id: qb_logic
    ip: 0.0.0.0
    port: 6017
    scope: PARTITION
  - id: scout_api
    ip: 0.0.0.0
    partition: pv  
    port: 4230
    scope: PARTITION

pose_precision_mapping_x: 0.30
pose_precision_mapping_y: 0.05
pose_precision_mapping_heading: 0.05
pose_precision_transition_x: 0.1
pose_precision_transition_y: 0.1
pose_precision_transition_heading: 0.1
cluster_align_distance: 0.02
cluster_align_time: 0.1
no_drift_max_time: 10
no_drift_max_distance: 5
drift_max_magnitude_x: 0.1
drift_max_magnitude_y: 0.1
drift_max_magnitude_heading: 0.1
wiggle_amplitude: 0.8
wiggle_cycles: 3
bf_amplitude: 0.3
bf_cycles: 3
sensors:
  - id: triton_left
  - id: triton_right
```

## 7.2 Environment
```
# Application Environment file
# Used by all versions
# Formatted by 
#ENV_PARAM=value
CMDARG1=-n
CMDARG2=scout
CMDARG3=-c
CMDARG4=/home/pvadmin/envs/cx-appcenter/apps/robot-mapping-module/config.yaml
TJESS_IP="<scout-ip>"
PV_IP="<scout-ip>"
```