# HyperOS-custom-AOD-guide
The following instructions gives the guidelines of making your HyperOS Always on display theme. Once the theme is created you can use this other repository on how to apply ot to your phone.


Developing custom Always-On Display (AOD) themes for Xiaomi HyperOS and MIUI requires adhering to OS-level rendering rules, strict power constraints, and specific XML data bindings.

# 1. Viewport Boundaries & OLED Constraints
HyperOS applies strict system-level clipping and pixel-shifting algorithms to protect OLED displays from burn-in and minimize battery drain.

# Safe Display Viewport: 
All visible elements must strictly reside within y = 80 to y = 1020 on a standard 1080 \times 1920 canvas. Any visual content placed below y \approx 1020 is automatically hidden by the system compositor mask.

# Pixel Shifting Offset: 
The OS periodically shifts the entire AOD UI by a few pixels in random directions. Keep visual elements at least 20–30px away from screen edges to prevent unexpected cropping during shift cycles.

# Canvas Density Adaptation:
Include extraScales and extraResources in the root <aod> tag to ensure your layout auto-scales across devices with different screen densities.


# 2. Core Variables & System Binding Reference
| Category | Variable Name | Type / Output | Description |
| :--- | :--- | :--- | :--- |
| **Time** | `#hour12` / `#hour` | Number (1–12 / 0–23) | Current hour |
| **Time** | `#minute` / `#second` | Number (0–59) | Current minute / second |
| **Date** | `#date` / `#month` / `#year` | Number | Day of month, month index, 4-digit year |
| **Date** | `#day_of_week` | Number (1–7) | 1 = Sunday, 7 = Saturday |
| **System** | `#battery_level` | Number (0–100) | Current battery percentage |
| **System** | `#battery_state` | Number | `1` or `3` = Charging; `2` = Discharging |
| **Toggles** | `#battery_enable` | Boolean (0/1) | User system setting for showing battery |
| **Toggles** | `#notification_enable` | Boolean (0/1) | User system setting for showing notifications |
| **Toggles** | `#preview_mode` | Boolean (0/1) | `1` when rendering inside Theme Manager preview |
| **Notifications** | `#hasnotifications` | Number | Total count of active notifications |
| **Notifications** | `notice_icon0` to `notice_icon3` | `blob.bitmap` | App icon bitmaps bound from rows 0 through 3 |
| **Notifications** | `noticePkg` | `string[]` | Array of notifying app package names |


# 3. Power Optimization Rules
Restrict useVariableUpdater: Only request variable updates required for your visual design. If your clock does not show ticking seconds, remove DateTime.Second from useVariableUpdater="DateTime.Minute,Battery".
Cap Frame Rates: Set frameRate="60" during entry animations, but ensure continuous animations pause or stop when not active. Avoid continuous loop="true" render loops unless strictly necessary.
Asset Compression: Use 8-bit PNGs or optimized WEBP files for static graphics. Keep total theme file size minimal to reduce memory footprint when the system wakes the display controller.

# Template.xml
Use the template.xml for any type of AOD theme (digital, analog, or graphic-based)

# 4. Folder structure
        theme.aodbackup
                content
                drawable
                    preview_aod_0.jpg
                    preview_aod_small_0.jpg
                aod_description.xml

  The content folder contains the code and the assets. The drawable folder contains the preview images. The aod_description contains the description for the aod.
  These two folders and aod_description.xml are compressed as a zip and their extension changed to .aodbackup.



 # 5. Testing
 Use the AOD theme manager  https://github.com/haadi76/AOD-Theme-Manager-HyperOS-  , to upload you aodbackup themes and apply them to your system.


