## yotta Target Description using ARMCC to compile for K64F Devkit

Use this target description to compile [mbed
OS](http://www.mbed.com/en/development/software/mbed-os/) for the [FRDM-K64F
development
board](http://www.mbed.com/en/development/hardware/boards/freescale/frdm_k64f/)
using the ARMCC v5 compiler.

<img src="https://mbed-media.s3.amazonaws.com/frdm-k64f.jpg" width="400">

This target description derives from the generic
[kinetis-k64-armcc](https://github.com/ARMmbed/target-kinetis-k64-armcc) target
description, which provides most of the information about how to compile for
the Mk64Fn1M0Vll12 chip used on this board.

If you are creating your own board based on the Mk64Fn1M0Vll12 chip, you should
copy this target description to use as a starting point, updating the pin
definitions in target.json as necessary.
