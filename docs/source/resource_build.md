# Introduction

This section is focused on the use of the resource build tool, which can easily generate the required binary file from the user's source and description file.

# Source files

The source files are placed in the `resource` folder of each project. The files generally contain three types of images, icons and sounds, which are placed in the corresponding folders respectively. The build tool has certain requirements on the code format of the source files, as shown below.

## Format of Image file

The image files are generated by `GUI-Guide` and automatically saved in the gui_guide/generated/images folder. The installation package for GUI-Guide V1.3.0 can be found at this address: [GUI-Guide Tool](https://www.nxp.com/design/software/development-software/gui-guider)

There are two types of image file, one is big-endian and the other is little-endian, so only data of the required image type need to be generated.

```c
const uint8_t _Americano_250x250_map[] = {
#if LV_COLOR_DEPTH == 16 && LV_COLOR_16_SWAP == 0
  0xff, 0xff, 0x00, 0xff, 0xff, 0x00, 0xff, 0xff, 0x00, 0xff, 0xff, ...
#endif
#if LV_COLOR_DEPTH == 16 && LV_COLOR_16_SWAP != 0
  0xff, 0xff, 0x00, 0xff, 0xff, 0x00, 0xff, 0xff, 0x00, 0xff, 0xff, ...
#endif
};
```

## Format of Icon file

The format of icon file needs to be consistent with the following.

```c
#ifndef _NXP_LOGO_H_
#define _NXP_LOGO_H_

#define NXP_LOGO_W 240
#define NXP_LOGO_H 86

static const unsigned short nxp_logo_240x86[] = {
  0xFDA4, 0xFD83, 0xFD83, 0xFD83, 0xFD83, 0xFD83, 0xFD83, 0xFD83, ...
};
#endif /* _NXP_LOGO_H_ */ 
```

## Format of Sound file

The format of sound file needs to be consistent with the following.

```c
/**********************************************************************
* Written by WAVToCode
* FileName:			Can_I_help.h
* Signed:			Yes
* No. of channels:	1
* No. of samples:	14211
* Bits/Sample:		16
**********************************************************************/

#define WW_DETECT_EN_LEN  sizeof(ww_detect_en)

short ww_detect_en[14211] = {
  0,   0,   0,   1,  -2,   2,  -1,   0, /* 0-7 */
  1,  -1,   1,  -2,   2,  -1,   0,   1, /* 8-15 */
  ...
  2,   0,  -1}; /* 14208-14210 */
```

***NOTE : The sound files can be generated using open source Audacity and WavToCode, and the sampling rate is set to 16000hz, with 16 bits per channel.***

# Description file

Each application has a description file to contain all the resources to be built. The resource build tool will read this descriptin file to build the final resource binary file.

Here is the basic design for the description file. Each line represents a source file to build, and the format is `<Type File_Name>`.
- Type : **image/icon/sound**
- File_Name : the path of source files **relative** to build tool.

```
// resource_build coffee_machine_resource.txt
/*
image ../../coffee_machine/resource/images/brewing_animimg_brewingf01.c
icon ../../coffee_machine/resource/icons/process_bar_240x14.h
sound ../../coffee_machine/resource/sounds/common/confirm_tone.h
...
*/
```

# Resource build tool

Provide `bat` for Windows and `bash` for Linux to invoke corresponding build tools respectively to generate resource binary file and offset table file from source files and description file. 

The build tools are placed in `tools/resource_build` folder. And the bat and bash tools are placed in `resource` folder of each project.

Modify the description file, binary file name and image file format in bat and bash tools to generate required binary file.
- description file : the name of description file;
- binary file name (optional) : the name of generated binary file, default is "resources.bin";
- image file format (optional) : 0/1, default is 0 (LV_COLOR_16_SWAP == 0);

```
<build tool path> <description file> <binary file name> <image file format>
```

Generate the binary file by **running project.bat** as administrator in Windows or executing **bash project.sh** in Linux command shell.
