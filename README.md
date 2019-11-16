# EEPROM
Some C++ macros to simplify creating a block of EEPROM data

This set of macros simplifies creating a block of data in EEPROM.  It allows you to create read, write, set, get, and print methods for each variable in a block.

Usage:
In the appropriate places (see below), you will write
   [code]
   #define EEPROM_something
   #include EEPROMstate.h
   [/code]
To do this right, you  have to set up a header file which will define all your variables.  This file is normally called EEPROMstate.h, and a sample is supplied with this source.  Its general form is shown below.  Note that there are no "include guards" because this file is included many times in your build, sometimes multiple times within a single module.  By doing different #defines before each include of this file, it will generate different kinds of code.
