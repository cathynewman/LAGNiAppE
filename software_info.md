# Software Instructions

These instructions are for MacOS. I have tested on MacBook Air M1 running [ADD HOME MACBOOK AIR VERSION] and MacBook Pro M2 running Sonoma 14.4.

## AliView

Download the ZIP file & unzip: [link](https://ormbunkar.se/aliview/downloads/mac/)

To run:
1. Double-click to open app. You will probably get a pop-up error.
2. Go to Settings: Privacy & Security. Scroll down to where it says AliView was blocked from opening, and click *Open Anyway*.
3. Try again to open the app. This time it should work.

## IQ-TREE

Download the latest release: [link](http://www.iqtree.org/)

You may have to take action to allow download. Chrome (at least) blocks it as "insecure" -- go to downloads list and click "Keep" to allow the download to proceed.

To run:
1. In your terminal app, navigate into IQ-TREE bin directory: `cd iqtree-3.0.0-macOS/bin`
2. Try to run the app (you will probably get a pop-up error): `./iqtree3`
3. Go to Settings: Privacy & Security. Scroll down to where it says iqtree3 was blocked from opening, and click *Open Anyway*.
4. Try again to open the app in terminal. This time it should work.

## ASTER

Download: Scroll down to "Installation" and find MacOS: [link](https://github.com/chaoszhang/ASTER)

Installation: If you are running at least MacOS 14, you shouldn't need to do anything else. If you are running an older version of MacOS, do the following:
1. Navigate into app directory: `cd ASTER-MacOS`
2. Run make: `make`

Note: on MacOS [HOME VERSION], I got warnings and errors during the make process, but the program still works for me. So try and see. Your mileage may vary.

To run:
1. In your terminal app, navigate into ASTER bin directory: `cd ASTER-MacOS/bin`
2. Run weighted ASTRAL: `./wastral`
