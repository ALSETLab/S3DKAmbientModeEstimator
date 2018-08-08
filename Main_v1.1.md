(picture of each stage)

# Version 1.1 of the Mode Estimator Application

This application's purpose is to acquire Auto-Regressive/Moving Average (ARMA) coefficients of a signal coming from a PDC.

Version 1.1 is a functional, updated version from version 1.0, yet lacks the user friendly interface and versatility of version 2.0.
It resembles the algorithm loop of version 2.0.

The stages of this VI are as follows:

-    Initialize
-    Read Real Data / Read Simulated Data
-    Preprocessing
-    Compute AR Coefficients
-    Get Spectrum


## Initialize

The program analyzes whether the current run is a simulation or not. This value is taken from the boolean control "Simulate Data?"
In addition, all indicators (graphs, arrays) are reset.

## Read Real Data

1. Obtains PMU data from either a PDC source or a CSV file (current supported formats: WAN OGE, OGnE)

**Acquisition Settings**

PDC options (example: PDC Simulator): Select Data/channels. If the user wants to do a phase comparison, select another data set or 
channel.

CSV options (CSV Reader Inputs): File Input - input a CSV file (WAN/OGnE), Measurement Type (Voltage/Current/Power), Frequency input
(Frequency or d(Frequency)/dt), OGE selection (must match format)

Outputs: Timestamps - date/time of samples taken, Phasor Data - voltage/current/power data - user selection, Analog Data -
frequency or d(Frequency)/dt - user selection

*Note: the CSV file will be read only once. The data will continue in the loop. New terminals can be selected, but 
in order to input a new file, the VI must be restarted.

2. Formats the data to match the user's signal selection.

**Signal Selection Options**

Real Component: Use the real component of the Phasor Data
Imaginary Component: Use the imaginary component of the Phasor Data
Analog: Use the Analog Data
Phase Comparison: Use a comparison of phase angles from two separate terminals
Magnitude: Use the magnitude of the Phasor Data


3. Creates a waveform with the data, using the user input sampling frequency and passes it on to Preprocessing.

## Read Simulated Data

The user can create a transfer function that is compunded with Gaussian White Noise to obtain a simulated signal.
Transfer Function options: Create by inputting a set of poles and zeros, or a damping ratio and input frequency.

The resulting waveform is passed to Preprocessing.

## Preprocessing

Detrends the input waveform, passes it through a low-pass filter, then downsamples it in Downsample.vi. Passes resulting waveform
to Compute AR Coeffs.

## Compute AR/MA Coeffs
