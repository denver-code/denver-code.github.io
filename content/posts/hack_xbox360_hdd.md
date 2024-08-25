+++
title = "Hacking Xbox 360 HDD"
date = "2024-01-18"
+++

I bet most people that been using xbox or gaming consoles at all have heard or been using xbox 360.
Yes, it’s that one legendary console from the past, and some people still use it now, but it has some cons that you might face while playing.
One of them is internal storage, some of the Xbox 360 models have really small internal hard drive, and some even don’t have it like Arcade edition, or the seller just removed it for some reason.

My xbox 360 didn’t have an HDD, so I thought I’m just gonna buy small used drive from one pf the local shops(CeX in my case).
And on the next day I bought 320GB drive for a couple of quids, but immediately noticed small damage and cracks on the top panel that can be removed, so it was opened and probably the HDD been changed.
Now this is not a big deal as I knew that most of the 2.5” HDD’s and SDD’s will work fine, but I didn’t know that brand new clean drive might not be formatted in the right way that xbox wants.
That was my case!
When drive is connected to the xbox via internal SATA slot it won’t appear in the “Storage” settings, however when you disconnect it - xbox will reboot, so it can see my drive and I possibly can fix it.

## Let’s try to format it
My first step was just format it from NTFS to ExFat and try like this - but no success.
Then I tries to format it to Fat32 and change to the MBR from GPT - no success, even with ExFat.

## Learning about Xbox drive structure
In short terms xbox can use only ExFat and Fat32 drives with right formatting, and essential Security Sectors value.
That means you have to manually change addresses of the drive, which depends on the drive size in case to make security sector.
While you have option to do it manually on xbox itself using serial number on options, your drive should be in the right format anyway, so that not a solution.
Second option is to connect your drive using SATA to USB adapter, and then format as a external flash drive with only 32GB, even if you have more rest of the space won’t be used and security sectors won’t be set, so not a solution either unfortunately.

## Exploring tutorials
Now exploring tutorials and wiki’s for that question will be problematic as most of them simply outdated (10+ years)
And of the all tutorials ways to fix it was:
Using HDDHACKR and Security Sector dump file to format hdd in the right way.
Now there’s a problem:
You need to have right Western Digital hard drive so hddhackr can “hack” it properly.
And then comes another fun dependency - it works only on MS-DOS or Free-DOS which means you should have “old” pc because my laptop doesn’t have legacy bios option, and do not have option to connect drives as ATA IDE mode. And also doesn’t have an SATA slot to connect this drive so even with right BIOS mode I won’t be able to reproduce tutorial steps.

Maybe I can do it through virtual box, but I haven’t tried as I found that there’s some apps that can do the same but without using deprecated OS. I didn’t wondered as I knew there’s should be some way to do it from Windows/Linux, so I found two tools:
- XPlorer 360 Extreme 2
- FATXplorer

And what helped me is FATXplorer, you will need to flash right security sector to your drive or clone from existing installed Xbox360 HDD, and then format it using FATXplorer.
After that you will be able to format it on Xbox360 itself and use it as a normal drive.
