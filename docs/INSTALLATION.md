# 4. Installation
Script for installing the scout from scratch.

## 4.1 Ubuntu Configuration 
usefull tools needs to be installed on the scout.
- update apt
    ```
    sudo apt update
    sudo apt upgrade
    ```
- Install base tools/network tools
    1. openssh-server
    2. net-tools
    3. nmap
    4. git

    ```
    sudo apt install openssh-server net-tools nmap git
    ```
- Open ssh server for scout.
    ```
    sudo apt update
    sudo ufw allow ssh
    ```
- Setup static ip address for the scout. Use
```
sudo nm-connection-editor
```
> note: connect with ssh using the flag -X

to see the available connections. Click on the wired connection you want to configure. The IPv4 settings must be set to manual. 
Name: scout
Address: 192.168.0.1
netmask: 24
Gateway: 192.168.0.1
DNS-servers: 8.8.8.8

Click on the wifi connection you want to configure. The IPv4 settings must be set to manual.
Name: as_demo
Addres: 192.168.8.<scout-ip+10>
netmask: 24
Gateway: 192.168.8.1
DNS-servers: 8.8.8.8

We also need the zeromq library. Install using:
```
sudo apt-get install libzmq3-dev
```

## 4.2 CX-Appcenter
The appcenter is a webserver, that runs as a service on the ubuntu server. A service is a process that runs in the background continuously and starts when booting the system.
To install it, access to the server is required. Set up an ssh connection or connect a monitor and keyboard.

### 4.2.1 Systemd
All services can be seen in /etc/systemd/system. Check this directory to see the existing services with

```
ls /etc/systemd/system
```
and check a service's contents with

```
cat <service-name>
```
systemd is the linux system and service manager with PID 1. It has a very useful command line interface. For example you can restart a service using:
```
sudo systemctl restart <service-name>
```
You can also check the status of a service:
```
sudo systemctl status <service-name>
```
While troubleshooting a service journalctl can be very usefull:
```
sudo journalctl -n <no-of-lines> -u <name-of-service>
```
When you make changes to service files you have reload the systemd daemon.
```
sudo systemctl daemon-reload
```
### 4.2.2 Dependecies
First install the correct python packages through pip

```
python3 -m pip install Jinja2==2.10 aiohttp==3.7.3 PyJWT==2.1.0 sanic==20.12.3 sanic_cors==0.10 tinydb==4.4.0 sanic_jwt fpdf lmdb deb_pkg_tools arpy i2cdev
```
> note: Install the python packages for python3

> note: If pip is not installed, install using

```
sudo apt install python3-pip
```
> note: If this also does not work, probably no wifi connection can be established. This since it assumes that it is connected to a internet cable. Then use the following commands to establish wifi connection:

```
sudo nmcli con mod as_demo ipv4.gateway 192.168.8.1
sudo nmcli con mod as_demo ipv4.dns 8.8.8.8
sudo nmcli con mod scout ipv4.gateway ""
```

Make journalctl persistent, and check if the file exisist with:

```
sudo mkdir /var/log/journal
```

### 4.2.3 Visudo
To continuously run in the background, the CX-services (CX-Appcenter and CX-Local) need sudo-rights. To give them sudo-rights, without requiring a password, the sudoers file requires alteration.

The sudoers file is edited using visudo, a text editer that allows safe editing of the sudoers file by validating the syntax upon saving.   
**Never** edit the sudoers file using a normal text editor.
```
echo 'pvadmin ALL = NOPASSWD: /bin/systemctl restart cx_*.service' | sudo EDITOR='tee -a' visudo
echo 'pvadmin ALL = NOPASSWD: /bin/systemctl start cx_*.service' | sudo EDITOR='tee -a' visudo
echo 'pvadmin ALL = NOPASSWD: /bin/systemctl stop cx_*.service' | sudo EDITOR='tee -a' visudo
echo 'pvadmin ALL = NOPASSWD: /bin/systemctl enable cx_*.service' | sudo EDITOR='tee -a' visudo
echo 'pvadmin ALL = NOPASSWD: /bin/systemctl disable cx_*.service' | sudo EDITOR='tee -a' visudo
echo 'pvadmin ALL = NOPASSWD: /bin/systemctl is-active cx_*.service' | sudo EDITOR='tee -a' visudo
echo 'pvadmin ALL = NOPASSWD: /bin/systemctl is-enabled cx_*.service' | sudo EDITOR='tee -a' visudo
echo 'pvadmin ALL = NOPASSWD: /bin/systemctl daemon-reload' | sudo EDITOR='tee -a' visudo
echo 'pvadmin ALL = NOPASSWD: /bin/journalctl -u cx_*.service --since * --no-pager' | sudo EDITOR='tee -a' visudo
echo 'pvadmin ALL = NOPASSWD: /bin/journalctl -u cx_*.service --since * --until * --no-pager' | sudo EDITOR='tee -a' visudo
echo 'pvadmin ALL = NOPASSWD: /bin/cp /home/pvadmin/envs/cx-appcenter/* /etc/systemd/system/cx_*@.service' | sudo EDITOR='tee -a' visudo
echo 'pvadmin ALL = NOPASSWD: /bin/rm /etc/systemd/system/cx_*@.service' | sudo EDITOR='tee -a' visudo
```

### 4.2.4 User Directories
The envs directory contains the environments folders. The projects directory contains the data which can be found on the repositories. Create the following directories for appcenter:
```
mkdir -p /home/pvadmin/projects/cx
mkdir -p /home/pvadmin/envs/cx-appcenter/apps
mkdir -p /home/pvadmin/envs/cx-appcenter/database
mkdir -p /home/pvadmin/envs/cx-appcenter/downloads
```
![robot_exploded](images/folder-structure.png)

In the projects/cx folder, clone the repositories through https (not ssh!)

```
git clone https://<username>@bitbucket.org/pv_as/cx-appcenter.git
git checkout -b 2f88b6a
```

### 4.2.5 Envs file
Create the Appcenter environment file:
```
touch /home/pvadmin/envs/cx-appcenter/appcenter.env
```

edit the following template (the correct ip must be written down) and paste inside the ```appcenter.env``` file

```
CX_APPCENTER_ENV_PATH=/home/pvadmin/envs/cx-appcenter
CX_AUTH_SECRET=lskdjflaksdlks

CX_APPCENTER_ENABLE_LOCAL_LOGIN=true
# you can change local user name, default is admin
# CX_APPCENTER_USERS_ADMIN=admin 
# password is required to set
CX_APPCENTER_USERS_ADMIN_PASSWORD="please-contact-primevision-for-the-password"

# change ip to scout ip
CX_APPCENTER_IP_ADDRESS=192.168.8.<scout-ip>
CX_APPCENTER_PORT=8000
```

Verify the ```EnvironmentFile```-variable in ```projects/cx/cx-appcenter/cx-appcenter.service``` is set correctly to the env-file. If so, copy the service file to the systemd folder

### 4.2.6 Install Systemd Service
```
cd /home/pvadmin/projects/cx/cx-appcenter
sudo cp cx-appcenter.service /etc/systemd/system/cx-appcenter.service
```

Use these commands to enable and start the service
```
sudo systemctl enable cx-appcenter.service 
sudo systemctl start cx-appcenter.service
```
---

### 4.2.7 Create Directories
We need to create two directories for the robot mapping module and change the yaml files.
- create directories
```
mkdir -p /home/pvadmin/mapping-settings/
mkdir -p /home/pvadmin/storage/
```
- fill the mapping settings file.
```
echo "pose_precision_mapping_x: 0.30
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
  - id: triton_right" > /home/pvadmin/mapping-settings/mapping-settings.yaml
```
- fill the storage.json (we will add something later maybe)
```
echo "" > /home/pvadmin/storage/finalize_state_storage.json
```
## 4.3 Sensor 
- create a shortcut for the accerion control center
```
echo "alias accerion-control-center=/usr/share/AccerionControlCenter/bin/AccerionControlCenter" >> ~/.bashrc
```
We need to know the ip addresses and serial numbers of the sensors. They can both be found from the accerion control center that is installed on the scout.
```
accerion-control-center
```
> remember to login with ssh and -X enabled

The sensor ip addresses and serial numbers cam be found in the sensor settings tab. Make sure to assign the ip addresses and serial numbers correct for the jupiter, triton-left and triton-right.

- update accerion firmware
In the GUI the firmware can be updated by selecting the correct asu file and to add the sensors to the queue.
```
TritonV1_5.4.0_rc6.asu
```
## 4.4 Odrive
- install odrive tools
```
python3 -m pip install odrive
```
- set permissions for USB
```
echo 'SUBSYSTEM=="usb", ATTR{idVendor}=="1209", ATTR{idProduct}=="0d[0-9][0-9]", MODE="0666"' | sudo tee /etc/udev/rules.d/91-odrive.rules
 sudo udevadm control --reload-rules
 sudo udevadm trigger
```
- restore configs from the odrive
```
echo "{"axis0": {"acim_estimator": {"config": {"slip_velocity": 14.706000328063965}}, "config": {"dir_gpio_pin": 2, "enable_sensorless_mode": false, "enable_step_dir": false, "enable_watchdog": false, "startup_closed_loop_control": false, "startup_encoder_index_search": false, "startup_encoder_offset_calibration": false, "startup_homing": false, "startup_motor_calibration": false, "step_dir_always_on": false, "step_gpio_pin": 1, "watchdog_timeout": 0.0, "calibration_lockin": {"accel": 20.0, "current": 10.0, "ramp_distance": 3.1415927410125732, "ramp_time": 0.4000000059604645, "vel": 40.0}, "can": {"encoder_rate_ms": 10, "heartbeat_rate_ms": 100, "is_extended": false, "node_id": 0}, "general_lockin": {"accel": 20.0, "current": 10.0, "finish_distance": 100.0, "finish_on_distance": false, "finish_on_enc_idx": false, "finish_on_vel": false, "ramp_distance": 3.1415927410125732, "ramp_time": 0.4000000059604645, "vel": 40.0}, "sensorless_ramp": {"accel": 200.0, "current": 10.0, "finish_distance": 100.0, "finish_on_distance": false, "finish_on_enc_idx": false, "finish_on_vel": true, "ramp_distance": 3.1415927410125732, "ramp_time": 0.4000000059604645, "vel": 400.0}}, "controller": {"config": {"axis_to_mirror": 255, "circular_setpoint_range": 1.0, "circular_setpoints": false, "control_mode": 3, "electrical_power_bandwidth": 20.0, "enable_current_mode_vel_limit": true, "enable_gain_scheduling": false, "enable_overspeed_error": true, "enable_vel_limit": true, "gain_scheduling_width": 10.0, "homing_speed": 0.25, "inertia": 0.0, "input_filter_bandwidth": 2.0, "input_mode": 1, "load_encoder_axis": 0, "mechanical_power_bandwidth": 20.0, "mirror_ratio": 1.0, "pos_gain": 15.0, "spinout_electrical_power_threshold": 10.0, "spinout_mechanical_power_threshold": -10.0, "steps_per_circular_range": 1024, "torque_mirror_ratio": 0.0, "torque_ramp_rate": 0.009999999776482582, "vel_gain": 0.15000000596046448, "vel_integrator_gain": 6.0, "vel_limit": 25.0, "vel_limit_tolerance": 1.2000000476837158, "vel_ramp_rate": 1.0, "anticogging": {"anticogging_enabled": true, "calib_anticogging": false, "calib_pos_threshold": 1.0, "calib_vel_threshold": 1.0, "cogging_ratio": 1.0, "index": 0, "pre_calibrated": true}}}, "encoder": {"config": {"abs_spi_cs_gpio_pin": 1, "bandwidth": 1000.0, "calib_range": 0.019999999552965164, "calib_scan_distance": 50.26548385620117, "calib_scan_omega": 12.566370964050293, "cpr": 8192, "direction": 1, "enable_phase_interpolation": true, "find_idx_on_lockin_only": false, "hall_polarity_calibrated": false, "hall_polarity": 0, "ignore_illegal_hall_state": false, "index_offset": 0.0, "mode": 0, "phase_offset_float": 0.7252781391143799, "phase_offset": 4264, "pre_calibrated": true, "sincos_gpio_pin_cos": 4, "sincos_gpio_pin_sin": 3, "use_index_offset": true, "use_index": true}}, "max_endstop": {"config": {"debounce_ms": 50, "enabled": false, "gpio_num": 0, "is_active_high": false, "offset": 0.0}}, "mechanical_brake": {"config": {"gpio_num": 0, "is_active_low": true}}, "min_endstop": {"config": {"debounce_ms": 50, "enabled": false, "gpio_num": 0, "is_active_high": false, "offset": 0.0}}, "motor": {"config": {"I_bus_hard_max": Infinity, "I_bus_hard_min": -Infinity, "I_leak_max": 0.10000000149011612, "R_wL_FF_enable": false, "acim_autoflux_attack_gain": 10.0, "acim_autoflux_decay_gain": 1.0, "acim_autoflux_enable": false, "acim_autoflux_min_Id": 10.0, "acim_gain_min_flux": 10.0, "bEMF_FF_enable": false, "calibration_current": 20.0, "current_control_bandwidth": 1000.0, "current_lim_margin": 8.0, "current_lim": 30.0, "dc_calib_tau": 0.20000000298023224, "inverter_temp_limit_lower": 100.0, "inverter_temp_limit_upper": 120.0, "motor_type": 0, "phase_inductance": 1.4499735698336735e-05, "phase_resistance": 0.03428403660655022, "pole_pairs": 7, "pre_calibrated": true, "requested_current_range": 60.0, "resistance_calib_max_voltage": 2.0, "torque_constant": 0.030629629269242287, "torque_lim": Infinity}, "fet_thermistor": {"config": {"enabled": true, "temp_limit_lower": 100.0, "temp_limit_upper": 120.0}}, "motor_thermistor": {"config": {"enabled": false, "gpio_pin": 4, "poly_coefficient_0": 0.0, "poly_coefficient_1": 0.0, "poly_coefficient_2": 0.0, "poly_coefficient_3": 0.0, "temp_limit_lower": 100.0, "temp_limit_upper": 120.0}}}, "sensorless_estimator": {"config": {"observer_gain": 1000.0, "pll_bandwidth": 1000.0, "pm_flux_linkage": 0.0015800000401213765}}, "trap_traj": {"config": {"accel_limit": 0.5, "decel_limit": 0.5, "vel_limit": 2.0}}}, "axis1": {"acim_estimator": {"config": {"slip_velocity": 14.706000328063965}}, "config": {"dir_gpio_pin": 8, "enable_sensorless_mode": false, "enable_step_dir": false, "enable_watchdog": false, "startup_closed_loop_control": false, "startup_encoder_index_search": false, "startup_encoder_offset_calibration": false, "startup_homing": false, "startup_motor_calibration": false, "step_dir_always_on": false, "step_gpio_pin": 7, "watchdog_timeout": 0.0, "calibration_lockin": {"accel": 20.0, "current": 10.0, "ramp_distance": 3.1415927410125732, "ramp_time": 0.4000000059604645, "vel": 40.0}, "can": {"encoder_rate_ms": 10, "heartbeat_rate_ms": 100, "is_extended": false, "node_id": 1}, "general_lockin": {"accel": 20.0, "current": 10.0, "finish_distance": 100.0, "finish_on_distance": false, "finish_on_enc_idx": false, "finish_on_vel": false, "ramp_distance": 3.1415927410125732, "ramp_time": 0.4000000059604645, "vel": 40.0}, "sensorless_ramp": {"accel": 200.0, "current": 10.0, "finish_distance": 100.0, "finish_on_distance": false, "finish_on_enc_idx": false, "finish_on_vel": true, "ramp_distance": 3.1415927410125732, "ramp_time": 0.4000000059604645, "vel": 400.0}}, "controller": {"config": {"axis_to_mirror": 255, "circular_setpoint_range": 1.0, "circular_setpoints": false, "control_mode": 3, "electrical_power_bandwidth": 20.0, "enable_current_mode_vel_limit": true, "enable_gain_scheduling": false, "enable_overspeed_error": true, "enable_vel_limit": true, "gain_scheduling_width": 10.0, "homing_speed": 0.25, "inertia": 0.0, "input_filter_bandwidth": 2.0, "input_mode": 1, "load_encoder_axis": 1, "mechanical_power_bandwidth": 20.0, "mirror_ratio": 1.0, "pos_gain": 15.0, "spinout_electrical_power_threshold": 10.0, "spinout_mechanical_power_threshold": -10.0, "steps_per_circular_range": 1024, "torque_mirror_ratio": 0.0, "torque_ramp_rate": 0.009999999776482582, "vel_gain": 0.15000000596046448, "vel_integrator_gain": 6.0, "vel_limit": 25.0, "vel_limit_tolerance": 1.2000000476837158, "vel_ramp_rate": 1.0, "anticogging": {"anticogging_enabled": true, "calib_anticogging": false, "calib_pos_threshold": 1.0, "calib_vel_threshold": 1.0, "cogging_ratio": 1.0, "index": 0, "pre_calibrated": true}}}, "encoder": {"config": {"abs_spi_cs_gpio_pin": 1, "bandwidth": 1000.0, "calib_range": 0.019999999552965164, "calib_scan_distance": 50.26548385620117, "calib_scan_omega": 12.566370964050293, "cpr": 8192, "direction": 1, "enable_phase_interpolation": true, "find_idx_on_lockin_only": false, "hall_polarity_calibrated": false, "hall_polarity": 0, "ignore_illegal_hall_state": false, "index_offset": 0.0, "mode": 0, "phase_offset_float": 1.2550944089889526, "phase_offset": 4596, "pre_calibrated": true, "sincos_gpio_pin_cos": 4, "sincos_gpio_pin_sin": 3, "use_index_offset": true, "use_index": true}}, "max_endstop": {"config": {"debounce_ms": 50, "enabled": false, "gpio_num": 0, "is_active_high": false, "offset": 0.0}}, "mechanical_brake": {"config": {"gpio_num": 0, "is_active_low": true}}, "min_endstop": {"config": {"debounce_ms": 50, "enabled": false, "gpio_num": 0, "is_active_high": false, "offset": 0.0}}, "motor": {"config": {"I_bus_hard_max": Infinity, "I_bus_hard_min": -Infinity, "I_leak_max": 0.10000000149011612, "R_wL_FF_enable": false, "acim_autoflux_attack_gain": 10.0, "acim_autoflux_decay_gain": 1.0, "acim_autoflux_enable": false, "acim_autoflux_min_Id": 10.0, "acim_gain_min_flux": 10.0, "bEMF_FF_enable": false, "calibration_current": 20.0, "current_control_bandwidth": 1000.0, "current_lim_margin": 8.0, "current_lim": 30.0, "dc_calib_tau": 0.20000000298023224, "inverter_temp_limit_lower": 100.0, "inverter_temp_limit_upper": 120.0, "motor_type": 0, "phase_inductance": 1.4372079931490589e-05, "phase_resistance": 0.042437147349119186, "pole_pairs": 7, "pre_calibrated": true, "requested_current_range": 60.0, "resistance_calib_max_voltage": 2.0, "torque_constant": 0.030629629269242287, "torque_lim": Infinity}, "fet_thermistor": {"config": {"enabled": true, "temp_limit_lower": 100.0, "temp_limit_upper": 120.0}}, "motor_thermistor": {"config": {"enabled": false, "gpio_pin": 4, "poly_coefficient_0": 0.0, "poly_coefficient_1": 0.0, "poly_coefficient_2": 0.0, "poly_coefficient_3": 0.0, "temp_limit_lower": 100.0, "temp_limit_upper": 120.0}}}, "sensorless_estimator": {"config": {"observer_gain": 1000.0, "pll_bandwidth": 1000.0, "pm_flux_linkage": 0.0015800000401213765}}, "trap_traj": {"config": {"accel_limit": 0.5, "decel_limit": 0.5, "vel_limit": 2.0}}}, "can": {"config": {"baud_rate": 250000, "protocol": 1}}, "config": {"brake_resistance": 0.4699999988079071, "dc_bus_overvoltage_ramp_end": 25.68000030517578, "dc_bus_overvoltage_ramp_start": 25.68000030517578, "dc_bus_overvoltage_trip_level": 25.68000030517578, "dc_bus_undervoltage_trip_level": 8.0, "dc_max_negative_current": -10.0, "dc_max_positive_current": Infinity, "enable_brake_resistor": false, "enable_can_a": true, "enable_dc_bus_overvoltage_ramp": false, "enable_i2c_a": false, "enable_uart_a": true, "enable_uart_b": false, "enable_uart_c": false, "error_gpio_pin": 0, "gpio10_mode": 11, "gpio11_mode": 2, "gpio12_mode": 12, "gpio13_mode": 12, "gpio14_mode": 2, "gpio15_mode": 7, "gpio16_mode": 7, "gpio1_mode": 4, "gpio2_mode": 4, "gpio3_mode": 3, "gpio4_mode": 3, "gpio5_mode": 3, "gpio6_mode": 0, "gpio7_mode": 0, "gpio8_mode": 0, "gpio9_mode": 11, "max_regen_current": 10.0, "uart0_protocol": 3, "uart1_protocol": 3, "uart2_protocol": 3, "uart_a_baudrate": 115200, "uart_b_baudrate": 115200, "uart_c_baudrate": 115200, "usb_cdc_protocol": 3, "gpio1_pwm_mapping": {"endpoint": null, "max": 0.0, "min": 0.0}, "gpio2_pwm_mapping": {"endpoint": null, "max": 0.0, "min": 0.0}, "gpio3_analog_mapping": {"endpoint": null, "max": 0.0, "min": 0.0}, "gpio3_pwm_mapping": {"endpoint": null, "max": 0.0, "min": 0.0}, "gpio4_analog_mapping": {"endpoint": null, "max": 0.0, "min": 0.0}, "gpio4_pwm_mapping": {"endpoint": null, "max": 0.0, "min": 0.0}}}" > odrive-config.json
odrivetool restore-config odrive-config.json
```
- calibration procedure
open the odrive tool and use the following commands in the correct order. Repeat commands for both axis0 and axis1.
```
odrivetool
```
```
odrv0.axis0.requested_state = 3
odrv0.axis0.requested_state = 6
odrv0.axis0.requested_state = 7
```
set configs
```
odrv0.axis0.encoder.config.pre_calibrated = True
odrv0.axis0.controller.config.anticogging.anticogging_enabled = False
odrv0.axis0.controller.config.control_mode = CONTROL_MODE_POSITION_CONTROL
```
> if error occurs on anitcogging it is likely you have old firmware. You can update with the following command.
```
sudo env PATH="$PATH" odrivetool dfu
```
Now everything is ready to create an anti-cogging table.
```
start_liveplotter(lambda:[odrv0.axis0.encoder.pos_estimate, odrv0.axis0.controller.pos_setpoint])
odrv0.axis0.requested_state = 8
odrv0.axis0.controller.start_anticogging_calibration()
odrv0.save_configuration()
```
## 4.5 Documentation
- clone the repo or download a zip file from the cloud portal.
```
unzip scout-docs.zip /home/pvadmin/envs/cx-appcenter/apps/
```
- copy the service file
```
sudo cp cx_scout_docs.service /etc/systemd/system/
```
- start and enable the systemd service
```
sudo systemctl start cx_scout_docs.service
sudo systemctl enable cx_scout_docs.service
```

## 4.6 Install Tjess
- clone the repo or download a deb file from the cloud portal.
 ```
 sudo dpkg -i <name-of-package>
 sudo apt install -f
 ```
