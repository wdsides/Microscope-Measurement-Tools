# Microscope-Measurement-Tools
Microscope Measurement plugin for [FIJI](http://fiji.sc)

This set of [FIJI](http://fiji.sc) plugins provides a quick way to save distance/length calibrations for various microscopes/objectives in a simple text file, and then draw calibrated distances onto your images.

You can then choose any of your prior calibrations to be applied to an open image (or all open images), as so:

![Choose Calibration window][MMT-Choose-Cal-Pic]


The "Draw Measurement" plugin then allows you to draw a line with the calibrated measurement length, as so:

![Annotation with Line][MMT-Annot-Line-Pic]

[MMT-Choose-Cal-Pic]: https://github.com/demisjohn/Microscope-Measurement-Tools/blob/master/img/microscope_calibrations.png
[MMT-Annot-Line-Pic]: https://github.com/demisjohn/Microscope-Measurement-Tools/blob/master/img/ALN%20Cross%20-%20Measurement%20Example%2003.png

## 🏗️ Installation
1. Download and install the [scientific image analysis program FIJI](http://fiji.sc)
1. Download the most recent [Microscope Tools Release from Github](https://github.com/Elaniobro/Microscope-Measurement-Tools/releases/tag/v2.4)
    1. Extract/Unzip the file you downloaded from Github called: `Microscope-Measurment-tools`
    1. Move the folder contents (or just the plugins and macros directories) into the FIJI directory (Fiji.app). When prompted, choose to overwrite the file StartupMacros.fiji.ijm
    <img src="https://github.com/Elaniobro/Microscope-Measurement-Tools/blob/master/img/pkg_contents.png?raw=true" width="600"/>

**note** _MacOS may prevent you from opening an unverified application follow steps below to open_
* In System Preferences, click `Security & Privacy > General`, then click the Lock button to allow you to make changes to your settings. You will need to provide your password, or use Touch ID, to unlock this.
* The last app you attempted to open will be listed underneath your App Store security options. To launch the app (or rather, the DMG image file containing your app), click Open Anyway.
* Once installed, if you have not previously opened the app, macOS will warn you that you are attempting to open an app from the internet. You’ll need to approve it for launch, so click the Open button to do this

## ⚖️ Calibration
1. Take photos of a known measurment sample with your microscope, at each magnification you want to calibrate
1. Open FIJI
1. Open an image file taken at the desired maginification with a measurment marker. e.g. Open a photo of a micrometer slide
1. Zoom in on the photo to view the scale
1. Draw a line `ROI` (Region Of Interest) along the calibration measurment feature. e.g. along the micrometer <img src="https://github.com/Elaniobro/Microscope-Measurement-Tools/blob/master/img/roi.png?raw=true" width="600"/>
1. Navigate to and select `Analyze > Set Scale`
1. The "Distance in Pixels" will already be set by your line ROI
1. Type in the "Known Distance" from your measurement feature <img src="https://github.com/Elaniobro/Microscope-Measurement-Tools/blob/master/img/set_scale.png?raw=true" width="600"/>
1. (Optional) Check 'Global' to apply this same scale to all images opened during this session
1. (Optional) Add the scale as a preset that can be accessed at Plugins\Analyze\Microscope Measurement Tools\Choose Microscope Calibration (F1)
    1. Record the resulting "Scale" value, e.g. 31.1716 pixel/unit, where unit is cm, mm, μm, etc
    1. The "Scale" value will be used in your `Microscope_Calibrations_user_settings.py` file, so recored both a name and the scale value. e.g:
        ```
        Swift 350T 4x: 0.9058 px/μm
        Swift 350T 10x: 1.81 px/μm
        Swift 350T 40X: 12.5455 px/μm
        Swift 350T 100X: 31.1716 px/μm
        ```
        **_these are just dummy values_**
    1. Open up `/Fiji.app/plugins/Analyze/Microscope Measurement Tools/Microscope_Calibrations_user_settings.py` in your preferred text editor or python IDE
    1. Edit the `names` list to reflect the name of each calibration on line 21:
          ```
          names = [
            'Swift 350T 4x',
            'Swift 350T 10x',
            'Swift 350T 40x',
            'Swift 350T 100x',
          ]
          ```
    1. Edit the `cals` list to reflect the corresponding `pixel-per-unit` calibration for each setting, from your previous records, on line 30:
          ```
          cals = [
            0.9058,
            1.81,
            12.5455,
            31.1716,
          ]
          ```
    1. Save the file, and delete the file "Microscope_Calibrations_user_settings$py.pyclass" (if present)
    1. Quit FIJI
    1. Re-start the FIJI application. This will allow the application to register the changes you made to the plugin
    **note** _for any subsquent changes, you will have to repeat the last three steps to see the changes_

## 📈 Usage
Several files are included, which will show up at Plugins\Analyze\Microscope Measurement Tools. Shortcuts are also added to the tool bar.

+ **Choose_Microscope_Calibration.py**
  + *Opens the "Choose Calibration" window, for setting the measurement scale to a preconfigured value (hotkey F1)*
+ **Draw_Measurement_-_Line.py**
  + *Converts a Line ROI into a drawn annotation with the measurement length indicated*
+ **Draw_Rectangle_Long_Length.py**
  + *Adds a drawn annotation of the length of a rectangle ROI along the long side (hotkey 'q')*
+ **Draw_Rectangle_Short_Length.py**
  + *Adds a drawn annotation of the length of a rectangle ROI along the short side (hotkey 'g')*
+ **Draw_Rectangle_Length_x_Width.py**
  + *Adds a drawn annotation of the length and width of a rectangle ROI (hotkey 'u')*

+ **Microscope_Calibrations_user_settings.py**
  + *User-editable Settings file that contains your pre-configured scale calibrations, along with settings for drawing annotations (background/text color etc.)*

View the [How-To Calibrate an Ocular Micrometer](https://www.youtube.com/watch?v=HaqgCtA-ioI&t=738s)

## 📐 Making + Drawing measurements
(Recommended) Set a scale either manually using the procedure outlined in the 'Calibration' Section, or from the presets by going to `Plugins > Analyze > Microscope Measurement Tools > Choose Microscope Calibration` (F1)

You can now drag a Line or Rectangle (or other type of ROI) on any feature, and the FIJI toolbar will show you the measurement dynamically.  Other FIJI functions can now also be used for calibrated measurements (areas etc.).

To draw this measurement on your image, drag the Line or Rectangle at the desired location, and select the desired measurement from `Plugins > Analyze > Microscope Measurement Tools`, click the toolbar icon, or type the hotkey (details for each measurement option are in 'Usage' section)

## 🔧 Custom Calibration Functions
A custom function can be added to the list of available calibrations (as opposed to a static scale value).  A sub-folder is included showing an example of how to do this. The example is for a JEOL SEM (scanning electron microscope), and the example function will determine the scale of the SEM image by parsing an accompanying text file.

See the files in the sub-folder "*MScopeCals - custom function example*" for more info, and move both of the `*.py` files into the main *Microscope Measurement Tools* folder to see how they can be used.  An example SEM image and TXT file from a JEOM 7600F SEM are included.
