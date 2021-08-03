# 8. Mapping planner
> current version : arq-mapp_0.1.1-210726-6fd8fd

## 8.1 Config
```
# YAML format config file# YAML format config filepeers:
peers:
  - id: scout_logic
    ip: <scout-ip>
    port: 3010
    partition: pv
    scope: PARTITION


# Scout variables
scout_id: scout

test: 1
sensor_spacing: 0.22
branch_spacing: 0.44

# Planner variables
sync_after_first_branch: true
sync_after_alignment_branch: true
penalty_dist_to_origin: 10
penalty_intersection_crossing: 1000


progress_storage_file: /home/pvadmin/mapping-progress/mapping_progress.json
save_progress_on_update: true
```

## 8.2 Environment
```

# Application Environment file
# Used by all versions
# Formatted by 
#ENV_PARAM=value
CMDARG1=-n
CMDARG2=scout_api
CMDARG3=-c
CMDARG4=/home/pvadmin/envs/cx-appcenter/apps/arq-mapp/config.yaml
TJESS_IP="<scout-ip>"
PV_IP="<scout-ip>"
PV_PORT="4220"
```