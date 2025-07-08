# Home Assistant Add-on: OLED I2C Screen Adapter

This Home Assistant add-on enables the use of an OLED screen via I2C on a Raspberry Pi running Home Assistant Operating System (HA OS).  
HA OS typically restricts direct access to I2C due to system permissions, but this add-on bypasses those limitations by leveraging the Supervisor.

## Features

- **I2C Screen Support**: Compatible with OLED screens using GME12864, SSD1306, or SH1106 controllers.
- **MQTT Integration**: Receives data via MQTT topics to display system info like CPU temperature, memory usage, and network status.
- **Customizable Display Logic**: The `display_controller.py` script can be modified to define new display modes, MQTT topics, or behaviors.
- **Supervisor Execution**: Ensures compatibility with HA OS by executing with Supervisor permissions.
- **UI-Based Configuration**: Add-on options are configurable through the Home Assistant interface 
## Installation

### Prerequisites

1. Ensure that I2C is enabled on your Raspberry Pi.  You must enable it manually by editing the `config.txt` file in the boot partition of your Raspberry Pi.:
    ```txt
    dtparam=i2c_arm=on
    dtparam=i2c1=on
    ```
    (This add-on does **not** enable I2C automatically.)

2. Access your Home Assistant instance via SSH or SFTP to upload the custom add-on.


### Steps

1. Clone or download this repository.
2. Copy the repository folder into the `addons` directory of your Home Assistant instance.
3. Restart Home Assistant to detect the new add-on.
4. Navigate to **Settings > Add-ons** in Home Assistant and install the add-on.
5. Configure the add-on via the Home Assistant UI.

For advanced users, you can manually edit the `config.yaml` file inside the add-on folder to customize settings beyond the UI options. However, this is usually not necessary.

Refer to the official documentation for more info:  
[Home Assistant Add-ons](https://www.home-assistant.io/docs/addons/)

## Configuration

### Add-on Options

All options are accessible through the Home Assistant UI:

- **mqtt_broker**: MQTT broker address (default: `core-mqtt`)
- **mqtt_port**: MQTT broker port (default: `1883`)
- **mqtt_user**: MQTT username (default: `homeassistant`)
- **mqtt_password**: MQTT password
- **i2c_address**: I2C address of the display (default: `0x3C`)
- **i2c_port**: I2C port number (default: `1`)
- **display_width**: Screen width in pixels (default: `128`)
- **display_height**: Screen height in pixels (default: `64`)
- **display_type**: OLED controller type (`ssd1306` or `sh1106`)
- **refresh_interval**: Time between screen refreshes in seconds (default: `5`)
- **auto_brightness**: Enable or disable auto brightness (default: `true`)
- **default_brightness**: Brightness level (0–255)

### MQTT Topics

The screen listens for MQTT messages on the following topics:

- `screen/gme12864/text`: Send custom text to display
- `screen/gme12864/command`: Send commands (e.g., `clear`, `off`, `on`)
- `screen/gme12864/mode`: Set the current display mode (`auto`, `manual`, `system`, `network`, `sensors`)
- `screen/gme12864/brightness`: Set brightness level
- `screen/gme12864/refresh`: Adjust refresh interval

You can freely extend or change these topics by editing the `display_controller.py` file.

## Usage 
###   Display Modes

The add-on supports multiple display modes:

- **Auto**: Cycles through various screens (system, network, sensors)
- **Manual**: Displays static custom text
- **System**: Shows CPU usage, memory usage, uptime
- **Network**: Displays IP address and connectivity status
- **Sensors**: Displays temperature, humidity, or other sensor data

###  Customization

You can customize the display logic by editing the `display_controller.py` script. This allows you to add new display modes, change MQTT topic handling, or build menu systems. Advanced users are encouraged to enhance this file according to their needs.

> While the `config.yaml` file can also be edited manually Most configurations can be handled through the Home Assistant UI.


## Dependencies

The following Python libraries are used by this add-on and are automatically installed:

- `paho-mqtt`
- `smbus`
- `smbus2`
- `Pillow`
- `luma.oled`
- `psutil`
- `netifaces`

## Limitations

- Requires Supervisor permissions to access I2C in HA OS.
- I2C must be enabled manually in the Raspberry Pi boot configuration.
- While designed for Raspberry Pi, this add-on may also work on other devices with I2C support.

## Contributing

Feel free to contribute by submitting issues or pull requests. You can:

- Add support for new OLED controllers
- Introduce new display modes or animations
- Improve MQTT topic parsing
- Refactor or extend `display_controller.py`

## License

This project is licensed under the MIT License. See the `LICENSE` file for more information.

## Acknowledgments

- Home Assistant for providing a powerful automation platform  
- Luma OLED for the screen rendering library  
- The Raspberry Pi community for tutorials and support

---

For any questions or issues, please open an issue on the GitHub repository.
