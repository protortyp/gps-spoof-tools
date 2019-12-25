# GPS Signal Generation and Transfer

This is my personal collection of scripts to generate gps signals and further send them using the [HackRF One](https://greatscottgadgets.com/hackrf/).

I used these scripts in the scope of university projects and for security testing on cars for a large OEM. Use at your own risk.

---

## Preparation

The scripts use the [gps-sdr-sim](https://github.com/osqzss/gps-sdr-sim) gps simulator. In order for the scripts to work straight out of the box you will need to follow these steps.

1. Install dependencies

```bash
sudo apt-get install git gcc hackrf uncompress -y
```

2. Create new project folder and clone the simulator and this repository in there.

```bash
mkdir project
cd project
git clone https://github.com/osqzss/gps-sdr-sim.git
git clone https://github.com/chrsengel/gps-spoof-tools.git

```

3. Compile the simulator as suggested

```bash
cd gps-sdr-sim
gcc gpssim.c -lm -O3 -o gps-sdr-sim -DUSER_MOTION_SIZE=4000
# adjust the motion size to fit your needs              👆
```

4. Get a free API token from [opencagedata](https://opencagedata.com/) to be able to use the address location fetch function.

5. Insert your API token in line 31 of `gps-gen-sig`.

---

## Usage


The usage is pretty much self explanatory. Hit `./gps-gen-sig` or `gps-spoof` to print the help.

### Generating Signal Files

In order to generate new GPS signals use the `gps-gen-sig` script. It comes with some pretty cool features that make your life much easier. The script is able to automatically fetch the latest broadcast file straight from the NASA servers and get the lat and long values of any given location.

Examples:
```bash
    ./gps-gen-sig -L "Berlin Germany" -g
    ./gps-gen-sig -l "30.286502,120.032669" -f brdc3570.19n -o gpssim.bin
```

If not specified otherwise the script will store the generated GPS signal file at `/tmp/gpssim.bin`. You might want to set it to an external storage device as these files can get large pretty fast, depending on the duration.

### Transmit

Use the `gps-spoof` script to transmit files using a HackRF One.

Example:
```bash
./gps-spoof -f gpssim.bin
```
