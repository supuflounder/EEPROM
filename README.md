# EEPROM
Some C++ macros to simplify creating a block of EEPROM data

This set of macros simplifies creating a block of data in EEPROM.  It allows you to create read, write, set, get, and print methods for each variable in a block.

Usage:
In the appropriate places (see below), you will write
```c++

   #define EEPROM_something
   #include "EEPROMstate.h"
```
To do this right, you  have to set up a header file which will define all your variables.  This file is normally called **EEPROMstate.h**, and a sample is supplied with this source.  Its general form is shown below.  Note that there are no "include guards" because this file is included many times in your build, sometimes multiple times within a single module.  By doing different #defines before each include of this file, it will generate different kinds of code.
```c++
// Note that there are no "include guards" because this file can be included several times
//#included "EEPROMstate.h"

#define EEPROM_DEFINE
#include "Access.h"

// Use the EEPROMstate and EEPROMstate2 macros to define the fields
// of your EEPROM data block

EEPROMstate2(offset, uint32_t, Signature, HEX, thisSignature);
EEPROMstate2(offset, uint16_t, Version, HEX, thisVersion);
EEPROMstate(offset,  bool,     Debugging, false);

// end of user-definable fields

#define EEPROM_CLEAR
#include "access.h"
```
In this example, there are three fields defined.  The first field is the "signature", a 32-bit value which identifies this block as a valid block of data.  You will typically set this to a specific value you have chosen, and when you read in the block, the first thing you do is compare this 32-bit value with the known 32-bit value.  If they are the same, then the rest of the data is valid and can be trusted.  If they are not the same, it means that the EEPROM has never been written to by this program and you should not turst any of the fields to be valid.  So at this point, you want to call a function that sets the values to default values, including the signature and version values.

In the above examples, "thisSignature" can be any 32-bit value, as long as the value is either a literal constant (e.g., **0xDEADBEEF**) or a name that is defined at the point where it can be set.  More on this below.

The version number is your own version number that tells you what "version" of the code you are using.  For example, the version number might first be **0x0001**.  Suppose that you add a new field (always add at the endf of the list).  You might then choose to indicate what has been written by changing the version number to **0x0002**.  It is up to you to write code that knows that if the version number is 1, version 2 values are not valid.  Tedious, but I have not come up with a better method.

Note also that you will not have to define these variables "by hand".  The EEPROMstate(2) macros handle this for you.

##Using EEPROMstate.h
###Declaring the variables
Use the **EEPROMstate** or **EEPROMstate2** macros to list the variables.  For these, the crucial parameter is the **type** and the **name** parameters:
```c++
EEPROMstate(_offset_, 
