# RPG Find Looping and Conditions Start and End

This is part of my **`RPG Utils`** series to help overcome some of the day-to-day activities which can be automated.  
During our analysis of old rpg or cl codes, we come across a section where its very confusing that which line is part of which if condition or dow loop. This happens moslty when we deal with old rpg codes or some linear code with no indentation. This ustility will help to find the line to start of conditions like (IF, SELECT etc.) and also looping conditions like (DOW, DOU, WHILE, FOR etc.)

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes.

### Prerequisites

You need to have AS400 Machine access (duh!)  
Create a RPGUtils Source File to store the files.
```
CRTSRCPF RPGUTILS
```

### Program and Object Descriptions  
  
  * `IFLBLF`  
  This is a PF source which will hold output after running the program on any give source code file.  

  * `IFLBLC`  
  Driver CL program.  

  * `IFLBLCMD`  
  Driver command source file.  
  
  * `IFLBLP`  
  Main program.  


### Installing

**Step 1.**
Upload all files to AS400 server, use ftp. <em>DO NOT CHANGE THE MODE TO BINARY</em>.
```
  open pub400.com
  username
  password
  cd /QSYS.LIB
  cd YOURLIB.LIB
  cd RPGUTILS.FILE
  mput *.MBR
  disconnect
  quit
```
**Step 2.**
Change the atribute type accordingly once uploaded.
```
  LIBLISTCMD  CMD     
  LIBLISTD    DSPF    
  LIBLISTF    PF      
  LIBLISTP    SQLRPGLE
  VALLSRCC    CLLE    
  VALLSRCCMD  CMD     
  VALLSRCD    DSPF    
  VALLSRCP    SQLRPGLE       
  CPYBK       CPY
```
**Step3.**
Use below command to compile.
```
DSPOBJD OBJ(YOURLIB) OBJTYPE(*LIB) OUTPUT(*OUTFILE) OUTFILE(QTEMP/LIBINFOF)   

CRTCMD CMD(YOURLIB/LIBLIST) PGM(*LIBL/LIBLISTP) SRCFILE(YOURLIB/RPGUTILS) SRCMBR(LIBLISTCMD) REPLACE(*YES)  

CRTDSPF FILE(YOURLIB/LIBLISTD) SRCFILE(YOURLIB/RPGUTILS) SRCMBR(LIBLISTD) REPLACE(*YES)  

CRTPF FILE(YOURLIB/LIBLISTF) SRCFILE(YOURLIB/RPGUTILS) SRCMBR(LIBLISTF)           

CRTSQLRPGI OBJ(YOURLIB/LIBLISTP) SRCFILE((YOURLIB/RPGUTILS) SRCMBR(LIBLISTP) OBJTYPE(*PGM) REPLACE(*YES)  

CRTBNDCL PGM(YOURLIB/VALLSRCC) SRCFILE((YOURLIB/RPGUTILS) SRCMBR(VALLSRCC) REPLACE(*NO)               

CRTCMD CMD(YOURLIB/FA) PGM(*LIBL/VALLSRCC) SRCFILE(YOURLIB/RPGUTILS) SRCMBR(VALLSRCCMD) REPLACE(*YES)  

CRTDSPF FILE(YOURLIB/VALLSRCD) SRCFILE(YOURLIB/RPGUTILS) SRCMBR(LIBLISTD) REPLACE(*YES)  

CRTSQLRPGI OBJ(YOURLIB/VALLSRCP) SRCFILE((YOURLIB/RPGUTILS) SRCMBR(VALLSRCP) OBJTYPE(*PGM) REPLACE(*YES)  

```

**Step 4.** 

Add libraries to library list using below command.
```
LIBLIST
```
This would ask for following things:
```
LIBRARY......:           
SEQUENCE.....:           
OBJECT/SOURCE:    (O/S/B)
PROGRAM/DATA.:    (P/D/B)
```
Library is the library you want to add. Leave the sequence blank for now. Used for search sequence of libraries.  

**O**bject or **S**ource or **B**oth. Since library list is used by my other utilites, this is some extra information for now.  
Object library would be library which contains objects only. Source would be for source. Both for Both :)  
You can enter 'B'.

**Programs** or **Data** or **Both**. If library is object library, then whether it holds only programs or files or both.  
You can enter 'B'.

## Running

```
FA <source member name>
```
E.g. Now, if we want to find all the places (libraries that we added in library list) where source for LIBLISTP is present, we would:
FA LIBLISTP (this will show the source from all the libraries, if present in that library)

You can the then position the cursor on select line and press enter to view the source.


## Authors

Ashish Bagaddeo

## License

This project is licensed under the Apache License v2.0 - see the [LICENSE.md](LICENSE.md) file for details

## Acknowledgments
Thanks www.PUB400.com for hosting a public server.
