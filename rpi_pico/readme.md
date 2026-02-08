# converter/thinkbook14_g5plus_arp/rpi_pico

![converter/thinkbook14_g5plus_arp/rpi_pico]

This is a converter PCB for Lenovo Thinkpad keyboards from a wide range of models (T60, T61, X220, ...), based on the Raspberry Pi Pico. The QMK implementation here is specific to the converter in the link below. Similar PCB designs to convert a Thinkpad keyboard to USB exist, but they use a different microcontroller and wiring of the keyboard connector with the MCU.

* Keyboard Maintainer: [KikuchiMakoto](https://github.com/KikuchiMakoto)
* Hardware Supported: The linked converter PCB with keyboards from various Thinkpad models
* Hardware Availability: [Custom PCB](https://github.com/KikuchiMakoto/Lenovo_keyboard)

Make example for this keyboard (after setting up your build environment):

    make converter/thinkbook14_g5plus_arp/rpi_pico:default

Flashing example for this keyboard:

    make converter/thinkbook14_g5plus_arp/rpi_pico:default:flash

See the [build environment setup](https://docs.qmk.fm/#/getting_started_build_tools) and the [make instructions](https://docs.qmk.fm/#/getting_started_make_guide) for more information. Brand new to QMK? Start with our [Complete Newbs Guide](https://docs.qmk.fm/#/newbs).
