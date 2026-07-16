# Correct video bounds

All videos are within 1440*1080 bounds, offset is from top left edge
For quick resizing, use the following command, where `w:h` is resolution, and `x:y` is offset.
```
ffmpeg -i "input" -filter_complex "color=size=1440x1080:c=black[bg];[0:v]scale=w:h[v];[bg][v]overlay=x:y" "output"
```

Sources listed in alphabetical order
| Resolution | Offset | Source |
| --- | --- | --- |
| 1475×1040?? | 1, -18 | 电玩大观园 1996 片尾 Super Mario 64 Beta Footage |
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
 
## TODO
 
- ワールドビジネスサテライト (World Business Satellite) 1995年12月
- トゥナイト2 (Tonight 2) 1995年12月 次世代ゲーム機 (Next Generation Game Console)
- Games World S1E06
- Bad Influence S4E13?
- La Saga Miyamoto
- Cyberflash #32, #55, #170
