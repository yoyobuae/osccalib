# Simple OSC calibration tool

## Content
Very simple tool for calibrating the min/max values for OSC float values sent by some app.

## Usage

Run `osccalib`, input the UDP port where it should listen for OSC packets and click on `Start Server` button.

All the float values received should pop-up in the `OSC values` window in many rows. Each row shows the min value observed so far, the current value and the max value observed so far (in that order).

You can use this with OSC apps like Project Babble to figure out the min/max values each OSC value can reach.
