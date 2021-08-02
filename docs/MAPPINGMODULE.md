# 6. Mapping module
> current version : robot-mapping-module_0.1.1-dev-210727-b5775e1

## 6.1 Config
```
mapping_plan: /home/pvadmin/mapping_plans/floorplan_3x3.json
mapping_settings: /home/pvadmin/mapping_plans/mapping_settings.yaml
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
```

## 6.2 Environment
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