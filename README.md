# SharpHap
====

SharpHap is a simple c# direct frame reader implementation, to allow Hap support in c# media backends.

This library only decodes frames, there is not playback system implemented (ask for a frame and get it, and upload in GPU)

Supported containers (readers)
---
* Mov : Parser written from the ground up, no need to have Quicktime installed (I don't want to pollute your drive with useless junk ;)
* Avi : Custom RIFF parser underway, for testing ONLY purposes a Video For Window reader is provided, but as everyone know this is really buggy so use at your own risk.
* Sco (see below)

Supported containers (encoding)
---
* Avi : At least for encoding VFW does the job, so you can Hap illy encode avi with it.
* Sco :  see below

SharpHap supported Formats
---
SharpHap supports natively (with and without Snappy compression)
* Hap1
* Hap5

Since many softwares are still based on DirectX9, old OpenGL versions (hint: move to modern graphics folks ;)
Standard Hap only supports BC1/BC3 compression schemes.

As SharpHap is for DirectX11 only, additional Block compression schemes have been added:
* Hap4 : Same size as Hap1, but single channel (black and white), for a better compression quality
* HapD : Equivalent to BC5 format, two channels
* Hahs : BC6 signed format, half float 3 channels signed textures
* Hahu : BC6 unsigned format, half float 3 channels unsigned textures
* Hap7 : BC7 , equivalent in size to Hap3 for better quality, no requirement to get an extra shader conversion pass, at the trade of slower encoding time.

HapQ is not supported for now, since anyway had better results with Hap7, 
but it might be in the future for compatibility purposes.


Sco
---

Sco stands for "Stupid Container", 
after parsing all those formats called Mov and Avi, those are what we call interchange formats.

While this is of course useful for people sharing files on whatever media/purpose,
the increased flexibility has a cost for format complexity.

On the other side, in most media shows, we have a few content files that we need to  for a client (never need to publish them),
and do this well, so those complex formats are way overdesigned (and overdesigned also implies : more error prone)

So Sco is the most stupid format you will imagine:
* Stupid purpose: Plays a single video, does not care about audio, subtitles...
* Stupid parsing: One header with codec and some frame info, One lookup table with frame index/location, and frame data. You can write a reader in 5 minutes, no need to read 600 pages of Atom documentation.
* Stupid reading: Lookup table is designed by the encore and is an exact file loction,so encoders can also easily optimize frame alignments, readers should never need to have to handle 'JUNK' blocks (please not this is part of Avi specification)
* Stupid 64 bits support : No need for random flags around to tell file is over 4 gigs, native 64 bits pointers
* Stupid dimension: Allow any tpye of "Resource Dimension", so feel free to encode/read Texture Arrays, Cube maps, Volume textures...

