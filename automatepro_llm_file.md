

# --- quick-start-guide.mdx ---

---
sidebar_position: 2
---

import automateproComponentsImg from './images/automatepro-components-1.png';
import automateproDevBoardImg from './images/automatepro_dev_board.png';
import automateproCLeavySealImg from './images/automatepro_TE_Leavyseal.png';
import connectBreakoutBoard from './mp4/connect-breakout-board.mp4';
import connectFakra from './mp4/connect-fakra.mp4';
import connectEthernet from './mp4/connect-ethernet-cable.mp4';
import connectPowerSupply from './mp4/connect-power-supply.mp4';
import foxgloveInstallation from './mp4/foxglove-installation.mp4';

# Quick Start Guide

Welcome to **AutomatePro**. This guide provides clear, step-by-step instructions to help you set up and begin using your AutomatePro system quickly.

AutomatePro offers two connection options. The production version uses TE Leavyseal connectors for full IP67 protection. The development-board option is intended for prototyping and rapid development, using terminal blocks for easier connectivity.

Both options are interchangeable; however, this Quick Start Guide uses the development-board version for easier prototyping and wiring.

<div style={{ columnCount: 2, columnGap: '150px' }}>
<div>
    <p><strong>AutomatePro with Development Board</strong></p>
    <img
      src={automateproDevBoardImg}
      alt="AutomatePro with development board"
      style={{ width: '100%' }}
    />
  </div>
  <div>
    <p><strong>AutomatePro with TE Leavyseal</strong></p>
    <img
      src={automateproCLeavySealImg}
      alt="AutomatePro with TE Leavyseal"
      style={{ width: '100%' }}
    />
  </div>
</div>

## What's in the Box

The exact contents depend on the configuration ordered. For this guide, assume the following components are included:

- **AutomatePro main unit**
- **Power supply**
- **Ethernet cable**
- **Optional accessories** (varies by configuration)

These are as follows:

<img src={automateproComponentsImg} alt="AutomatePro components" style={{width: '100%'}}/>

## Connecting AutomatePro

### Hardware Connection

Use the provided power supply and Ethernet cable to connect your AutomatePro device, as shown in the illustrations below.

1. **Connect the breakout board, if applicable.**

   If your configuration includes a breakout board, securely attach it to the AutomatePro unit. Ensure all pins are properly aligned before applying pressure to avoid damage.

   <div style={{ display: 'flex', justifyContent: 'center', padding: '20px', margin: '0px 0px 30px 0px' }}>
       <video src={connectBreakoutBoard} controls preload="metadata" playsInline aria-label="Connect breakout board" style={{width: '70%'}}/>
   </div>

2. **Connect FAKRA antennas and cameras.**

   Attach GNSS, Wi-Fi, 5G, and camera FAKRA connectors to their designated ports. The connectors are color-coded for easy identification. Make sure each connector is fully inserted and securely locked to ensure a stable and reliable connection.

   <div style={{ display: 'flex', justifyContent: 'center', padding: '20px', margin: '0px 0px 30px 0px' }}>
       <video src={connectFakra} controls preload="metadata" playsInline aria-label="Connect FAKRA cables" style={{width: '70%'}}/>
   </div>

3. **Connect Ethernet.**

   Use the supplied Ethernet cable to establish a wired network connection. Insert one end into AutomatePro and connect the other end to your router, switch, or development network.

   <div style={{ display: 'flex', justifyContent: 'center', padding: '20px', margin: '0px 0px 30px 0px' }}>
       <video src={connectEthernet} controls preload="metadata" playsInline aria-label="Connect Ethernet cable" style={{width: '70%'}}/>
   </div>

4. **Connect power.**

   Plug the provided power supply into AutomatePro. Ensure the connector polarity aligns with the labeled indicators to prevent improper connection.

   <div style={{ display: 'flex', justifyContent: 'center', padding: '20px', margin: '0px 0px 30px 0px' }}>
       <video src={connectPowerSupply} controls preload="metadata" playsInline aria-label="Connect power supply" style={{width: '70%'}}/>
   </div>

5. **Power on AutomatePro.**

   Once all connections are in place, turn on the power supply to start AutomatePro. The device begins its boot sequence and the status LEDs start blinking. Refer to the [Status LED functions](/automatepro/system-overview#led-functions) for more information on interpreting LED behavior.

### Accessing AutomatePro

Once powered on and connected to the network, AutomatePro receives an IP address from your network's DHCP server. You can identify this IP address by checking the DHCP client list on your router, or by using a network scanning tool such as `nmap` or `arp-scan` to detect connected devices on your network.

After locating the IP address, you can access AutomatePro through either **SSH** for command-line access or **Remote Desktop** for graphical access.

#### SSH (Command-Line Access)

Use SSH for command-line access:

```bash
ssh admin@<ip-address>
```

:::info
Default credentials:

```text
username: admin
password: password
```
:::

:::warning[Important]
Change the default password after the first login.
:::

#### Remote Desktop (Graphical Access)

For graphical access, follow the [Remote Desktop Guide](/automatepro/manual/misc/desktop#remote-desktop).

To configure Wi-Fi, refer to the [Wi-Fi Setup Guide](/automatepro/manual/connectivity/wifi#using-nmcli).

## Verify the System

Run these checks from an SSH session on AutomatePro.

1. Confirm the main services are running:

   ```bash
   systemctl status automatepro-core-driver automatepro-io-agent automatepro-foxglove-bridge
   ```

   Healthy services show `active (running)`. If a service is stopped, inspect logs with:

   ```bash
   sudo journalctl -u automatepro-core-driver -n 100 --no-pager
   ```

2. Confirm ROS 2 can see the driver nodes:

   ```bash
   source /opt/ros/humble/setup.bash
   ros2 node list
   ```

   Depending on the installed hardware options, you should see nodes such as:

   ```text
   /automatepro_driver_manager
   /automatepro_gnss_position_node
   /automatepro_gnss_heading_node
   /automatepro_imu_driver
   /automatepro_cam1_node
   /automatepro_cam2_node
   ```

3. Confirm common sensor topics are present:

   ```bash
   ros2 topic list | grep -E '/sensor/imu|/sensor/gnss|/camera/.*/image_raw|/io/'
   ```

   Optional hardware only appears when that hardware and its driver are enabled.

:::tip
For service management, configuration paths, package installation, and log commands, see [Software Services](/automatepro/manual/misc/software-services).
:::

## Foxglove Interface

AutomatePro can be monitored and controlled using [Foxglove Studio](https://foxglove.dev/), a powerful visualization and observability tool designed for robotics applications.

1. **Install Foxglove Studio.**

   Download and install Foxglove Studio from the [official website](https://foxglove.dev/download). Follow the installation instructions appropriate for your operating system.

2. **Import AutomatePro dashboards and extensions.**

   Download the AutomatePro Foxglove extension and preconfigured dashboards from the [automatepro_foxglove repository](https://github.com/Lemvos/automatepro_foxglove). Import both into Foxglove Studio by following the steps in the video tutorial below or in the repository documentation.

   <div style={{ display: 'flex', justifyContent: 'center', padding: '20px', margin: '0px 0px 30px 0px' }}>
       <video src={foxgloveInstallation} controls preload="metadata" playsInline aria-label="Foxglove installation" style={{width: '100%'}}/>
   </div>

3. **Connect Foxglove Studio to AutomatePro.**

   Connect Foxglove Studio to AutomatePro by entering the IP address and default bridge port in the connection dialog:

   ```bash
   ws://<ip-address>:8765
   ```

<div style={{ display: 'flex', justifyContent: 'center', margin: '0px 0px 30px 0px' }}>
    <iframe width="100%" style={{"aspect-ratio": "16 / 9", "justifyContent": 'center', "padding": '20px', "margin": '0px 0px 30px 0px'}}
        src="https://www.youtube.com/embed/WDgLLtXSkhQ?si=cR46lrNCMHGbpyEx" title="YouTube video player"
        frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
        referrerpolicy="strict-origin-when-cross-origin" allowfullscreen>
    </iframe>
</div>


# --- release-notes.mdx ---

---
sidebar_position: 3
---

# Release Notes

Stay up to date with the latest changes and improvements.

## Software

### Version 1.0.2
- Enhanced the IMU driver with calibration save and reinitialize services, plus magnetometer and accuracy reporting.
- Added H.264 compressed streaming frequency configuration and automatic hardware recovery for the GMSL2 cameras.
- Separated the dedicated Foxglove Bridge service for remote monitoring.
- Added GPIO line recovery for GNSS dead reckoning.

### Version 1.0.1
- Added an installer to automate initial setup and streamline future updates.
- Enhanced IO Controller with new features for flexible analog and digital input configuration.
- Resolved minor bugs to improve overall stability and reliability.

### Version 1.0.0
- First official release
- Core features implemented


# --- system-overview.mdx ---

---
sidebar_position: 2
toc_max_heading_level: 5
---

# System Overview

import AutomateProImg from './images/AutomatePro_Breakout.png'

AutomatePro is a rugged, all-in-one ROS 2 platform designed for outdoor robotics and automation. It combines critical subsystems such as high-performance computing (with AI/ML acceleration), extensive I/O, an integrated 9-axis IMU and RTK GNSS, and robust IP67-rated housing. With support for 4G/5G, Wi-Fi, Bluetooth, and a wide voltage input range, it provides a reliable and flexible foundation for advanced robotic applications. Pre-configured software tools ensure fast and seamless deployment.

The system’s integrated I/O significantly reduces the need for external components. Features include H-bridges or solid-state switches, digital and analog inputs/outputs, and support for warning systems (lights and sound). For navigation, it supports precise RTK GNSS (3–6 cm accuracy), dead reckoning via wheel inputs, differential heading, and satellite-based RTK correction—ideal for environments where magnetometers are unreliable.

AutomatePro supports a wide range of communication protocols including CAN, Ethernet, RS485, GMSL2, and USB 3.0. This makes it easy to connect with cameras, LIDARs, radars, motor controllers, and remote monitoring systems. Internally, it includes two regulated power outputs (12V and 5V) with voltage and current monitoring to support additional peripherals like sensors or actuators.

Available with four levels of compute based on the NVIDIA [Jetson Orin Series](https://developer.nvidia.com/embedded/jetson-modules) (from Nano 4GB to Orin NX 16GB), AutomatePro comes preloaded with JetPack 6.0 and ROS 2 drivers for all onboard hardware. These drivers are pre-installed and tested, offered by default in Docker containers for modular updates and maintenance. For advanced users, native host execution is also supported.

An overview of the AutomatePro:

<img src={AutomateProImg} alt="AutomatePro with breakout board" style={{width: '100%'}}/>

### Features

Depending on the configuration, AutomatePro can be equipped with the following features:



<div style={{ columnCount: 2, columnGap: '20px' }}>
  <p>
    <strong>Software:</strong><br />
    - Base ROS 2 drivers for all sensors and IO<br />
    - Linux Ubuntu 22.04<br />
    - JetPack 6.0<br /><br />

    <strong>Sensors:</strong><br />
    - 9-axis IMU (3-axis compass, 3-axis gyro, 3-axis accelerometer)<br />
    - GNSS RTK positioning (Wheel tick and direction input optional)<br />
    - GNSS Heading<br />
    - 2x or 4x GMSL2 Camera Inputs with PoC (Power Over Coax)<br /><br />

    <strong>Integrated IO:</strong><br />
    - 10x Digital Inputs<br />
    - 12x Digital Outputs (~500mA)<br />
    - 12x Digital Drive software configurable as half or full-bridge (7A)<br />
    - 14x Analog Inputs (0-10V or 4-20mA (on request))<br />
    - E-stop Input<br />
    - Warning Light and Buzzer Drivers<br /><br />

    <strong>Connectivity:</strong><br />
    - 5G/4G Connectivity<br />
    - Wi-Fi<br />
    - Bluetooth<br />
    - RS485<br />
    - CAN FD Bus<br />
    - 1GbE Ethernet<br />
    - USB 3.1 with Alt Mode for DisplayPort connection<br /><br />

    <strong>Power System:</strong><br />
    - 15.5V to 58V Supply Voltage Range<br />
    - Extensive power supply monitoring with current and voltage sensing on most onboard power supplies and all external facing supplies<br />
    - 12V 8A Onboard power supply for external systems<br />
    - 5V 3A onboard power supply for external systems<br /><br />

    <strong>Environmental:</strong><br />
    - IP67 water ingress protection<br />
    - Robust keyed connectors to prevent systems from being incorrectly connected<br /><br />

    <strong>Market Ready:</strong><br />
    - Rapid development with dev tool kit
    - Range of supported external sensors and components
  </p>
</div>



## Hardware Architecture

A high-level layout of AutomatePro shows the interconnections between the different systems. Most IO is controlled by the MCU, while high-bandwidth systems such as GMSL, USB3, Wi-Fi, and GbE are fed directly to the Jetson SOM. A communication bridge between the MCU and SOM transfers data and is initiated on startup.

import AutomatePro_Hardware_Diagram_Connection_Overview from './images/AutomatePro_Hardware_Diagrams_Connection_Overview.png'

<img src={AutomatePro_Hardware_Diagram_Connection_Overview} alt="AutomatePro hardware connection overview" style={{width: '90%'}}/>
<br/>
<br/>

A simplified layout of AutomatePro's internal power supplies is shown below. This includes only the power supplies that are relevant to external systems and excludes regulators for onboard systems. The diagram also shows onboard current and voltage measurements for the different rails, along with onboard fuses.

import AutomatePro_Hardware_Diagram_PSU from './images/AutomatePro_Hardware_Diagram_PSU.png'

<img src={AutomatePro_Hardware_Diagram_PSU} alt="AutomatePro power supply diagram" style={{width: '100%'}}/>
<br/>
<br/>




### Connector Pin Map & Functions
The two primary connectors used on AutomatePro are from the TE Leavyseal connector series. The pin allocation is shown below:

import connector_def from './images/Connector_Definition.png'

<img src={connector_def} style={{width: '60%'}}/>

#### Connector A
import ReactTable from './components/sortable_table';

<ReactTable
  columns={[
    {
      Header: 'PIN NO.',
      accessor: 'pinNo',
    },
    {
      Header: 'PIN NAME',
      accessor: 'pinName',
    },
    {
      Header: 'I/O TYPE',
      accessor: 'ioType',
    },
    {
      Header: 'DESCRIPTION',
      accessor: 'description',
    },
  ]}
  data={[
  { pinNo: 'A1', pinName: 'NC', ioType: 'N/A', description: 'Not Connected' },
  { pinNo: 'A2', pinName: '+12V', ioType: 'Power Out', description: '12V out with 0.75A self-resetting fuse' },
  { pinNo: 'A3', pinName: 'GND', ioType: 'Ground', description: 'Sensor Ground' },
  { pinNo: 'A4', pinName: 'AIN_01', ioType: 'Input', description: '0-10V analog input (or 4-20mA on request)' },
  { pinNo: 'A5', pinName: 'E-STOP', ioType: 'Input', description: 'E-Stop button sensor for detecting when E-Stop has been pressed.' },
  { pinNo: 'A6', pinName: '+12V', ioType: 'Power Out', description: '12V out with 0.75A self-resetting fuse' },
  { pinNo: 'A7', pinName: 'GND', ioType: 'Ground', description: 'Sensor Ground' },
  { pinNo: 'A8', pinName: 'AIN_02', ioType: 'Input', description: '0-10V analog input (or 4-20mA on request)' },
  { pinNo: 'A9', pinName: 'GND', ioType: 'Ground', description: '' },
  { pinNo: 'A10', pinName: '+12V', ioType: 'Power Out', description: '12V out with 0.75A self-resetting fuse' },
  { pinNo: 'A11', pinName: 'GND', ioType: 'Ground', description: 'Sensor Ground' },
  { pinNo: 'A12', pinName: 'AIN_03', ioType: 'Input', description: '0-10V analog input (or 4-20mA on request)' },
  { pinNo: 'A13', pinName: 'RS485_P', ioType: 'Input/Output', description: 'RS485 serial communications' },
  { pinNo: 'A14', pinName: '+12V', ioType: 'Power Out', description: '12V out with 0.75A self-resetting fuse' },
  { pinNo: 'A15', pinName: 'GND', ioType: 'Ground', description: 'Sensor Ground' },
  { pinNo: 'A16', pinName: 'AIN_04', ioType: 'Input', description: '0-10V analog input (or 4-20mA on request)' },
  { pinNo: 'A17', pinName: 'RS485_N', ioType: 'Input/Output', description: 'RS485 serial communications' },
  { pinNo: 'A18', pinName: '+12V', ioType: 'Power Out', description: '12V out with 0.75A self-resetting fuse' },
  { pinNo: 'A19', pinName: 'GND', ioType: 'Ground', description: 'Sensor Ground' },
  { pinNo: 'A20', pinName: 'AIN05', ioType: 'Input', description: '0-10V analog input (or 4-20mA on request)' },
  { pinNo: 'A21', pinName: 'GND', ioType: 'Ground', description: '' },
  { pinNo: 'A22', pinName: '+12V', ioType: 'Power Out', description: '12V out with 0.75A self-resetting fuse' },
  { pinNo: 'A23', pinName: 'GND', ioType: 'Ground', description: 'Sensor Ground' },
  { pinNo: 'A24', pinName: 'AIN06', ioType: 'Input', description: '0-10V analog input (or 4-20mA on request)' },
  { pinNo: 'A25', pinName: 'CAN1_L', ioType: 'Input/Output', description: 'CAN channel 1 communications' },
  { pinNo: 'A26', pinName: '+12V', ioType: 'Power Out', description: '12V out with 0.75A self-resetting fuse' },
  { pinNo: 'A27', pinName: 'GND', ioType: 'Ground', description: 'Sensor Ground' },
  { pinNo: 'A28', pinName: 'AIN07', ioType: 'Input', description: '0-10V analog input (or 4-20mA on request)' },
  { pinNo: 'A29', pinName: 'CAN1_H', ioType: 'Input/Output', description: 'CAN channel 1 communications' },
  { pinNo: 'A30', pinName: '+5V', ioType: 'Power Out', description: '5V out with 0.75A self-resetting fuse' },
  { pinNo: 'A31', pinName: 'GND', ioType: 'Ground', description: 'Sensor Ground' },
  { pinNo: 'A32', pinName: 'AIN08', ioType: 'Input', description: '0-10V analog input (or 4-20mA on request)' },
  { pinNo: 'A33', pinName: 'GND', ioType: 'Ground', description: '' },
  { pinNo: 'A34', pinName: '+5V', ioType: 'Power Out', description: '5V out with 0.75A self-resetting fuse' },
  { pinNo: 'A35', pinName: 'GND', ioType: 'Ground', description: 'Sensor Ground' },
  { pinNo: 'A36', pinName: 'AIN09', ioType: 'Input', description: '0-10V analog input (or 4-20mA on request)' },
  { pinNo: 'A37', pinName: 'CAN2_L', ioType: 'Input/Output', description: 'CAN channel 2 communications' },
  { pinNo: 'A38', pinName: '+5V', ioType: 'Power Out', description: '5V out with 0.75A self-resetting fuse' },
  { pinNo: 'A39', pinName: 'GND', ioType: 'Ground', description: 'Sensor Ground' },
  { pinNo: 'A40', pinName: 'AIN10', ioType: 'Input', description: '0-10V analog input (or 4-20mA on request)' },
  { pinNo: 'A41', pinName: 'CAN2_H', ioType: 'Input/Output', description: 'CAN channel 2 communications' },
  { pinNo: 'A42', pinName: '+5V', ioType: 'Power Out', description: '5V out with 0.75A self-resetting fuse' },
  { pinNo: 'A43', pinName: 'GND', ioType: 'Ground', description: 'Sensor Ground' },
  { pinNo: 'A44', pinName: 'AIN11', ioType: 'Input', description: '0-10V analog input (or 4-20mA on request)' },
  { pinNo: 'A45', pinName: 'GND', ioType: 'Ground', description: '' },
  { pinNo: 'A46', pinName: '+5V', ioType: 'Power Out', description: '5V out with 0.75A self-resetting fuse' },
  { pinNo: 'A47', pinName: 'GND', ioType: 'Ground', description: 'Sensor Ground' },
  { pinNo: 'A48', pinName: 'AIN12', ioType: 'Input', description: '0-10V analog input (or 4-20mA on request)' },
  { pinNo: 'A49', pinName: 'NC', ioType: 'N/A', description: 'Do not connect' },
  { pinNo: 'A50', pinName: '+5V', ioType: 'Power Out', description: '5V out with 0.75A self-resetting fuse' },
  { pinNo: 'A51', pinName: 'GND', ioType: 'Ground', description: 'Sensor Ground' },
  { pinNo: 'A52', pinName: 'AIN13', ioType: 'Input', description: '0-10V analog input (or 4-20mA on request)' },
  { pinNo: 'A53', pinName: 'NC', ioType: 'N/A', description: 'Do not connect' },
  { pinNo: 'A54', pinName: '+5V', ioType: 'Power Out', description: '5V out with 0.75A self-resetting fuse' },
  { pinNo: 'A55', pinName: 'GND', ioType: 'Ground', description: 'Sensor Ground' },
  { pinNo: 'A56', pinName: 'AIN14', ioType: 'Input', description: '0-10V analog input (or 4-20mA on request)' },
  { pinNo: 'A57', pinName: 'ACTU_PWR', ioType: 'Power Input', description: 'Power Supply for all Digital Drives' },
  { pinNo: 'A58', pinName: 'NC', ioType: 'N/A', description: 'Do not connect' },
  { pinNo: 'A59', pinName: 'NC', ioType: 'N/A', description: 'Do not connect' },
  { pinNo: 'A60', pinName: 'ACTU_PWR', ioType: 'Power Input', description: 'Power Supply for all Digital Drives' },
  { pinNo: 'A61', pinName: '+12V_IO_OUT', ioType: 'Power Out', description: '+12V out with 8A self-resetting fuse - Used for any high current applications' },
  { pinNo: 'A62', pinName: 'NC', ioType: 'N/A', description: 'Do not connect' },
  ]}
/>
import AutomateProConnectorA from './images/AutomatePro_Connector_A.png'

##### Connector A - Pin map looking top down
<img src={AutomateProConnectorA} alt="AutomatePro connector A pin map" style={{width: '50%'}}/>



#### Connector B

  <ReactTable
  columns = {[
    {
      Header: 'PIN NO.',
      accessor: 'pinNo',
    },
    {
      Header: 'PIN NAME',
      accessor: 'pinName',
    },
    {
      Header: 'I/O TYPE',
      accessor: 'ioType',
    },
    {
      Header: 'DESCRIPTION',
      accessor: 'description',
    },
  ]}

  data = {[
    { pinNo: 'B1', pinName: 'DI_01', ioType: 'Input', description: 'Digital Inputs' },
    { pinNo: 'B2', pinName: 'DO_H_01', ioType: 'Output', description: 'Highside Digital Output @12V' },
    { pinNo: 'B3', pinName: 'DD_H_01', ioType: 'Output', description: 'High Current Drive Output' },
    { pinNo: 'B4', pinName: 'DD_L_01', ioType: 'Sink', description: 'High Current Sink' },
    { pinNo: 'B5', pinName: 'DI_02', ioType: 'Input', description: 'Digital Inputs' },
    { pinNo: 'B6', pinName: 'DO_H_02', ioType: 'Output', description: 'Highside Digital Output @12V' },
    { pinNo: 'B7', pinName: 'DD_H_02', ioType: 'Output', description: 'High Current Drive Output' },
    { pinNo: 'B8', pinName: 'DD_L_02', ioType: 'Sink', description: 'High Current Sink' },
    { pinNo: 'B9', pinName: 'DI_03', ioType: 'Input', description: 'Digital Inputs' },
    { pinNo: 'B10', pinName: 'DO_H_03', ioType: 'Output', description: 'Highside Digital Output @12V' },
    { pinNo: 'B11', pinName: 'DD_H_03', ioType: 'Output', description: 'High Current Drive Output' },
    { pinNo: 'B12', pinName: 'DD_L_03', ioType: 'Sink', description: 'High Current Sink' },
    { pinNo: 'B13', pinName: 'DI_04', ioType: 'Input', description: 'Digital Inputs' },
    { pinNo: 'B14', pinName: 'DO_H_04', ioType: 'Output', description: 'Highside Digital Output @12V' },
    { pinNo: 'B15', pinName: 'DD_H_04', ioType: 'Output', description: 'High Current Drive Output' },
    { pinNo: 'B16', pinName: 'DD_L_04', ioType: 'Sink', description: 'High Current Sink' },
    { pinNo: 'B17', pinName: 'DI_05', ioType: 'Input', description: 'Digital Inputs' },
    { pinNo: 'B18', pinName: 'DO_H_05', ioType: 'Output', description: 'Highside Digital Output @12V' },
    { pinNo: 'B19', pinName: 'DD_H_05', ioType: 'Output', description: 'High Current Drive Output' },
    { pinNo: 'B20', pinName: 'DD_L_05', ioType: 'Sink', description: 'High Current Sink' },
    { pinNo: 'B21', pinName: 'DI_06', ioType: 'Input', description: 'Digital Inputs' },
    { pinNo: 'B22', pinName: 'DO_H_06', ioType: 'Output', description: 'Highside Digital Output @12V' },
    { pinNo: 'B23', pinName: 'DD_H_06', ioType: 'Output', description: 'High Current Drive Output' },
    { pinNo: 'B24', pinName: 'DD_L_06', ioType: 'Sink', description: 'High Current Sink' },
    { pinNo: 'B25', pinName: 'DI_07', ioType: 'Input', description: 'Digital Inputs' },
    { pinNo: 'B26', pinName: 'DO_L_01', ioType: 'Sink', description: 'Low side Sink' },
    { pinNo: 'B27', pinName: 'DD_H_07', ioType: 'Output', description: 'High Current Drive Output' },
    { pinNo: 'B28', pinName: 'DD_L_07', ioType: 'Sink', description: 'High Current Sink' },
    { pinNo: 'B29', pinName: 'DI_08', ioType: 'Input', description: 'Digital Inputs' },
    { pinNo: 'B30', pinName: 'DO_L_02', ioType: 'Sink', description: 'Low side Sink' },
    { pinNo: 'B31', pinName: 'DD_H_08', ioType: 'Output', description: 'High Current Drive Output' },
    { pinNo: 'B32', pinName: 'DD_L_08', ioType: 'Sink', description: 'High Current Sink' },
    { pinNo: 'B33', pinName: 'DI_09', ioType: 'Input', description: 'Digital Inputs' },
    { pinNo: 'B34', pinName: 'DO_L_03', ioType: 'Sink', description: 'Low side Sink' },
    { pinNo: 'B35', pinName: 'DD_H_09', ioType: 'Output', description: 'High Current Drive Output' },
    { pinNo: 'B36', pinName: 'DD_L_09', ioType: 'Sink', description: 'High Current Sink' },
    { pinNo: 'B37', pinName: 'DI_10', ioType: 'Input', description: 'Digital Inputs' },
    { pinNo: 'B38', pinName: 'DO_L_04', ioType: 'Sink', description: 'Low side Sink' },
    { pinNo: 'B39', pinName: 'DD_H_10', ioType: 'Output', description: 'High Current Drive Output' },
    { pinNo: 'B40', pinName: 'DD_L_10', ioType: 'Sink', description: 'High Current Sink' },
    { pinNo: 'B41', pinName: 'NC', ioType: 'N/A', description: 'Not Connected' },
    { pinNo: 'B42', pinName: 'DO_L_05', ioType: 'Sink', description: 'Low side Sink' },
    { pinNo: 'B43', pinName: 'DD_H_11', ioType: 'Output', description: 'High Current Drive Output' },
    { pinNo: 'B44', pinName: 'DD_L_11', ioType: 'Sink', description: 'High Current Sink' },
    { pinNo: 'B45', pinName: 'NC', ioType: 'N/A', description: 'Not Connected' },
    { pinNo: 'B46', pinName: 'DO_L_06', ioType: 'Sink', description: 'Low side Sink' },
    { pinNo: 'B47', pinName: 'DD_H_12', ioType: 'Output', description: 'High Current Drive Output' },
    { pinNo: 'B48', pinName: 'DD_L_12', ioType: 'Sink', description: 'High Current Sink' },
    { pinNo: 'B49', pinName: 'WARNING_LIGHT_1', ioType: 'Sink', description: 'High Current Sink for connecting external light' },
    { pinNo: 'B50', pinName: 'WARNING_LIGHT_2', ioType: 'Sink', description: 'High Current Sink for connecting external light' },
    { pinNo: 'B51', pinName: 'WHEEL_TICK', ioType: 'Input', description: 'Non standard. Contact Lemvos for more information' },
    { pinNo: 'B52', pinName: 'WHEEL_DIRECTION', ioType: 'Input', description: 'Non standard. Contact Lemvos for more information' },
    { pinNo: 'B53', pinName: 'NC', ioType: 'N/A', description: 'Not Connected' },
    { pinNo: 'B54', pinName: 'WARNING_BUZZER', ioType: 'Sink', description: 'High Current Sink for connecting external warning buzzer/alarm' },
    { pinNo: 'B55', pinName: 'NC', ioType: 'N/A', description: 'Not Connected' },
    { pinNo: 'B56', pinName: 'NC', ioType: 'N/A', description: 'Not Connected' },
    { pinNo: 'B57', pinName: 'VBATT', ioType: 'Power Input', description: 'Provides Power to the AutomatePro' },
    { pinNo: 'B58', pinName: 'NC', ioType: 'N/A', description: 'Not Connected' },
    { pinNo: 'B59', pinName: 'GND', ioType: 'Ground', description: 'AutomatePro ground' },
    { pinNo: 'B60', pinName: 'VBATT', ioType: 'Power Input', description: 'Provides Power to the AutomatePro' },
    { pinNo: 'B61', pinName: 'NC', ioType: 'N/A', description: 'Not Connected' },
    { pinNo: 'B62', pinName: 'GND', ioType: 'Ground', description: 'AutomatePro ground' },
  ]}
  />



import AutomateProConnectorB from './images/AutomatePro_Connector_B.png'

##### Connector B - Pin map looking top down
<img src={AutomateProConnectorB} alt="AutomatePro connector B pin map" style={{width: '50%'}}/>

### Operating Conditions
The table provides both the absolute maximum and minimum limits, as well as the recommended operating conditions. It is strongly advised that nominal operations remain within the recommended operating values.
  <ReactTable
  columns = {[
    {
      Header: 'DESCRIPTION',
      accessor: 'description',
    },
    {
      Header: 'ABSOLUTE MAX/MINS',
      accessor: 'absoluteMaxMins',
    },
    {
      Header: 'RECOMMENDED OPERATING VALUES',
      accessor: 'recommendedOperatingValues',
    },
    {
      Header: 'UNITS',
      accessor: 'units',
    },
    {
      Header: 'TOLERANCE',
      accessor: 'tolerance',
    },
  ]}

  data = {[  {
    description: 'Main AutomatePro Power Input [VBATT]',
    absoluteMaxMins: '',
    recommendedOperatingValues: '',
    units: '',
    tolerance: '',
  },
  {
    description: 'Supply Voltage Range',
    absoluteMaxMins: '15.00-60',
    recommendedOperatingValues: '15.5-58',
    units: 'VDC',
    tolerance: '',
  },
  {
    description: 'Expected Power Consumption',
    absoluteMaxMins: '',
    recommendedOperatingValues: '10-200¹',
    units: 'W',
    tolerance: '',
  },
  {
    description: 'Actuator Power Input (Digital Drive) [ACTU_PWR]',
    absoluteMaxMins: '',
    recommendedOperatingValues: '',
    units: '',
    tolerance: '',
  },
  {
    description: 'Voltage Range',
    absoluteMaxMins: '6-60',
    recommendedOperatingValues: '7-42',
    units: 'VDC',
    tolerance: '',
  },
  {
    description: 'Max Current',
    absoluteMaxMins: '60',
    recommendedOperatingValues: '55',
    units: 'A',
    tolerance: '',
  },
  {
    description: 'IO 5V Supply Output [+5V]',
    absoluteMaxMins: '',
    recommendedOperatingValues: '',
    units: '',
    tolerance: '',
  },
  {
    description: 'Output Voltage',
    absoluteMaxMins: '-',
    recommendedOperatingValues: '5',
    units: 'VDC',
    tolerance: '0.5',
  },
  {
    description: 'Current Cutoff',
    absoluteMaxMins: '-',
    recommendedOperatingValues: '3',
    units: 'A',
    tolerance: '',
  },
  {
    description: 'IO 12V Supply Output [+12V]',
    absoluteMaxMins: '',
    recommendedOperatingValues: '',
    units: '',
    tolerance: '',
  },
  {
    description: 'Output Voltage',
    absoluteMaxMins: '-',
    recommendedOperatingValues: '12',
    units: 'VDC',
    tolerance: '0.5',
  },
  {
    description: 'Current Cutoff',
    absoluteMaxMins: '-',
    recommendedOperatingValues: '8',
    units: 'A',
    tolerance: '',
  },
  {
    description: 'Digital Output [DO_H_01-06]',
    absoluteMaxMins: '',
    recommendedOperatingValues: '',
    units: '',
    tolerance: '',
  },
  {
    description: 'Output Voltage',
    absoluteMaxMins: '-',
    recommendedOperatingValues: '12.0',
    units: 'VDC',
    tolerance: '0.5',
  },
  {
    description: 'Max Current Source',
    absoluteMaxMins: '-',
    recommendedOperatingValues: '600.0',
    units: 'mA',
    tolerance: '',
  },
  {
    description: 'PWM Frequency',
    absoluteMaxMins: '-',
    recommendedOperatingValues: '0.5',
    units: 'kHz',
    tolerance: '',
  },
  {
    description: 'Duty Cycle',
    absoluteMaxMins: '-',
    recommendedOperatingValues: '0-100',
    units: '%',
    tolerance: '',
  },
  {
    description: 'Software Response Rate',
    absoluteMaxMins: '-',
    recommendedOperatingValues: '200.0',
    units: 'ms',
    tolerance: '',
  },
  {
    description: 'Digital Output [DO_L_01-06]',
    absoluteMaxMins: '',
    recommendedOperatingValues: '',
    units: '',
    tolerance: '',
  },
  {
    description: 'Sink Voltage',
    absoluteMaxMins: '0-60',
    recommendedOperatingValues: '0-42',
    units: 'VDC',
    tolerance: '',
  },
  {
    description: 'Max Current Sink',
    absoluteMaxMins: '1000',
    recommendedOperatingValues: '600.0',
    units: 'mA',
    tolerance: '',
  },
  {
    description: 'PWM Frequency (DO_L_01-04)',
    absoluteMaxMins: '-',
    recommendedOperatingValues: '15.0',
    units: 'kHz',
    tolerance: '',
  },
  {
    description: 'Duty Cycle',
    absoluteMaxMins: '-',
    recommendedOperatingValues: '0-100',
    units: '%',
    tolerance: '',
  },
  {
    description: 'Software Response Rate',
    absoluteMaxMins: '-',
    recommendedOperatingValues: '200.0',
    units: 'ms',
    tolerance: '',
  },
  {
    description: 'Digital Drive [DD_x_xx]',
    absoluteMaxMins: '',
    recommendedOperatingValues: '',
    units: '',
    tolerance: '',
  },
  {
    description: 'Drive Voltage',
    absoluteMaxMins: '6-60',
    recommendedOperatingValues: '7-42',
    units: 'VDC',
    tolerance: '',
  },
  {
    description: 'Rated Current/Channel',
    absoluteMaxMins: '7',
    recommendedOperatingValues: '5.0',
    units: 'A',
    tolerance: '',
  },
  {
    description: 'PWM Frequency',
    absoluteMaxMins: '-',
    recommendedOperatingValues: '15.0',
    units: 'kHz',
    tolerance: '',
  },
  {
    description: 'Duty Cycle',
    absoluteMaxMins: '-',
    recommendedOperatingValues: '0-100',
    units: '%',
    tolerance: '',
  },
  {
    description: 'Software Response Rate',
    absoluteMaxMins: '-',
    recommendedOperatingValues: '200.0',
    units: 'ms',
    tolerance: '',
  },
  {
    description: 'Warning System',
    absoluteMaxMins: '',
    recommendedOperatingValues: '',
    units: '',
    tolerance: '',
  },
  {
    description: 'Drive Voltage',
    absoluteMaxMins: '0-40',
    recommendedOperatingValues: '0-24',
    units: 'VDC',
    tolerance: '',
  },
  {
    description: 'Rated Current/Channel',
    absoluteMaxMins: '5',
    recommendedOperatingValues: '3.0',
    units: 'A',
    tolerance: '',
  },
  {
    description: 'Software Response Rate',
    absoluteMaxMins: '-',
    recommendedOperatingValues: '200.0',
    units: 'ms',
    tolerance: '',
  },
  {
    description: 'Analog Inputs [AIN_xx]',
    absoluteMaxMins: '',
    recommendedOperatingValues: '',
    units: '',
    tolerance: '',
  },
  {
    description: 'Input Voltage',
    absoluteMaxMins: '0-12',
    recommendedOperatingValues: '0-10',
    units: 'VDC',
    tolerance: '',
  },
  {
    description: 'Sample Rate',
    absoluteMaxMins: '-',
    recommendedOperatingValues: '30.0',
    units: 'Hz',
    tolerance: '',
  },
  {
    description: 'Digital Input [DI_xx]',
    absoluteMaxMins: '',
    recommendedOperatingValues: '',
    units: '',
    tolerance: '',
  },
  {
    description: 'Input Voltage Range',
    absoluteMaxMins: '5-30',
    recommendedOperatingValues: '5-24',
    units: 'VDC',
    tolerance: '1',
  },
  {
    description: 'Max Sample Rate',
    absoluteMaxMins: '-',
    recommendedOperatingValues: '10',
    units: 'Hz',
    tolerance: '',
  },
  {
    description: 'Environmental',
    absoluteMaxMins: '',
    recommendedOperatingValues: '',
    units: '',
    tolerance: '',
  },
  {
    description: 'Operational Temperature Range',
    absoluteMaxMins: '',
    recommendedOperatingValues: '-20 to +45²',
    units: '°C',
    tolerance: '',
  },
  {
    description: 'Ingress Protection',
    absoluteMaxMins: '',
    recommendedOperatingValues: 'IP67',
    units: '-',
    tolerance: '',
  },
  {
    description: 'Mass',
    absoluteMaxMins: '',
    recommendedOperatingValues: '2.2',
    units: 'kg',
    tolerance: '',
  },

  ]}
  />
:::info[Info]
<sup>1</sup> Power consumption depends heavily on attached external systems. Without any external systems connected, the maximum consumption is < 30 W (NX 16GB).

<sup>2</sup> Extended-temperature devices exist; contact Lemvos for more information.
:::
### LED Functions

import LEDs from './images/LEDs.png'

<div style={{ columnCount: 2, columnGap: '20px'}}>
  <p>
    <strong></strong><br />
| LED Designator | LED Colour | Function                                      |
|----------------|------------|-----------------------------------------------|
| L1             | GREEN      | Power good - checks all onboard power supplies   |
| L1             | BLUE       | Wi-Fi - WLAN                                   |
| L1             | RED        | nE-STOP - LED will be off when E-STOP is pressed (E-STOP input low) |
| L2             | BLUE       | Ethernet Link                                 |
| L2             | RED        | Ethernet ACT                                  |
| L3             | GREEN      | SSD Read/Write LED                            |
| L3             | BLUE       | WWAN - 5G RF function is on                   |
| L3             | RED        | Wi-Fi - Bluetooth                              |


    <strong></strong><br />
      <img src={LEDs} style={{width: '30%', paddingTop: '150px'}}/>

    </p>
</div>



### Dimensions

import AMP_GA from './images/AMP-100-AMP-ASSY-REV-A.png'

<img src={AMP_GA} style={{width: '100%'}}/>

All dimensions are in millimeters (mm).



### Compute Modules
AutomatePro supports a range of NVIDIA Jetson Orin modules, each catering to different performance and resource requirements.

Supported NVIDIA Jetson Orin modules:
- Jetson Orin Nano 4GB
- Jetson Orin Nano 8GB
- Jetson Orin NX 8GB
- Jetson Orin NX 16GB

:::info[Info]
For more information on the performance of the different modules, refer to the [Jetson Orin Series page](https://developer.nvidia.com/embedded/jetson-modules).
:::

## Software Architecture

import SoftwareArchImg from './images/software-arch.png'


<div className="image-container">
  <img src={SoftwareArchImg} alt="AutomatePro software architecture"/>
</div>

### OS
AutomatePro operates on Jetson Linux 36.3, which features Linux kernel 5.15 and an Ubuntu 22.04-based root filesystem.
This OS is optimized for the NVIDIA Jetson platform and includes all necessary drivers and libraries pre-installed, ensuring seamless integration and performance.

For more information, refer to the [NVIDIA Developer Guide](https://docs.nvidia.com/jetson/archives/r36.3/DeveloperGuide/index.html).

Jetson Linux 36.3 is part of the [NVIDIA JetPack SDK (ver. 6.0)](https://developer.nvidia.com/embedded/jetpack), which provides a comprehensive set of tools and libraries for developing end-to-end accelerated
AI and ML applications on the Jetson platform.

The kernel is configured with the PREEMPT_RT patch to ensure low-latency and high-priority task execution,
which is crucial for real-time robotics applications.
For more information on the PREEMPT_RT patch, refer to the [official documentation](https://wiki.linuxfoundation.org/realtime/start).

AutomatePro is pre-flashed with the compatible OS. If required, the OS can also be flashed manually. Refer to the
[flashing guide](./manual/diagnostics/flashing.mdx) for detailed instructions.

### ROS 2
AutomatePro is ROS 2 native and provides necessary abstractions to interact with the hardware.
Below is a high-level overview of the ROS 2 nodes.


import ROSImg2 from './images/ros2-overview2.png'

<div className="image-container">
  <img src={ROSImg2} className="ros-img" alt="ROS 2 node overview"/>
</div>
### Docker
AutomatePro leverages Docker for containerized deployment, enhancing ease of deployment, scalability, and management.

#### Containers
By default, the core driver and IO Agent run in Docker containers, while hardware setup and bridge services run as host-level systemd services.
Advanced deployments can still run ROS 2 nodes directly on the host system if required.

All Docker Compose files are in the `/opt/automatepro` directory. They are further orchestrated using `systemd` services.
- `automatepro-core-driver.service` - Core driver nodes for handling GNSS, IMU, Cameras, etc.
- `automatepro-io-agent.service` - IO agent for handling digital and analog I/Os, warning systems, etc.
- `automatepro-foxglove-bridge.service` - Host-side Foxglove Bridge for remote monitoring.
- `automatepro-ros-utils.service` - Optional ROS 2 utilities container for visualization nodes.

##### Core Driver
The `automatepro-core-driver` container runs the essential drivers that abstract the hardware to the ROS 2 nodes.

- [RTK Positioning](./manual/sensors/gnss/position.mdx)
- [RTK Heading](./manual/sensors/gnss/heading.mdx)
- [RTK Correction](./manual/sensors/gnss/correction.mdx)
- [IMU](./manual/sensors/imu.mdx)
- [GMSL2 Camera](./manual/sensors/gmsl2-camera.mdx)

##### IO Agent
The `automatepro-io-agent` container is responsible for the communication between ROS 2 and the micro-ROS nodes running on the MCU, which handles analog and digital I/Os, warning systems, etc.

For more details on IO capabilities, see:
- [Analog Input](./manual/io/analog-in.mdx)
- [Digital Input](./manual/io/digital-in.mdx)
- [Digital Output](./manual/io/digital-out.mdx)
- [Digital Drive Out](./manual/io/digital-drive-out.mdx)
- [Warning Systems](./manual/io/warning-systems.mdx)

For IO controller hardware and firmware diagnostic topics, fields, and fault-code decoding, see [IO Controller](./manual/diagnostics/io-controller.mdx).

<details>
  <summary>Developer Notes: IO Controller firmware</summary>

The IO controller firmware runs on a Renesas RA6M5 MCU as a micro-ROS node named `io_controller`. The firmware uses ROS domain ID `255` on the MCU side and communicates with the compute system through the micro-ROS Agent container started by `automatepro-io-agent.service`.

The packaged IO Agent uses the serial transport path `/dev/serial/by-id/usb-LEMVOS_AMP_IO-Controller_0000000000001-if00`. The upstream micro-ROS Agent also supports other transports such as UDP and TCP, but the AutomatePro service is configured for the serial IO-controller path.

</details>

##### Foxglove Bridge and ROS Utils
`automatepro-foxglove-bridge.service` starts the Foxglove Bridge used by the [Quick Start Guide](./quick-start-guide.mdx). It runs on the host, loads `/opt/automatepro/.env`, and launches the bridge from `/opt/automatepro/config/ros/foxglove_bridge_launch.xml`.

`automatepro-ros-utils.service` remains available as an optional container for additional ROS 2 visualization utilities. It is installed with the base package but is not enabled by the base package post-install script.

Production systems that do not need remote visualization can disable the bridge service:

```bash
sudo systemctl disable --now automatepro-foxglove-bridge.service
```


# --- din-rail-mount.mdx ---

---
description: DIN rail mounting bracket for the AutomatePro.
---
# DIN Rail Mount

# --- gmsl2-camera.mdx ---

---
description: Sony ISX031 GMSL2 monocular camera for the AutomatePro.
---
# Sony ISX031 GMSL2 Monocular Camera

import gmsl2_cam from './images/camera-ISX031.png'

<div style={{ columnCount: 2, columnGap: '20px' }}>
  <p>
    <strong></strong><br />
    | **Parameter**   | **Value**                                                                 |
    |-----------------|--------------------------------------------------------------------------|
    | Part Number     | AMP-1003-60F (60° FOV)<br />AMP-1003-100F (100° FOV)<br />AMP-1003-118F (118° FOV) |
    | FOV             | 60°/100°/118°                                                            |
    | ISP             | Built-in - with integrated HDR for improved low light performance        |
    | Image Sensor    | SONY 2.95MP ISX031 RGGB                                                  |
    | Power           | Power Over Coax (PoC)                                                    |
    | Output Data     | YUV422@8bit                                                              |
    | Frame Rate      | 1920*1536@30fps                                                          |
    | Connector       | FAKRA Z Coded                                                            |
    | Operating temperature range | -40~+85℃                                                |
    | Ingress Rating  | IP67                                                                     |
    | Dimensions      | 25mm x 25mm x 18.6mm                                                     |


    <strong></strong><br />

    <img src={gmsl2_cam} style={{width: '80%'}}/>

  </p>
</div>
<br />

[Device Datasheet](./datasheets/SG3S-ISX031C-GMSL2F.pdf)


# --- breakout-board.mdx ---

---
icon: images/breakout_board.png
description: 'Breakout Board for Development on the AutomatePro'
---
import AutomatePro_Breakout_Board from './images/automatepro+breakout-board.png'
import Breakout_board from './images/breakout-board.png'

# AutomatePro Dev Breakout Board
The development board is designed to be placed directly on top of the AutomatePro via the 124-pin connector, breaking out the I/O, Power, and Communications into a set of terminal blocks. This setup speeds up development time by eliminating the need to create cable harnesses during the initial development phase.

<img src={Breakout_board} style={{width: '80%'}}/>


<img src={AutomatePro_Breakout_Board} style={{width: '80%'}}/>


# --- rtk-gnss-antenna.mdx ---

---
description: Taoglas RTK GNSS antenna for high-precision positioning.
---
# RTK GNSS Antenna - Taoglas

import RTK_GNSS_Antenna from './images/rtk-gnss-antenna.png'

<div style={{ columnCount: 2, columnGap: '20px' }}>
  <p>
    <strong></strong><br />
    | Parameter            | Value                                                                 |
    |----------------------|-----------------------------------------------------------------------|
    | Part Number          | AMP-1002-3M                                                           |
    | Number of Antennas   | 1x GNSS only                                                          |
    | GNSS Connector       | 1x FAKRA Type C - Signal Blue<br />IP67 Connector                     |
    | Mounting Type        | Screw (through hole panel)                                            |
    | Cable Length         | 3m                                                                    |
    | Ingress Protection   | IP67                                                                  |




    <strong></strong><br />

    <img src={RTK_GNSS_Antenna} style={{width: '55%'}}/>

  </p>
</div>
<br />
[Device Datasheet](https://cdn.taoglas.com/datasheets/MA850.A.LBICG.001.pdf)

# --- 7in1-antenna.mdx ---

---
description: Taoglas 7-in-1 combination antenna covering GNSS, Wi-Fi, and cellular bands.
---
# 7-in-1 Combination Antenna - Taoglas
import combo_antenna from './images/multi-antenna.png'

<div style={{ columnCount: 2, columnGap: '20px' }}>
  <p>
    <strong></strong><br />
    | Parameter            | Value                                                                 |
    |----------------------|-----------------------------------------------------------------------|
    | Part Number          | AMP-1001-3M                                                           |
    | Number of Antennas   | 7                                                                     |
    | 5G Connector         | 4x FAKRA Type D - Claret Violet<br />IP67 Connector                   |
    | GNSS Connector       | 1x FAKRA Type C - Signal Blue<br />IP67 Connector                     |
    | Wi-Fi/BLE Connector  | 2x FAKRA Type I - Beige<br />IP67 Connector                           |
    | Mounting Type        | Screw (through hole panel)                                            |
    | Cable Length         | 3m                                                                    |
    | Ingress Protection   | IP67                                                                  |



    <strong></strong><br />

    <img src={combo_antenna} style={{width: '90%'}}/>

  </p>
</div>
<br />


[Device Datasheet](https://www.taoglas.com/datasheets/MA1559.A.001.pdf)



# --- m12-rj45-cable.mdx ---

---
description: M12 X-coded to RJ45 cable for AutomatePro Ethernet.
---
# M12x to RJ45 Cable

import M12x_Cable from './images/M12X-to-RJ45.jpg'

<div style={{ columnCount: 2, columnGap: '20px' }}>
  <p>
    <strong></strong><br />
    | **Parameter**  | **Value**                |
    |----------------|--------------------------|
    | Part Number    | AMP-1006-2M (Cable Length 2m +-0.15m)           |
    | Connector 1    | M12 X-coded<br />IP67 Connector |
    | Connector 2    | RJ45 |
    | Cable Length   | 2m                       |
    | Cable Type     | Cat5e                    |

    <strong></strong><br />

    <img src={M12x_Cable} style={{width: '100%'}}/>

  </p>
</div>

# --- fakra-z-cables.mdx ---

---
description: FAKRA Type Z cables for connecting GMSL2 cameras.
---
# FAKRA Type Z Camera Cables

import FAKRA_Cable from './images/FAKRA-cable.jpg'

<div style={{ columnCount: 2, columnGap: '20px' }}>
  <p>
    <strong></strong><br />
    | **Parameter** | **Value** |
    |---------------|-----------|
    | Part Number   | AMP-1007-15M (Cable Length 15m +-0.15m)<br />AMP-1007-05M (Cable Length 5m +-0.15m)<br />AMP-1007-02M (Cable Length 2m +-0.15m) |
    | Connector 1   | FAKRA Type Z - Water Blue<br />IP67 Connector |
    | Connector 2   | FAKRA Type Z - Water Blue<br />IP67 Connector |
    | Cable Type    | Dacar302-4 or Dacar302-3 |

    <strong></strong><br />

    <img src={FAKRA_Cable} style={{width: '100%'}}/>

  </p>
</div>








# --- rs485.mdx ---

---
description: Integrated RS485/RS422 serial bus with pinout and termination guidance.
---
# RS485
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import AutomatePro_RS485 from './images/AutomatePro-RS485.png'
import BreakoutBoard_MISC from './images/BreakoutBoard-MISC.png'

## Hardware
One RS485/RS422 bus is integrated into AutomatePro with a max data rate of 500 kbps. A 120 ohm termination resistor should be placed between RS485P and RS485N at both ends of the RS485 network.



### Connector Pinout
<Tabs>
  <TabItem value="Connector" label="Connector" default>

    <img src={AutomatePro_RS485} style={{width: '100%'}}/>

</TabItem>
  <TabItem value="Development Breakout Board" label="Development Breakout Board">

    <img src={BreakoutBoard_MISC} style={{width: '100%'}}/>

  </TabItem>
</Tabs>

### RS485 Specifications
| Parameter       | Value         |
|-----------------|---------------|
| Transceiver     | THVD1400DR    |
| Data Rate       | 500 kbps      |
| RS485 Standard  | TIA/EIA-485A  |
| Duplex          | Half          |

## Software

#### Serial Port
`/dev/ttyTHS1` is the serial port used for RS485 communication. The `RTS` pin controls the direction of the transceiver.
`RTS` is inverted and active low: `RTS` high enables the receiver, and `RTS` low enables the transmitter.

## Example

<Tabs>
  <TabItem value="Python" label="Python" default>
    ```python
      import serial
      import serial.rs485
      import time

      # Configure the serial connections
      ser = serial.rs485.RS485(
          port='/dev/ttyTHS1',
          baudrate=115200,            # Set your baud rate
          parity=serial.PARITY_NONE,
          stopbits=serial.STOPBITS_ONE,
          bytesize=serial.EIGHTBITS,
          timeout=0.1                 # Timeout for read operations
      )

      ser.rs485_mode = serial.rs485.RS485Settings(
        rts_level_for_tx=False,
        rts_level_for_rx=True,
        )
      def send_data(data):
          ser.write(data.encode())
          print("Sent:", data)

      def receive_data():
          incoming_data = ser.read(100)  # Read up to 100 bytes or until timeout
          if incoming_data:
              print("Received:", incoming_data.decode())  # Decode bytes to string

      if __name__ == '__main__':
          try:
              while True:
                  send_data("Hello RS485")
                  time.sleep(1)  # Wait a bit for a response
                  receive_data()
          except Exception as e:
              print("An error occurred:", str(e))
          finally:
              ser.close()  # Always close the serial port
    ```
  </TabItem>

  <TabItem value="C++" label="C++">
  ```cpp
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <fcntl.h>
#include <errno.h>
#include <termios.h>
#include <sys/ioctl.h>
#include <linux/serial.h>

int setup_serial(const char* device, int speed) {
    int fd = open(device, O_RDWR | O_NOCTTY | O_SYNC);
    if (fd < 0) {
        perror("Error opening serial port");
        return -1;
    }

    struct termios tty;
    memset(&tty, 0, sizeof tty);
    if (tcgetattr(fd, &tty) != 0) {
        perror("Error from tcgetattr");
        close(fd);
        return -1;
    }

    cfsetospeed(&tty, speed);
    cfsetispeed(&tty, speed);

    tty.c_cflag |= (CLOCAL | CREAD);    // Ignore modem controls, enable reading
    tty.c_cflag &= ~CSIZE;
    tty.c_cflag |= CS8;                 // 8-bit characters
    tty.c_cflag &= ~PARENB;             // No parity bit
    tty.c_cflag &= ~CSTOPB;             // 1 stop bit
    tty.c_cflag &= ~CRTSCTS;            // No hardware flow control

    tty.c_iflag &= ~(IXON | IXOFF | IXANY); // Turn off s/w flow ctrl
    tty.c_lflag &= ~(ICANON | ECHO | ECHOE | ISIG); // Disable canonical mode, echo, and signal chars
    tty.c_oflag &= ~OPOST; // Prevent special interpretation of output bytes (e.g., newline chars)
    tty.c_cc[VMIN]  = 0;   // Non-blocking read
    tty.c_cc[VTIME] = 20;  // 2 seconds read timeout

    if (tcsetattr(fd, TCSANOW, &tty) != 0) {
        perror("Error from tcsetattr");
        close(fd);
        return -1;
    }

    return fd;
}

void set_rts(int fd, int level) {
    int status;
    ioctl(fd, TIOCMGET, &status);  // Get the current status of the modem lines
    if (level)
        status |= TIOCM_RTS;  // Set RTS high
    else
        status &= ~TIOCM_RTS; // Set RTS low
    ioctl(fd, TIOCMSET, &status);
}

void send_data(int fd, const char* data) {
    set_rts(fd, 0);  // Set RTS low to send
    write(fd, data, strlen(data)); // Send data
    printf("Sent: %s\n", data);
    set_rts(fd, 1);  // Set RTS high after sending
}

void receive_data(int fd) {
    char buf[100];
    int n = read(fd, buf, sizeof(buf)-1);
    if (n > 0) {
        buf[n] = '\0';
        printf("Received: %s\n", buf);
    }
}

int main() {
    int fd = setup_serial("/dev/ttyTHS1", B9600);
    if (fd < 0) {
        exit(EXIT_FAILURE);
    }

    while (1) {
        send_data(fd, "Hello RS485");
        sleep(1); // Wait a bit for a response
        receive_data(fd);
        sleep(1);
    }

    close(fd); // Always close the serial port
    return 0;
}

  ```
  </TabItem>
</Tabs>


## Troubleshooting

- **No data is being sent or received**: Check the wiring and termination resistors. Check the user permissions and serial port parameters such as baudrate, etc.
- **Data is being sent but not received**: Check whether the RTS pin is toggled correctly. RTS should be high to receive data.
- **Data is being received but not sent**: Check whether the RTS pin is toggled correctly. RTS should be low to send data.

If you are using terminal emulators such as `picocom`, toggle the RTS pin manually using the commands offered by the terminal emulator.


# --- eth.mdx ---

---
description: Onboard 1GbE Ethernet over an M12 X-coded connector.
---
# Ethernet

import amp_lan from './images/AutomatePro-LAN.png'
import m12x from './images/M12x.png'

## Hardware
AutomatePro includes a 1GbE Ethernet port onboard, accessible through an M12x connector. Ethernet cables for the M12x are available [here](/automatepro/category/cables--connectors).

### Connector Location
<img src={amp_lan} style={{width: '50%'}}/>

### Ethernet Specifications
| Parameter        | Value          |
|------------------|----------------|
| Supported Speed  | 10/100/1000 Mbps |
| Connector        | M12x - IP67    |
| PoE              | No             |

### M12x Connector Pinout
<div style={{ columnCount: 2, columnGap: '10px' }}>
  <p>
    <strong></strong>
    | Pin Number | LAN Signal |
    |------------|------------|
    | 1          | MDI0+      |
    | 2          | MDI0-      |
    | 3          | MDI1+      |
    | 4          | MDI1-      |
    | 5          | MDI3+      |
    | 6          | MDI3-      |
    | 7          | MDI2-      |
    | 8          | MDI2+      |



    <strong></strong><br />
    <br />
    <br />
    <br />
    <br />
    <br />
    <img src={m12x} style={{width: '40%'}}/>

  </p>
</div>


# --- usb-type-c.mdx ---

---
description: USB Type-C with DisplayPort ALT mode and device reflashing.
---
# USB Type-C

import amp_usb from './images/AutomatePro-USB.png'
import m12x from './images/M12x.png'

## Hardware
AutomatePro features a USB Type-C connector with Alt Mode, enabling it to function as a DisplayPort for connecting external screens.
Mating cables can be [found here](/automatepro/category/cables--connectors).

In addition to connecting to peripheral systems and transferring large amounts of data, the USB-C connector can also be used for reflashing the AutomatePro.

### Connector Location
<img src={amp_usb} style={{width: '70%'}}/>

### USB Specifications
| Parameter        | Value                              |
|------------------|------------------------------------|
| Transceiver      | TUSB546-DCI                        |
| USB Version      | 3.1 Gen 1                          |
| Max data rate    | 5 Gbps                             |
| Alternative Mode | Yes - can be used as a DisplayPort |
| Connector        | M12 USB-C - IP67                   |




# --- can-bus.mdx ---

---
description: Integrated CAN FD bus with connector pinout and termination guidance.
---
# CAN Bus
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import breakoutboard from './images/BreakoutBoard-MISC.png'
import automatepro from './images/AutomatePro-CAN.png'

## Hardware
One CAN FD bus is integrated into AutomatePro with a max data rate of 8 Mbps. A 120 ohm termination resistor should be placed between CAN_H and CAN_L at both ends of the CAN network.

### Connector Pinout
<Tabs>
  <TabItem value="Connector" label="Connector" default>

    <img src={automatepro} style={{width: '100%'}}/>

</TabItem>
  <TabItem value="Development Breakout Board" label="Development Breakout Board">

    <img src={breakoutboard} style={{width: '100%'}}/>

  </TabItem>
</Tabs>


### CAN Specifications

| Parameter                                | Value          |
|------------------------------------------|----------------|
| Transceiver                              | TCAN3413DDFR   |
| CAN FD                                   | Yes            |
| Max data rate                            | 8 Mbps         |
| Integrated 120 ohm termination resistor | No             |
| CAN H and L input protection            | < ±58V         |


## Software
To use the network, configure the interface properties. For instance, to set the network interface with a bus bit rate of 500 kbps, use the following command:
```bash
sudo ip link set can0 up type can bitrate 500000 berr-reporting on
```
Install CAN utilities for testing:
```bash
sudo apt-get install can-utils
```

This command transfers a packet to can0 with CAN ID 123 and data 'abcdabcd':
```bash
cansend can0 123#abcdabcd
```
This command receives packets from any CAN node connected to the bus:
```bash
candump -x any
```
For more information about the use of `cansend`, enter this command:
```bash
cansend --help
```
You can use other tools, such as `cangen`, for different filtering options.

:::tip
C++ and Python examples can be found [here](https://github.com/Lemvos/automatepro_tutorials/tree/master/misc).
:::

## ROS API

The IO controller firmware bridges CAN frames through ROS topics when the firmware-side CAN configuration is enabled.

| Topic | Type | Direction | Description |
|-------|------|-----------|-------------|
| `/io/can/rx` | [`automatepro_interfaces/msg/CAN`](https://github.com/Lemvos/automatepro_interfaces/blob/master/msg/CAN.msg) | Published | CAN frames received by the IO controller. |
| `/io/can/tx` | [`automatepro_interfaces/msg/CAN`](https://github.com/Lemvos/automatepro_interfaces/blob/master/msg/CAN.msg) | Subscribed | CAN frames requested for transmission by the IO controller. |

## Troubleshooting

### Loopback test
You can perform a loopback test to determine whether the controller is working.

Ensure the CAN drivers are loaded. To check whether the drivers are loaded, use the following command:
    ```bash
    lsmod | grep mttcan
    ```
    If the drivers are not loaded, enter this command:
    ```bash
    sudo modprobe mttcan
    ```

To perform a loopback test, run the following commands:
```bash
sudo ip link set can0 type can bitrate 500000 loopback on
sudo ip link set can0 up
candump can0 &
cansend can0 123#abcdabcd
```
If the loopback test is successful, the last command displays this:

```bash
can0 123 [4] AB CD AB CD
can0 123 [4] AB CD AB CD
```
### Other Methods
Additional debugging techniques to consider:

1. If a loopback test shows that the controller is working correctly and you still cannot send or receive packets, try reconnecting the transceiver and confirm that the connections are correct.

2. Check if the `mttcan` driver is loaded.

3. Connect an oscilloscope to analyze the bus signal integrity and verify proper operation.

4. If the device logs a `No buffer space available` message during send, enter this command to use the polling mechanism:
    ```bash
    cangen -L 8 can0 -p 1000
    ```


# --- 5g.mdx ---

---
description: Optional 5G cellular connectivity over FAKRA D antennas.
---
# 5G
## Hardware
AutomatePro has optional 5G connectivity. To use it, connect an external antenna to the highlighted FAKRA D-coded (Claret Violet) connectors shown below.
### Connector Location
![AutomatePro 5G connector location](images/AutomatePro-5G.png)

### 5G Specifications
| Parameter               | Value                                                                                           |
|-------------------------|-------------------------------------------------------------------------------------------------|
| Transceiver             | RM520N-GL                                                                                       |
| Max upload data rates   | 550 Mbps (NSA) or 900 Mbps (SA)                                                                 |
| Max download data rates | 3.4 Gbps (NSA) or 2.4 Gbps (SA)                                                                 |
| Frequency               | sub-6 GHz                                                                                       |
| Protocol                | 5G, LTE, WCDMA                                                                                  |
| Coverage                | Worldwide 5G and LTE-A coverage                                                                 |
| NSA Bands               | n1/n2/n3/n5/n7/n8/n12/n13/n14/n20/n25/n26/n28/n29<br />/n30/n38/n40/n41/n48/n66/n71/n75/n76/n77/n78/n79 |
| SA Bands                | n1/n2/n3/n5/n7/n8/n12/n13/n14/n20/n25/n26/n28/n29<br />/n30/n38/n40/n41/n48/n66/n71/n75/n76/n77/n78/n79 |

:::note
The maximum upload and download rates are based on the manufacturer’s specifications for the 5G device. Actual performance may vary depending on the antenna used and the location of the device.
:::

### External Antenna
The AutomatePro system is compatible with a variety of antennas. More information on compatible antennas can be found [here](/automatepro/category/antennas).

### SIM Card
For devices shipped without a SIM card, you can add one as follows:

#### Installing or Removing the SIM Card

import AMP_SIM_INSTALL from './images/AMP-100-AMP-ASSY-REV-A-SIM.PNG'

<div style={{ display: 'flex', justifyContent: 'center', alignItems: 'center' }}>
  <img src={AMP_SIM_INSTALL} alt="AutomatePro SIM card installation" style={{ width: '100%' }} />
</div>

1. Ensure the device is powered off and disconnect any external connectors. This operation is easiest on a bench with AutomatePro unsecured.
2. Loosen and remove the eight M4x16 socket head cap screws (1).
3. Carefully lift the casing off from the heat sink (2).
4. Using tweezers, carefully install the Nano SIM. To remove it, push the SIM card inward until it ejects.
5. Check the heat sink (4) for sufficient thermal paste.
6. Verify that the O-ring (3) is correctly installed and seated.
7. Reposition the casing onto the heat sink (2) and tighten the eight M4 cap screws in a cross-star pattern. Do not over-torque the screws.




## Configuration

{/* ### eSIM To be added once we have a good solution
*/}

### SIM
:::warning[Important]
Be cautious when using AT commands. Incorrect AT commands can cause the device to malfunction.
For more information, refer to the [manual](./resources/Quectel_RG50xQRM5xxQ_Series_AT_Commands_Manual_V1.1.pdf).
:::

Open a serial terminal using `minicom` or another serial terminal program to access the 5G module AT interface.
```bash
sudo minicom -D /dev/ttyUSB3
```

Run AT commands to check the SIM status:
```bash
AT+CPIN?
```

Output:
```bash
+CPIN: READY

OK
```

If the SIM card is not ready, you will need to enter the PIN code using the following command:
```bash
AT+CPIN=XXXX
```
Replace `XXXX` with the SIM card PIN code.
This command will unlock the SIM card temporarily, and it will be locked again after a reboot.

To unlock the SIM card permanently, use the following command:
```bash
AT+CLCK="SC",0,"XXXX"
```

### Driver
The 5G connection is managed by a systemd service called `automatepro-wwan.service`. The service runs Quectel-CM, is enabled by the `automatepro-installer-wwan` package, and will automatically connect to the network when the device is powered on.
This service can be enabled, disabled, started, stopped, and restarted using the `systemctl` command.

By default, the 5G connection is configured to automatically connect to the network. If you need to manually configure the 5G connection, follow the steps below.

- Start the 5G connection service (temporary start, will not persist after reboot):
```bash
sudo systemctl start automatepro-wwan
```

- Stop the 5G connection service (temporary stop, will restart on reboot):
```bash
sudo systemctl stop automatepro-wwan
```

- Disable automatic connection:

```bash
sudo systemctl disable automatepro-wwan
```

- Enable automatic connection:
```bash
sudo systemctl enable automatepro-wwan
```

## Troubleshooting
If you encounter issues connecting to 5G, try the following steps:

### Verify the driver status
```bash
sudo systemctl status automatepro-wwan
```
If it is not running, start the service using the following command:
```bash
sudo systemctl restart automatepro-wwan
```

### Check the signal strength

Open a serial terminal using `minicom` or another serial terminal program to access the 5G module AT interface.
```bash
sudo minicom -D /dev/ttyUSB3
```
Run AT commands to check the signal strength:
```bash
AT+CSQ
```
Output:
```bash
+CSQ: <RSSI>,<ber>

OK
```
| RSSI | dBm Level | Signal Quality  |
|------|-----------------|--------|
| 0    | -113 dBm or less | ▃ (Poor)|
| 1    | -111 dBm | ▃ ▅ (Fair)|
| 2 - 30 | -109 dBm to -53 dBm | ▃ ▅ ▇ (Good)|
| 31   | -51 dBm or greater | ▃ ▅ ▇ ▉ (Excellent)|
| 99   | Not known or not detectable | Signal Fault: Check Antenna Connection |

If the problem persists, please contact support for further assistance. When contacting support, include the output of the following command:
```bash
sudo journalctl -u automatepro-wwan
```


# --- wifi.mdx ---

---
description: Wi-Fi and Bluetooth over FAKRA I antennas.
---
# Wi-Fi & Bluetooth
import amp_wifi from './images/AutomatePro-WIFI.png'

## Hardware
To use Wi-Fi/Bluetooth, connect two external antennas to the highlighted FAKRA I-coded (Beige) connectors shown below.

### Connector Location
 <img src={amp_wifi} style={{width: '70%'}}/>

### Wi-Fi & Bluetooth Specifications
| Parameter          | Value                                              |
|--------------------|----------------------------------------------------|
| Transceiver        | AW-CB375NF                                         |
| Frequencies        | 2.4 GHz and 5 GHz                                  |
| Max data rate      | 300 Mbps (@ 2.4 GHz) and 867 Mbps (@ 5 GHz)        |
| WLAN Standard      | IEEE 802.11 a/b/g/n/ac MIMO                        |
| Bluetooth Standard | BT 5.0 standard & compatible with Bluetooth 2.1+EDR|


## Configuration

The Wi-Fi connection can be configured using either NMCLI (NetworkManager Command Line Interface) or Remote Desktop.

### Using NMCLI

1. Open a terminal (using SSH or serial) on the AutomatePro device.
2. List the available Wi-Fi networks to ensure the desired network is available:
   ```bash
   sudo nmcli dev wifi list
   ```
3. Connect to your desired Wi-Fi network:
   ```bash
   sudo nmcli dev wifi connect "YOUR_SSID" password "YOUR_PASSWORD"
   ```
4. Verify the connection:
   ```bash
   nmcli dev status
   ```
   Ensure that the Wi-Fi device shows as connected.
   ```bash
   DEVICE         TYPE      STATE         CONNECTION
   wlan0          wifi      connected     `your_wifi_network`
   ```
## Troubleshooting
If you encounter issues connecting to Wi-Fi, try the following steps:

- Ensure that the antennas are securely connected to the FAKRA connectors.
- Verify that the Wi-Fi network credentials (SSID and password) are correct.
- If the issue persists, restart the NetworkManager service and try connecting again.
   ```bash
   sudo systemctl restart NetworkManager
   ```

If the problem persists, please contact support for further assistance. When contacting support, include the output of the following command:
   ```bash
   sudo journalctl -u NetworkManager
   ```




# --- flashing.mdx ---

---
sidebar_position: 3
description: Access the flashing port and reflash the AutomatePro firmware.
---

# Flashing

## Accessing and Reinstalling the Breather Vent

import AMP_Flash_INSTALL from './images/AMP-Flashing.PNG'

<figure style={{ textAlign: 'center' }}>
  <img src={AMP_Flash_INSTALL} alt="AutomatePro Flashing Process" style={{ width: '60%' }} />
  <figcaption>Accessing Flashing Port</figcaption>
</figure>

**1.** To access the flashing port, the breather vent (1) must first be removed. To remove it, turn the breather vent by hand until it is fully detached.
**2.** Once removed, insert the small adapter PCB. Further instructions can be found [here].
**3.** After flashing is complete, ensure there is sufficient grease on the breather vent O-ring.
**4.** Reinstall the breather vent by hand-tightening it.


# --- io-controller.mdx ---

---
sidebar_position: 2
description: IO controller diagnostics — onboard voltage and current monitoring, message fields, and firmware fault codes.
---

# IO Controller
## Hardware
The AutomatePro is equipped with comprehensive onboard current and voltage monitoring systems.
This reduces the need for external sensors and enables effective downstream monitoring,
facilitating early detection of potential issues. The monitored power supply diagram is shown below.

import AutomatePro_Hardware_Diagram_PSU from '../../images/AutomatePro_Hardware_Diagram_PSU.png'

<img src={AutomatePro_Hardware_Diagram_PSU} alt="AutomatePro power supply diagram" style={{width: '100%'}}/>

## ROS API

### Publishers
| Topic | Type | Description |
|-------|------|-------------|
| `/diagnostic/io_controller_hw` | [`automatepro_interfaces`<br/>`/msg/IOControllerDiagnostic`](https://github.com/Lemvos/automatepro_interfaces/blob/master/msg/IOControllerDiagnostic.msg) | Hardware and board diagnostics, including rail voltages and currents, power-good states, digital-drive faults, and board temperature. |
| `/diagnostic/io_controller_fw` | [`automatepro_interfaces`<br/>`/msg/IOFirmwareDiagnostic`](https://github.com/Lemvos/automatepro_interfaces/blob/master/msg/IOFirmwareDiagnostic.msg) | Firmware diagnostics, including controller uptime and fault codes. |

`IOControllerDiagnostic` publishes these fields:

| Field | Type | Description |
|-------|------|-------------|
| `header` | `std_msgs/Header` | Message timestamp and frame metadata. |
| `vbatt_voltage_monitor` | `float32` | Battery voltage in V. |
| `v12_io_smps_current_monitor` | `float32` | 12V IO SMPS current in A. |
| `v12_io_smps_voltage_monitor` | `float32` | 12V IO SMPS voltage in V. |
| `main_sbc_smps_current_monitor` | `float32` | Main SBC SMPS current in A. |
| `main_sbc_smps_voltage_monitor` | `float32` | Main SBC SMPS voltage in V. |
| `v5_io_smps_voltage_monitor` | `float32` | 5V IO SMPS voltage in V. |
| `v5_io_smps_current_monitor` | `float32` | 5V IO SMPS current in A. |
| `amp_total_power_monitor` | `float32` | Total power monitor in W. |
| `v12_io_power_good` | `bool` | 12V IO rail power-good state. |
| `v5_io_power_good` | `bool` | 5V IO rail power-good state. |
| `v3_3_io_power_good` | `bool` | 3.3V IO rail power-good state. |
| `main_sbc_power_good` | `bool` | Main SBC rail power-good state. |
| `v5_sbc_power_good` | `bool` | 5V SBC rail power-good state. |
| `v3_3_sbc_power_good` | `bool` | 3.3V SBC rail power-good state. |
| `v1_8_sbc_power_good` | `bool` | 1.8V SBC rail power-good state. |
| `digital_drive_fault_1` | `bool` | Digital-drive fault 1 state. |
| `digital_drive_fault_2` | `bool` | Digital-drive fault 2 state. |
| `digital_drive_fault_3` | `bool` | Digital-drive fault 3 state. |
| `digital_drive_fault_4` | `bool` | Digital-drive fault 4 state. |
| `board_temp` | `int8` | Board temperature in degrees Celsius. |

`IOFirmwareDiagnostic` publishes these fields:

| Field | Type | Description |
|-------|------|-------------|
| `header` | `std_msgs/Header` | Message timestamp and frame metadata. |
| `uptime` | `uint32` | IO controller firmware uptime in seconds. |
| `fault_code` | `uint8[10]` | Firmware fault-code array. |

## Fault Code Index

| Constant | Value | Description |
|----------|-------|-------------|
| `NO_ERROR` | `0x00` | No fault bits are set. |
| `ERROR_INDEX_CAN` | `0x01` | CAN fault bitfield index; read CAN faults from `fault_code[1]`. |
| `MASK_CAN_RX_BUF_OVERFLOW` | `0x01` | Set in `fault_code[1]` when the CAN RX FIFO reports an overflow or RX overflow condition. |
| `MASK_CAN_TX_BUF_OVERFLOW` | `0x02` | Set in `fault_code[1]` when the CAN TX send buffer overflows. |

No other `fault_code` indexes are defined for AutomatePro firmware diagnostics. Firmware diagnostics are published every second on `/diagnostic/io_controller_fw`; after each publish, the firmware clears all firmware error bits so persistent faults must be reasserted by the failing subsystem.

## Example

This example prints diagnostic information about the IO Controller by subscribing to the `/diagnostic/io_controller_hw` topic.
The ROS package is available [here](https://github.com/Lemvos/automatepro_tutorials).

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

<Tabs>
  <TabItem value="Python" label="Python" default>
    ```python
    import rclpy
    from rclpy.node import Node
    from automatepro_interfaces.msg import IOControllerDiagnostic

    class IOControllerDiagnosticSubscriber(Node):

        def __init__(self):
            super().__init__('io_controller_diagnostic_subscriber')
            self.subscription = self.create_subscription(
                IOControllerDiagnostic,
                '/diagnostic/io_controller_hw',
                self.listener_callback,
                10)

        def listener_callback(self, msg):
            self.get_logger().info(f'Battery Voltage: {msg.vbatt_voltage_monitor} V')
            self.get_logger().info(f'12V IO SMPS Current: {msg.v12_io_smps_current_monitor} A')
            self.get_logger().info(f'12V IO SMPS Voltage: {msg.v12_io_smps_voltage_monitor} V')
            self.get_logger().info(f'Main SBC SMPS Current: {msg.main_sbc_smps_current_monitor} A')
            self.get_logger().info(f'Main SBC SMPS Voltage: {msg.main_sbc_smps_voltage_monitor} V')
            self.get_logger().info(f'5V IO SMPS Voltage: {msg.v5_io_smps_voltage_monitor} V')
            self.get_logger().info(f'5V IO SMPS Current: {msg.v5_io_smps_current_monitor} A')
            self.get_logger().info(f'Total Power: {msg.amp_total_power_monitor} W')

            self.get_logger().info(f'12V IO Power Good: {msg.v12_io_power_good}')
            self.get_logger().info(f'5V IO Power Good: {msg.v5_io_power_good}')
            self.get_logger().info(f'3.3V IO Power Good: {msg.v3_3_io_power_good}')
            self.get_logger().info(f'Main SBC Power Good: {msg.main_sbc_power_good}')
            self.get_logger().info(f'5V SBC Power Good: {msg.v5_sbc_power_good}')
            self.get_logger().info(f'3.3V SBC Power Good: {msg.v3_3_sbc_power_good}')
            self.get_logger().info(f'1.8V SBC Power Good: {msg.v1_8_sbc_power_good}')

            self.get_logger().info(f'Digital Drive Fault 1: {msg.digital_drive_fault_1}')
            self.get_logger().info(f'Digital Drive Fault 2: {msg.digital_drive_fault_2}')
            self.get_logger().info(f'Digital Drive Fault 3: {msg.digital_drive_fault_3}')
            self.get_logger().info(f'Digital Drive Fault 4: {msg.digital_drive_fault_4}')

            self.get_logger().info(f'Board Temperature: {msg.board_temp} °C')

    def main(args=None):
        rclpy.init(args=args)
        node = IOControllerDiagnosticSubscriber()
        rclpy.spin(node)
        node.destroy_node()
        rclpy.shutdown()

    if __name__ == '__main__':
        main()
    ```

    Run the node using the following command:

    ```bash
    ros2 run automatepro_python_tutorials io_controller_diagnostic_node
    ```
    </TabItem>
    <TabItem value="C++" label="C++">
    ```cpp
    #include <rclcpp/rclcpp.hpp>
    #include <automatepro_interfaces/msg/io_controller_diagnostic.hpp>

    class IOControllerDiagnosticSubscriber : public rclcpp::Node
    {
    public:
        IOControllerDiagnosticSubscriber()
            : Node("io_controller_diagnostic_subscriber")
        {
            subscription_ = this->create_subscription<automatepro_interfaces::msg::IOControllerDiagnostic>(
                "/diagnostic/io_controller_hw", 10,
                std::bind(&IOControllerDiagnosticSubscriber::listener_callback, this, std::placeholders::_1));
        }

    private:
        void listener_callback(const automatepro_interfaces::msg::IOControllerDiagnostic::SharedPtr msg) const
        {
            RCLCPP_INFO(this->get_logger(), "Battery Voltage: %.3f V", msg->vbatt_voltage_monitor);
            RCLCPP_INFO(this->get_logger(), "12V IO SMPS Current: %.3f A", msg->v12_io_smps_current_monitor);
            RCLCPP_INFO(this->get_logger(), "12V IO SMPS Voltage: %.3f V", msg->v12_io_smps_voltage_monitor);
            RCLCPP_INFO(this->get_logger(), "Main SBC SMPS Current: %.3f A", msg->main_sbc_smps_current_monitor);
            RCLCPP_INFO(this->get_logger(), "Main SBC SMPS Voltage: %.3f V", msg->main_sbc_smps_voltage_monitor);
            RCLCPP_INFO(this->get_logger(), "5V IO SMPS Voltage: %.3f V", msg->v5_io_smps_voltage_monitor);
            RCLCPP_INFO(this->get_logger(), "5V IO SMPS Current: %.3f A", msg->v5_io_smps_current_monitor);
            RCLCPP_INFO(this->get_logger(), "Total Power: %.3f W", msg->amp_total_power_monitor);

            RCLCPP_INFO(this->get_logger(), "12V IO Power Good: %d", msg->v12_io_power_good);
            RCLCPP_INFO(this->get_logger(), "5V IO Power Good: %d", msg->v5_io_power_good);
            RCLCPP_INFO(this->get_logger(), "3.3V IO Power Good: %d", msg->v3_3_io_power_good);
            RCLCPP_INFO(this->get_logger(), "Main SBC Power Good: %d", msg->main_sbc_power_good);
            RCLCPP_INFO(this->get_logger(), "5V SBC Power Good: %d", msg->v5_sbc_power_good);
            RCLCPP_INFO(this->get_logger(), "3.3V SBC Power Good: %d", msg->v3_3_sbc_power_good);
            RCLCPP_INFO(this->get_logger(), "1.8V SBC Power Good: %d", msg->v1_8_sbc_power_good);

            RCLCPP_INFO(this->get_logger(), "Digital Drive Fault 1: %d", msg->digital_drive_fault_1);
            RCLCPP_INFO(this->get_logger(), "Digital Drive Fault 2: %d", msg->digital_drive_fault_2);
            RCLCPP_INFO(this->get_logger(), "Digital Drive Fault 3: %d", msg->digital_drive_fault_3);
            RCLCPP_INFO(this->get_logger(), "Digital Drive Fault 4: %d", msg->digital_drive_fault_4);

            RCLCPP_INFO(this->get_logger(), "Board Temperature: %.2f °C", msg->board_temp);

        }

        rclcpp::Subscription<automatepro_interfaces::msg::IOControllerDiagnostic>::SharedPtr subscription_;
    };

    int main(int argc, char *argv[])
    {
        rclcpp::init(argc, argv);
        auto node = std::make_shared<IOControllerDiagnosticSubscriber>();
        rclcpp::spin(node);
        rclcpp::shutdown();
        return 0;
    }
    ```
    Run the node using the following command:

    ```bash
    ros2 run automatepro_cpp_tutorials io_controller_diagnostic_node
    ```
    </TabItem>
</Tabs>


# --- estop.mdx ---

---
sidebar_position: 1
description: E-STOP hardware paths and how to verify emergency-stop behavior on the device.
---

# E-STOP
## Hardware

The AutomatePro features an onboard E-STOP input, allowing it to read the state of an external E-STOP.
While the E-STOP state disables the Digital Drives, its primary function is as an input and it has not
been designed to meet any functional safety standards.

The E-STOP input is located on Pin A5 (main connector A) and requires an input voltage ranging from 5-24VDC.
If the E-STOP pin does not need to be sensed, it must be connected to the onboard 12V_IO supply.

## Software Interface

E-STOP is a hardware function: the `E_STOP` signal gates the digital-drive drivers directly.

- **Release E-STOP**:

  ```bash
  gpioset --mode=exit gpiochip0 102=1
  ```

- **Read E-STOP status**:

  ```bash
  gpioget gpiochip1 25
  ```

  Returns `1` when engaged and `0` when released.


# --- digital-out.mdx ---

---
sidebar_position: 3
description: Digital output channels with pinout and specifications.
---

# Digital Out

## Hardware
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import AutomatePro_DOut from './images/AutomatePro_DOut.png'
import BreakoutBoard_DOut from './images/Breakout_Board_Digital_Outs.png'


AutomatePro comes with 12x digital outputs for controlling external systems, from small solenoids to LEDs or small DC motors.
Two different digital outputs are available. One is set up as a high-side output (DO_H_0x), which PWMs the onboard 12V as an output.
The second is a low-side sink (DO_L_0x), which sinks up to 36V. The first four low-side channels are PWM controllable, and the final two are binary on/off switches.

### Connector Pinout

<Tabs>
  <TabItem value="Connector" label="Connector" default>

    <img src={AutomatePro_DOut} style={{width: '100%'}}/>

</TabItem>
  <TabItem value="Development Breakout Board" label="Development Breakout Board">

    <img src={BreakoutBoard_DOut} style={{width: '100%'}}/>

  </TabItem>
</Tabs>

import DOx_Highside from './images/DO_H_x.svg';
import DOx_Lowside from './images/DO_L_x.svg';

### Digital Output High-Side (DO_H_x) Specifications
There are 6x high-side digital outputs, all of which are PWM-controllable switches.
<div style={{ columnCount: 2, columnGap: '20px' }}>
  <p>
    <strong></strong><br />
    | Parameter       | Value         |
    |-----------------|---------------|
    | Output Voltage (On-state)     | +12V (from on-board 12V)  |
    | Drive Current      | 500mA  |
    | PWM Controllable  | All (DO_H_01 - DO_H_06)  |
    | PWM Switching Frequency  | 500Hz  |
    | Number of Outputs        | 6          |

    <strong></strong><br />
  <DOx_Highside style={{width: '400px', height: 'auto'}}/>

  </p>
</div>
<br />

### Digital Output Low-Side (DO_L_x) Specifications
There are 6x low-side digital outputs. The first four are PWM-controllable, while the final two are binary on/off switches.
<div style={{ columnCount: 2, columnGap: '20px' }}>
  <p>
    <strong></strong><br />
    | Parameter       | Value         |
    |-----------------|---------------|
    | On-state     | Sinks up to 36V |
    | Drive Current      |  500mA |
    | PWM Controllable  | DO_L_01 <br/>DO_L_02 <br/>DO_L_03 <br/>DO_L_04 <br/> *DO_L_05 & DO_L_06 are On/Off switches*|
    | PWM Switching Frequency  | 10Hz  |
    | Number of Outputs        | 6          |

    <strong></strong><br />
  <DOx_Lowside style={{width: '400px', height: 'auto'}}/>

  </p>
</div>
<br />

## ROS API

### Subscribers

| Topic | Type | Description |
|-------|------|-------------|
| `/io/digital_out` | [`automatepro_interfaces/msg/DigitalOut`](https://github.com/Lemvos/automatepro_interfaces/blob/master/msg/DigitalOut.msg) | Publish the desired digital output states to this topic. AutomatePro supports 12 digital output pins (6 high-side and 6 low-side). Each output pin is referred to as `DIGITAL_OUT_H_XX` or `DIGITAL_OUT_L_XX`, where `H/L` refers to `High Side/Low Side` and `XX` is the pin number, ranging from `01` to `06`. <br/> <br/> `d_out_pin_id`: <br/> Data type: `uint8` <br/> (Use ENUM constants defined in the message definition. `DigitalOut.DIGITAL_OUT_H_XX`: `01-06` <br/> `DigitalOut.DIGITAL_OUT_L_XX`: `01-06`) <br/> <br/> `duty_cycle_percent`: <br/> Data type: `uint8` <br/> Min: `0` in percentage `%` <br/> Max: `100` in percentage `%`<br/> <br/> Response rate: `200 ms` |


## Example

This example toggles the duty cycle of Digital Out 01 by publishing to the `/io/digital_out` topic.
The ROS package is available [here](https://github.com/Lemvos/automatepro_tutorials).

<Tabs>
  <TabItem value="Python" label="Python" default>
    ```python
    import rclpy
    from rclpy.node import Node
    from automatepro_interfaces.msg import DigitalOut

    class DigitalOutPublisher(Node):

        def __init__(self):
            super().__init__('digital_out_publisher')
            self.publisher_ = self.create_publisher(DigitalOut, '/io/digital_out', 10)
            self.timer = self.create_timer(1.0, self.timer_callback)  # 1s
            self.duty_cycle_sequence = [0, 50, 100, 50]
            self.sequence_index = 0

        def timer_callback(self):
            msg = DigitalOut()
            msg.d_out_pin_id = DigitalOut.DIGITAL_OUT_H_01 # Digital Out High Side Pin 01
            msg.duty_cycle_percent = self.duty_cycle_sequence[self.sequence_index] # Duty Cycle:  0%, 50%, 100%, 50%
                                                                                   # 0% - OFF, 100% - ON
            self.publisher_.publish(msg)
            self.get_logger().info('Publishing: "%s"' % msg)
            self.sequence_index = (self.sequence_index + 1) % len(self.duty_cycle_sequence)

    def main(args=None):
        rclpy.init(args=args)
        node = DigitalOutPublisher()
        rclpy.spin(node)
        node.destroy_node()
        rclpy.shutdown()

    if __name__ == '__main__':
        main()

    ```
    Run the node using the following command:

    ```bash
    ros2 run automatepro_python_tutorials digital_out_node
    ```

  </TabItem>
  <TabItem value="C++" label="C++">
    ```cpp
    #include <vector>
    #include <rclcpp/rclcpp.hpp>
    #include <automatepro_interfaces/msg/digital_out.hpp>

    class DigitalOutPublisher : public rclcpp::Node
    {
    public:
        DigitalOutPublisher()
            : Node("digital_out_publisher"),
              duty_cycle_sequence_{0, 50, 100, 50},
              sequence_index_(0)
        {
            publisher_ = this->create_publisher<automatepro_interfaces::msg::DigitalOut>("/io/digital_out", 10);
            timer_ = this->create_wall_timer(
                std::chrono::seconds(1),
                std::bind(&DigitalOutPublisher::timer_callback, this));
        }

    private:
        void timer_callback()
        {
            auto msg = automatepro_interfaces::msg::DigitalOut();
            msg.d_out_pin_id = automatepro_interfaces::msg::DigitalOut::DIGITAL_OUT_H_01; // Digital Out High Side Pin 01
            msg.duty_cycle_percent = duty_cycle_sequence_[sequence_index_]; // Duty Cycle: 0%, 50%, 100%, 50%
                                                                            // 0% - OFF, 100% - ON
            publisher_->publish(msg);
            RCLCPP_INFO(this->get_logger(), "Publishing: d_out_pin_id=%d, duty_cycle_percent=%d", msg.d_out_pin_id, msg.duty_cycle_percent);

            // Update the sequence index
            sequence_index_ = (sequence_index_ + 1) % duty_cycle_sequence_.size();
        }

        rclcpp::Publisher<automatepro_interfaces::msg::DigitalOut>::SharedPtr publisher_;
        rclcpp::TimerBase::SharedPtr timer_;
        std::vector<int> duty_cycle_sequence_;
        size_t sequence_index_;
    };

    int main(int argc, char *argv[])
    {
        rclcpp::init(argc, argv);
        rclcpp::spin(std::make_shared<DigitalOutPublisher>());
        rclcpp::shutdown();
        return 0;
    }

    ```
    Run the node using the following command:

    ```bash
    ros2 run automatepro_cpp_tutorials digital_out_node
    ```

    :::warning[Important]
    It would be good to have a service that can change pin number and duty cycle instead of changing it from the code.
    :::

  </TabItem>
</Tabs>


# --- analog-in.mdx ---

---
sidebar_position: 1
description: 14 analog inputs for automotive and industrial sensors, with pinout and specifications.
---
# Analog In
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import AutomatePro_AIN from './images/AutomatePro_AnalogINs.png'
import BreakoutBoard_AIN from './images/Breakout_Board_Analog_Ins.png'

## Hardware
AutomatePro comes with 14x analog inputs for measuring a range of different analog sensor types, such as automotive or industrial sensors.
### Connector Pinout

<Tabs>
  <TabItem value="Connector" label="Connector" default>

    <img src={AutomatePro_AIN} style={{width: '100%'}}/>

</TabItem>
  <TabItem value="Development Breakout Board" label="Development Breakout Board">

    <img src={BreakoutBoard_AIN} style={{width: '100%'}}/>

  </TabItem>
</Tabs>

### Analog Input Specifications
| Parameter       | Value         |
|-----------------|---------------|
| Voltage Input Range       | 0-10V (optional 4-20mA input or integrated automotive 5V pullup on request)      |
| Sample Rate  | 30 Hz  |
| Measurement Resolution | 2.4414 mV/increment (12-bit)  |
| Number of Inputs        | 14          |


## ROS API

### Publishers

| Topic | Type | Description |
|-------|------|-------------|
|`/io/ain` | [`automatepro_interfaces/msg/AnalogIn`](https://github.com/Lemvos/automatepro_interfaces/blob/master/msg/AnalogIn.msg) | Contains the states of the analog inputs. AutomatePro supports 14 analog input pins. Each input pin is referred to as `ain_XX`, where `XX` is the pin number, ranging from `01` to `14`. <br/> <br/> Input Pins: `ain_01 - ain_14` <br/> DataType: `int16` <br/> Unit: `user-defined` (default: `mV`) <br/> Min: `-32768` <br/> Max: `32768` <br/> Sample Rate: `30Hz` |

:::tip
Each message includes a timestamp in the header, indicating when the IO-Controller collected the data.
It is recommended to use this timestamp for any time-sensitive data processing, as it ensures that transport
delays are accounted for, providing accurate timing for downstream operations.
:::

## Configuration
Analog input channels can be configured with gain, offset, and filter parameters via the IO Config service for precise scaling, calibration, and signal smoothing.
:::info
**Default Configuration:**
Unless reconfigured, all analog inputs use the following defaults:
- **Gain:** 1.0
- **Offset:** 0.0
- **Filter Coefficient:** 1.0 (no filtering)

Raw millivolt (mV) values are published without any signal processing applied.
:::

### Services

|Service | Type | Description  |
|---------|---------|---------|
|`/io/config` | [`automatepro_interfaces/srv/IOConfig`](https://github.com/Lemvos/automatepro_interfaces/blob/master/srv/IOConfig.srv) | Configure analog input parameters including gain, offset, and exponential moving average filter. <br/> <br/> **Request Parameters:** <br/> `flag`: <br/> DataType: `uint8` <br/> `0` for read, `1` for write <br/> `key`: <br/> DataType: `uint16` <br/> Key associated with the value to be changed (0-65535) <br/> `val`: <br/> DataType: `float32` <br/> Corresponding value for the key <br/> <br/> **Response Parameters:** <br/> `ret_code`: <br/> DataType: `uint16` <br/> Return code from MCU (0-65535) <br/> `ret_val`: <br/> DataType: `float32` <br/> Returns the set value if successful |

### Configuration Keys

The analog input configuration uses keys in the format `1XXY` where:
- `XX` is the analog input index (01-14)
- `Y` is the parameter type:
  - `0` for gain (m)
  - `1` for offset (c)
  - `2` for exponential moving average filter

#### Configuration Parameters

**Set Gain (m):**
- Key: `1XX0` where XX is the AIN index (01-14)
- Value: Desired gain value as float
- Example: Key `1010` sets gain for AIN_01

**Set Offset (c):**
- Key: `1XX1` where XX is the AIN index (01-14)
- Value: Desired offset value as float
- Example: Key `1011` sets offset for AIN_01

**Set Exponential Moving Average Filter:**
- Key: `1XX2` where XX is the AIN index (01-14)
- Value: Filter coefficient (0.0 - 1.0)
  - `1.0` = No filtering
  - Lower values = More filtering (slower response)
- Example: Key `1012` sets filter for AIN_01

:::tip[**Signal Processing Formula**]

1. **Scaling and Calibration:**
   ```
   Scaled_Value = (Raw_Value × Gain) + Offset
   ```

2. **Exponential Moving Average Filter:**
   ```
   Final_Value = Previous_Value × (1 - α) + Scaled_Value × α
   ```
   Where `α` (alpha) is the filter coefficient (0.0 - 1.0)
:::

:::warning
The final calculated value `(Raw_Value * Gain) + Offset` must not exceed the `int16` range (-32768 to 32768).
Results outside this range will be clamped.
:::
### Configuration Example

<Tabs>
  <TabItem value="CLI" label="CLI" default>
    ```bash
    # Set gain for AIN_01 to 2.0
    ros2 service call /io/config automatepro_interfaces/srv/IOConfig "{flag: 1, key: 1010, val: 2.0}"

    # Set offset for AIN_01 to 100.0
    ros2 service call /io/config automatepro_interfaces/srv/IOConfig "{flag: 1, key: 1011, val: 100.0}"

    # Set filter coefficient for AIN_01 to 0.9
    ros2 service call /io/config automatepro_interfaces/srv/IOConfig "{flag: 1, key: 1012, val: 0.9}"
    ```
  </TabItem>
</Tabs>

## Example

This example prints the states of the analog inputs by subscribing to the `/io/ain` topic.
The ROS package is available [here](https://github.com/Lemvos/automatepro_tutorials).

<Tabs>
  <TabItem value="Python" label="Python" default>
    ```python
    import rclpy
    from rclpy.node import Node
    from automatepro_interfaces.msg import AnalogIn

    class AnalogInSubscriber(Node):

        def __init__(self):
            super().__init__('analog_in_subscriber')
            self.subscription = self.create_subscription(
                AnalogIn,
                '/io/ain',
                self.subscriber_callback,
                10)

        def subscriber_callback(self, msg):
            self.get_logger().info(
                f'Received AnalogIn message:\n'
                f'AIN_01: {msg.ain_01}\nAIN_02: {msg.ain_02}\nAIN_03: {msg.ain_03}\n'
                f'AIN_04: {msg.ain_04}\nAIN_05: {msg.ain_05}\nAIN_06: {msg.ain_06}\n'
                f'AIN_07: {msg.ain_07}\nAIN_08: {msg.ain_08}\nAIN_09: {msg.ain_09}\n'
                f'AIN_10: {msg.ain_10}\nAIN_11: {msg.ain_11}\nAIN_12: {msg.ain_12}\n'
                f'AIN_13: {msg.ain_13}\nAIN_14: {msg.ain_14}'
            )

    def main(args=None):
        rclpy.init(args=args)
        node = AnalogInSubscriber()
        rclpy.spin(node)
        node.destroy_node()
        rclpy.shutdown()

    if __name__ == '__main__':
        main()
    ```
    Run the node using the following command:

    ```bash
    ros2 run automatepro_python_tutorials analog_in_node
    ```

    </TabItem>
      <TabItem value="C++" label="C++">
    ```cpp
    #include <rclcpp/rclcpp.hpp>
    #include <automatepro_interfaces/msg/analog_in.hpp>

    class AnalogInSubscriber : public rclcpp::Node
    {
    public:
        AnalogInSubscriber()
            : Node("analog_in_subscriber")
        {
            subscription_ = this->create_subscription<automatepro_interfaces::msg::AnalogIn>(
                "/io/ain", 10,
                std::bind(&AnalogInSubscriber::listener_callback, this, std::placeholders::_1));
        }

    private:
        void listener_callback(const automatepro_interfaces::msg::AnalogIn::SharedPtr msg) const
        {
            RCLCPP_INFO(this->get_logger(),
                "Received AnalogIn message:\n"
                "AIN_01: %u\nAIN_02: %u\nAIN_03: %u\n"
                "AIN_04: %u\nAIN_05: %u\nAIN_06: %u\n"
                "AIN_07: %u\nAIN_08: %u\nAIN_09: %u\n"
                "AIN_10: %u\nAIN_11: %u\nAIN_12: %u\n"
                "AIN_13: %u\nAIN_14: %u",
                msg->ain_01, msg->ain_02, msg->ain_03,
                msg->ain_04, msg->ain_05, msg->ain_06,
                msg->ain_07, msg->ain_08, msg->ain_09,
                msg->ain_10, msg->ain_11, msg->ain_12,
                msg->ain_13, msg->ain_14);
        }

        rclcpp::Subscription<automatepro_interfaces::msg::AnalogIn>::SharedPtr subscription_;
    };

    int main(int argc, char *argv[])
    {
        rclcpp::init(argc, argv);
        rclcpp::spin(std::make_shared<AnalogInSubscriber>());
        rclcpp::shutdown();
        return 0;
    }
    ```
    Run the node using the following command:

    ```bash
    ros2 run automatepro_cpp_tutorials analog_in_node
    ```
  </TabItem>
</Tabs>


# --- digital-drive-out.mdx ---

---
sidebar_position: 4
description: 24 solid-state switches configurable as PWM full-bridge or half-bridge drive outputs.
---

# Digital Drive Out

## Hardware
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import AutomatePro_DDOut from './images/AutomatePro_DDOut.png'
import BreakoutBoard_DDOut from './images/Breakout_Board_Digital_Drive.png'

AutomatePro features 24 controllable solid-state switches, configurable via software as
either 6x PWM-controlled full-bridge drives or 12x half-bridges, or any combination thereof
(e.g., 2x half-bridges and 5x full-bridges, or 10x half-bridges and 1x full-bridge). The drive voltage is
configurable by setting the two ACTU_PWR pins to the desired voltage source, allowing the user to select
the voltage for the downstream actuators.
Digital drives are disabled when the E-STOP input is low. This means that when the E-STOP is activated,
all digital drive outputs will be disabled until the E-STOP signal is restored.






### Connector Pinout

<Tabs>
  <TabItem value="Connector" label="Connector" default>

    <img src={AutomatePro_DDOut} style={{width: '100%'}}/>

</TabItem>
  <TabItem value="Development Breakout Board" label="Development Breakout Board">

    <img src={BreakoutBoard_DDOut} style={{width: '100%'}}/>

  </TabItem>
</Tabs>

import Fullbridge from './images/Fullbridge.svg';
import Halfbridge from './images/Halfbridge.svg';
import FullDriveSystem from './images/Full_Drive_System.svg';


### Digital Drive Power
All digital drive outputs are powered via the ACTU_PWR pins (A57 and A60) on Connector A.
These pins must be connected to a power supply capable of sourcing the appropriate current
for the application, with a voltage range of 6-42 VDC. The voltage bus is distributed across all
digital drive outputs, requiring uniform voltage tolerance for all downstream actuators.
Each ACTU_PWR pin is rated at 40A, allowing single-pin use if the current remains below 40A.
The GND power connections on pins B59 and B62 should match the current rating of the ACTU_PWR pins.
The onboard fuses are non-resettable and limited to a maximum of 60A.

The diagram below shows the internal configuration of the Digital Drives.

<FullDriveSystem style={{width: '700px', height: 'auto'}}/>


:::warning[Important]
The breakout board is limited to 40A on ACTU_PWR and should not exceed this.
:::
### Digital Drive Full-Bridge Specifications
The digital drives full-bridge can be configured as 6x H-bridges, enabling bidirectional control of up to 6 DC motors. These are PWM-controllable with a 0-100% duty cycle range.
<div style={{ columnCount: 2, columnGap: '20px' }}>
  <p>
    <strong></strong>
    | Parameter       | Value         |
    |-----------------|---------------|
    | Voltage Range (Set by ACTU_PWR)     | 6-42V |
    | Drive Current Max      | 5A |
    | PWM controllable      | All Outputs  <br/>FULL_BRIDGE_DRIVE_01 <br/> FULL_BRIDGE_DRIVE_02 <br/> FULL_BRIDGE_DRIVE_03 <br/> FULL_BRIDGE_DRIVE_04 <br/> FULL_BRIDGE_DRIVE_05 <br/> FULL_BRIDGE_DRIVE_06  |
    | PWM Switching Frequency  | 15 kHz  |
    | Number of Full-bridges        | 6          |

    <strong></strong><br />


  <Fullbridge style={{width: '400px', height: 'auto'}}/>

  </p>
</div>
<br />

:::warning[Important]
1. Ensure ACTU_PWR is connected to an external power supply, or wire +12V_IO_OUT (pin A61) to the ACTU_PWR pin to use the onboard 12V supply.

2. Ensure that E-STOP input is high to enable digital drives. You can check this via the AutomatePro LEDs.
:::

### Digital Drive Half-Bridge Specifications
The half-bridge allows PWM controllability on all low-side drives. The first 6 high-side switches also offer PWM control,
while the remaining 6-12 function as on/off switches only.
<div style={{ columnCount: 2, columnGap: '20px' }}>
  <p>
    <strong></strong>
    | Parameter       | Value         |
    |-----------------|---------------|
    | Voltage Range (Set by ACTU_PWR)     | 6-42V |
    | Drive Current      | 5A  |
    | PWM controllable      | HALF_BRIDGE_DRIVE_01<br /> HALF_BRIDGE_DRIVE_02<br /> HALF_BRIDGE_DRIVE_03<br />HALF_BRIDGE_DRIVE_04<br />HALF_BRIDGE_DRIVE_05<br />HALF_BRIDGE_DRIVE_06 <br />HALF_BRIDGE_DRIVE_07 (only low-side switch)<br />HALF_BRIDGE_DRIVE_08 (only low-side switch)<br />HALF_BRIDGE_DRIVE_09 (only low-side switch)<br />HALF_BRIDGE_DRIVE_10 (only low-side switch)<br />HALF_BRIDGE_DRIVE_11 (only low-side switch)<br />HALF_BRIDGE_DRIVE_12 (only low-side switch)  |
    | PWM Switching Frequency  | 15 kHz  |
    | Number of Half-bridges        | 12         |

    <strong></strong><br />
  <Halfbridge style={{width: '300px', height: 'auto'}}/>

  </p>
</div>
<br />
:::warning[Important]
1. Ensure ACTU_PWR is connected to an external power supply, or wire +12V_IO_OUT (pin A61) to the ACTU_PWR pin to use the onboard 12V supply.

2. Ensure that E-STOP input is high to enable digital drives. You can check this via the AutomatePro LEDs.

:::




## ROS API

### Subscribers

| Topic | Type | Description |
|-------|------|-------------|
| `/io/digital_drive_out` | [`automatepro_interfaces`<br/>`/msg/DigitalDriveOut`](https://github.com/Lemvos/automatepro_interfaces/blob/master/msg/DigitalDriveOut.msg) | Publish the desired digital drive states to this topic. Digital drives can be configured in various ways as described in the [Hardware](#hardware) section. <br/> Each digital drive output is referred to as `FUNCTION_XX`, where `FUNCTION` is the configured mode. Options are `FULL_BRIDGE_DRIVE`, `HALF_BRIDGE_DRIVE`, `LOW_SIDE_DRIVE`, and `HIGH_SIDE_DRIVE`. `XX` is the output number; ranges for each mode are listed below. <br/> <br/> `LOW_SIDE_DRIVE` and `HIGH_SIDE_DRIVE` require hardware changes. Please contact support if you want to use them. <br/> <br/> Inputs:<br/> `d_drive_pin_id`: <br/> Data type: `uint8` <br/> _(Use ENUM constants defined in the message definition. <br/> `DigitalDriveOut.FULL_BRIDGE_DRIVE_XX`: `01-06` <br/> `DigitalDriveOut.HALF_BRIDGE_DRIVE_XX`: `01-12` <br/> `DigitalDriveOut.LOW_SIDE_DRIVE_XX`: `01-12` <br/> `DigitalDriveOut.HIGH_SIDE_DRIVE_XX`: `01-12`)_  <br/> <br/> `direction`:<br/>Data type: `bool` <br/> _(Use ENUM constants defined in the message definition. <br/> `DigitalDriveOut.FORWARD` for forward <br/> `DigitalDriveOut.REVERSE` for reverse)_ <br/> <br/> `duty_cycle_percent`: <br/> Data type: `uint8` <br/> Min: `0` in percentage `%` <br/> Max: `100` in percentage `%`<br/> <br/> Response rate: `200 ms` |



## Example

Example code snippets toggle the duty cycle of Digital Drive Out 01 by publishing to the `/io/digital_drive_out` topic.
The ROS package is available [here](https://github.com/Lemvos/automatepro_tutorials).

<Tabs>
  <TabItem value="Python" label="Python" default>
    ```python
    import rclpy
    from rclpy.node import Node
    from automatepro_interfaces.msg import DigitalDriveOut

    class DigitalDriveOutPublisher(Node):

        def __init__(self):
            super().__init__('digital_drive_out_publisher')
            self.publisher_ = self.create_publisher(DigitalDriveOut, '/io/digital_drive_out', 10)
            self.timer = self.create_timer(1.0, self.timer_callback)  # 1s
            self.duty_cycle = 0

        def timer_callback(self):
            msg = DigitalDriveOut()
            msg.d_drive_pin_id = DigitalDriveOut.HALF_BRIDGE_DRIVE_01
            msg.direction = DigitalDriveOut.FORWARD
            msg.duty_cycle_percent = self.duty_cycle
            self.publisher_.publish(msg)
            self.get_logger().info(
                'Publishing DigitalDriveOut: d_drive_pin_id=%d, direction=%d, duty_cycle_percent=%d' %
                (msg.d_drive_pin_id, msg.direction, msg.duty_cycle_percent))
            self.duty_cycle = 100 if self.duty_cycle == 0 else 0 # Toggle duty cycle between 0 (ON) and 100 (OFF)

    def main(args=None):
        rclpy.init(args=args)
        node = DigitalDriveOutPublisher()
        rclpy.spin(node)
        node.destroy_node()
        rclpy.shutdown()

    if __name__ == '__main__':
        main()
    ```
    Run the node using the following command:

    ```bash
    ros2 run automatepro_python_tutorials digital_drive_out_node
    ```
</TabItem>
    <TabItem value="C++" label="C++">
    ```cpp
    #include <rclcpp/rclcpp.hpp>
    #include <automatepro_interfaces/msg/digital_drive_out.hpp>

    class DigitalDriveOutPublisher : public rclcpp::Node
    {
    public:
        DigitalDriveOutPublisher()
            : Node("digital_drive_out_publisher"), duty_cycle_(0)
        {
            publisher_ = this->create_publisher<automatepro_interfaces::msg::DigitalDriveOut>("/io/digital_drive_out", 10);
            timer_ = this->create_wall_timer(
                std::chrono::seconds(1),
                std::bind(&DigitalDriveOutPublisher::timer_callback, this));
        }

    private:
        void timer_callback()
        {
            auto msg = automatepro_interfaces::msg::DigitalDriveOut();
            msg.d_drive_pin_id = automatepro_interfaces::msg::DigitalDriveOut::HALF_BRIDGE_DRIVE_01;
            msg.direction = automatepro_interfaces::msg::DigitalDriveOut::FORWARD;
            msg.duty_cycle_percent = duty_cycle_;
            publisher_->publish(msg);
            RCLCPP_INFO(this->get_logger(), "Publishing DigitalDriveOut: d_drive_pin_id=%d, direction=%d, duty_cycle_percent=%d",
                        msg.d_drive_pin_id, msg.direction, msg.duty_cycle_percent);
            duty_cycle_ = (duty_cycle_ == 0) ? 100 : 0; // Toggle duty cycle between 0 (ON) and 100 (OFF)
        }

        rclcpp::Publisher<automatepro_interfaces::msg::DigitalDriveOut>::SharedPtr publisher_;
        rclcpp::TimerBase::SharedPtr timer_;
        int duty_cycle_;
    };

    int main(int argc, char *argv[])
    {
        rclcpp::init(argc, argv);
        rclcpp::spin(std::make_shared<DigitalDriveOutPublisher>());
        rclcpp::shutdown();
        return 0;
    }
    ```

    Run the node using the following command:

    ```bash
    ros2 run automatepro_cpp_tutorials digital_drive_out_node
    ```

    :::warning[Important]
    A service for changing the pin number and duty cycle would avoid editing the example code.
    :::

    </TabItem>
</Tabs>


# --- warning-systems.mdx ---

---
sidebar_position: 5
description: Drive outputs for warning lights and a buzzer/horn, with pinout and control.
---

# Warning Systems

## Hardware
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import AutomatePro_DOut from './images/AutomatePro_DOut.png'
import BreakoutBoard_DOut from '../connectivity/images/BreakoutBoard-MISC.png'

Warning system outputs are configured to control standard components such as warning lights and buzzers.
Two drivers are allocated for the lighting system, and one driver is designated for a warning buzzer/horn.

### Connector Pinout

<Tabs>
  <TabItem value="Connector" label="Connector" default>

    <img src={AutomatePro_DOut} style={{width: '100%'}}/>

</TabItem>
  <TabItem value="Development Breakout Board" label="Development Breakout Board">

    <img src={BreakoutBoard_DOut} style={{width: '100%'}}/>

  </TabItem>
</Tabs>

import WarningLight from './images/Warning_System.svg';
import WarningBuzzer from './images/Warning_Buzzer.svg';

### Warning Systems Specifications
<div style={{ columnCount:3, columnGap: '20px' }}>
  <p>
    <strong></strong><br />
    | Parameter       | Value         |
    |-----------------|---------------|
    | On-state     | Sinks up to 24V |
    | Drive Current      | 2A  |
    | Controllability  | ON/OFF  |
    | Number of Sinks | 3          |

    <strong></strong><br />
<WarningBuzzer style={{width: 'auto', height: '270px', paddingTop: '20px'}} />
  <WarningLight style={{width: 'auto', height: '340px',paddingBottom: '10px'}}/>

  </p>
</div>
<br />


## ROS API

### Subscribers

| Topic | Type | Description |
|-------|------|-------------|
| `/io/warning_system_out` | [`automatepro_interfaces/msg/WarningSystems`](https://github.com/Lemvos/automatepro_interfaces/blob/master/msg/WarningSystems.msg) | Publish the desired states for the warning system to this topic. <br/> <br/> Inputs:<br/> `warning_system_id`: <br/> Data type: `uint8` <br/> _(Use ENUM constants defined in the message definition. Options: `WarningSystems.WARNING_BUZZER`, `WarningSystems.WARNING_LIGHT_01`, `WarningSystems.WARNING_LIGHT_02`)_ <br/> <br/> `state`: <br/> Data type: `bool` <br/> _(Use ENUM constants defined in the message definition. Options: `WarningSystems.ON`, `WarningSystems.OFF`)_ <br/> <br/> Response rate: `200 ms` |


## Example

This example toggles the Warning Buzzer ON/OFF by publishing to the `/io/warning_system_out` topic.
The ROS package is available [here](https://github.com/Lemvos/automatepro_tutorials).

<Tabs>
  <TabItem value="Python" label="Python" default>
    ```python
    import rclpy
    from rclpy.node import Node
    from automatepro_interfaces.msg import WarningSystems

    class WarningSystemsPublisher(Node):

        def __init__(self):
            super().__init__('warning_systems_publisher')
            self.publisher_ = self.create_publisher(WarningSystems, '/io/warning_system_out', 10)
            self.timer = self.create_timer(1.0, self.timer_callback)  # 1s
            self.state = False

        def timer_callback(self):
            msg = WarningSystems()
            msg.warning_system_id = WarningSystems.WARNING_BUZZER
            msg.state = self.state
            self.publisher_.publish(msg)
            self.get_logger().info(
                'Publishing WarningSystems: warning_system_id=%d, state=%d' %
                (msg.warning_system_id, msg.state))
            self.state = not self.state # Toggle state

    def main(args=None):
        rclpy.init(args=args)
        node = WarningSystemsPublisher()
        rclpy.spin(node)
        node.destroy_node()
        rclpy.shutdown()

    if __name__ == '__main__':
        main()
    ```
    Run the node using the following command:

    ```bash
    ros2 run automatepro_python_tutorials warning_system_out_node
    ```
  </TabItem>
  <TabItem value="C++" label="C++">
    ```cpp
    #include <rclcpp/rclcpp.hpp>
    #include <automatepro_interfaces/msg/warning_systems.hpp>

    class WarningSystemsPublisher : public rclcpp::Node
    {
    public:
        WarningSystemsPublisher()
            : Node("warning_systems_publisher"), state_(false)
        {
            publisher_ = this->create_publisher<automatepro_interfaces::msg::WarningSystems>("/io/warning_system_out", 10);
            timer_ = this->create_wall_timer(
                std::chrono::seconds(1),
                std::bind(&WarningSystemsPublisher::timer_callback, this));
        }

    private:
        void timer_callback()
        {
            auto msg = automatepro_interfaces::msg::WarningSystems();
            msg.warning_system_id = automatepro_interfaces::msg::WarningSystems::WARNING_BUZZER;  // Change to WARNING_LIGHT_01 or WARNING_LIGHT_02 as needed
            msg.state = state_;
            publisher_->publish(msg);
            RCLCPP_INFO(this->get_logger(), "Publishing WarningSystems: warning_system_id=%d, state=%d",
                        msg.warning_system_id, msg.state);
            state_ = !state_;  // Toggle state
        }

        rclcpp::Publisher<automatepro_interfaces::msg::WarningSystems>::SharedPtr publisher_;
        rclcpp::TimerBase::SharedPtr timer_;
        bool state_;
    };

    int main(int argc, char *argv[])
    {
        rclcpp::init(argc, argv);
        rclcpp::spin(std::make_shared<WarningSystemsPublisher>());
        rclcpp::shutdown();
        return 0;
    }

    ```
    Run the node using the following command:

    ```bash
    ros2 run automatepro_cpp_tutorials warning_system_out_node
    ```
  </TabItem>
</Tabs>


# --- digital-in.mdx ---

---
sidebar_position: 2
description: Digital input channels with pinout and specifications.
---

# Digital In

## Hardware
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import AutomatePro_DIN from './images/AutomatePro_DigitalIn.png'
import BreakoutBoard_DIN from './images/BreakoutBoard_DI.png'


The AutomatePro comes with 10x digital inputs for checking the binary state of external systems. It is designed to work with a wide range of different sensors and switches.
### Connector Pinout

<Tabs>
  <TabItem value="Connector" label="Connector" default>

    <img src={AutomatePro_DIN} style={{width: '100%'}}/>

</TabItem>
  <TabItem value="Development Breakout Board" label="Development Breakout Board">

    <img src={BreakoutBoard_DIN} style={{width: '100%'}}/>

  </TabItem>
</Tabs>

import Digital_Input from './images/DIN_x.svg';

### Digital Input Specifications
<div style={{ columnCount: 2, columnGap: '0px' }}>
  <p>
    <strong></strong><br />
        | Parameter       | Value         |
        |-----------------|---------------|
        | High Voltage State      | 5-24V  |
        | Low Voltage State      | &lt;2V  |
        | Input Impedance      | 2kΩ  |
        | Sample Rate  | 10Hz  |
        | Number of Inputs        | 10          |

    <strong></strong><br />
  <Digital_Input style={{width: '400px', height: 'auto'}}/>

  </p>
</div>
<br />




## ROS API

### Publishers

| Topic | Type | Description |
|-------|------|-------------|
|`/io/din` | [`automatepro_interfaces/msg/DigitalIn`](https://github.com/Lemvos/automatepro_interfaces/blob/master/msg/DigitalIn.msg) | Contains the states of the digital inputs. <br/> Only published on state change or when requested. <br/> AutomatePro supports 10 digital input pins. Each input pin is referred to as `din_XX`, where `XX` is the pin number, ranging from `01` to `10`. <br/> <br/> Input Pins: `din_01`-`din_10` <br/> DataType: `bool` <br/> Inputs are checked approximately every 30 ms. |

### Services

|Service | Type | Description  |
|---------|---------|---------|
|`/io/din/request` |  [`automatepro_interfaces/srv/ReqDigitalIn`](https://github.com/Lemvos/automatepro_interfaces/blob/master/srv/ReqDigitalIn.srv) | Request to send the current state of the digital inputs. When requested, current states will be published to `/io/din`. |
|`/io/config` | [`automatepro_interfaces/srv/IOConfig`](https://github.com/Lemvos/automatepro_interfaces/blob/master/srv/IOConfig.srv) | Configure digital input parameters such as state inversion. <br/> <br/> **Request Parameters:** <br/> `flag`: <br/> DataType: `uint8` <br/> `0` for read, `1` for write <br/> `key`: <br/> DataType: `uint16` <br/> Key associated with the value to be changed (0-65535) <br/> `val`: <br/> DataType: `float32` <br/> Corresponding value for the key <br/> <br/> **Response Parameters:** <br/> `ret_code`: <br/> DataType: `uint16` <br/> Return code from MCU (0-65535) <br/> `ret_val`: <br/> DataType: `float32` <br/> Returns the set value if successful |

### Configuration Keys

The digital input configuration uses keys in the format `3XXY` where:
- `XX` is the digital input index (01-10)
- `Y` is the parameter type:
    - `0`: State Inversion

#### Configuration Parameters

**Set State Inversion:**
- Key: `3XX0` where XX is the DIN index (01-10)
- Value: 0 for non-inverted, 1 for inverted
- Example: Key `3010` sets inversion for DIN_01

:::tip
It is recommended to request the states of the digital inputs by calling the service,
immediately after subscribing to `/io/din` to ensure you have the most up-to-date information.
:::


## Example

This example prints the states of the digital inputs by subscribing to the `/io/din` topic.
The ROS package is available [here](https://github.com/Lemvos/automatepro_tutorials).


<Tabs>
  <TabItem value="Python" label="Python" default>
    ```python
    import rclpy
    from rclpy.node import Node
    from automatepro_interfaces.msg import DigitalIn
    from automatepro_interfaces.srv import ReqDigitalIn


    class DigitalInSubscriber(Node):

        def __init__(self):
            super().__init__('digital_in_subscriber')
            self.subscription = self.create_subscription(
                DigitalIn,
                '/io/din',
                self.listener_callback,
                10)
            self.client = self.create_client(ReqDigitalIn, '/io/din/request')
            self.request_state() # Request the current state of the digital inputs

        def listener_callback(self, msg):
            self.get_logger().info('Received DigitalIn message: %s' % [
                msg.din_01, msg.din_02, msg.din_03, msg.din_04, msg.din_05,
                msg.din_06, msg.din_07, msg.din_08, msg.din_09, msg.din_10])

        def request_state(self):
            while not self.client.wait_for_service(timeout_sec=1.0):
                self.get_logger().info('service not available, waiting again...')
            request = ReqDigitalIn.Request()
            self.future = self.client.call_async(request)
            self.future.add_done_callback(self.service_callback)

        def service_callback(self, future):
            try:
                response = future.result()
                self.get_logger().info('Service response: %s' %
                                        ('Success' if response.success else 'Failure'))
            except Exception as e:
                self.get_logger().info('Service call failed %r' % (e,))

    def main(args=None):
        rclpy.init(args=args)
        node = DigitalInSubscriber()
        rclpy.spin(node)
        node.destroy_node()
        rclpy.shutdown()

    if __name__ == '__main__':
        main()

    ```
    Run the node using the following command:

    ```bash
    ros2 run automatepro_python_tutorials digital_in_node
    ```
</TabItem>
    <TabItem value="C++" label="C++">
    ```cpp
    #include <rclcpp/rclcpp.hpp>
    #include <automatepro_interfaces/msg/digital_in.hpp>
    #include <automatepro_interfaces/srv/req_digital_in.hpp>

    class DigitalInSubscriber : public rclcpp::Node
    {
    public:
        DigitalInSubscriber()
            : Node("digital_in_subscriber")
        {
            subscription_ = this->create_subscription<automatepro_interfaces::msg::DigitalIn>(
                "/io/din", 10,
                std::bind(&DigitalInSubscriber::listener_callback, this, std::placeholders::_1));

            client_ = this->create_client<automatepro_interfaces::srv::ReqDigitalIn>("/io/din/request");
            request_state(); // Request the current state of the digital inputs
        }

    private:
        /*
        * Callback function for the subscription
        */
        void listener_callback(const automatepro_interfaces::msg::DigitalIn::SharedPtr msg) const
        {
            RCLCPP_INFO(this->get_logger(), "Received DigitalIn message: %d %d %d %d %d %d %d %d %d %d",
                        msg->din_01, msg->din_02, msg->din_03, msg->din_04, msg->din_05,
                        msg->din_06, msg->din_07, msg->din_08, msg->din_09, msg->din_10);
        }

        /*
        * Request the current state of the digital inputs
        */
        void request_state()
        {
            while (!client_->wait_for_service(std::chrono::seconds(1))) {
                RCLCPP_INFO(this->get_logger(), "service not available, waiting again...");
            }

            auto request = std::make_shared<automatepro_interfaces::srv::ReqDigitalIn::Request>();
            using ServiceResponseFuture = rclcpp::Client<automatepro_interfaces::srv::ReqDigitalIn>::SharedFuture;
            auto response_received_callback = [this](ServiceResponseFuture future) {
                auto response = future.get();
                if (response->success) {
                    RCLCPP_INFO(this->get_logger(), "Service response received: success= %d", response->success);
                } else {
                    RCLCPP_ERROR(this->get_logger(), "Service call failed");
                }
            };

            auto result = client_->async_send_request(request, response_received_callback);
        }

        rclcpp::Subscription<automatepro_interfaces::msg::DigitalIn>::SharedPtr subscription_;
        rclcpp::Client<automatepro_interfaces::srv::ReqDigitalIn>::SharedPtr client_;
    };

    int main(int argc, char *argv[])
    {
        rclcpp::init(argc, argv);
        rclcpp::spin(std::make_shared<DigitalInSubscriber>());
        rclcpp::shutdown();
        return 0;
    }
    ```
    Run the node using the following command:

    ```bash
    ros2 run automatepro_cpp_tutorials digital_in_node
    ```
</TabItem>
</Tabs>


# --- mechanical.mdx ---

---
description: Mechanical mounting and installation of the AutomatePro.
---
# Mechanical
## AutomatePro Installation

import AMP_INSTALL from './images/AMP-INSTALL-GA.PNG'

<figure style={{ textAlign: 'center' }}>
  <img src={AMP_INSTALL} alt="AutomatePro Installation" style={{ width: '60%' }} />
  <figcaption>**AutomatePro Nominal Installation**</figcaption>
</figure>




**1.** Install the four provided rubber stand-offs (2) and tighten them until they come to a hard stop. For through-hole installations, secure each stand-off with a nut on the underside before tightening.
**2.** Position the AutomatePro on top of the stand-offs and secure it using the four provided DIN912-M6x16 socket head cap screws (1).

## Breakout Board
import AMP_BREAKOUT_INSTALL from './images/AMP-100-AMP-ASSY-REV-A-BREAKOUT-INSTALL-F.PNG'

<figure style={{ textAlign: 'center' }}>
  <img src={AMP_BREAKOUT_INSTALL} alt="AutomatePro Breakout Installation" style={{ width: '100%' }} />
  <figcaption>**AutomatePro Breakout Board Installation**</figcaption>
</figure>

### Installation

**1.** When installing the breakout board, first ensure that the device is powered down and remove the main connectors.
**2.** Align the connector and carefully push the breakout board onto the AutomatePro. **Warning:** Only push inside the area highlighted. Push the board down until it comes to a hard stop.
**3.** By hand, install all four M3 x 30 bolts [1].
**4.** Begin tightening the bolts in a cross-star pattern. Tighten the bolts until the part comes to a hard stop, as shown in the image.


### Removal

**1.** Ensure that the device is powered down and remove any external connectors.
**2.** Using a tool, carefully loosen and remove the four M3 x 30 bolts [1].
**3.** Once the bolts are removed, carefully lift the breakout board off the AutomatePro. **Warning:** Only lift from the area highlighted to avoid damaging the connectors.

### DIN Rail Installation

import AMP_DIN_INSTALL from './images/AMP-DIN-INSTALL.PNG'

<figure style={{ textAlign: 'center' }}>
  <img src={AMP_DIN_INSTALL} alt="AutomatePro DIN Rail Installation" style={{ width: '60%' }} />
  <figcaption>**AutomatePro DIN Rail Installation**</figcaption>
</figure>




**1.** Install the four rubber stand-offs onto the DIN rail mount (4) using the M6 nuts (3).
**2.** Position the AutomatePro on top of the stand-offs and secure it using the four M6x16 socket head cap screws (1).
**3.** Secure the entire assembly to the DIN rail (5).

#### General Installation Notes

- In high-vibration environments, use a threadlocker or wedge washers to ensure the fasteners remain secure.
- If custom stand-offs are used, maintain a minimum clearance of 6mm between the bottom of the heatsink and the mounting surface.
- The AutomatePro can be mounted in any orientation; however, mounting the unit with the Y+ axis pointing upward is recommended to prevent water pooling and improve convective cooling.








# --- desktop.mdx ---

---
description: Ubuntu desktop environment accessed over remote desktop (XRDP).
---
# Desktop Environment

AutomatePro comes with a generic Ubuntu desktop environment. This allows users to interact with the system using a graphical user interface (GUI).
Currently, the desktop environment is only available on the remote desktop.

## Remote Desktop

AutomatePro comes pre-installed with the XRDP server, which allows users to establish a remote desktop connection using the RDP protocol.

To use the remote desktop feature, you need an RDP client on your computer. Any client software that supports the RDP protocol should work.

Here are our recommendations:

### Linux:

#### Remmina
Remmina is a remote desktop client that supports multiple protocols, including RDP. It is usually
pre-installed on Ubuntu systems and a few other distributions.

If it is not installed, follow this [guide](https://remmina.org/how-to-install-remmina/) to install.

Once installed, follow these steps to connect to AutomatePro:
1. Click "+" (New Connection).
2. Set Protocol to RDP - Remote Desktop Protocol.
3. Enter AutomatePro's IP address in the Server field.
4. Enter the credentials:
        - Username: `admin`
        - Password: `password` or the password you set during the setup.
5. Click Save and Connect.

### Windows:

You can use the pre-installed Remote Desktop software to access AutomatePro.

1. Open Remote Desktop Connection (`mstsc` in the Run dialog).
2. Enter AutomatePro’s IP address and click Connect.
3. Enter the credentials:
    - Username: `admin`
    - Password: `password` or the password you set during the setup.
4. Click OK to access AutomatePro’s desktop.


:::info
The XRDP server on AutomatePro runs on port `3389`. Ensure that your firewall settings allow traffic through this port.
:::

## Troubleshooting

If you encounter issues, please contact support for further assistance. When contacting support, include the following log files.

```bash
~/.xsessionrc
~/.xsession-errors
```

# --- software-services.mdx ---

---
description: Install, update, and manage AutomatePro packages and services.
---
# Software Services

AutomatePro software is managed with Debian packages, systemd services, Docker Compose stacks, and ROS 2 configuration files under `/opt/automatepro`.

Use this page when you need to install or update the AutomatePro stack, check whether the drivers are running, inspect logs, or change which hardware drivers start at boot.

## Install or Update AutomatePro

AutomatePro systems are normally installed from the Lemvos AutomatePro package registry.

:::info
Registry access may require a Cloudsmith API key provided by Lemvos.
:::

1. Add the AutomatePro package repository:

   ```bash
   curl -1sLf 'https://dl.cloudsmith.io/API_KEY/lemvos/automatepro/setup.deb.sh' | sudo -E bash
   ```

2. Install or update the base package:

   ```bash
   sudo apt update
   sudo apt install automatepro-installer
   ```

The base `automatepro-installer` package depends on `automatepro-installer-wwan`, which installs the Quectel 5G connection manager.

## Installation Prompts

During installation, the package may ask configuration questions.

| Prompt | Select or enter | Result |
|--------|-----------------|--------|
| `automatepro-installer/override_cfg` | `true` only for a fresh installation or when you intentionally want packaged defaults. | Replaces `/opt/automatepro/config` with files from `/opt/automatepro/defconfig`. When `false`, only missing config files are copied into the existing config directory. |
| `automatepro-installer/domain_id_override` | A ROS 2 domain ID from `0` to `254`. | Updates both `ROS_DOMAIN_ID` and `XRCE_DOMAIN_ID_OVERRIDE` in `/opt/automatepro/.env`. Invalid values leave the existing `.env` unchanged. |

:::warning[Important]
Do not override configuration on a tuned field system unless you have backed up `/opt/automatepro/config`.
:::

## Service Overview

The main services are:

| Service | Purpose |
|---------|---------|
| `automatepro-gmsl2.service` | Initializes GMSL2 camera kernel modules and camera hardware. |
| `automatepro-gmsl2-ipc.path` | Watches the camera recovery IPC path for restart requests. |
| `automatepro-gmsl2-reactor.service` | Restarts GMSL2 hardware after the IPC path is triggered. |
| `automatepro-io-agent.service` | Starts the micro-ROS Agent container for the IO controller MCU. |
| `automatepro-core-driver.service` | Starts the core ROS 2 driver container for GNSS, IMU, cameras, RTK correction clients, and the driver manager. |
| `automatepro-foxglove-bridge.service` | Starts the host-side Foxglove Bridge on the configured bridge port. |
| `automatepro-ros-utils.service` | Optional visualization container for ROS 2 utilities. This service is installed but is not enabled by the base package post-install script. |

For IO controller diagnostic topics, message fields, and firmware fault-code decoding, see [IO Controller](/automatepro/manual/diagnostics/io-controller).

Check the current status:

```bash
systemctl status automatepro-gmsl2 automatepro-io-agent automatepro-core-driver automatepro-foxglove-bridge
```

Healthy services show `active (running)` or, for oneshot setup services such as `automatepro-gmsl2.service`, `active (exited)`.

## Start, Stop, and Restart Services

Restart a single service after changing its configuration:

```bash
sudo systemctl restart automatepro-core-driver
```

Restart the IO Agent:

```bash
sudo systemctl restart automatepro-io-agent
```

Restart Foxglove Bridge after changing the bridge launch XML:

```bash
sudo systemctl restart automatepro-foxglove-bridge
```

Restart camera hardware and the core driver:

```bash
sudo systemctl restart automatepro-gmsl2
sudo systemctl restart automatepro-core-driver
```

Stop all AutomatePro services during maintenance:

```bash
sudo systemctl stop automatepro-core-driver automatepro-foxglove-bridge automatepro-io-agent
```

## Logs

View recent logs for the core driver:

```bash
sudo journalctl -u automatepro-core-driver -n 100 --no-pager
```

Follow logs while reproducing an issue:

```bash
sudo journalctl -f -u automatepro-core-driver
```

Useful service logs include:

```bash
sudo journalctl -u automatepro-gmsl2 -n 100 --no-pager
sudo journalctl -u automatepro-io-agent -n 100 --no-pager
sudo journalctl -u automatepro-foxglove-bridge -n 100 --no-pager
```

## Docker Containers

The core driver and IO Agent services start Docker containers. Foxglove Bridge runs as a host ROS 2 service by default.

Check the running AutomatePro containers:

```bash
docker ps --filter name=automatepro
```

Expected containers can include:

```text
automatepro-core-driver
automatepro-io-agent
```

View container logs when systemd status is not enough:

```bash
docker logs automatepro-core-driver
docker logs automatepro-io-agent
```

If you intentionally enable `automatepro-ros-utils.service`, `automatepro-ros-utils` also appears in `docker ps`.

## Configuration Files

Runtime configuration lives under `/opt/automatepro/config`. Packaged defaults live under `/opt/automatepro/defconfig`.

| Path | Purpose |
|------|---------|
| `/opt/automatepro/.env` | Enables or disables drivers and sets ROS 2 environment values. |
| `/opt/automatepro/config/ros/camera_params.yaml` | Camera driver parameters. |
| `/opt/automatepro/config/ros/gnss_position_params.yaml` | GNSS position receiver parameters. |
| `/opt/automatepro/config/ros/gnss_heading_params.yaml` | GNSS heading receiver parameters. |
| `/opt/automatepro/config/ros/imu_params.yaml` | IMU driver parameters. |
| `/opt/automatepro/config/ros/ntrip_params.yaml` | NTRIP correction client parameters. |
| `/opt/automatepro/config/ros/spartn_params.yaml` | SPARTN correction client parameters. |
| `/opt/automatepro/config/ros/foxglove_bridge_launch.xml` | Foxglove Bridge launch configuration used by `automatepro-foxglove-bridge.service`. |
| `/opt/automatepro/defconfig/ros/` | Default copies installed by the package. |

After changing files in `/opt/automatepro/config/ros`, restart the service that uses them. For sensor driver changes, restart `automatepro-core-driver`.

## Enable or Disable Drivers

Edit `/opt/automatepro/.env` to control which core drivers start:

```bash
sudo nano /opt/automatepro/.env
```

Common flags:

| Variable | Packaged default | Set to `1` to enable |
|----------|------------------|----------------------|
| `ROS_DRIVER_GNSS_POSITION` | `1` | GNSS position driver |
| `ROS_DRIVER_GNSS_HEADING` | `1` | GNSS heading driver |
| `ROS_DRIVER_GNSS_RTK_CORRECTION_SPARTN` | `0` | SPARTN correction client |
| `ROS_DRIVER_GNSS_RTK_CORRECTION_NTRIP` | `0` | NTRIP correction client |
| `ROS_DRIVER_IMU` | `1` | IMU driver |
| `ROS_DRIVER_CAM_1` | `1` | Camera 1 driver |
| `ROS_DRIVER_CAM_2` | `1` | Camera 2 driver |
| `ROS_DRIVER_MANAGER` | `1` | Driver manager and camera recovery monitor |

Apply changes:

```bash
sudo systemctl restart automatepro-core-driver
```

## Verify ROS 2 After Changes

Source ROS 2 and inspect nodes:

```bash
source /opt/ros/humble/setup.bash
ros2 node list
```

Check topics:

```bash
ros2 topic list
```

For a quick live-data check:

```bash
ros2 topic echo /sensor/imu/data --once --qos-reliability best_effort
ros2 topic echo /sensor/gnss/position/fix --once
```

Optional hardware only publishes when installed and enabled.

<details>
  <summary>Developer Notes: core driver launch</summary>

The core driver container launches `automatepro_bringup` with `core_driver.launch.py`. The launch file accepts these boolean arguments:

| Argument | Default |
|----------|---------|
| `enable_gnss_position` | `true` |
| `enable_gnss_heading` | `true` |
| `enable_imu` | `true` |
| `enable_cam1` | `true` |
| `enable_cam2` | `true` |
| `enable_ntrip_client` | `true` |
| `enable_spartn_client` | `true` |
| `enable_driver_manager` | `true` |

The `config_dir` argument points the launch file at `/automatepro/config` inside the container, which is mounted from `/opt/automatepro/config/ros` on the host.

</details>


# --- imu.mdx ---

---
description: Onboard BNO085 9-axis IMU and its ROS 2 data.
---
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import automateproImag from '@site/docs/manual/sensors/images/IMUaxis.png';

# IMU

## Hardware

AutomatePro includes an onboard BNO085 9-axis MEMS IMU. The IMU combines a 3-axis accelerometer, 3-axis gyroscope, and 3-axis magnetometer. The driver publishes ROS 2 IMU data and, when enabled, magnetic-field and accuracy data.

The arrows on the device indicate the IMU vector directions:

<img src={automateproImag} alt="AutomatePro IMU axis directions" style={{ maxWidth: '100%', height: 'auto' }} />

### IMU Specifications

| Parameter | Value |
|-----------|-------|
| Sensor | BNO085 |
| Sensing types | 3-axis gyroscope, 3-axis magnetometer, 3-axis accelerometer |
| Rotation vector error (dynamic)<sup>1</sup> | 3.5 deg |
| Accelerometer accuracy<sup>1</sup> | 0.3 m/s<sup>2</sup> |
| Gyroscope accuracy<sup>1</sup> | 3.1 deg/s |
| Magnetometer accuracy<sup>1</sup> | 1.4 uT |
| Default IMU sample rate | 100 Hz |
| Configurable IMU rate | Up to 400 Hz, hardware dependent |

:::info[Info]
<sup>1</sup> Based on IC manufacturer tests. Results on AutomatePro can vary by mounting, calibration, and magnetic environment. See the [BNO080/BNO085 datasheet](https://www.ceva-ip.com/wp-content/uploads/2019/10/BNO080_085-Datasheet.pdf) for manufacturer details.
:::

## ROS API

#### Node: `automatepro_imu_driver`

### Publishers

| Topic | Type | Description |
|-------|------|-------------|
| `/sensor/imu/data` | [`sensor_msgs/msg/Imu`](https://docs.ros2.org/latest/api/sensor_msgs/msg/Imu.html) | Orientation, angular velocity, and linear acceleration. |
| `/sensor/imu/magnetic_field` | [`sensor_msgs/msg/MagneticField`](https://docs.ros2.org/latest/api/sensor_msgs/msg/MagneticField.html) | Magnetic field data, when magnetometer publishing is enabled. |
| `/sensor/imu/accuracy` | [`automatepro_imu_driver/msg/ImuAccuracy`](https://github.com/Lemvos/automatepro_imu_driver/blob/master/msg/ImuAccuracy.msg) | Accuracy levels for orientation, accelerometer, gyroscope, and magnetic-field reports. |

### Services

| Service | Type | Description |
|---------|------|-------------|
| `/sensor/imu/save_tare` | `std_srvs/srv/Trigger` | Persists the current tare to sensor flash. |
| `/sensor/imu/save_calibration` | `std_srvs/srv/Trigger` | Saves Dynamic Calibration Data (DCD) to sensor flash. |
| `/sensor/imu/reinit` | `std_srvs/srv/Trigger` | Reinitializes the IMU driver and sensor. |

## Verify IMU Data

Run these checks from AutomatePro over SSH:

```bash
source /opt/ros/humble/setup.bash
ros2 node list | grep automatepro_imu_driver
ros2 topic echo /sensor/imu/data --once --qos-reliability best_effort
```

If accuracy publishing is enabled, check calibration state:

```bash
ros2 topic echo /sensor/imu/accuracy --once
```

Accuracy levels use:

| Value | Meaning |
|-------|---------|
| `0` | Unreliable |
| `1` | Low |
| `2` | Medium |
| `3` | High |
| `255` | Unknown |

## Configuration

**Driver:** [automatepro_imu_driver](https://github.com/Lemvos/automatepro_imu_driver)<br/>
**Container:** [automatepro-core-driver](https://hub.docker.com/repository/docker/lemvos/automatepro_core_driver/general)

:::info
Runtime ROS configuration is stored in `/opt/automatepro/config/ros`. Packaged defaults are stored in `/opt/automatepro/defconfig/ros`.
:::

Edit the IMU parameter file:

```bash
sudo nano /opt/automatepro/config/ros/imu_params.yaml
```

Important parameters:

| Parameter | Default | Description |
|-----------|---------|-------------|
| `log_level` | `info` | Driver log level. Launch accepts `debug`, `info`, `warn`, `error`, or `fatal`. |
| `frame_id` | `lm450_imu_link` | Frame ID stamped on published IMU messages. |
| `rotation_vector_mode` | `game_rotation_vector` | Orientation report type. Use `rotation_vector` for magnetic absolute heading, `game_rotation_vector` for relative heading without magnetometer, or `geomagnetic_rotation_vector` for geomagnetic heading. |
| `tare.on_startup` | `false` | Applies one-shot tare when the node starts. Leave disabled after commissioning. |
| `tare.axes` | `xyz` | Axes included in tare. |
| `tare.persist` | `false` | Saves tare to flash when enabled. |
| `tare.orientation.enabled` | `true` | Applies the configured chip-to-body orientation mapping at startup and watchdog reinitialization. |
| `tare.orientation.use_quaternion` | `false` | Uses `qx`, `qy`, `qz`, and `qw` instead of roll, pitch, and yaw when enabled. |
| `tare.orientation.qx/qy/qz/qw` | `0/0/0/1` | Quaternion orientation mapping when `use_quaternion` is `true`. |
| `calibration.accelerometer.enabled` | `true` | Enables BNO08x accelerometer dynamic calibration. |
| `calibration.accelerometer.planar` | `false` | Enables planar accelerometer calibration mode. |
| `calibration.gyroscope.enabled` | `true` | Enables BNO08x gyroscope dynamic calibration. |
| `calibration.gyroscope.on_table` | `true` | Enables stationary on-table gyroscope calibration assist. |
| `calibration.magnetic_field.enabled` | `false` | Enables BNO08x magnetometer dynamic calibration. Enable only when magnetic heading is required and the environment is suitable. |
| `calibration.save_dcd` | `true` | Periodically saves Dynamic Calibration Data to flash. |
| `i2c.enabled` | `true` | Uses I2C communication. |
| `i2c.bus` | `/dev/i2c-7` | I2C bus path. |
| `i2c.address` | `0x4B` | BNO08x I2C address. |
| `uart.enabled` | `false` | Uses UART communication instead of I2C when enabled. |
| `uart.device` | `/dev/ttyACM0` | UART device path when UART transport is enabled. |
| `publish.accuracy.enabled` | `true` | Publishes `/sensor/imu/accuracy`. |
| `publish.imu.enabled` | `true` | Publishes `/sensor/imu/data`. |
| `publish.imu.rate` | `100` | Requested IMU report rate in Hz. |
| `publish.magnetic_field.enabled` | `false` | Publishes `/sensor/imu/magnetic_field`. Use `--qos-reliability best_effort` when echoing this topic from the CLI. |
| `publish.magnetic_field.rate` | `100` | Requested magnetometer report rate in Hz. |
| `publish.imu.orientation_offset.enabled` | `false` | Applies a software quaternion offset before publishing orientation. |

:::warning[Important]
Restart `automatepro-core-driver` after changing the configuration. Configuration values are loaded when the driver container starts and are only applied after the service restarts.

```bash
sudo systemctl restart automatepro-core-driver
```
:::

### Default Configuration Shape

```yaml
automatepro_imu_driver:
  ros__parameters:
    log_level: "info"
    frame_id: "lm450_imu_link"
    rotation_vector_mode: "game_rotation_vector"
    tare:
      on_startup: false
      axes: "xyz"
      persist: false
      orientation:
        enabled: true
        roll_deg: 0.0
        pitch_deg: 0.0
        yaw_deg: -80.0
        use_quaternion: false
        qx: 0.0
        qy: 0.0
        qz: 0.0
        qw: 1.0
    calibration:
      accelerometer:
        enabled: true
        planar: false
      gyroscope:
        enabled: true
        on_table: true
      magnetic_field:
        enabled: false
      save_dcd: true
    i2c:
      enabled: true
      bus: "/dev/i2c-7"
      address: "0x4B"
    uart:
      enabled: false
      device: "/dev/ttyACM0"
    publish:
      accuracy:
        enabled: true
      magnetic_field:
        enabled: false
        rate: 100
      imu:
        enabled: true
        rate: 100
```

## Example

This example prints IMU and magnetic-field messages by subscribing to `/sensor/imu/data` and `/sensor/imu/magnetic_field`.

The tutorial package is available in the [AutomatePro tutorials repository](https://github.com/Lemvos/automatepro_tutorials).

<Tabs>
  <TabItem value="Python" label="Python" default>

    ```python
    import rclpy
    from rclpy.node import Node
    from sensor_msgs.msg import Imu, MagneticField


    class ImuSubscriber(Node):
        def __init__(self):
            super().__init__("imu_subscriber")
            self.imu_subscription = self.create_subscription(
                Imu,
                "/sensor/imu/data",
                self.imu_callback,
                10,
            )
            self.magnetic_field_subscription = self.create_subscription(
                MagneticField,
                "/sensor/imu/magnetic_field",
                self.magnetic_field_callback,
                10,
            )

        def imu_callback(self, msg):
            self.get_logger().info(
                "Received IMU message: orientation=%s, angular_velocity=%s, linear_acceleration=%s"
                % (msg.orientation, msg.angular_velocity, msg.linear_acceleration)
            )

        def magnetic_field_callback(self, msg):
            self.get_logger().info(
                "Received MagneticField message: magnetic_field=%s" % msg.magnetic_field
            )


    def main(args=None):
        rclpy.init(args=args)
        node = ImuSubscriber()
        rclpy.spin(node)
        node.destroy_node()
        rclpy.shutdown()


    if __name__ == "__main__":
        main()
    ```

    Run the node:

    ```bash
    ros2 run automatepro_py_tutorials imu_node
    ```

  </TabItem>
  <TabItem value="C++" label="C++">

    ```cpp
    #include <memory>

    #include <rclcpp/rclcpp.hpp>
    #include <sensor_msgs/msg/imu.hpp>
    #include <sensor_msgs/msg/magnetic_field.hpp>

    class ImuSubscriber : public rclcpp::Node
    {
    public:
      ImuSubscriber()
      : Node("imu_subscriber")
      {
        imu_subscription_ = this->create_subscription<sensor_msgs::msg::Imu>(
          "/sensor/imu/data",
          10,
          std::bind(&ImuSubscriber::imu_callback, this, std::placeholders::_1));

        magnetic_field_subscription_ =
          this->create_subscription<sensor_msgs::msg::MagneticField>(
            "/sensor/imu/magnetic_field",
            10,
            std::bind(&ImuSubscriber::magnetic_field_callback, this, std::placeholders::_1));
      }

    private:
      void imu_callback(const sensor_msgs::msg::Imu::SharedPtr msg) const
      {
        RCLCPP_INFO(
          this->get_logger(),
          "Received IMU message: orientation=[x: %f, y: %f, z: %f, w: %f], angular_velocity=[x: %f, y: %f, z: %f], linear_acceleration=[x: %f, y: %f, z: %f]",
          msg->orientation.x, msg->orientation.y, msg->orientation.z, msg->orientation.w,
          msg->angular_velocity.x, msg->angular_velocity.y, msg->angular_velocity.z,
          msg->linear_acceleration.x, msg->linear_acceleration.y, msg->linear_acceleration.z);
      }

      void magnetic_field_callback(const sensor_msgs::msg::MagneticField::SharedPtr msg) const
      {
        RCLCPP_INFO(
          this->get_logger(),
          "Received MagneticField message: magnetic_field=[x: %f, y: %f, z: %f]",
          msg->magnetic_field.x, msg->magnetic_field.y, msg->magnetic_field.z);
      }

      rclcpp::Subscription<sensor_msgs::msg::Imu>::SharedPtr imu_subscription_;
      rclcpp::Subscription<sensor_msgs::msg::MagneticField>::SharedPtr magnetic_field_subscription_;
    };

    int main(int argc, char * argv[])
    {
      rclcpp::init(argc, argv);
      rclcpp::spin(std::make_shared<ImuSubscriber>());
      rclcpp::shutdown();
      return 0;
    }
    ```

    Run the node:

    ```bash
    ros2 run automatepro_cpp_tutorials imu_node
    ```

  </TabItem>
</Tabs>

## Orientation Commissioning

Use this workflow when the IMU is remounted, when the reference orientation changes, or when a system needs a known rest pose.

:::warning[Important]
Perform tare and calibration only when the device is in its final mounting position and away from motors, steel structures, strong magnets, and current-carrying wires.
:::

1. **Complete dynamic calibration.**

   Move the device through several stable orientations for accelerometer calibration, leave it stationary for gyroscope calibration, and perform magnetic rotations only if magnetic-field calibration is enabled and required.

2. **Tare the reference pose.**

   Place AutomatePro in the desired zero orientation. Temporarily set:

   ```yaml
   tare:
     on_startup: true
     axes: "xyz"
     persist: true
   ```

   Restart the core driver and wait for tare completion in logs:

   ```bash
   sudo systemctl restart automatepro-core-driver
   sudo journalctl -u automatepro-core-driver -f
   ```

   After tare completes, set `tare.on_startup: false` so it is not applied on every boot.

3. **Set mounting orientation.**

   Use `tare.orientation.roll_deg`, `pitch_deg`, and `yaw_deg` to map the chip axes to the device body frame. This orientation mapping is re-applied on startup and watchdog recovery.

4. **Apply software heading offset when needed.**

   If the device in the rest pose should publish identity orientation `(0, 0, 0, 1)`, capture the current quaternion:

   ```bash
   ros2 topic echo /sensor/imu/data --field orientation --once
   ```

   Set `publish.imu.orientation_offset.*` to the conjugate of that quaternion and enable `publish.imu.orientation_offset.enabled`.

5. **Verify the final output.**

   Restart the core driver and verify that the rest pose and motion directions match your robot frame:

   ```bash
   sudo systemctl restart automatepro-core-driver
   ros2 topic echo /sensor/imu/data --field orientation --once
   ```

<details>
  <summary>Developer Notes: rotation modes and persistence</summary>

`rotation_vector_mode` selects which BNO08x SH-2 report is enabled. `game_rotation_vector` does not use the magnetometer and reports relative heading. `rotation_vector` and `geomagnetic_rotation_vector` use magnetic information for heading.

Dynamic tare runs only on initial node startup when `tare.on_startup` is enabled. The fixed mounting orientation is re-applied on every sensor initialization, including watchdog recovery. The software orientation offset is applied as `q_published = q_offset * q_sensor`.

</details>

## Troubleshooting

### IMU Topic Is Missing

Check that the driver is enabled:

```bash
grep ROS_DRIVER_IMU /opt/automatepro/.env
```

Check logs:

```bash
sudo journalctl -u automatepro-core-driver -n 100 --no-pager
```

### Orientation Is Correct After Startup but Wrong After Recovery

Verify `tare.orientation.enabled: true`. The fixed orientation mapping is the part reapplied on every sensor initialization.

### Heading Drifts or Jumps

Check the rotation-vector mode and magnetic environment. If the system does not require magnetic absolute heading, `game_rotation_vector` is less sensitive to magnetic disturbances. If magnetic heading is required, calibrate in the deployment environment and monitor `/sensor/imu/accuracy`.


# --- gmsl2-camera.mdx ---

---
description: Onboard GMSL2 camera inputs and configuration.
---
# GMSL2 Camera

## Hardware

AutomatePro can be ordered with 0, 2, or 4 GMSL2 (Gigabit Multimedia Serial Link) camera inputs. GMSL2 is a high-speed, low-latency camera link commonly used in automotive applications. It supports cable lengths up to 15 m and Power over Coax (PoC). AutomatePro can supply up to 12 W total to downstream GMSL2 cameras.

Recommended cameras are listed in the [camera accessories](/automatepro/category/cameras). The number of populated FAKRA Z connectors depends on the ordered device configuration.

![GMSL2 camera connectors](images/AutomatePro_GMSL.png)

### Onboard GMSL2 Specifications

| Parameter | Value |
|-----------|-------|
| Onboard IC | MAX9296AGT |
| Standard | GMSL2 |
| Max cable length | 15 m |
| Max data rate | 4.5 Gbps |
| PoC voltage | 12 VDC |
| FAKRA type | Type Z (Water Blue) |
| Max power | 12 W total |

## ROS API

#### Nodes: `automatepro_cam1_node`, `automatepro_cam2_node`

### Publishers

Replace `<camera_name>` with the configured camera name, usually `cam1` or `cam2`.

| Topic | Type | Description |
|-------|------|-------------|
| `/camera/<camera_name>/image_raw` | [`sensor_msgs/msg/Image`](https://docs.ros2.org/latest/api/sensor_msgs/msg/Image.html) | Raw camera frames in the configured output encoding. |
| `/camera/<camera_name>/camera_info` | [`sensor_msgs/msg/CameraInfo`](https://docs.ros2.org/latest/api/sensor_msgs/msg/CameraInfo.html) | Camera calibration and projection information. |
| `/camera/<camera_name>/h264/video` | [`foxglove_msgs/msg/CompressedVideo`](https://docs.foxglove.dev/docs/visualization/message-schemas/compressed-video) | H.264 compressed video frames. |
| `/camera/<camera_name>/h264/calib` | [`foxglove_msgs/msg/CameraCalibration`](https://docs.foxglove.dev/docs/visualization/message-schemas/camera-calibration) | Calibration metadata for the compressed stream. |

### Debug Service

| Service | Type | Description |
|---------|------|-------------|
| `/automatepro_cam<N>_node/disable_camera_inputs` | `std_srvs/srv/SetBool` | Debug-only service for simulating camera input loss and recovery behavior. |

## Verify Cameras

Run these checks from AutomatePro over SSH:

```bash
source /opt/ros/humble/setup.bash
ros2 node list | grep automatepro_cam
ros2 topic list | grep '/camera/'
```

For a single-frame check:

```bash
ros2 topic echo /camera/cam1/camera_info --once
```

If camera compression is enabled, verify the H.264 stream:

```bash
ros2 topic hz /camera/cam1/h264/video
```

## Configuration

Cameras are exposed as V4L2 devices such as `/dev/video0` and `/dev/video1`. The AutomatePro camera driver uses these devices through GStreamer and publishes ROS 2 image and compressed-video topics.

**Driver:** [automatepro_camera_driver](https://github.com/Lemvos/automatepro_camera_driver)<br/>
**Container:** [automatepro-core-driver](https://hub.docker.com/repository/docker/lemvos/automatepro_core_driver/general)

:::info
Runtime ROS configuration is stored in `/opt/automatepro/config/ros`. Packaged defaults are stored in `/opt/automatepro/defconfig/ros`.
:::

Edit the camera parameter file:

```bash
sudo nano /opt/automatepro/config/ros/camera_params.yaml
```

Important parameter groups:

| Parameter | Default | Description |
|-----------|---------|-------------|
| `enable` | `true` | Enables the camera node when the launch file starts it. |
| `camera_name` | `cam1`, `cam2` | Name used in camera topic namespaces. |
| `frame_id` | `cam1`, `cam2` | Frame ID stamped on camera messages. |
| `resolution` | `[640, 512]` | ROS output resolution. |
| `format` | `bgr8` | ROS image encoding. |
| `qos_history_depth` | `1` | Queue depth for raw image publishers. |
| `qos_reliability` | `reliable` | Raw image QoS reliability, either `reliable` or `best_effort`. |
| `qos_durability` | `volatile` | Raw image QoS durability, either `volatile` or `transient_local`. |
| `camera_info.*` | Identity calibration by default | Calibration values published on `camera_info`. |
| `compression.enable` | `true` | Enables H.264 output. |
| `compression.encoder` | `auto` | Selects the encoder: `auto` uses GPU encoding when available and falls back to CPU, `gpu` forces `nvv4l2h264enc`, and `cpu` forces `x264enc`. |
| `compression.bitrate` | `5000` | Target bitrate in kbps. Set to `0` for automatic bitrate calculation. |
| `compression.bits_per_pixel` | `0.1` | Used for automatic bitrate calculation when `bitrate` is `0`. |
| `compression.resolution` | `[640, 512]` | Compressed stream resolution. |
| `compression.framerate` | `30` | Compressed stream frame rate. |
| `compression.qos_*` | varies by camera | QoS settings for the compressed-video publishers. |
| `rtp.enable` | `false` | Sends the H.264 stream over RTP/UDP when enabled. |
| `rtp.ip` | `192.168.20.100` | RTP destination IP address. |
| `rtp.port` | `5000` / `5001` | RTP destination UDP port. |
| `watchdog.timeout` | `2000` | Time in milliseconds without frames before recovery starts. |
| `watchdog.cycle_time` | `500` | Watchdog check interval in milliseconds. |
| `recovery.max_recovery_attempts` | `1` | Pipeline recovery attempts before GMSL2 restart escalation. |
| `frame_validation.sample_pixels` | optional | Enables additional frame validation sampling when uncommented. |
| `logging.level` | `info` | Driver log level. |
| `logging.throttle_ms` | `10000` | Throttle interval for repeated debug logs. |
| `publish_rate` | `0.0` | Raw image publish rate limit in Hz. `0.0` publishes every frame; H.264 output rate is controlled by `compression.framerate`. |
| `sensor.device` | `/dev/video0`, `/dev/video1` | V4L2 device. Normally fixed by hardware. |
| `sensor.resolution` | `[1920, 1536]` | Sensor capture resolution. Normally fixed by hardware. |
| `sensor.frame_rate` | `30` | Sensor capture frame rate. |

### Default Config

The default file includes one section for each camera node. The most commonly edited values are shown below:

```yaml
automatepro_cam1_node:
  ros__parameters:
    enable: true
    camera_name: "cam1"
    frame_id: "cam1"
    resolution: [640, 512]
    format: "bgr8"

    camera_info:
      distortion_model: "plumb_bob"
      d: [0.0, 0.0, 0.0, 0.0, 0.0]
      k: [1.0, 0.0, 0.0, 0.0, 1.0, 0.0, 0.0, 0.0, 1.0]
      r: [1.0, 0.0, 0.0, 0.0, 1.0, 0.0, 0.0, 0.0, 1.0]
      p: [1.0, 0.0, 0.0, 0.0, 0.0, 1.0, 0.0, 0.0, 0.0, 0.0, 1.0, 0.0]

    compression:
      enable: true
      encoder: "auto"
      bitrate: 5000
      bits_per_pixel: 0.1
      keyframe: 30
      resolution: [640, 512]
      framerate: 30

    rtp:
      enable: false
      ip: "192.168.20.100"
      port: 5000

    watchdog:
      timeout: 2000
      cycle_time: 500

    recovery:
      wait_time_gmsl2_restart: 10
      max_recovery_attempts: 1
      window_seconds: 60

    publish_rate: 0.0

    # Do not change the sensor block unless Lemvos support instructs you to.
    sensor:
      device: "/dev/video0"
      model: "SG3S-ISX031C"
      resolution: [1920, 1536]
      frame_rate: 30
      format: "UYVY"

automatepro_cam2_node:
  ros__parameters:
    enable: true
    camera_name: "cam2"
    frame_id: "cam2"
    resolution: [640, 512]
    format: "bgr8"

    # Same structure as cam1. Defaults use /dev/video1 and RTP port 5001.
    sensor:
      device: "/dev/video1"
      model: "SG3S-ISX031C"
      resolution: [1920, 1536]
      frame_rate: 30
      format: "UYVY"
```

:::warning[Important]
Restart `automatepro-core-driver` after changing the configuration. Configuration values are loaded when the driver container starts and are only applied after the service restarts.

```bash
sudo systemctl restart automatepro-core-driver
```
:::

## Enable or Disable Camera Drivers

To disable the AutomatePro camera driver and use a different V4L2, OpenCV, GStreamer, or custom camera stack, edit `/opt/automatepro/.env`:

```bash
sudo nano /opt/automatepro/.env
```

Set the relevant flags to `0`:

```text
ROS_DRIVER_CAM_1=0
ROS_DRIVER_CAM_2=0
```

Apply the change:

```bash
sudo systemctl restart automatepro-core-driver
```

Now you can use the cameras with any standard V4L2 driver, OpenCV, GStreamer, or another camera driver. For more details on hardware acceleration and GStreamer, see the NVIDIA Jetson [multimedia documentation](https://docs.nvidia.com/jetson/archives/r36.3/DeveloperGuide/SD/Multimedia.html).

## Troubleshooting

### Camera Topics Are Missing

1. Check whether the camera driver is enabled:

   ```bash
   grep ROS_DRIVER_CAM /opt/automatepro/.env
   ```

2. Check the services:

   ```bash
   systemctl status automatepro-gmsl2 automatepro-core-driver
   ```

3. Inspect recent logs:

   ```bash
   sudo journalctl -u automatepro-gmsl2 -n 100 --no-pager
   sudo journalctl -u automatepro-core-driver -n 100 --no-pager
   ```

### Restart Camera Hardware

If the camera hardware needs to be reinitialized:

```bash
sudo systemctl stop automatepro-core-driver
sudo systemctl restart automatepro-gmsl2
sudo systemctl start automatepro-core-driver
```

### Automatic Recovery

The camera driver includes a watchdog. If frames stop arriving, it first attempts driver-level recovery. If hardware-level recovery is required, the driver sets `recovery.reset_fail=true`; the AutomatePro Driver Manager watches that parameter and triggers GMSL2 restart through the AutomatePro IPC path and systemd services.

Verify that the driver manager is running:

```bash
ros2 node list | grep automatepro_driver_manager
```

:::warning[Important]
Automatic GMSL2 hardware recovery requires `ROS_DRIVER_MANAGER=1` in `/opt/automatepro/.env` and the `automatepro_driver_manager` node must be running.
:::

<details>
  <summary>Developer Notes: launch and recovery internals</summary>

`automatepro_bringup` starts camera nodes with `core_driver.launch.py`. Camera 1 and Camera 2 are controlled by the launch arguments `enable_cam1` and `enable_cam2`.

The Driver Manager discovers camera nodes matching `/automatepro_cam.*`, checks lifecycle state, reads the `recovery.reset_fail` parameter, and creates a restart request under `/opt/automatepro/ipc/` when a camera node is unconfigured with `recovery.reset_fail=true`. The `automatepro-gmsl2-ipc.path` unit triggers `automatepro-gmsl2-reactor.service`, which restarts `automatepro-gmsl2.service`.

</details>


# --- configuration.mdx ---

---
sidebar_position: 4
toc_max_heading_level: 5
description: Configure the u-blox GNSS receiver with the PyGPSClient GUI.
---
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Configuration GUI
GNSS configuration can be done through the GUI, which provides extensive options for configuring GNSS receiver settings.

PyGPSClient is a graphical tool for configuring u-blox GNSS receivers. The interface is similar to the official u-blox configuration tools, `u-center` and `u-center2`.

## Identify the USB Device
List connected serial devices to locate your GNSS receiver:
```bash
ls -l /dev/serial/by-id/
```
Example output:
```text
lrwxrwxrwx 1 root root 13 Aug 13 11:13 usb-u-blox_AG_-_www.u-blox.com_u-blox_GNSS_receiver_F9P-if00 -> ../../ttyACM1
lrwxrwxrwx 1 root root 13 Aug 13 11:13 usb-u-blox_AG_-_www.u-blox.com_u-blox_GNSS_receiver_F9H-if00 -> ../../ttyACM0
```
Note the device path (e.g., `/dev/ttyACM1` for F9H).

## Install PyGPSClient
Install PyGPSClient using pip:
```bash
python3 -m pip install --upgrade pygpsclient
```
For further details, refer to the [PyGPSClient GitHub page](https://github.com/semuconsulting/PyGPSClient).

## Launch the Configuration GUI
Start PyGPSClient:
```bash
pygpsclient
```
Select the appropriate serial port for your GNSS receiver in the GUI.

![PyGPSClient Main Screen](images/pygpsclient_main.png)

The main screen displays location and GNSS status in real time.

## Configure GNSS Receiver Settings
Access the UBX configuration menu for advanced settings:

![UBX Config Menu](images/ubx_config_menu.png)

![UBX Config Window](images/ubx_config.png)

The UBX Configuration window provides full access to all available settings for the connected GNSS receiver. For detailed parameter descriptions, consult the [PyGPSClient interface manual](https://content.u-blox.com/sites/default/files/documents/u-blox-F9-HPG-1.32_InterfaceDescription_UBX-22008968.pdf).

## Differential Heading Configuration

### u-blox ZED-F9P

1. Configure UART2 to send RTCM messages to F9H:
    ```text
    CFG-UART2OUTPORT_RTCM3X 0x1
    ```
2. Enable RTCM message types on UART2:
    ```text
    CFG-MSGOUT-RTCM_3X_TYPE1077_UART2 0x1
    CFG-MSGOUT-RTCM_3X_TYPE1087_UART2 0x1
    CFG-MSGOUT-RTCM_3X_TYPE1097_UART2 0x1
    CFG-MSGOUT-RTCM_3X_TYPE1127_UART2 0x1
    CFG-MSGOUT-RTCM_3X_TYPE1230_UART2 0x1
    CFG-MSGOUT-RTCM_3X_TYPE4072_0_UART2 0x1
    ```

### u-blox ZED-F9H

1. Configure UART2 to receive RTCM messages from F9P:
    ```text
    CFG-UART2INPORT_RTCM3X 0x1
    ```
2. Enable RTCM message types on UART2:
    ```text
    CFG-MSGOUT-RTCM_3X_TYPE1077_UART2 0x1
    CFG-MSGOUT-RTCM_3X_TYPE1087_UART2 0x1
    CFG-MSGOUT-RTCM_3X_TYPE1097_UART2 0x1
    CFG-MSGOUT-RTCM_3X_TYPE1127_UART2 0x1
    CFG-MSGOUT-RTCM_3X_TYPE1230_UART2 0x1
    CFG-MSGOUT-RTCM_3X_TYPE4072_0_UART2 0x1
    ```


# --- correction.mdx ---

---
sidebar_position: 3
toc_max_heading_level: 4
description: RTK correction data over cellular, Wi-Fi, or optional L-band reception.
---
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# RTK Correction
## Hardware
The default correction data is received over a cellular or Wi-Fi connection.

:::info[Info]
The AutomatePro offers an optional L-band RTK correction data receiver, which receives correction data from L-Band satellites. This allows the RTK GNSS to function without a cellular connection and provides correction redundancy. For more information, contact Lemvos.
:::
## ROS API

**Node**: `automatepro_ntrip_client`/`automatepro_spartn_client`


### Publishers

| Topic | Type | Description |
|-------|------|-------------|
| `/sensor/gnss/correction` | [`rtcm_msgs/msg/Message`](https://github.com/tilk/rtcm_msgs/blob/master/msg/Message.msg) | RTCM or SPARTN correction data. |


### Parameters
| Parameter | Type | Values | Runtime R/W | Description |
|-----------|------|--------|-------------|-------------|
| `use_https` | bool | `true/false` | `read-only` | Uses HTTPS when set to `true`. |
| `host` | string | | `read-only` | NTRIP server hostname. |
| `port` | string | `2101/2102` | `read-only` | NTRIP server port. |
| `mountpoint` | string | | `read-only` | Mountpoint to connect to. |
| `username` | string | | `read-only` | Login credentials for the NTRIP subscription. |
| `password` | string | | `read-only` | Login credentials for the NTRIP subscription. |


## RTK Correction Services

RTK (Real-Time Kinematic) correction service is a technique used to enhance the precision of position data derived from satellite-based positioning systems such as GPS, GLONASS, Galileo, and BeiDou. RTK achieves centimeter-level accuracy by using corrections transmitted from a network of ground-based reference stations to a receiver in real time. AutomatePro supports RTCM and SPARTN correction data over the NTRIP protocol.

If you already have login credentials for the NTRIP caster you want to use, proceed with configuring parameters as shown below. If you would like to set up a new one, follow the instructions below.

:::info
The following services have been tested with AutomatePro. If you use a different service provider, you may need to configure it according to that provider's instructions.
:::


Here are the suggested free and paid correction services:

### u-blox PointPerfect (Paid)

u-blox correction data uses SPARTN format, which is more efficient than RTCM. More details can be found [here](https://www.u-blox.com/en/technologies/ppp-rtk-gnss-correction-services-pointperfect).
Before proceeding to the next step, you should have a `hostname`, `mountpoint`, `username`, and `password`.


The configuration file is:
```bash
/opt/automatepro/config/ros/spartn_client.yaml
```

```yaml
automatepro_spartn_client:
  ros__parameters:
    tls: false
    host: "ppntrip.services.u-blox.com"
    port: "2101"  # [http: 2101, https: 2102]
    mountpoint: "EU" # [Europe: EU, United States: US, Australia: AU, Korea: KR]
    username: "username" # Your USERNAME
    password: "password" # Your PASSWORD
```


### RTK2go (Free)

RTK2go is a community NTRIP caster that provides free RTK correction service. To use it, select a mountpoint near your location.
You can find the list of available mountpoints [here](http://monitor.use-snip.com/?hostUrl=rtk2go.com&port=2101). Click on **`view-all`** to view mountpoints on the map.

Once you have the mountpoint you would like to connect to, add that to the config file as shown below.

The configuration file is:
```bash
/opt/automatepro/config/ros/ntrip_client.yaml
```

```yaml
ntrip_client:
  ros__parameters:
    use_https: false
    host: rtk2go.com
    port: 2101
    mountpoint: Prittlbach  # Add your mountpoint here
    username: "noname"
    password: "password"
```


:::warning[Important]
Restart the `automatepro-core-driver` Docker container after changing the configuration.
Configuration changes are applied only after the container restarts.

```bash
docker restart automatepro-core-driver
```
:::


# --- heading.mdx ---

---
sidebar_position: 2
toc_max_heading_level: 3
description: Integrated dual-antenna RTK GNSS heading for precise bearing.
---

# RTK Heading
## Hardware
AutomatePro can be configured with an integrated RTK GNSS heading sensor, providing precise heading.
This is particularly useful when the onboard IMU’s magnetic bearing accuracy is insufficient or when the
magnetometer is affected by the operational environment, such as around large steel structures.
Depending on antenna placement, the GNSS heading sensor can also measure the robot’s roll or pitch.

The differential heading is provided by the u-blox F9H series. If included, AutomatePro will
feature a blue FAKRA connector for antenna connection, with “GNSS-2” used for position and
“GNSS-1” for heading.

![AutomatePro GNSS connector location](images/AutomatePro_GNSS.png)

### GNSS Heading Specifications
| Parameter               | Value                                                                                           |
|-------------------------|-------------------------------------------------------------------------------------------------|
| Receiver                | u-blox F9H                                                                                 |
| Connector               | GNSS-1 (FAKRA C - Signal Blue)                                                            |
| Supported Constellation | GPS, Galileo, BeiDou, GLONASS                                                             |
| Heading Accuracy        |  0.4° <sup>1</sup>    (with antenna spacing &gt;1m)                                                  |
| Max Navigation Update Rate     | Default 1Hz (Can be configured up to 8Hz)                                                                                        |
:::info[Info]
<sup>1</sup> Depends on antennas and spacing between the two antennas. [See the datasheet for more information](https://content.u-blox.com/sites/default/files/ZED-F9H-01B_DataSheet_UBX-21025012.pdf).
:::

### External Antenna
The AutomatePro system is compatible with a variety of multi-band high-precision GNSS antennas that have a FAKRA Type C connector (Signal Blue). More information on antennas used with AutomatePro is available [here](/automatepro/category/antennas).

Antenna Separation: Ensure a minimum separation of 1 meter (≥1m) between the two GNSS antennas. Configuration options are heading (Yaw) and pitch or heading (Yaw) and roll.

import PitchYaw from './images/PitchYaw.png';
import YawRoll from './images/YawRoll.png';

<div style={{ columnCount: 2, columnGap: '0px' }}>
  <div>
    <strong></strong><br />
    <h2>Yaw & Pitch </h2>
    <img src={PitchYaw} style={{width: '90%'}}/>
  </div>
  <div>
    <strong></strong><br />

    <h2>Yaw & Roll</h2>
    <img src={YawRoll} style={{width: '90%', paddingTop: '10px'}}/>
  </div>
</div>
<br />

## ROS API

**Node**: `automatepro_gnss_heading_node`

### Publishers

| Topic | Type | Description |
|-------|------|-------------|
| `/sensor/gnss/heading/fix` | [`sensor_msgs/msg/NavSatFix`](https://docs.ros2.org/latest/api/sensor_msgs/msg/NavSatFix.html) | GNSS fix from the heading receiver. |
| `/sensor/gnss/heading/fix_velocity` | [`geometry_msgs/msg/`<br/>`TwistWithCovarianceStamped`](https://docs.ros2.org/latest/api/geometry_msgs/msg/TwistWithCovarianceStamped.html) | Velocity from the heading receiver. |
| `/sensor/gnss/heading/true_heading` | [`sensor_msgs/msg/Imu`](https://docs.ros2.org/latest/api/sensor_msgs/msg/Imu.html) | Differential heading as an orientation quaternion with covariance. |
| `/sensor/gnss/heading/navrelposned` | [`ublox_msgs/msg/NavRELPOSNED9`](https://github.com/Lemvos/automatepro_gnss_driver/blob/ros2/ublox_msgs/msg/NavRELPOSNED9.msg) | Detailed differential heading with additional information for calculating either roll or pitch. |
| `/sensor/gnss/heading/monhw` | [`ublox_msgs/msg/MonHW`](https://github.com/Lemvos/automatepro_gnss_driver/blob/ros2/ublox_msgs/msg/MonHW.msg) | Hardware status of the GNSS heading module. |
| `/sensor/gnss/heading/monsys` | [`ublox_msgs/msg/MonSYS`](https://github.com/Lemvos/automatepro_gnss_driver/blob/ros2/ublox_msgs/msg/MonSYS.msg) | System monitor status of the GNSS heading module. |
| `/diagnostics` | [`diagnostic_msgs/msg/DiagnosticArray`](https://docs.ros2.org/latest/api/diagnostic_msgs/msg/DiagnosticArray.html) | GNSS heading ROS driver diagnostics. |

:::tip[Topic for heading data]
Even though `/sensor/gnss/heading/navrelposned` provides more information, it is not required for normal use cases.
It is recommended to use `/sensor/gnss/heading/true_heading` for your application, which provides heading as a quaternion with covariance.
:::

### Parameters
| Parameter | Type | Packaged default | Runtime R/W | Description |
|-----------|------|------------------|-------------|-------------|
| `device` | string | `/dev/serial/by-id/usb-u-blox_AG_-_www.u-blox.com_u-blox_GNSS_receiver-if00` | `read-only` | Serial port for the F9H receiver. |
| `frame_id` | string | `gnss_heading` | `read-only` | Frame ID for published messages. |
| `config_on_startup` | bool | `false` | `read-only` | Leaves receiver flash configuration unchanged at startup. |
| `gpio.chipname` | string | `gpiochip0` | `read-only` | GPIO chip used for receiver reset recovery. |
| `gpio.line_num` | int | `134` | `read-only` | GPIO line used for receiver reset recovery. |
| `gpio.reset_time` | int | `1` | `read-only` | Reset pulse duration in seconds. |
| `recovery.cycle_time` | int | `1` | `read-only` | Delay between recovery attempts in seconds. |
| `watchdog.timeout` | int | `3000` | `read-only` | Watchdog timeout in milliseconds. |
| `watchdog.cycle_time` | int | `500` | `read-only` | Watchdog check interval in milliseconds. |
| `publish.all` | bool | `false` | `read-only` | Enables all publisher groups when true. |
| `publish.aid.hui` | bool | `false` | `read-only` | Enables AID-HUI message publishing. |
| `publish.nav.relposned` | bool | `true` | `read-only` | Enables NavRELPOSNED9 publishing. |
| `publish.nav.heading` | bool | `true` | `read-only` | Enables heading publishing on `/sensor/gnss/heading/true_heading`. |
| `publish.nmea` | bool | `false` | `read-only` | Keeps NMEA sentence publishing disabled for the heading receiver. |
| `publish.mon.all` | bool | `true` | `read-only` | Enables monitor-message publishers, including hardware status. |
| `debug` | int | `0` | `read-only` | Debug verbosity; range `0-4`. |

## Configuration

**Driver**: [automatepro_gnss_driver](http://github.com/Lemvos/automatepro_gnss_driver)
**Container**: [automatepro-core-driver](https://hub.docker.com/repository/docker/lemvos/automatepro_core_driver/general)

:::info
Runtime configuration files are in the `/opt/automatepro/config/ros` directory.
Packaged defaults are installed in `/opt/automatepro/defconfig/ros`.
The runtime files are mounted into the Docker container and used by the ROS driver.
Environment variables can be set in the `/opt/automatepro/.env` file.
:::

The runtime configuration file is:

```bash
/opt/automatepro/config/ros/gnss_heading_params.yaml
```

The packaged default source is:

```bash
/opt/automatepro/defconfig/ros/gnss_heading_params.yaml
```

### Default Config
```yaml
# Configuration Settings for F9H device in Rover Mode
automatepro_gnss_heading_node:
  ros__parameters:
    debug: 0                    # Range 0-4 (0 means no debug statements will print)

    device: /dev/serial/by-id/usb-u-blox_AG_-_www.u-blox.com_u-blox_GNSS_receiver-if00 #/dev/ttyACM0
    frame_id: gnss_heading

    config_on_startup: false    # If false, the node will not override configurations of the device on startup
                                # Device should be configured with u-center and configurations should be saved in flash
                                # Please use the configuration file provided by Lemvos

    # GPIO configuration
    gpio:
      chipname: "gpiochip0"
      line_num: 134
      reset_time: 1

    # Recovery configuration
    recovery:
      cycle_time: 1

    # Watchdog configuration
    watchdog:
      timeout: 3000
      cycle_time: 500

    # Enable u-blox gnss message publishers
    publish:
      all: false
      aid:
        hui: false
      nav:
        relposned: true
        heading: true
      nmea: false
      mon:
        all: true
```

:::warning[Important]
Restart the `automatepro-core-driver` Docker container after changing the configuration.
Configuration changes are applied only after the container restarts.

```bash
docker restart automatepro-core-driver
```
:::


# --- position.mdx ---

---
sidebar_position: 1
description: RTK GNSS positioning hardware, setup, and ROS 2 position output.
---
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import AutomatePro_GNSS_WHEEL_IN from './images/AutomatePro_GNSS_WHEEL_IN.png'
import automatepro_gnss from './images/AutomatePro_GNSS.png'
import automatepro_breakoutboard_wheelspeed from './images/BreakoutBoard_MISC.png'

# RTK Positioning

## Hardware
AutomatePro offers an optional RTK GNSS position sensor, giving it cm-level positional accuracy. This is based on the u-blox F9P and R series devices and includes positional accuracy down to 2.5 cm. If the option is included, there will be a blue FAKRA connector for antenna connection. "GNSS-2" is used for position and "GNSS-1" is used for [heading](./heading.mdx).

![AutomatePro GNSS connector location](images/AutomatePro_GNSS.png)

### GNSS Position Specifications
| Parameter               | Value                                                                                           |
|-------------------------|-------------------------------------------------------------------------------------------------|
| Receiver                | u-blox F9P or F9R ([see Wheel Speed & Direction Input](#wheel-speed--direction-input))                          |
| Connector               | GNSS-2 (FAKRA C - Signal Blue)                                                                                    |
| Supported Constellation | GPS, Galileo, BeiDou, GLONASS                                                             |
| Positional Accuracy     | Down to 1 cm <sup>1</sup>. Using u-blox PointPerfect correction &lt;6 cm                                      |
| Max Navigation Update Rate | Default 1 Hz (can be configured up to 7 Hz)                                                               |
:::info[Info]
<sup>1</sup> Depends on antenna, distance to base station, or correction service. [See the receiver datasheet for more information](https://content.u-blox.com/sites/default/files/ZED-F9P-04B_DataSheet_UBX-21044850.pdf).
:::

### External Antenna
The AutomatePro system is compatible with a variety of multi-band high-precision GNSS antennas that have a FAKRA Type C connector (Signal Blue). More information on antennas used with AutomatePro is available [here](/automatepro/category/antennas).

### Wheel Speed & Direction Input
The AutomatePro provides options for both wheel speed and direction input for applications requiring dead reckoning.
<details>
  <summary> Connection Details </summary>
  |Pin Name & Number | Function | Description  |
|---------|---------|---------|
|WHEEL TIC / PIN 51 | Tachometer input | 5-24VDC input range (with respect to the common GND) |
|WHEEL DIR / PIN 52 | Wheel rotation direction input | 5-24VDC input range (with respect to the common GND) |

  <Tabs>
    <TabItem value="Connector" label="Connector" default>

      <img src={AutomatePro_GNSS_WHEEL_IN} style={{width: '100%'}}/>

  </TabItem>
    <TabItem value="Development Breakout Board" label="Development Breakout Board">

      <img src={automatepro_breakoutboard_wheelspeed} style={{width: '100%'}}/>

    </TabItem>
  </Tabs>
</details>

:::warning[Important]
This is a non-standard option. Contact Lemvos for more information.
:::

## ROS API

#### Node: `automatepro_gnss_position_node`

### Publishers

| Topic | Type | Description |
|-------|------|-------------|
| `/sensor/gnss/position/fix` | [`sensor_msgs/msg/NavSatFix`](https://docs.ros2.org/latest/api/sensor_msgs/msg/NavSatFix.html) | GNSS position data (navigation satellite fix). |
| `/sensor/gnss/position/fix_velocity` | [`geometry_msgs/msg/`<br/>`TwistWithCovarianceStamped`](https://docs.ros2.org/latest/api/geometry_msgs/msg/TwistWithCovarianceStamped.html) | Velocity in local ENU frame. |
| `/sensor/gnss/position/monhw` | [`ublox_msgs/msg/MonHW`](https://github.com/Lemvos/automatepro_gnss_driver/blob/ros2/ublox_msgs/msg/MonHW.msg) | Hardware status of the GNSS position module. |
| `/sensor/gnss/position/monsys` | [`ublox_msgs/msg/MonSYS`](https://github.com/Lemvos/automatepro_gnss_driver/blob/ros2/ublox_msgs/msg/MonSYS.msg) | System monitor status of the GNSS position module. |
| `/sensor/gnss/position/nmea` | [`nmea_msgs/msg/Sentence`](https://docs.ros.org/en/ros2_packages/humble/api/nmea_msgs/interfaces/msg/Sentence.html) | Raw NMEA messages. |
| `/diagnostics` | [`diagnostic_msgs/msg/`<br/>`DiagnosticArray`](https://docs.ros2.org/latest/api/diagnostic_msgs/msg/DiagnosticArray.html) | GNSS position ROS driver diagnostics. |

### Subscribers
|Topic | Type | Description  |
|---------|---------|---------|
| `/sensor/gnss/correction` | [`rtcm_msgs/msg/Message`](https://github.com/tilk/rtcm_msgs/blob/master/msg/Message.msg) | RTCM or SPARTN Correction data |

### Parameters
| Parameter | Type | Packaged default | Runtime R/W | Description |
|-----------|------|------------------|-------------|-------------|
| `device` | string | `/dev/serial/by-id/usb-u-blox_AG_-_www.u-blox.com_u-blox_GNSS_receiver_F9P-if00` | `read-only` | Serial port for the F9P receiver. |
| `frame_id` | string | `gnss_position` | `read-only` | Frame ID for published messages. |
| `config_on_startup` | bool | `false` | `read-only` | Leaves receiver flash configuration unchanged at startup. |
| `gpio.chipname` | string | `gpiochip0` | `read-only` | GPIO chip used for receiver reset recovery. |
| `gpio.line_num` | int | `134` | `read-only` | GPIO line used for receiver reset recovery. |
| `gpio.reset_time` | int | `1` | `read-only` | Reset pulse duration in seconds. |
| `recovery.cycle_time` | int | `1` | `read-only` | Delay between recovery attempts in seconds. |
| `watchdog.timeout` | int | `3000` | `read-only` | Watchdog timeout in milliseconds. |
| `watchdog.cycle_time` | int | `500` | `read-only` | Watchdog check interval in milliseconds. |
| `publish.all` | bool | `false` | `read-only` | Enables all publisher groups when true. |
| `publish.aid.hui` | bool | `false` | `read-only` | Enables AID-HUI message publishing. |
| `publish.nav.posllh` | bool | `false` | `read-only` | Enables NAV-POSLLH message publishing. |
| `publish.nav.posecef` | bool | `false` | `read-only` | Enables NAV-POSECEF message publishing. |
| `publish.nav.relposned` | bool | `false` | `read-only` | Keeps relative-position heading messages disabled for the position receiver. |
| `publish.nmea` | bool | `true` | `read-only` | Enables NMEA sentence publishing. |
| `publish.mon.all` | bool | `true` | `read-only` | Enables monitor-message publishers, including hardware status. |
| `debug` | int | `0` | `read-only` | Debug verbosity; range `0-4`. |

## Configuration

**Driver**: [automatepro_gnss_driver](http://github.com/Lemvos/automatepro_gnss_driver)
**Container**: [automatepro-core-driver](https://hub.docker.com/repository/docker/lemvos/automatepro_core_driver/general)

:::info
Runtime configuration files are in the `/opt/automatepro/config/ros` directory.
Packaged defaults are installed in `/opt/automatepro/defconfig/ros`.
The runtime files are mounted into the Docker container and used by the ROS driver.
Environment variables can be set in the `/opt/automatepro/.env` file.
:::

The runtime configuration file is:

```bash
/opt/automatepro/config/ros/gnss_position_params.yaml
```

The packaged default source is:

```bash
/opt/automatepro/defconfig/ros/gnss_position_params.yaml
```

### ROS Configuration
```yaml
# Configuration Settings for F9P device in Moving Base Mode
automatepro_gnss_position_node:
  ros__parameters:
    debug: 0                    # Range 0-4 (0 means no debug statements will print)

    device: /dev/serial/by-id/usb-u-blox_AG_-_www.u-blox.com_u-blox_GNSS_receiver_F9P-if00 #/dev/ttyACM1
    frame_id: gnss_position

    config_on_startup: false    # If false, the node will not override configurations of the device on startup
                                # Device should be configured with u-center and configurations should be saved in flash
                                # Please use the configuration file provided by Lemvos

    # GPIO configuration
    gpio:
      chipname: "gpiochip0"
      line_num: 134
      reset_time: 1

    # Recovery configuration
    recovery:
      cycle_time: 1

    # Watchdog configuration
    watchdog:
      timeout: 3000
      cycle_time: 500

    # Enable u-blox gnss message publishers
    publish:
      all: false
      aid:
        hui: false
      nav:
        all: false
        posllh: false
        posecef: false
        relposned: false
      nmea: true
      mon:
        all: true
```

:::warning[Important]
Restart the `automatepro-core-driver` Docker container after changing the configuration.
Configuration changes are applied only after the container restarts.

```bash
docker restart automatepro-core-driver
```
:::

### Device Configuration
This section describes the configuration of the GNSS device. To explore the capabilities of the GNSS device,
refer to the [u-blox documentation](https://www.u-blox.com/en/product-resources?legacy=Current) for more details.

Check the [configuration](./configuration.mdx) section for more details.

## Example

Example code snippets read GNSS position data from the `/sensor/gnss/position/fix` topic.
The ROS package is available [here](https://github.com/Lemvos/automatepro_tutorials).

<Tabs>
  <TabItem value="Python" label="Python" default>
    ```python
    import rclpy
    from rclpy.node import Node
    from sensor_msgs.msg import NavSatFix

    class GNSSPositionSubscriber(Node):

        def __init__(self):
            super().__init__('gnss_position_subscriber')
            self.subscription = self.create_subscription(
                NavSatFix,
                '/sensor/gnss/position/fix',
                self.listener_callback,
                10)

        def listener_callback(self, msg):
            self.get_logger().info('Latitude: %f' % msg.latitude)
            self.get_logger().info('Longitude: %f' % msg.longitude)
            self.get_logger().info('Altitude: %f' % msg.altitude)

    def main(args=None):
        rclpy.init(args=args)
        node = GNSSPositionSubscriber()
        rclpy.spin(node)
        node.destroy_node()
        rclpy.shutdown()

    if __name__ == '__main__':
        main()
    ```
    Run the node using the following command:
    ```bash
    ros2 run automatepro_python_tutorials gnss_position_node
    ```

  </TabItem>
  <TabItem value="C++" label="C++">
    ```cpp
    #include <rclcpp/rclcpp.hpp>
    #include <sensor_msgs/msg/NavSatFix.hpp>

    class GNSSPositionSubscriber : public rclcpp::Node
    {
    public:
        GNSSPositionSubscriber()
            : Node("gnss_position_subscriber")
        {
            subscription_ = this->create_subscription<sensor_msgs::msg::NavSatFix>(
                "/sensor/gnss/position/fix",
                10,
                std::bind(&GNSSPositionSubscriber::listener_callback, this, std::placeholders::_1));
        }

    private:
        void listener_callback(const sensor_msgs::msg::NavSatFix::SharedPtr msg)
        {
            RCLCPP_INFO(this->get_logger(), "Latitude: %f", msg->latitude);
            RCLCPP_INFO(this->get_logger(), "Longitude: %f", msg->longitude);
            RCLCPP_INFO(this->get_logger(), "Altitude: %f", msg->altitude);
        }
        rclcpp::Subscription<sensor_msgs::msg::NavSatFix>::SharedPtr subscription_;
    };

    int main(int argc, char *argv[])
    {
        rclcpp::init(argc, argv);
        auto node = std::make_shared<GNSSPositionSubscriber>();
        rclcpp::spin(node);
        rclcpp::shutdown();
        return 0;
    }
    ```

    :::warning[Important]
    Install the missing dependencies before running the node
    :::

    Run the node using the following command:
    ```bash
    ros2 run automatepro_cpp_tutorials gnss_position_node
    ```
  </TabItem>
</Tabs>


# --- localization.mdx ---

---
description: Fuse GNSS and IMU data into filtered odometry with robot_localization.
---
# Localization

AutomatePro localization is provided by the [`automatepro_localization`](https://github.com/Lemvos/automatepro_localization) ROS 2 package. It converts GNSS fixes into a local planar pose and can launch a `robot_localization` EKF that fuses GNSS pose, GNSS velocity, and IMU data into filtered odometry.

Use this page to run or verify the localization pipeline on AutomatePro.

## Pipeline

The launch file starts the localization components from one YAML profile:

| Component | Purpose |
|-----------|---------|
| `gps_to_pose_node` | Converts `sensor_msgs/msg/NavSatFix` into `geometry_msgs/msg/PoseWithCovarianceStamped` using GeographicLib UTM conversion. |
| `ekf_node` | Runs `robot_localization/ekf_node` and remaps `/odometry/filtered` to `/localization/odometry/filtered`. |

## Configuration

The package loads its parameters from a YAML profile. AutomatePro uses the default profile, `config/config_lm450.yaml`, which feeds the EKF from the AutomatePro GNSS and IMU topics:

| Input | Topic |
|-------|-------|
| GNSS position | `/sensor/gnss/position/fix` |
| GNSS velocity | `/sensor/gnss/position/fix_velocity` |
| GNSS heading | `/sensor/gnss/heading/true_heading` |
| IMU | `/sensor/imu/data` |

## Launch

Run the default profile:

```bash
source /opt/ros/humble/setup.bash
ros2 launch automatepro_localization localization.launch.py
```

Common launch arguments:

| Argument | Default | Description |
|----------|---------|-------------|
| `config_file` | package `config/config_lm450.yaml` | YAML file loaded by all localization nodes. |
| `enable_gps_to_pose` | `True` | Starts `gps_to_pose_node`. |
| `enable_ekf` | `True` | Starts `robot_localization/ekf_node`. |
| `log_level` | `info` | ROS log level for launched nodes. |

## Verify Localization

Check that the expected nodes are present:

```bash
ros2 node list | grep -E 'gps_to_pose|ekf_node'
```

Verify the input topics first:

```bash
ros2 topic echo /sensor/gnss/position/fix --once
ros2 topic echo /sensor/imu/data --once --qos-reliability best_effort
```

Then verify localization outputs:

```bash
ros2 topic echo /localization/gps_pose --once
ros2 topic echo /localization/odometry/filtered --once
```

You should see a `PoseWithCovarianceStamped` message on `/localization/gps_pose` after the first valid GNSS fix. The EKF publishes `nav_msgs/msg/Odometry` on `/localization/odometry/filtered` when its configured inputs are available.

## Troubleshooting

### `/localization/gps_pose` Is Missing

Confirm that GNSS is publishing a valid fix:

```bash
ros2 topic echo /sensor/gnss/position/fix --once
```

`gps_to_pose_node` initializes its local datum only after a valid `NavSatFix`. If the GNSS driver is disabled or has no fix, localization output will not appear.

### Launch Fails Before Nodes Start

Make sure the required runtime dependencies are installed:

```bash
sudo apt install libgeographic-dev ros-humble-robot-localization
```

`gps_to_pose_node` uses GeographicLib for UTM conversion, and the filtered odometry node comes from `robot_localization`.

<details>
  <summary>Developer Notes: EKF inputs</summary>

On AutomatePro the EKF takes velocity from `/sensor/gnss/position/fix_velocity` and IMU data from `/sensor/imu/data`. The package-local `gps_velocity_estimator_node` is disabled by default (`gps_velocity_estimator_node.ros__parameters.enable` is `false`). The EKF runs in `two_d_mode` for planar navigation.

</details>
