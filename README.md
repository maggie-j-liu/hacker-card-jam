# Make your own PCB Hacker Card

[intro]

## Schematic

The first step is creating a schematic for your PCB design. A schematic shows the components that will be used in the circuit, as well as how each of these components connect to each other.

For our card, we'll be using

- A [NT3H2111W0FHKH](https://jlcpcb.com/partdetail/NxpSemicon-NT3H2111W0FHKH/C710403) NFC Chip: this will be the main component of our card -- it handles the NFC functionality as well as harvesting energy from the phone to light up the LED
- An ~2V LED. I'm using [C2296](https://jlcpcb.com/partdetail/Hubei_KentoElec-17_21SUYCTR8/C2296), but feel free to pick a different color (note: this part may also be called 17-21SUYC/TR8)
- A [47Ω resistor](https://jlcpcb.com/partdetail/23909-0603WAF470JT5E/C23182)
- A [220nF capacitor](https://jlcpcb.com/partdetail/21832-CL10B224KA8NNNC/C21120)

First, let's open up [EasyEDA](https://easyeda.com). This is a browser based PCB designer, so all you need is an account.

Once you've logged in, select `File` > `New` > `Project` to create a new project.
![](/photos/new-project.png)

This will open up the schematic editor. Some important parts:

- `Library` on the left toolbar: This is where we will search for components and import them.
- `Wiring Tools` panel: We'll be using the wire tool to connect the components together.

![](/photos/schematic-unannotated.png)

## Adding components to the schematic

Click the `Library` button to open the parts picker. Search for a part (the part number works best), and make sure `JLCPCB Assembled` is selected. This will make sure we're choosing parts from JLCPCB's parts library, so that JLCPCB can assemble the boards when we order them.

![](/photos/searching-for-parts-unannotated.png)

Select the part, then hit the `Place` button to add it to your schematic. Repeat for all 4 of the parts listed above.

![](/photos/parts-on-schematic-unannotated.png)

### Adding the antenna

The NFC chip needs an antenna to work. We'll be using a PCB trace as the antenna, so we'll need to add that to our schematic.

Note: this is not an actual part, so it doesn't need to be assembled onto the board -- it's formed by the copper traces on the PCB.

To add the antenna, search for `25X48MM_NFC_ANTENNA` in the Library, and instead of `JLCPCB Assembled`, select `User Contributed`. Select one of the options (check the preview to make sure it looks similar to the one I used here, but most of them should work), and place it on your schematic.

![](/photos/selecting-antenna-unannotated.png)

## Wiring up the components

Now that all of our components are placed, let's connect them together! We'll use the `Wire` tool to indicated an electrical connection between the different parts -- this is similar to how you would connect wires on a breadboard. On the actual PCB, copper traces will go where we added wires, to form those electrical connections.

We'll reference the NFC Chip's Datasheet, specifically [this explanation](https://www.nxp.com/docs/en/data-sheet/NT3H2111_2211.pdf#page=38) for creating an energy harvesting circuit.

The datasheet says

> A complete total connected capacitor in the range of typically 150 nF up to 220 nF
> maximum shall be connected between VOUT and GND

So we'll connect the capacitor between `VOUT` and GND (the `VSS` pin).

![](/videos/wire-tool.mov)

- The pins `LA` and `LB` are for the antenna, so we'll wire up the antenna between those two pins.
- > If NTAG I2C also powers the I2C bus, then VCC must be connected to VOUT

  Since the NFC chip's energy harvesting _will_ be powering the chip itself, we'll connect `VCC` to `VOUT`.

- Finally, to complete the circuit and light up the LED, we'll connect the LED and resistor between `VOUT` and GND (the `VSS` pin).

Here's what your schematic should look like when completed!

![](/photos/completed-schematic.png)
