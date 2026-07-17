# Comprehensive camera matching guide (showfloor)
 
This guide covers using Blender to manually recreate the camera angles seen in clean feed Shoshinkai/B-roll footage. 
The end goal is to have a camera within Blender at the same coordinates as the original footage, which can then be 
used to recreate UVs, geometry, or textures.
 
## Step 1: Correct reference bounds
 
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
| 1480×1092 | -8, -23 | Bravo TV - Nintendo 64 (Computermesse) (01-28-1996) (0:41, 1:06, 2:21, 2:26) |
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
  
## Step 2: Find rough camera positions
We will branch into 2 paths here (a, b). Both have unique applications.
 
### a) Find camera position & rotation using in-game display
*The following instructions assume you have a local clone of the showfloor repository set up and building.*
 
This method is ideal for lining up stages with unchanged geometry between early and final builds (CG, DDD, LLL, WF, etc)

To match the camera, we need to know where the in-game camera needs to sit to reproduce that framing. By default, you can't see camera coordinates,
so we've set up an overlay and commented it out for ease of use.
 
Open up `camera.c` and find:
 
```c
    // camera matching
    /*
    vec3f_get_dist_and_angle(c->focus, c->pos, &camDist, &camPitch, &camYaw);
    
    print_text_fmt_int(16, 48, "X%d", c->pos[0]);
    print_text_fmt_int(16, 32, "Y%d", c->pos[2]);
    print_text_fmt_int(16, 16, "Z%d", c->pos[1]);

    print_text_fmt_int(96, 48, "X%d", 900 - (camPitch * 3600 / 0x10000));
    print_text_fmt_int(96, 32, "Y%d", gLakituState.roll * 3600 / 0x10000);
    print_text_fmt_int(96, 16, "Z%d", camYaw * 3600 / 0x10000);
    */

```
 
Uncomment and rebuild. This prints the camera's live X, Y, Z position to the bottom-left of the screen, updating in real time.
 
With that overlay active, boot showfloor and go to the level pictured in your reference. Put Mario in the same place as in the reference (or as close to the same place as possible) and rotate the camera until it matches. If the camera is being stubborn, center it, and then move perpendicularly to the area center to slowly turn in in your desired direction.
 
Once the framing matches, note down the X, Y, Z values on screen; those are the coordinates for that shot. If you are on a flat surface, make SPECIAL note of the Y value, and ensure that you do not change it.
 
### b) Find camera rotation using fSpy
 
This method is ideal for recreating completely unique geometry with no remnants in the final game or the gigaleak (e.g. Snow Slider, JRB, Ghost House)
 
## Step 3: Bring the camera into Blender
 
Regardless of which method you used in Step 2, the goal here is the same: end up with a camera in Blender positioned correctly relative to your reconstructed level geometry.
 
### a) Use collected coordinates to recreate angle

- Navigate to the scene settings and set your resolution to any 4:3 resolution (e.g. 1440x1080)
- Make sure your Blender is set to metric, (1 in-game unit is equal to 1 cm in Blender)

- Create a new camera (Add > Camera)
<img width="200" height="300" alt="image" src="https://github.com/user-attachments/assets/54157661-17f8-40e1-a6fc-8e98905217b9" />

- In its settings, go to Lens, then change the lens unit from 'Milimeters' to 'Field of View'
- If your FOV is confirmed in the table below, take note of the level your screenshot is sourced from. If it is not confirmed in the table below, assume it is 57.6°.
- Under 'Background Images', click 'Add Image' and select your reference


| Stage | FOV |
|---|---|
| Castle Courtyard | 57.6° |
| Castle Grounds | 57.6° |
| Castle Inside (Lobby) | 79.4° |
| Castle Inside (Rooms) | 57.6° |
| Firebubble Land | 57.6° |
| Ghost House | 57.6° |
| Mountain | 58.0° |
| Snow Slider | 57.6° |
| Water Land | 57.6° |


### b) Import fSpy camera and adjust manually
 
