Frequently Asked Questions
==========================

.. seo::
    :description: Frequently asked questions in ESPHome.
    :image: question_answer.png

Tips for using ESPHome
----------------------

1. ESPHome supports (most of) `Home Assistant's YAML configuration directives
   <https://www.home-assistant.io/docs/configuration/splitting_configuration/>`__ like
   ``!include``, ``!secret``. So you can store all your secret WiFi passwords and so on
   in a file called ``secrets.yaml`` within the directory where the configuration file is.

   For even more configuration templating, take a look at :ref:`config-substitutions`.

2. If you want to see how ESPHome interprets your configuration, run

   .. code-block:: bash

       esphome livingroom.yaml config

3. To view the logs from your node without uploading, run

   .. code-block:: bash

       esphome livingroom.yaml logs

4. You can always find the source ESPHome generates under ``<NODE_NAME>/src/main.cpp``. It's even
   possible to edit anything outside of the ``AUTO GENERATED CODE BEGIN/END`` lines for creating
   :doc:`custom sensors </components/sensor/custom>`.

5. You can view the full command line interface options here: :doc:`/guides/cli`

6. Use :ref:`substitutions <config-substitutions>` to reduce repetition in your configuration files.

.. |secret| replace:: ``!secret``
.. _secret: https://www.home-assistant.io/docs/configuration/secrets/
.. |include| replace:: ``!include``
.. _include: https://www.home-assistant.io/docs/configuration/splitting_configuration/

.. _esphome-flasher:

I can't get flashing over USB to work.
--------------------------------------

ESPHome depends on the operating system the tool is running on to recognize
the ESP. This can sometimes fail. Common causes are that you did not install
the drivers (see note below) or you are trying to upload from a Docker container
and did not mount the ESP device into your container using ``--device=/dev/ttyUSB0``.

Starting with ESPHome 1.9.0, the ESPHome suite provides
`esphome-flasher <https://github.com/esphome/esphome-flasher>`__, a tool to flash ESPs over USB.

First, you need to get the firmware file to flash. For Hass.io add-on based installs you can
use the ``COMPILE`` button (click the overflow icon with the three dots) and then press
``Download Binary``. For command line based installs you can access the file under
``<CONFIG_DIR>/<NODE_NAME>/.pioenvs/<NODE_NAME>/firmware.bin``.

Then, install esphome-flasher by going to the `releases page <https://github.com/esphome/esphome-flasher/releases>`__
and downloading one of the pre-compiled binaries. Open up the application and select the serial port
you want to flash to (on windows you can use the "device manager" to check if it's the right one).

.. figure:: images/esphomeflasher-ui.png
    :align: center
    :width: 80%

Select the firmware binary and finally press "Flash ESP".

.. note::

    If the serial port is not showing up, you might not have the required drivers installed.
    ESPs usually ship with one of these two UART chips:

     * CP2102 (square chip): `driver <https://www.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers>`__
     * CH341: `driver <https://github.com/nodemcu/nodemcu-devkit/tree/master/Drivers>`__

.. note::

    If you're just seeing ``Connecting....____....`` on the screen and the flashing fails, that might
    be a sign that the ESP is defect or cannot be programmed. Please double check the UART wires
    are connected correctly if flashing using a USB to UART bridge. For some devices you need to
    keep pressing the BOOT button until flashing has begun (ie. Geekcreit DOIT ESP32 DEVKIT V1).

Help! Something's not working!1!
--------------------------------

That's no good. Here are some steps that resolve some problems:

-  **If you're having WiFi problems**: See :ref:`wifi-problems`.
-  Enable verbose logs in the logger: section.
-  **Still an error?** Please file a bug report over in the `ESPHome issue tracker <https://github.com/esphome/issues>`__.
   I will take a look at it as soon as I can. Thanks!

.. _faq-bug_report:

How to submit an issue report
-----------------------------

First of all, thank you very much to everybody submitting issue reports! While I try to test ESPHome/yaml as much as
I can using my own hardware, I don't own every single device type and mostly only do tests with my own home automation
system. When doing some changes in the core, it can quickly happen that something somewhere breaks. Issue reports are a
great way for me to track and (hopefully) fix issues, so thank you!

For me to fix the issue the quickest, there are some things that would be really helpful:

1.  **Just writing "X doesn't work" or "X gives bug" is not helpful!!!** Seriously, how do you expect
    help given just that information?
2.  A snippet of the code/configuration file used is always great to reproduce this issue.
    Please read `How to create a Minimal, Complete, and Verifiable example <https://stackoverflow.com/help/mcve>`__.
3.  If it's an i2c or hardware communication issue please also try setting the
    :ref:`log level <logger-log_levels>` to ``VERY_VERBOSE`` as it provides helpful information
    about what is going on.
4.  Please also include what you've already tried and didn't work as that can help us track down the issue.

You can find the issue tracker here https://github.com/esphome/issues

How do I update to the latest version?
--------------------------------------

It's simple. Run:

.. code-block:: bash

    pip2 install -U esphome
    # From docker:
    docker pull esphome/esphome:latest

And in Hass.io, there's a simple UPDATE button when there's an update available as with all add-ons

.. _faq-beta:

How do I update to the latest beta release?
-------------------------------------------

ESPHome has a beta release cycle so that new releases can easily be tested before
the changes are deployed to the stable channel. You can help test esphome (and use new features)
by installing the esphome beta:

.. code-block:: bash

    # For pip-based installs
    pip2 install --pre -U esphome

    # For docker-based installs
    docker run [...] -it esphome/esphome:beta livingroom.yaml run

And for Hass.io, you will see a "ESPHome Beta" Add-On for the beta channel.

The beta docs can be viewed at `beta.esphome.io <https://beta.esphome.io>`__

How do I use the latest bleeding edge version?
----------------------------------------------

First, a fair warning that the latest bleeding edge version is not always stable and might have issues.
If you find some, please do however report them if you have time :)

To install the dev version of ESPHome:

- In Hass.io: In the ESPHome add-on repository there's also a second add-on called ``ESPHome Dev``.
  Install that and stop the stable version (both can't run at the same time without port collisions).
- From ``pip``: Run ``pip install https://github.com/esphome/esphome/archive/dev.zip``
- From docker, you need to build the docker image yourself (automated dev builds are not possible
  due to docker hubs limited build quota)

  .. code-block:: bash

      git clone https://github.com/esphome/esphome.git
      cd esphome
      docker build -t esphome-dev -f docker/Dockerfile .
      docker run [...] -it esphome-dev livingroom.yaml compile

      # Update image and rebuild
      git pull
      docker build -t esphome-dev -f docker/Dockerfile .

The latest dev docs are here: `next.esphome.io <https://next.esphome.io/>`__

Does ESPHome support [this device/feature]?
-------------------------------------------

If it's not in :doc:`the docs </index>`, it's probably sadly not
supported. However, I'm always trying to add support for new features, so feel free to create a feature
request in the `ESPHome feature request tracker <https://github.com/esphome/feature-requests>`__. Thanks!

I have a question... How can I contact you?
-------------------------------------------

Sure! I'd be happy to help :) You can contact me here:

-  `Discord <https://discord.gg/KhAMKrd>`__
-  `Home Assistant Community Forums <https://community.home-assistant.io/c/third-party/esphome>`__
-  ESPHome `issue <https://github.com/esphome/issues>`__ and
   `feature request <https://github.com/esphome/feature-requests>`__ issue trackers. Preferably only for issues and
   feature requests.
-  Alternatively, also under contact (at) esphome.io (NO SUPPORT!)

.. _wifi-problems:

My node keeps reconnecting randomly
-----------------------------------

Jep, that's a known issue. However, it seems to be very low-level and I don't really know
how to solve it. I'm working on possible workarounds for the issue but currently I do
not have a real solution.

Some steps that can help with the issue:

- If you're using a hidden WiFi network, make sure to enable ``fast_connect`` mode in the WiFi
  configuration (also sometimes helps with non-hidden networks)
- Give your ESP a :ref:`static IP <wifi-manual_ip>`.
- Set the ``power_save_mode`` to ``light`` in the ``wifi:`` config (only helps in some cases,
  in other it can make things works). See :ref:`wifi-power_save_mode`.
- The issue seems to be happen with cheap boards more frequently. Especially the "cheap" NodeMCU
  boards from eBay sometimes have quite bad antennas.

Docker Reference
----------------

Install versions:

.. code-block:: bash

    # Stable Release
    docker pull esphome/esphome
    # Beta
    docker pull esphome/esphome:beta
    # Dev version
    docker pull esphome/esphome:dev

Command reference:

.. code-block:: bash

    # Start a new file wizard for file livingroom.yaml
    docker run --rm -v "${PWD}":/config -it esphome/esphome livingroom.yaml wizard

    # Compile and upload livingroom.yaml
    docker run --rm -v "${PWD}":/config -it esphome/esphome livingroom.yaml run

    # View logs
    docker run --rm -v "${PWD}":/config -it esphome/esphome livingroom.yaml logs

    # Map /dev/ttyUSB0 into container
    docker run --rm -v "${PWD}":/config --device=/dev/ttyUSB0 -it esphome/esphome ...

    # Start dashboard on port 6052
    docker run --rm -v "${PWD}":/config --net=host -it esphome/esphome

And a docker compose file looks like this:

.. code-block:: yaml

    version: '3'

    services:
      esphome:
        image: esphome/esphome
        volumes:
          - ./:/config:rw
          # Use local time for logging timestamps
          - /etc/localtime:/etc/localtime:ro
        network_mode: host
        restart: always

.. note::

    ESPHome uses mDNS to show online/offline state in the dashboard view. So for that feature
    to work you need to enable host networking mode

    mDNS might not work if your Home Assistant server and your ESPHome nodes are on different subnets.
    If your router supports Avahi, you are able to get mDNS working over different subnets.

    Just follow the next steps:

    1. Enable Avahi on both subnets.
    2. Enable UDP traffic from ESPHome node's subnet to 224.0.0.251/32 on port 5353.

See Also
--------

- :doc:`ESPHome index </index>`
- :doc:`contributing`
- :ghedit:`Edit`
