# Comprehensive camera matching guide (showfloor)
 
This guide covers using Blender to manually recreate the camera angles seen in clean feed Shoshinkai/B-roll footage. 
The end goal is to have a camera within Blender at the same coordinates as the original footage, which can then be 
used to recreate UVs, geometry, or textures. This process takes time to get used to and you're not guaranteed good
results on your first attempt.
 
# Step 1: Correct reference bounds
 
All video sources need to be corrected so that they fall within the same bounds you'd see on an emulator. 
Skipping this step and trying to match against an uncorrected clip will give you incorrect results.
I've taken the time to document every clean feed video source and its corrected dimensions in 1440x1080 below - 
if a source here isn't listed, open a pull request or message me on Discord (@stoicfart39)
 
For quick resizing and repositioning of video clips, run:
 
```
ffmpeg -i "input" -filter_complex "color=size=1440x1080:c=black[bg];[0:v]scale=w:h[v];[bg][v]overlay=x:y" "output"
```
 
Replace `w:h` with the source's resolution (e.g. `1460:1080`) and `x:y` with its offset (e.g. `-14:-18`).
 
Sources are listed alphabetically. Entries marked `?` are uncertain.
 
| Resolution | Offset | Source |
| --- | --- | --- |
| 1475×1040? | 1, -18 | 电玩大观园 1996 片尾 Super Mario 64 Beta Footage |
| 1460×1080 | -14, -18 | トゥナイト2 1995年11月27日 次世代ゲーム機 (2:57) |
| 1460×1080 | -4, -18 | ゲームカタログ２ 1995年12月02日 (0:08, 5:54, 6:27) |
| 2430×1795 | -736, -195 | ゲームカタログ２ 1995年12月02日 (6:44) |
| 1980×1470 | -261, -215 | ゲームカタログ２ 1995年12月02日 (7:45) |
| 1424×1070 | 13, -25 | Bad Influence S4E11 (8:42) |
| 1350×1000 | 42, 18 | Bad Influence S4E14 (9:36) |
| 1430×1074 | 0, -21 | Bad Influence S4E14 (15:02) |
| 1480×1092 | -8, -23 | Bravo TV - Nintendo 64 (Computermesse) (01-28-1996) (0:41, 1:06, 2:21, 2:26, 4:23) |
| 1431×1075 | VAR, -16 | C-Band Wild Feed |
| 1858×1024 | -206, 16 | CNN Showbiz (12/24/95) (1:54, 2:58) |
| 1478×1100 | -25, 2 | Cyber Flash #1 (2:40) |
| 1450×1100 | -6, -23 | Gamesmaster S5E11 (22:06) |
| 2056×1530 | -56, -238 | Gamesmaster S5E15 (20:43) |
| 1432×1078 | -17, -12 | GDC 1999 Miyamoto Keynote (7:30) |
| 1480×1092 | -60, -21 | Promotional Video (Software) (POW) |
| 1480×1092 | -30, -21 | Promotional Video (Software) (POWER) |
| 1460×1074 | -14, -26 | Spaceworld 1995 Nintendo 64 Demonstration - Interview with Miyamoto (Eng sub) |
| 1390×1027 | 15, 9 | [SuperMarioStadium2] |
| 1854×1022 | -210, 9 | [スーパーマリオ・スタジアム3 / SuperMarioStadium3] |
 
Not yet corrected:
- ワールドビジネスサテライト (World Business Satellite) 1995年12月
- トゥナイト2 (Tonight 2) 1995年12月 次世代ゲーム機 (Next Generation Game Console)
- Games World S1E06
- Bad Influence S4E13?
- La Saga Miyamoto
- Cyberflash #32, #55, #170
  
# Step 2: Find rough camera positions

### IMPORTANT
If you are already familiar with SM64's engine, you'll know that it has differences from Blender's 
camera system; YZ is flipped, Y axis sign is inverted, and pitch is stored differently. Those have been accounted 
for on the display, and this guide will refer to position and rotation with Blender's camera system.
 
## a) Find camera position & rotation using in-game display
 
This method is ideal for lining up stages with unchanged geometry between early and final builds (CG, DDD, LLL, 
WF, etc)

Using coordinates directly from the game isn't always necessary, but it is highly reccomended. A situation in 
which you might not need to grab new coordinates is if you already have a similar shot aligned in your blend.

To match the camera, we need to know where the in-game camera needs to sit to reproduce that framing. Pressing 
the L button during gameplay prints the camera's live X, Y, Z position and rotation to the bottom-left of the 
screen.

Put Mario in the same place as in the reference (or as close to the same place as possible) and rotate the camera 
until it matches. If the camera is being stubborn, center it, and slowly move perpendicular to the area center to 
turn in in your desired direction. Once the framing matches, note down the values you see.
 
## b) Find camera rotation using fSpy
 
This method is ideal as a starting point for recreating completely unique geometry with no remnants in the final 
game or the gigaleak (e.g. Snow Slider, JRB, Ghost House). 
 
# Step 3: Bring the camera into Blender
 
## a) Use collected coordinates to recreate angle

Ensure that you have the latest version of the Fast64 plugin installed, and that your Blender is set to metric 
(1 in-game unit is equal to 1 cm in Blender). 

Navigate to the scene settings and set your resolution to 1440x1080 (or any 4:3 resolution), then create a new 
camera (Add > Camera) and select it

<img width="200" height="300" alt="image" src="https://github.com/user-attachments/assets/54157661-17f8-40e1-a6fc-8e98905217b9" />

In its settings, go to Lens, then change the lens unit from 'Milimeters' to 'Field of View'. If you are aligning 
a shot of the castle lobby (not one of the rooms), set the FOV to 79.4°. If it's any other stage, use 57.8°. 
Under 'Background Images', click 'Add Image' and select your reference. Scroll down and ensure that "View as Render" 
is checked, opacity is set to 100%, and the image renders in FRONT of the model.

In the transform tab, set the XYZ position and rotation to the values you previously collected.
Keep in mind:
  
  - `XYZ position = n / 100`
  
  - `XYZ rotation = n / 10`
  
<img width="450" height="180" alt="image" src="https://github.com/user-attachments/assets/aaf92f56-150b-4536-9c59-5cf4b51ca824" />

### _**Your camera is now aligned perfectly to your in-game coordinates!**_

Depending on how good your shot was aligned, you might need to adjust position. When moving your camera, remember 
a few crucial things:

- DO NOT CHANGE THE Y ROTATION! IT IS ALWAYS ZERO (0) NO EXCEPTIONS

- If you are on a flat surface, do not change the Z position

- The most important parts of the camera to focus on are X rotation (yaw) and XY position. 

- Adjusting the camera is something you have to get a feel for, don't be discouraged if you can't get it right away

- The game's internal resolution is 320x240. Some information will be lost, and sometimes aligning it seems impossible.
You just have to get a feel for it... the following shot is aligned perfectly, but due to the resolution it looks like
the top platform and the slope next to it are aligned wrong:
<img width="318" height="250" alt="image" src="https://github.com/user-attachments/assets/3b2b5e26-4bfa-45b6-8456-255de48b04f1" />

- Always use a combination of checking edges with wireframe mode, and toggling visibility on/off in render preview mode to 
verify your position.

Good luck!

### b) Import fSpy camera and adjust manually
 
