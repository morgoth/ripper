# Convert your cd to mp3 files, compress video files, resize photos

## Prerequisites

* Ruby
* thor gem

Packages:

* cdparanoia
* lame
* mplayer (for converting wma files to mp3)
* mencoder (for video compress)
* imagemagick

For splitting flac files:

* cuetools
* shntool
* flac

## Usage

Ripping CD to mp3 files:

```
thor ripper:disk "Music/Destination directory"
```

Converting wma files to mp3:

```
thor ripper:wma "Music/Directory with wma files"
```

Compressing video files:

```
thor ripper:video your-wideo-file
```

Splitting flac album file into separate ones

```
thor ripper:split "Directory with flac file"
```

Resizing photos when bigger than given dimensions

```
thor ripper:resize_photos --width 1024 --height 1024 "Directory with flac file"
```

You can see all options by:

```
thor -T
```

When using thor you don't have to download Thorfile, simply just write:

```
  thor install https://github.com/morgoth/ripper/raw/master/Thorfile --as ripper --force
```

Then you will have always available command thor ripper
(it is stored in ~/.thor/thor.yml file)

Updating script can be done by:

```
  thor update ripper
```

## Copyright

Copyright (c) 2011 Wojciech Wnętrzak. See LICENSE for details.
