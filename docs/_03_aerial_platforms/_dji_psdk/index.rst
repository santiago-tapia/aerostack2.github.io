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
some minor differences are required to use the wrapper with AS2, so use the fork at: `PSDK Wrapper fork<https://github.com/RPS98/psdk_ros2>`_

  1. Get the sources and installation instructions.
  2. Check out `PSDK Wrapper fork RPS98_devel branch<https://github.com/RPS98/psdk_ros2/tree/RPS98_devel>`_.  
  2. Fill up PSDK license at yaml file (`psdk_wrapper/cfg/psdk_params.yaml`). 
  3. Set up simulation.
  4. Run the node. 

.. warning:: When running the node, mind that it is a managed node, you have to activate it. 
  Futhermore, after activating you have to get 'Joystick Control Authority' mode to control the drone by 
  a request to the "psdk_ros2/obtain_ctrl_authority" service. 

Install Aerostack2  
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. attention:: Since PSDK as2 platform includes gimbal control, it needs aerostack2 v.1.0.9 or later. 

Install the platform package
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Download the sources from: `AS2 PSDK platform <https://github.com/aerostack2/as2_platform_dji_psdk>`_

1. Build the node.
2. Run it.

The as2 platform node will launch the PSDK wrapper, activate it and obtain the control authority to 
navigate the drone. 

.. _aerial_platform_dji_psdk_common_interface:

---------------------------
Aerostack2 Common Interface
---------------------------

For more details about platform control modes and sensors, see :ref:`Aerostack2 Aerial Platform Concepts <as2_concepts_aerial_platform>`.

.. _aerial_platform_dji_psdk_as2_common_interface_control_modes:

Control Modes
=============

These are supported control modes:

.. list-table:: Control Modes DJI PSDK Platform
   :widths: 50 50 50
   :header-rows: 1

   * - Control Mode
     - Yaw Mode
     - Reference Frame
   * - Hover
     - None
     - None
   * - Speed
     - Yaw rate
     - ENU

.. _aerial_platform_dji_psdk_as2_common_interface_sensors:

Sensors
=======

Since the wrapper is already publishing the sensor measurements as ROS2 topics, all the 
supported sensors at the PSDK wrapper are also supported. Besides, some other sensor values
are generated and published to provide compatibility with the rest of AS2 nodes.

These are supported sensors:
  
.. list-table:: Sensors DJI PSDK Platform
   :widths: 50 50 50
   :header-rows: 1

   * - Sensor
     - Topic
     - Type
   * - IMU
     - sensor_measurements/imu
     - sensor_msgs/Imu
   * - GPS
     - sensor_measurements/gps
     - sensor_msgs/NavSatFix
   * - Camera
     - sensor_measurements/main_camera/image_raw
     - sensor_msgs/Image
   * - Gimbal rotation
     - sensor_measurements/gimbal/attitude
     - geometry_msgs/QuaternionStamped

.. _aerial_platform_dji_psdk_platform_launch:

---------------
Platform Launch
---------------

Aerostack2 DJI PSDK platform provides an adapted launch file for running the wrapper node, 
which parameters are:

.. list-table:: PSDK Wrapper Parameters
   :widths: 50 50 50
   :header-rows: 1

   * - Parameter
     - Type
     - Description
   * - namespace
     - string
     - Namespace of the wrapper node.
   * - psdk_params_file_path
     - string
     - | default: as2_platform_dji_psdk/config/psdk_params.yaml
       | The path to a yaml file that holds the params for the PSDK api (license and so on).
   * - link_config_file_path
     - string
     - | default: as2_platform_dji_psdk/config/link_config.json
       | The path to a json file that holds the params for the PSDK api link information: uart device names,
       | networking and usb bulk config. 
   * - hms_return_codes_path
     - string
     - | default: as2_platform_dji_psdk/config/hms_2023_08_22.json
       | The path to a json file that holds the internationalization of logging messages. 
   * - tf_frame_prefix
     - string
     - Default: ''. The TF frame prefix.


Aerostack2 DJI PSDK platform provides another launch file for running the platform node, 
which parameters are:

.. list-table:: DJI PSDK Platform Parameters
   :widths: 50 50 50
   :header-rows: 1

   * - Parameter
     - Type
     - Description
   * - namespace
     - string
     - Namespace of the platform, also named as drone id.
   * - log_level
     - string
     - default: 'info', log severity level.
   * - control_modes_file
     - string
     - | Default: as2_platform_dji_psdk/config/control_modes.yaml
       | The path to a yaml file that holds the platform control modes.
   * - config_file
     - string
     - | Default: as2_platform_dji_psdk/config/platform_config_file.yaml
       | The path to a yaml file that holds the platform node parameters: cmd_freq, info_freq, enable_camera, 
       | enable_gimbal, gimbal_base_frame_id and tf_timeout_threshold.

Example of launch commands:

.. code-block:: bash

  ros2 launch as2_platform_dji_psdk as2_platform_dji_psdk.launch.py namespace:=drone1
  ros2 launch as2_platform_dji_psdk wrapper.launch.py psdk_params_file_path:=./my_psdk_file_pars.yaml
