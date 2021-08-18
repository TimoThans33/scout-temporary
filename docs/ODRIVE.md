# 10. Odrive-client
> current version : scout-odrive-client_0.1.1-dev-210707-63092ee

## 10.1 Config
```
```

## 10.2 Environment
```
# Application Environment file
# Used by all versions
# Formatted by 
#ENV_PARAM=value
CMDARG1=-n
CMDARG2=scout
CMDARG3=-c
CMDARG4=/home/pvadmin/envs/cx-appcenter/apps/scout-base/config.yaml
```
## 10.3 Odrive commands
- show odrive errors
```
dump_errors(odrv0)
```
- clear odrive errors
```
odrv0.clear_errors()
```
- disable watchdog timeout
```
odrv0.axis0.config.enable_watchdog = False
```