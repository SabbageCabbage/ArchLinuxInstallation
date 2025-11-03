### ISO installation
There are obviously many ways to install the ISO. Personally, I decided to torrent it because the concept of torrenting seems pretty cool. Anyways if you do decide to torrent it, please make sure that what you're downloading is actually what it says it is.  
(A quick way to verify before booting is just to check the signature or checksum before booting which can be found at ✨https://archlinux.org/download/#checksums✨)
After doing so, the set up process is pretty much similar to setting up any other VM (Do make sure, you've allocated enough ram and storage (I picked the default amount storage that virtual box recommended the first time and found out that it was in fact, not enough...))
### Booting into the installer 
Before being told that I should probably install arch via EUFI, I already started my installation using the BIOS instead so whoopsies on my part anywho,,,
    
    ip link
(to set up the internet, since we're on a VM, our wifi is recognized as a wired connection but if connecting using wifi use **iwctl**)
    
    timedatecttl set-ntp true
(keeps the time accurate (which btw is suprisingly important for when installing packages later..))

### Disk Partitioning 
Find your drive using:   
    
    lsblk

open your partioning tool of choice (in my case cfdisk and when asked about the label type pick dos (since we're doing an MBR install))
You’ll create two partitions:
1. A swap partition (about 4GB, or match your RAM if you want to be fancy).
   * Size: 4G
   * Type: Linux swap (that’s number 82 on the list)
2. A root partition that takes up the rest of the disk.
   * Type: Linux (number 83)
   * After creating both, move over to Write, type yes, and then Quit.

