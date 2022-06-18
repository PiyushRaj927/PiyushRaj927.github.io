# **MemLabs Lab 6 - The Reckoning**
This is a writeup for [Memlab 6](https://github.com/stuxnet999/MemLabs/tree/master/Lab%206) by [stuxnet999](https://github.com/stuxnet999)
***

## **Challenge Description**
> 
> We received this memory dump from the Intelligence Bureau Department. They say this evidence might hold some secrets of the underworld gangster David Benjamin. This memory dump was taken from one of his workers whom the FBI busted earlier this week. Your job is to go through the memory dump and see if you can figure something out. FBI also says that David communicated with his workers via the internet, so that might be a good place to start.
> 
> **Note**: This challenge is composed of 1 flag split into two parts.
> 
> The flag format for this lab is: **inctf{s0me_l33t_Str1ng}**
> 
> **Challenge file**: [MemLabs_Lab6](https://mega.nz/#!C0pjUKxI!LnedePAfsJvFgD-Uaa4-f1Tu0kl5bFDzW6Mn2Ng6pnM)

## Tools used
- [volatility](https://github.com/volatilityfoundation) with default plugins

-------

## imageinfo
> We use the `imageinfo` plugin of vol.py and get the following summary

> ![](images/imageinfo.png)


We will use the 1st suggested profile `Win7SP1x64`.

---
## Gathering Data 
>Since pugins take time to produce output we will save the output of common  pugins to their corresponding file
```bash
vol.py -f MemoryDump_Lab6.raw --profile=Win7SP1x64 iehistory > iehisotry.txt
vol.py -f MemoryDump_Lab6.raw --profile=Win7SP1x64 filescan > filescan.txt
vol.py -f MemoryDump_Lab6.raw --profile=Win7SP1x64 envars > envars.txt
vol.py -f MemoryDump_Lab6.raw --profile=Win7SP1x64 pslist > pslist.txt
```

---
## pslist

```
Volatility Foundation Volatility Framework 2.6.1
Offset(V)          Name                    PID   PPID   Thds     Hnds   Sess  Wow64 Start                          Exit
------------------ -------------------- ------ ------ ------ -------- ------ ------ ------------------------------ ------------------------------
0xfffffa80012a5040 System                    4      0     78      495 ------      0 2019-08-19 14:40:07 UTC+0000
0xfffffa8002971470 smss.exe                264      4      2       29 ------      0 2019-08-19 14:40:07 UTC+0000
0xfffffa800234cb30 csrss.exe               336    328     10      415      0      0 2019-08-19 14:40:10 UTC+0000
0xfffffa8002aae910 wininit.exe             384    328      3       74      0      0 2019-08-19 14:40:11 UTC+0000
0xfffffa8002ab7060 csrss.exe               396    376      9      499      1      0 2019-08-19 14:40:11 UTC+0000
0xfffffa8002b66560 winlogon.exe            436    376      6      116      1      0 2019-08-19 14:40:11 UTC+0000
0xfffffa8002b99200 services.exe            480    384      9      194      0      0 2019-08-19 14:40:11 UTC+0000
0xfffffa8002bb4600 lsass.exe               496    384      7      513      0      0 2019-08-19 14:40:11 UTC+0000
0xfffffa80022ff910 lsm.exe                 504    384     10      152      0      0 2019-08-19 14:40:11 UTC+0000
0xfffffa8002ce8740 svchost.exe             608    480     10      358      0      0 2019-08-19 14:40:11 UTC+0000
0xfffffa8002d13060 VBoxService.ex          668    480     13      136      0      0 2019-08-19 14:40:11 UTC+0000
0xfffffa8002d4bb30 svchost.exe             724    480      6      257      0      0 2019-08-19 14:40:11 UTC+0000
0xfffffa8002d4fb30 svchost.exe             780    480     19      405      0      0 2019-08-19 14:40:11 UTC+0000
0xfffffa8002dcf5f0 svchost.exe             896    480     22      452      0      0 2019-08-19 14:40:12 UTC+0000
0xfffffa8002de1b30 svchost.exe             948    480     35      893      0      0 2019-08-19 14:40:12 UTC+0000
0xfffffa8002e0b1c0 audiodg.exe            1008    780      7      132      0      0 2019-08-19 14:40:12 UTC+0000
0xfffffa8002e645f0 svchost.exe             400    480     13      275      0      0 2019-08-19 14:40:12 UTC+0000
0xfffffa8002eac740 svchost.exe            1052    480     17      368      0      0 2019-08-19 14:40:12 UTC+0000
0xfffffa8002e76b30 spoolsv.exe            1176    480     14      279      0      0 2019-08-19 14:40:13 UTC+0000
0xfffffa8002f4d780 svchost.exe            1212    480     21      311      0      0 2019-08-19 14:40:13 UTC+0000
0xfffffa8002f79b30 svchost.exe            1308    480     17      253      0      0 2019-08-19 14:40:13 UTC+0000
0xfffffa8003144250 taskhost.exe           1812    480      9      147      1      0 2019-08-19 14:40:18 UTC+0000
0xfffffa8003160120 dwm.exe                1868    896      4       70      1      0 2019-08-19 14:40:18 UTC+0000
0xfffffa8003164b30 taskeng.exe            1876    948      5       81      0      0 2019-08-19 14:40:18 UTC+0000
0xfffffa800319a060 explorer.exe           1944   1844     35      894      1      0 2019-08-19 14:40:19 UTC+0000
0xfffffa8003227060 GoogleCrashHan         1292   1928      7      105      0      1 2019-08-19 14:40:19 UTC+0000
0xfffffa8003219060 GoogleCrashHan          924   1928      6       93      0      0 2019-08-19 14:40:19 UTC+0000
0xfffffa8003277810 VBoxTray.exe           1108   1944     14      139      1      0 2019-08-19 14:40:20 UTC+0000
0xfffffa8002324b30 cmd.exe                 880   1944      1       21      1      0 2019-08-19 14:40:26 UTC+0000
0xfffffa800231e370 conhost.exe             916    396      3       50      1      0 2019-08-19 14:40:26 UTC+0000
0xfffffa8003315060 SearchIndexer.          856    480     13      689      0      0 2019-08-19 14:40:27 UTC+0000
0xfffffa800234eb30 chrome.exe             2124   1944     27      662      1      0 2019-08-19 14:40:46 UTC+0000
0xfffffa800234f780 chrome.exe             2132   2124      9       75      1      0 2019-08-19 14:40:46 UTC+0000
0xfffffa800314fab0 chrome.exe             2168   2124      3       55      1      0 2019-08-19 14:40:49 UTC+0000
0xfffffa80032d9060 WmiPrvSE.exe           2292    608     13      288      0      0 2019-08-19 14:40:52 UTC+0000
0xfffffa80032f9a70 chrome.exe             2340   2124     12      282      1      0 2019-08-19 14:40:52 UTC+0000
0xfffffa8003741b30 chrome.exe             2440   2124     13      263      1      0 2019-08-19 14:40:54 UTC+0000
0xfffffa800374bb30 chrome.exe             2452   2124     14      167      1      0 2019-08-19 14:40:54 UTC+0000
0xfffffa8002b74060 WmiApSrv.exe           2800    480      6      115      0      0 2019-08-19 14:40:57 UTC+0000
0xfffffa8002d9eab0 WmiPrvSE.exe           2896    608      7      124      0      0 2019-08-19 14:40:57 UTC+0000
0xfffffa80032d4380 chrome.exe             2940   2124      9      172      1      0 2019-08-19 14:41:06 UTC+0000
0xfffffa8003905b30 firefox.exe            2080   3060     59      970      1      1 2019-08-19 14:41:08 UTC+0000
0xfffffa80021fa630 firefox.exe            2860   2080     11      210      1      1 2019-08-19 14:41:09 UTC+0000
0xfffffa80013a4580 firefox.exe            3016   2080     31      413      1      1 2019-08-19 14:41:10 UTC+0000
0xfffffa8001415b30 firefox.exe            2968   2080     22      323      1      1 2019-08-19 14:41:11 UTC+0000
0xfffffa8001454b30 firefox.exe            3316   2080     21      307      1      1 2019-08-19 14:41:13 UTC+0000
0xfffffa80035e71e0 WinRAR.exe             3716   1944      7      201      1      0 2019-08-19 14:41:43 UTC+0000
0xfffffa800156e400 DumpIt.exe             4084   1944      5       46      1      1 2019-08-19 14:41:55 UTC+0000
0xfffffa80014c1060 conhost.exe            4092    396      2       50      1      0 2019-08-19 14:41:55 UTC+0000
0xfffffa80014aa060 sppsvc.exe             1224    480      5        0 ------      0 2019-08-19 14:42:39 UTC+0000
0xfffffa800157eb30 GoogleUpdate.e         2256   2396      3      118 ------      1 2019-08-19 14:42:40 UTC+0000
0xfffffa80014f9060 GoogleCrashHan         1192   2256      3       46 ------      1 2019-08-19 14:42:41 UTC+0000
0xfffffa80035e3700 GoogleCrashHan          864   2256      1 127...45      0      0 2019-08-19 14:42:41 UTC+0000
```
Interresting pocesses
> 1. firefox`
> 2. WinRAR
> 3. Chrome

---

## RAR
>Since WinRAR is running we grep for `.rar` files
>
>![](./images/grep_flag2.png)
>and dump it with `dumpsfile` and rename it to flag.rar
>![](images/flag.rar.png)
>
>`flag.rar` contains `flag2.png`, but a passwd is required to open it.
>
>using grep in all the files for `pass`  keyword, we get the password for the file as 
>`easypeasyvirus`
>![](images/grep_rar_passwd.png)
>
>using the passwd, we the get the second part of the flag
>![](images/flag2.png)

## Chrome
>Dumping the Chrome sqlite history file and viewing its contents we get a `pastebin.com`  url
>![](images/chrome_grep.png)
>![](images/sql_url.png)
>`https://pastebin.com/RSGSi1hk`
>![](images/pastebin.png)
>the content of the pastebin give us a google docs [file](https://www.google.com/url?q=https://docs.google.com/document/d/1lptcksPt1l_w7Y29V4o6vkEnHToAPqiCkgNNZfS9rCk/edit?usp%3Dsharing&sa=D&source=hangouts&ust=1566208765722000&usg=AFQjCNHXd6Ck6F22MNQEsxdZo21JayPKug)
>![](images/gdoc.png)
>Docs contain a mega link but the file is encrypted with a key

---

## key
>seaching the chace files i found that firefox had few gmail chace files
```bash
❯ grep mail filescan.txt
0x00000000053f9070      3      0 -W-rw- \Device\HarddiskVolume2\Users\Jaffa\AppData\Roaming\Mozilla\Firefox\Profiles\84kisw0a.default-release\storage\default\https+++mail.google.com\cache\morgue\75\{f4ec0805-5e72-4cbe-a262-5a99a7abb04b}.tmp
0x00000000053fde20     16      0 R--rw- \Device\HarddiskVolume2\Users\Jaffa\AppData\Roaming\Mozilla\Firefox\Profiles\84kisw0a.default-release\storage\default\https+++mail.google.com\cache\morgue\63\{401978b9-d8ee-4339-8725-fcdb3224fd3f}.final
0x000000005d8e6f20     16      0 RW-rw- \Device\HarddiskVolume2\Users\Jaffa\AppData\Roaming\Mozilla\Firefox\Profiles\84kisw0a.default-release\storage\default\https+++mail.google.com\idb\1593787212uysretrs_irge.sqlite
0x000000005dac6440     16      0 R--rw- \Device\HarddiskVolume2\Users\Jaffa\AppData\Roaming\Mozilla\Firefox\Profiles\84kisw0a.default-release\storage\default\https+++mail.google.com\cache\morgue\154\{156622a5-b5aa-431f-b4f6-34922d9cb19a}.final
....
....

```
>
but I was not able to find the key in them, so I dumped memory of all the firefox processes with [memdump](https://github.com/volatilityfoundation/volatility/wiki/Command-Reference#memdump) plugin
>![](images/memdump.png)
>
>greping key on dump.txt
```bash
❯ grep -i " key "  dump.txt
```
>![](images/key.png)
we obtain the key `zyWxCjCYYSEMA-hZe552qWVXiPwa5TecODbjnsscMIU`
so we download the flag_.png from the [megalink](https://mega.nz/#!SrxQxYTQ)

---
## Not yet
>We are unable to open the flag_.png (╯°□°)╯︵ ┻━┻
>file the appears to be corrupted   
>running `pngcheck` 
```bash
❯ pngcheck flag_.png
flag_.png  first chunk must be IHDR
ERROR: flag_.png
```

>IHDR seems to be corrupted 
>googlefu found [IHDR bytes](https://stackoverflow.com/questions/54845745/not-able-to-read-ihdr-chunk-of-a-png-file#54863549)
>![](images/googlefu.png)
>
and staring bytes of `flag_.png` are
>![](images/xd.png)
>
>changing `iHDR` to `IHDR` fixes the png file
>
>`flag_.png`
>![](images/flag_.png)


---

>finally we get the full flag
>`infctf{thi5_cH4LL3Ng3_!s_g0nn4_b3_?_aN_Am4zINg_!_i_gU3Ss???_} 
