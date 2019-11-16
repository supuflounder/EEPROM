# EEPROM
Some C++ macros to simplify creating a block of EEPROM data

This set of macros simplifies creating a block of data in EEPROM.  It allows you to create read, write, set, get, and print methods for each variable in a block

Usage:
In the appropriate places (see below), you will write
   [code]
   #define EEPROM_something
   #include EEPROMstate.h
   [/code]
