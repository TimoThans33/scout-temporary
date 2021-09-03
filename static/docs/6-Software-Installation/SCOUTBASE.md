# 9. Scout-base
Cofiguration and usage of scout-base.
> current version : scout-base_0.1.1-dev-210708-0d3043a

## 9.1 Config
You should change the following accordingly:
- scout-ip
- jupiter-serial-number
- triton-left-serial-number
- triton-right-serial-number

```
simulator_model: scout
store_last_known_state: false
state_storage_file: /home/pvadmin/se_storage.json

local_ip:
  - 192
  - 168
  - 0
  - 1

peers:
  - id: scout_api
    partition: pv
    ip: <scout-ip>
    port: 4230
    scope: PARTITION

sensors:
  - name: jupiter
    serial_number: <jupiter-serial-number>
    # pose: [0.0, 0.0, 0.0]
    # pose: [-0.315, 0.0, 0.0]
    pose: [-0.320, 0.00, 3.14159]
    search_radius_coeff: [0.05, 0.02, 0.0]
    drift_always_on: false
    use_external_reference: false
    use_for_mapping: false
    external_reference_transmit_delay: 1500
    wait_for_map_loaded: false
    toggle_recovery_mode_after_set_pose: false
    ip:
      - 192
      - 168
      - 0
      - <ip-address>
    net_mask:
      - 255
      - 255
      - 255
      - 0
    gate_way:
      - 192
      - 168
      - 0
      - 1
  - name: triton_left
    serial_number: <triton-left-serial-number>
    # pose: [0.0, 0.11 , 0.0]
    pose: [0, 0.11 , -1.5708]
    # pose: [0.0, 0.0 , 0.0]
    search_radius_coeff: [0.05, 0.02, 0.2]
    use_external_reference: true
    use_for_mapping: true
    external_reference_transmit_delay: 350
    drift_always_on: true
    wait_for_map_loaded: false
    toggle_recovery_mode_after_set_pose: false
    ip:
      - 192
      - 168
      - 0
      - <ip-address>
    net_mask:
      - 255
      - 255
      - 255
      - 0
    gate_way:
      - 192
      - 168
      - 0
      - 1
  - name: triton_right
    serial_number: <triton-right-serial-number>
    # pose: [0.0, -0.11 , 0.0]
    pose: [0, -0.11 , 1.5708]
    # pose: [0.0, 0.0, 0.0]
    search_radius_coeff: [0.05, 0.02, 0.2]
    use_external_reference: true
    use_for_mapping: true
    external_reference_transmit_delay: 350
    drift_always_on: true
    wait_for_map_loaded: false
    toggle_recovery_mode_after_set_pose: false
    ip:
      - 192
      - 168
      - 0
      - <ip-address>
    net_mask:
      - 255
      - 255
      - 255
      - 0
    gate_way:
      - 192
      - 168
      - 0
      - 1


model_parameters:
  robot_width: 0.41
  robot_wheel_radius: 0.0625
  robot_moment_of_intertia: 5.5
  robot_mass: 60
  robot_outer_width: 0.5
  robot_length: 0.72


pose_manager:
  odometry_providing_sensor: jupiter

max_reference_error_x: 100.0
max_reference_error_y: 100.0
max_reference_error_rot_z: 100.0

calibrate_on_startup: true

upper_limits_orientation_control: [1.0,1.0]
lower_limits_orientation_control: [-1.0,-1.0]

K1_follower: [0.0]
K2_follower: [3.0]
K3_follower: [3.0]

 
K2_final_pose: [0.1,0]

 
lower_limits_follower : [0.0,-2.5]
upper_limits_follower : [1.0,2.5]
max_terminal_vel_orientation_control: 0.3

K1_orientation_control: [0.1, 1.0]
K2_orientation_control: [0.1, 0.1]
K3_orientation_control: [0.0, 0.3]
 
K1_wheel_speed: [0.05, 0.05]
K2_wheel_speed: [0.2, 0.2]
K3_wheel_speed: [0.00, 0.00]
```
---
# 9.2 Environment
You should change the following accordingly
- scout-ip
```
# Application Environment file
# Used by all versions
# Formatted by 
#ENV_PARAM=value
CMDARG1=-n
CMDARG2=scout
CMDARG3=-c
CMDARG4=/home/pvadmin/envs/cx-appcenter/apps/scout-base/config.yaml
TJESS_IP="<scout-ip>"
PV_IP="<scout-ip>"
```
---