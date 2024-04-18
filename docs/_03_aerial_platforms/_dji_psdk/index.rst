.. _aerial_platform_dji_PSDK:

==================
DJI Payload SDK
==================

.. contents:: Table of Contents
   :depth: 3
   :local:


.. _aerial_platform_dji_psdk_introduction:

------------
Introduction
------------

`DJI Payload SDK <https://github.com/dji-sdk/Payload-SDK>`_ is the lastest api for DJI Drones. 

Check DJI Drones using Payload-SDK and see compatibility information of Payload-SDK at: `DJI Payload SDK documentation <https://developer.dji.com/doc/payload-sdk-tutorial/en/>`_.

.. figure:: ../_dji/resources/DJI_M300.jpg
   :scale: 15
   :class: with-shadow

.. _aerial_platform_dji_psdk_installation:

------------
Installation
------------

Before installing the software you will need to have the right hardware devices and connections, 
follow the instructions at: `Aircraft Hardware Connection <https://developer.dji.com/doc/payload-sdk-tutorial/en/quick-start/device-connect.html>`

.. warning:: This package and some of its dependencies are experimental, try anything in simulation before go to a real scenario.

.. _aerial_platform_dji_psdk_installation_package:

Install platform package
========================

* For binary installation, install by running:

.. warning:: This package is not available for binary installation yet.

* For source installation: 

  1. Install Aerostack2 `Link <../../_00_getting_started/index.html>`_.
  2. Install dependencies (See below).

.. _aerial_platform_dji_psdk_dependencies_install:

Install dependencies
^^^^^^^^^^^^^^^^^^^^^

Aerostack PSDK platform depends on the PSDK library and a ROS2 node wrapper, you could either
try to install PSDK library and then the wrapper or skip the PSDK library installation and follow
the instructions to install directly the wrapper. The main difference is that you have to fill up
the configuration files to compile the examples from PSDK but this information is already used 
by means of ROS2 parameters in the wrapper in a yaml file.

Install PSDK from DJI
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This package depends on DJI - PSDK library headers and binaries available at:
`DJI Payload SDK library <https://github.com/dji-sdk/Payload-SDK>`_. You should:

  1. Get an application license.
  2. Clone or download the repository.
  3. Fill up PSDK license information at file `dji_sdk_app_info.h` in the corresponding path depending on your platform.  
  4. Build the example.
  5. Setup the simulation.
  6. Run it.

.. attention:: Please note the dependencies of the PSDK itself. 

You should have got the example running before continue. But, you might find that building
the wrapper is easier due to some enhancements in the CMakeLists.txt to build the software. 

Install PSDK wrapper 
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

`Unmanned Life <https://unmanned.life/>`_ wrapper for PSDK is a ROS2 node that forwards ROS2 communication to/from PSDK API. 

The original sources are at `PSDK wrapper <https://github.com/umdlife/psdk_ros2>`_, but
we need some minor differences, so we use a fork at: `PSDK Wrapper fork<https://github.com/RPS98/psdk_ros2>`_

  1. Get the sources and installation instructions.
  2. Check out `PSDK Wrapper fork RPS98_devel branch<https://github.com/RPS98/psdk_ros2/tree/RPS98_devel>`_.  
  2. Fill up PSDK license at yaml file. 
  3. Set up simulation.
  4. Run the node. 

.. warning:: When running the node, mind that it is a managed node, you have to activate it. 
  Futhermore, after activating you have to enter TODO mode to control the drone.

Install Aerostack2  
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. attention:: Since PSDK as2 platform includes gimbal control, it needs aerostack2 v.1.0.9 or later. 

Install the platform package
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Download the sources from: `AS2 PSDK platform <https://github.com/aerostack2/as2_platform_dji_psdk>`_

1. Build the node.
2. Run it.

The as2 platform node will launch the PSDK wrapper, activate it and set the mode to control the drone. 

.. _aerial_platform_dji_psdk_common_interface:

---------------------------
Aerostack2 Common Interface
---------------------------

For more details about platform control modes and sensors, see :ref:`Aerostack2 Aerial Platform Concepts <as2_concepts_aerial_platform>`.

.. _aerial_platform_dji_psdk_as2_common_interface_control_modes:

Control Modes
=============

These are supported control modes:

.. list-table:: Control Modes DJI OSDK Platform
   :widths: 50 50 50
   :header-rows: 1

   * - Control Mode
     - Yaw Mode
     - Reference Frame
   * - Hover
     - None
     - None
   * - Speed
     - Speed
     - ENU

.. _aerial_platform_dji_psdk_as2_common_interface_sensors:

Sensors
=======

Since the wrapper is already publishing the sensor measurements as ROS2 topics, all the 
supported sensors at the PSDK wrapper are also supported. Besides, some other sensor values
are generated to provide compatibility with the rest of AS2 nodes.

These are supported sensors:
  
.. list-table:: Sensors DJI PSDK Platform
   :widths: 50 50 50
   :header-rows: 1

   * - Sensor
     - Topic
     - Type
   * - Odometry
     - sensor_measurements/odom
     - nav_msgs/Odometry
   * - IMU
     - sensor_measurements/imu
     - sensor_msgs/Imu
   * - Battery
     - sensor_measurements/battery
     - sensor_msgs/BatteryState
   * - GPS
     - sensor_measurements/gps
     - sensor_msgs/NavSatFix



.. _aerial_platform_dji_psdk_platform_launch:

---------------
Platform Launch
---------------

Aerostack2 DJI OSDK platform provides a launch file, which parameters are:

.. list-table:: DJI OSDK Platform Parameters
   :widths: 50 50 50
   :header-rows: 1

   * - Parameter
     - Type
     - Description
   * - namespace
     - string
     - Namespace of the platform, also named as drone id.
   * - config
     - string
     - | Optional. File yaml path with the config file that set: 
       | command frequency in Hz (cmd_freq), info frequency in Hz (info_freq)  and
       | file path with the control modes configuration (control_modes_file). Default the file in the package.
   * - dji_app_config
     - string
     - | Text file with the DJI app configuration. Must have the following format: 
       | app_id: <your_app_id>
       | app_key: <your_app_key>
       | device: /dev/ttyUSB0
       | baudrate: 921600
       | acm_port: /dev/ttyACM0
   * - simulation_mode
     - bool
     - Optional, default false. Use for simulation with `DJI Assistant 2 <https://www.dji.com/es/downloads/softwares/assistant-dji-2-for-matrice>`_.

Example of launch command:

.. code-block:: bash

  ros2 launch as2_platform_dji_osdk as2_platform_dji_osdk_launch.py namespace:=drone1 dji_app_config:=UserConfig.txt

