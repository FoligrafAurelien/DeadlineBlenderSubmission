# Deadline Blender Submission

This repository provides a custom submission script for **Thinkbox Deadline**, allowing users to submit **Blender** scenes with the ability to **choose the desired Blender version** at submission time.

## Features

- Custom submission UI for Blender jobs in Deadline
- Selectable Blender version directly in the submission window
- Full job configuration (name, pool, priority, dependencies, etc.)
- Optional integration with Shotgun / FTrack
- Optional scene file submission
- Support for setting output paths, threads, frame range, and chunk size
- Compatible with both 32-bit and 64-bit Blender builds

## File Overview

- `scripts/Submission/BlenderSubmission.py`:  
  Adds a new tab in the Deadline Monitor UI to configure and submit Blender jobs. Introduces a dropdown for selecting the Blender version (`BlenderVersionBox`), which is passed to the render plugin.

  **To add new Blender versions to the dropdown menu**, modify the following line (line 127):
  ```python
  scriptDialog.AddComboControlToGrid( "BlenderVersionBox", "ComboControl", "4.3", ("4.3","4.4","4.2"), 7, 1, expand=False )
  ```
  Example (to add version 4.5):
  ```python
  scriptDialog.AddComboControlToGrid( "BlenderVersionBox", "ComboControl", "4.5", ("4.5","4.4","4.3","4.2"), 7, 1, expand=False )
  ```

- `plugins/Blender/Blender.py`:  
  The Deadline plugin script that executes the Blender render. It retrieves the version set in the submission UI and selects the corresponding executable path defined in the plugin configuration (e.g., `Blender_4.4_RenderExecutable`).

- `plugins/Blender/Blender.param`:  
  Plugin parameter file used by Deadline to configure the plugin settings in the Monitor. Must be modified to expose the `Version` parameter (already configured in this repository).

## Plugin Configuration

In the Deadline Monitor:

1. Open **Tools > Configure Plugins > Blender**.
2. Add one or more versioned entries such as:
   ```
   Blender_4.2_RenderExecutable=/path/to/blender-4.2/blender
   Blender_4.3_RenderExecutable=/path/to/blender-4.3/blender
   Blender_4.4_RenderExecutable=/path/to/blender-4.4/blender
   Blender_4.5_RenderExecutable=/path/to/blender-4.5/blender
   ```
3. Make sure `Blender_RenderExecutable` is defined as a fallback.

## Installation

1. Copy the contents of this repository into your Deadline Repository:
   - `scripts/Submission/BlenderSubmission.py` → `DeadlineRepository/scripts/Submission/BlenderSubmission/`
   - `plugins/Blender/Blender.py` → `DeadlineRepository/plugins/Blender/`
   - `plugins/Blender/Blender.param` → `DeadlineRepository/plugins/Blender/`

2. Restart the Deadline Monitor to enable the new submission script.

3. You should now see the option **"Blender Version"** in the Blender submission window.

## License

MIT License — Feel free to use, modify and distribute this script.
