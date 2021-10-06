# marlin-configurator
_Concept originally imagined by [The-EG](https://github.com/The-EG) using PowerShell_

## Build Script for Marlin Configurations
The purpose of this project is to partially eliminate the most common configuration questions for compiling [Marlin](https://github.com/MarlinFirmware/Marlin) by providing a mechanism to create configuration files based on an existing Marlin Configuration example.  It can also work with local files through options. 

## Changes to Marin Configuration Directives
Marlin is constantly adding, removing, and changing directives in the configuration files. Even within the same bugfix branch between releases these can change. It is up to the user to be aware of and maintain these options. Not all of the directives are in the Marlin Configuration (.h) files and there are definately some that are valid to be added (such as a PIN reference for a feature). 

## Configurations
This code will pull an example from the live Marlin Configurations Repo:
https://github.com/MarlinFirmware/Configurations

## Included Examples
There is an _example.json_ included in this repo under the _user_ directory. Eventually we will write a parser to automatically traverse the [Marlin Configurations Repo](https://github.com/MarlinFirmware/Configurations) and kick out a series of example json files that are identical to the stock examples from Marlin. From there you can add/remove as you see fit. This directory structure will mimick that of the Marlin Configurations repo(s).

## Pre-tested Configurations for Marlin Firmware
The user community can contribute their .json files to the repo under the contrib folder. 

## Command-Line Arguments
Defer to `py marlin-configuration.py --help` for assistance with all of the command line arguments.

```
  -h, --help            show this help message and exit
  --importpath SOURCE_CONFIG_PATH
                        Import a local file or config example path
  --config JSON_CONFIG_FILE
                        JSON Configuration File
  --target MARLIN_ROOT_DIR
                        The directory in which the files will be saved. Default is current directory. Usually this is the      
                        directory platformio.ini is in.
  --argsfile {True,False}
                        Uses marlin-configurator.ini. !! Using this file overrides all other args on the command-line !!       
  --force {True,False}  Forces running in batch mode, removing all prompts & preferring args over configuration values
  --validate {True,False}
                        Validate JSON Configuration file syntax.
  --createdir {True,False}
                        Creates the target directory if it does not exist.
  --silent {True,False}
                        Suppress Configuration Change Information. Default: false
  --prefer {config,args}
                        Prefer either the JSON config, or the command-line when there is a conflict.
  --missing {add,skip}  Add missing directives instead of skipping them. Default: skip.
  --mode {batch,interactive}
                        Batch mode will skip all prompts except preference. Interactive mode will present choices when
                        conflicts arise.
```

### Argument Configuration File
_Online Reference_: [Python Argparse](https://docs.python.org/3/library/argparse.html#fromfile-prefix-chars)

**filename**: _marlin-configurator.ini_

** USING THIS FILE WILL IGNORE ALL OTHER PASSED PARAMETERS **

This file can be used in place of using command-line arguments. The format of the file is:
```
--option
option_value
```

For example, if you wanted to enable --silent by default (the default is False) your file would look like this:
```
--silent
True
```

## JSON Configuration File
JSON Configuration File called with argument `--config [JSON_CONFIG_FILE]` or from _marlin-configurator.ini_.

Section|Subsection|Purpose
---------|---------|---------
settings||_default configuration for the environment when not using command-line parameters._
useExample||_which example configuration to use and which files to copy._
options||_directives adjusted in Configuration.h and Configuration_Adv.h._
||enable|_directives to enable (if disabled)_
||disable|_directives to disable (if enabled)_
||values|_directives to enable (if disabled) and replace value_

**Example JSON Configuration**
```json
{  
  "settings": {
    "marlinroot" : "D:/localadmin/Documents/marlin_build/Git-Marlin",
    "targetdir" : "D:/localadmin/Documents/marlin_build/Git-Marlin"
  },
  "useExample": {
    "branch" : "bugfix-2.0.x",
    "path" : "Creality/CR-10 S5/CrealityV1",
    "files" : ["Configuration.h","Configuration_adv.h","_Bootscreen.h","_Statusscreen.h"]
  },
  "options": {
    "enable": {
      "SHOW_BOOTSCREEN" : true,
      "SHOW_CUSTOM_BOOTSCREEN" : true,
      "CUSTOM_STATUS_SCREEN_IMAGE" : true,
      "PID_BED_DEBUG" : true,
      "S_CURVE_ACCELERATION" : true,
      "ARC_P_CIRCLES" : true
    },
    "disable": {
      "JD_HANDLE_SMALL_SEGMENTS" : false,
      "PROBE_MANUALLY" : false,
      "G26_MESH_VALIDATION" : false,
      "LEVEL_BED_CORNERS" : false
    },
    "values": {
      "STRING_CONFIG_H_AUTHOR": "\"(devpeeps.com, James Swart)\"",
      "CUSTOM_MACHINE_NAME": "\"CR-10 S5\"",
      "MACHINE_UUID": "\"cede2a2f-41a2-4748-9b12-c55c62f367ff\"",
      "TEMP_SENSOR_BED" : "5",
      "DEFAULT_Kp" : "24.9685",
      "DEFAULT_Ki" : "2.0183",
      "DEFAULT_Kd" : "77.2068"
    }
  }
}
```

## Structure (Files & Directories)
  Name|Type|Purpose
  --------|---|-------
  contrib|Dir|_JSON Configuration files provided by the community._
  examples|Dir|_Direct extractions of the Marlin Configuration Repo(s)._
  legacy|Dir|_Legacy Code which is no longer maintained._
  user|Dir|_Your JSON Configuration files for your printers._
  README.md|File|_README for the project._
  marlin-configurator.ini|File|_Command-Line Argument Configuration File._
  marlin-configurator.py|File|_Python program for this project._

## Requirements
- Marlin Build Environment (has Python already) or python environment.

## Troubleshooting & Help
Please do not reach out to individuals for assistance with this project. Use the Issues section if you run into problems. Most likely we can be found on the [Marlin Discord] (https://discord.gg/ARyMeuBV) somewhere. This is not _officially_ a marlin sponsored project.
