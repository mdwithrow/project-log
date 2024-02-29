:loudspeaker: **Going to try rebooting this project!!**

## 2/28/2024 restart notes
OK, so a lot has happend in the last week or so. 
1) reached out to a sanyo expert
2) talked with some colleauges to solicit ideas
3) reached out to a genealogy library that hopefully (going on a lot of assumptions) may be the source of this data   

I ended up getting another device and while I was trying it out, i found a very interesting set of instructions around jumpers for my drive (TEAC FD-55GFR 149-US). Lo and behold, setting these jumpers and I get clean reads! I need to get organized now and try and split what I have into app and data images. Right now the process is check if I can browse to the disk, make an image in 'Disk Image and Browse' which came with the FC5025, then explore the image with OSFMount. I did run a few in dosbox Also so I don't forget, I think some of the 'weirdness' maybe coming from EasyWriter? Like the Don Draper, Capn'Crunch? I need to look a little closer. Maybe they were just named the same. There is also a WordStar and a 'Family History' software bundle I think I'll need to look at a bit closer. All the data disks I pull have .TXT files but they are either extreemly trashed or not text files at all. More to come. I'll work out a recording process and upload images, pictures and other content as I go. 

![img](https://raw.githubusercontent.com/mdwithrow/project-log/main/images/PROCOMM-1.png) 
![img](https://raw.githubusercontent.com/mdwithrow/project-log/main/images/PROCOMM-2.png) 
![img](https://raw.githubusercontent.com/mdwithrow/project-log/main/images/PROCOMM-3.png) 

# Overview
Move old geneaology data to something modern for backup, review and usage. 

## Phases
- Getting the data - how to get from 5.25 to modern/backedup location
- Exploring the content - how to see what data and what formats we have
- Building something - how to do something interesting with the family tree data (model, visualization, etc.)

**Shoutouts:** These were great sources of info while trying to piece this puzzle together.
- http://www.nj7p.org/Computers/Disk%20Subsystems/floppies.html
- https://winworldpc.com/home
- https://github.com/archivistsguidetokryoflux/archivists-guide-to-kryoflux
- https://www.dpconline.org/blog/wdpd/extracting-info-from-floppy-disks
- https://www.nostalgianerd.com/why-are-floppy-cables-twisted
- https://nerdlypleasures.blogspot.com/2011/05/scourge-of-preservation-disk-based-copy.html
- https://diskpreservation.com/dp.php?pg=protection
- http://sidpreservation.6581.org/floppy-loaders/
- https://www.eriscreations.com/sanyo/
- https://eriscreations.com/sanyo-mbc-550-555/
- https://uisapp2.iu.edu/confluence-prd/display/DIGIPRES/8.+Troubleshooting
- https://www.manualslib.com/manual/2929133/Teac-Fc5025.html?page=3#manual
- https://github.com/mesquidar/ForensicsTools?tab=readme-ov-file#disk-image-handling
- https://www.osforensics.com/tools/mount-disk-images.html
- https://www.osforensics.com/tools/mount-disk-images.html
- https://winworldpc.com/product/easy-writer/10
- https://www.atarimagazines.com/creative/v10n9/12_Sanyo_555_small_business.php


# Getting the data 
- Will use the [greaseweazle](https://github.com/keirf/Greaseweazle) project hardware and software notes in the [project wiki](https://github.com/keirf/greaseweazle/wiki)

## Hardware
   - [x] working 5.25 drive (Thank, Ralph!)
   - [x] test disks - figure out the commands and steps on a program disk or something replaceable before running the actual valuable disks through the process
   - [x] USB cable
   - [x] Power supply with molex - jump start the powersupply w/ a bit of wire or paper clip ([more details on that here](https://www.overclockersclub.com/guides/atx_psu_startup/))
   - [x] greaseweazle controller
   - [x] twisted ribbon cable
   - [x] DeviceSide FC5025 (Thanks for the link, MGE!)
   
## Hook it up
```
[powersupply / molex lead] --> [5.25 drive] <-- ribbon cable --> [greaseweazle controller] <-- usb cable --> [laptop]
```    
- I had some trouble with basic stuff like what is up, what is down, which way does disk go in, etc....
   - the ribbon cable has a colored strip that is colored to help with orientation, line this polarity stripe up with the triangle on the greaseweazle connector. (The greaseweazle female 34 pin is keyed, but the cable I got is not; so pay attention to this)
  - On the connection to the actual drive, it is keyed so there is only one way to attach it. Connect the last position on the cable to the drive (the 'A:' position in a lot of pictures)
  - I left my drive jumper settings to D1
  - Keep the drive flat. For me, the jumpers and spinning drum would be down against the table
  - Disks go in with the 'window' first. Typically this means the label will be closest to you and facing up
  - I didn't do the 'terminator resistor' part mentined in the wiki. I don't understand where/how/what is being connected.... maybe this is why I'm getting hit and miss results?

## Software
-  will start with these pieces of software: 
   - [greaseweazle](https://github.com/keirf/greaseweazle) - for taking the initial flux capture
   - [fluxvis](https://github.com/adafruit/fluxvis) - just for fun to 'visualize' the flux capture
   - [HxC Floppy Emulator](https://hxc2001.com/download/floppy_drive_emulator/)
- other things I stumbled across but didn't get figured out
   - [disk-utilities](https://github.com/keirf/disk-utilities)
   - [fluxEngine](http://cowlark.com/fluxengine/index.html)

1) With everything hooked up, do a read with greaseweasle to an .scp file 

        gw read filename.scp

> I had a lot of trouble even reaching the greaseweasle device consisently. use the command `gw info` to see your device. Also `ls /dev/tty*` (look for AM0) and `lsusb` to confirm it was there. Even after I initially got it responding, sometimes I would have to run the commands twice (fatal error fist time, then just works second time). Not sure what was up with that. Maybe kernel bug based on some issues I saw, but who knows. Also tried the udev rules mentioned in the wiki and adding myself to uucp and tty groups (manjaro/arch)... not sure what actually made it happy. Once a read did get started, it always went through all 81 tracks. Also may just be my laptop.

2) After you have this .scp file, you can load it in HxC emulator. I ran HxC with Wine and it worked fine. 
3) Once loaded (takes a few seconds) you can do a few interesting things
   - Export to an .img and other formats
   - Analyze the tracks
   - Browse the contents

![img](https://raw.githubusercontent.com/mdwithrow/project-log/main/track-analyzer.gif)

> So looking through the track analyzer I can see the content I want somewhere in there (from that hex preview pane - names, details, etc.). However more often then not, if I tried to browse the contents it would be a no go. If it let me browse and see files in HxC, I would export to an .img and then mount it locally to review. BUT even then, the files are all garbled up, even just things like txt seemed very odd when opening. I tried mounting from the file explorer as well as mounting inside dosbox. I am starting to wonder if I have a mix of DOS and non-DOS formated disks. I don't know how to tell that part. 

![img](https://raw.githubusercontent.com/mdwithrow/project-log/main/null-sized-dos-image-invalid-disk.png)

4) Just to see the flux in a cool way, I used fluxvis. Give it the flux file and the filename to save the image to

        fluxvis write filename.scp fileout.png

![img](https://raw.githubusercontent.com/mdwithrow/project-log/main/fluxvis.png)

# Exploring the content
- I recieved a much larger collection than initially expected; 50+ misc 5.25 disks
- Very few clues on the systems grandpa ran or if these are DOS, Apple, etc. I also don't know enough about the disks themselves to gleen anything just visually. There are a few sleeves that indicate DOS, so I'll assume that till later.
- Some disks seem to be 'dd' and have the 'material' around the disk hole. But I don't exactly understand the signifigance of that or if it changes the archiving and file review process. 
- To get started, I've labeled the different groupings with a chalk pen incase the groups are signifigant. I'm going to catalog the labels and try too prioiritize an approach 
   - I'm thinking maybe I can find good flux and img files on line for some of the abandonware and compare my work to theirs
   - Also if there is some kind of menainful organization going on, I want to be able to get somewhat back to original creator's thought process (data disks, applications, etc.)

Here are a few of the disks and their content and notes. Again no clue if I'm doing this right or why I can't get to the stuff I want

|Initial Container| Name                               | Photo            | Content                        |
|-----------------|------------------------------------|------------------|--------------------------------|
| 1               | EasyWriter 1.3                     | [png](clickhere) | scp<br>visual<br>img<br>       |
| 1               | WhiteCreek Cemetery Backup (1984)  | [png](clickhere) | scp<br>visual<br>img<br>       |
| 1               | Untitled 001                       | [png](clickhere) | scp<br>visual<br>img<br>       |
| 1               | Untitled 002                       | [png](clickhere) | scp<br>visual<br>img<br>       |
| 1               | Untitled 003                       | [png](clickhere) | scp<br>visual<br>img<br>       |
| 1               | Untitled 004                       | [png](clickhere) | scp<br>visual<br>img<br>       |
| 1               | Untitled 005                       | [png](clickhere) | scp<br>visual<br>img<br>       |

# Building something
- this all started because had a hunch the printed binders were too well organized to not have some kind of electronic data structure
- there was a lot of key value stype things, but no over all family tree visual
- I wanted to make a tree you can navigate through, click on to pop out details, stuff like that.
