# Convert your cd to mp3 files, compress video files

## Prerequisites

* cdparanoia
* lame
* mplayer (for converting wma files to mp3)
* mencoder (for video compress)

For splitting flac files:

* cuetools
* shntool
* flac

You can install them by:

```
sudo apt-get install cdparanoia lame
```

* Ruby
* One of the gems:
  * thor (preferable)
  * rake

## Usage

### Thor

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

### Rake

```
rake cdparanoia["Music/Destination directory"]
rake lame["Music/Destination directory"]
rake remove_wav["Music/Destination directory"]
```

You can see all options by:

```
rake -T
```

## Copyright

Copyright (c) 2011 Wojciech Wnętrzak. See LICENSE for details.
