# CBT542
Converted to GitHub via [cbt2git](https://github.com/wizardofzos/cbt2git)

This is still a work in progress. 
Due to amazing work by Alison Zhang and Jake Choi repos are no longer deleted.

```
//***FILE 542 is from Alastair Gray and contains some handy tools   *   FILE 542
//*           (so far), which are described below.  One is a REXX   *   FILE 542
//*           which is an MCNVTCAT replacement, and the other is    *   FILE 542
//*           a tool to find all the catalog alias names in the     *   FILE 542
//*           system.  There's some more too...                     *   FILE 542
//*                                                                 *   FILE 542
//*           emails:  Alastair.Gray@pmi.com                        *   FILE 542
//*                    famille_gray@freesurf.ch                     *   FILE 542
//*                                                                 *   FILE 542
//*           email :  Robert.Richards@opm.gov                      *   FILE 542
//*                                                                 *   FILE 542
//*      The overview for this PDS                                  *   FILE 542
//*                                                                 *   FILE 542
//*      Here are a number of bits of handy REXX code, the list     *   FILE 542
//*      may well grow as I tidy up some of my REXX library.        *   FILE 542
//*                                                                 *   FILE 542
//*      There are some of samples of using the DFSMS/MVS           *   FILE 542
//*      Catalog Search Interface (CSI). More detail can be         *   FILE 542
//*      found in :                                                 *   FILE 542
//*                                                                 *   FILE 542
//*      DFSMS/MVS - Managing Catalogs - Document Number            *   FILE 542
//*      SC26-4914 Appendix D "Catalog Search Interface User's      *   FILE 542
//*      Guide"                                                     *   FILE 542
//*                                                                 *   FILE 542
//*      The original code was derived from the IBM provided        *   FILE 542
//*      sample in 'SYS1.SAMPLIB(IGGCSIRX)' but has been heavily    *   FILE 542
//*      modified (including correcting the bugs in that code).     *   FILE 542
//*                                                                 *   FILE 542
//*      Hopefully it is now correct and should work as             *   FILE 542
//*      intended.  However as usual, no guarantee is implied.      *   FILE 542
//*                                                                 *   FILE 542
//*      All code is designed for either foreground or background   *   FILE 542
//*      execution.                                                 *   FILE 542
//*                                                                 *   FILE 542
//*      The pieces are as follows :                                *   FILE 542
//*                                                                 *   FILE 542
//*      #DELDUP  - Edit macro to delete duplicate lines            *   FILE 542
//*                                                                 *   FILE 542
//*      #DELNDUP - Edit macro to delete non-duplicate lines        *   FILE 542
//*                                                                 *   FILE 542
//*      ALICOUNT - This simply finds all of the aliases in the     *   FILE 542
//*                 system and gives a count of datasets that       *   FILE 542
//*                 are using them.  Handy for finding all those    *   FILE 542
//*                 redundant aliases cluttering up your            *   FILE 542
//*                 mastercat.                                      *   FILE 542
//*                                                                 *   FILE 542
//*      ALIMAKE  - So you have just disconnected a usercat and     *   FILE 542
//*                 lost all the alises ... Reconnect the catalog   *   FILE 542
//*                 and then run this to get a DEF ALIAS for all    *   FILE 542
//*                 the 'suitable' HLQs in the catalog.             *   FILE 542
//*                                                                 *   FILE 542
//*      BODGECAT - A sample workaround for a LISTDS on an          *   FILE 542
//*                 uncataloged dsn.                                *   FILE 542
//*                                                                 *   FILE 542
//*      CSICODE  - Base CSI code, setup to be modified for other   *   FILE 542
//*                 functions. As provided, it simply lists ALL     *   FILE 542
//*                 entries.                                        *   FILE 542
//*                                                                 *   FILE 542
//*      CSICODEO - Pre-munge version taken before the fullword     *   FILE 542
//*                 code was added for anyone who is running old.   *   FILE 542
//*                                                                 *   FILE 542
//*      CSICODEV - Generates a LISTCAT like output for a VSAM file.*   FILE 542
//*                 Contains most of the CSI VSAM fields.           *   FILE 542
//*                                                                 *   FILE 542
//*      CSITAPES - Stripped down version of above code to simply   *   FILE 542
//*                 list all tape based datasets in the catalogs.   *   FILE 542
//*                                                                 *   FILE 542
//*      DDSCAN   - Search a selected DD in JCL for a particular    *   FILE 542
//*                 member.                                         *   FILE 542
//*                                                                 *   FILE 542
//*      HFSSTAT  - Provide statistics for HFS files prior to the   *   FILE 542
//*                 DSNINFO ISPF service provided at OS/390 V2R10.  *   FILE 542
//*                                                                 *   FILE 542
//*      SCNVTCAT - A replacement for MCNVTCAT, converted and       *   FILE 542
//*                 adapted for use under UNIX by John McKown.      *   FILE 542
//*                 Works exactly like RCNVTCAT under TSO.          *   FILE 542
//*                                                                 *   FILE 542
//*                 See member $$NOTE04 for much detail on John's   *   FILE 542
//*                 changes and adaptations for UNIX.               *   FILE 542
//*                                                                 *   FILE 542
//*                 ALIAS entries fixed by Bob Richards.            *   FILE 542
//*                                                                 *   FILE 542
//*      RCNVTCAT - A replacement for MCNVTCAT.                     *   FILE 542
//*                                                                 *   FILE 542
//*                 This should allow those who are unhappy with    *   FILE 542
//*                 IBMs removal of MCNVTCAT support to feel        *   FILE 542
//*                 'safe'.  It is faster than MCNVTCAT and         *   FILE 542
//*                 (hopefully) provides directly compatible        *   FILE 542
//*                 output.  (Just in case 'anyone' has rolled      *   FILE 542
//*                 their own code to use the MCNVTCAT output).     *   FILE 542
//*                                                                 *   FILE 542
//*                 Also generates RECATALOG statements for PAGE    *   FILE 542
//*                 and SYS1.** clusters.                           *   FILE 542
//*                                                                 *   FILE 542
//*                 Also can be used to compare two catalogs.       *   FILE 542
//*                                                                 *   FILE 542
//*                 Fixed to run under TSO when SWA=ABOVE for       *   FILE 542
//*                 TSUCLASS or JOBCLASS(TSU) in JES2 parms.        *   FILE 542
//*                 See member $SWABOVE in this pds for details.    *   FILE 542
//*                                                                 *   FILE 542
//*                 ALIAS entries fixed by Bob Richards.            *   FILE 542
//*                                                                 *   FILE 542
//*                 Bug fixed at z/OS 1.13 level - Sept 2012.       *   FILE 542
//*                                                                 *   FILE 542
//*      RDA      - An SDSF DA 'replacement' displays various       *   FILE 542
//*                 fields that you would normally see in the SDSF  *   FILE 542
//*                 DA display.                                     *   FILE 542
//*                                                                 *   FILE 542
//*      RINIT    - An SDSF INIT 'replacement' displays various     *   FILE 542
//*                 fields that you would normally see in the SDSF  *   FILE 542
//*                 INIT display.                                   *   FILE 542
//*                                                                 *   FILE 542
//*      SPACE    - Displays SMS pools and allow drill down to      *   FILE 542
//*                 volume/dataset level. Has various extras        *   FILE 542
//*                 including displaying the catalog status of all  *   FILE 542
//*                 datasets on a volume.                           *   FILE 542
//*                                                                 *   FILE 542
//*      SPACENEW - SPACE command enhanced for EAV.                 *   FILE 542
//*                                                                 *   FILE 542
//*      SPACEZ23 - SPACE command enhanced for EAV and z/OS 2.3     *   FILE 542
//*                 and higher.                                     *   FILE 542
//*                                                                 *   FILE 542
//*      SYSINF   - Another system information REXX.                *   FILE 542
//*                 There are others, some are worse and some are   *   FILE 542
//*                 better - this is mine :-)                       *   FILE 542
//*                                                                 *   FILE 542
//*      TABLSTAT - Want to know when all those tables/profile      *   FILE 542
//*                 members in a PDS were created/updated? Well     *   FILE 542
//*                 this will add normal PDS stats to all of the    *   FILE 542
//*                 members matching the detail inside the table/s. *   FILE 542
//*                                                                 *   FILE 542
//*      I have also added MAKEINDX in case anyone is wondering     *   FILE 542
//*      what the point of the strange comments in everything       *   FILE 542
//*      are (and where the $$$INDEX came from).                    *   FILE 542
//*                                                                 *   FILE 542
//*      Have fun and I hope these help someone.                    *   FILE 542
//*                                                                 *   FILE 542
//*      Cheers - Alastair Gray (Consultant Systems Type)           *   FILE 542
//*               Lausanne, Switzerland 22nd November,2002          *   FILE 542
//*                                                                 *   FILE 542
```
