# utl-resetting-sas-using-the-macic-string
Resetting sas using the macic string
    Resetting sas using the macic string

    GitHub
    https://github.com/rogerjdeangelis/utl-resetting-sas-using-the-magic-string

    Magic string enhancement

    THe only condition I have not be able to recover from is
     '%if  .. %do' in open code without a %end;

    I recently added '%utl_close' to free up 'in use' objects.
    The squigly, '~' ,in front of the magic string pastes the magic string where the cursor is located;

    On function key  PF12

    ~;;;;/*'*/ *);*};*];*/;/*"*/;%mend;run;quit;%end;end;run;endcomp;%utlfix;

    If I hit' PF1' with my left hand and my right hand is on the mouse, I can
    execute the magic string in one moverment. Drag over string and tap RMB(submit).
    I often tap the RMB mutiple times, especially if the log screen indiicated the first tap
    was not successful in reactivating SAS.

    /*
     _ __ ___   __ _  ___ _ __ ___  ___
    | `_ ` _ \ / _` |/ __| `__/ _ \/ __|
    | | | | | | (_| | (__| | | (_) \__ \
    |_| |_| |_|\__,_|\___|_|  \___/|___/

    */

        %macro utlfix(dum);                              
    * fix frozen sas and restore to invocation ;     
     dm "odsresults;clear;";                         
     ods results off;                                
     options ls=255 ps=65;run;quit;                  
     %utlnopts;                                      
     %utl_close;                                     
     %utlopts;                                       
     ods listing;                                    
     ods select all;                                 
     ods excel close;                                
     ods graphics off;                               
     proc printto;run;                               
     goptions reset=all;                             
     endsubmit;                                      
     endrsubmit;                                     
     run;quit;                                       
    %mend utlfix;                                    



    %macro utl_close;

      %utlnopts;

      /* https://communities.sas.com/t5/user/viewprofilepage/user-id/12151 */

      %local i rc;
      %do i=1 %to 1000;
        %let rc=%sysfunc(close(&i));
      %end;
      %utlopts;

    %mend utl_close;

    %macro utlnopts(note2err=nonote2err,nonotes=nonotes)
        / des = "Turn  debugging options off";

    OPTIONS
         MSGLEVEL=N
         FIRSTOBS=1
         NONUMBER
         MLOGICNEST
       /*  MCOMPILENOTE */
         MPRINTNEST
         lrecl=384
         MAUTOLOCDISPLAY
         NOFMTERR     /* turn  Format Error off                           */
         NOMACROGEN   /* turn  MACROGENERATON off                         */
         NOSYMBOLGEN  /* turn  SYMBOLGENERATION off                       */
         &NONOTES     /* turn  NOTES off                                  */
         NOOVP        /* never overstike                                  */
         NOCMDMAC     /* turn  CMDMAC command macros on                   */
         NOSOURCE    /* turn  source off * are you sure?                 */
         NOSOURCE2    /* turn  SOURCE2   show gererated source off        */
         NOMLOGIC     /* turn  MLOGIC    macro logic off                  */
         NOMPRINT     /* turn  MPRINT    macro statements off             */
         NOCENTER     /* turn  NOCENTER  I do not like centering          */
         NOMTRACE     /* turn  MTRACE    macro tracing                    */
         NOSERROR     /* turn  SERROR    show unresolved macro refs       */
         NOMERROR     /* turn  MERROR    show macro errors                */
         OBS=MAX      /* turn  max obs on                                 */
         NOFULLSTIMER /* turn  FULLSTIMER  give me all space/time stats   */
         NODATE       /* turn  NODATE      suppress date                  */
         DSOPTIONS=&NOTE2ERR
         ERRORCHECK=STRICT /*  syntax-check mode when an error occurs in a LIBNAME or FILENAME statement */
         DKRICOND=ERROR    /*  variable is missing from input data during a DROP=, KEEP=, or RENAME=     */

         /* NO$SYNTAXCHECK  be careful with this one */
    ;

    RUN;quit;

    %MEND UTLNOPTS;



    %MACRO UTLOPTS
             / des = "Turn all debugging options off forgiving options";

    OPTIONS
       MSGLEVEL=I
       OBS=MAX
       FIRSTOBS=1
       lrecl=384
       NOFMTERR      /* DO NOT FAIL ON MISSING FORMATS                              */
       SOURCE      /* turn sas source statements on                               */
       SOURCe2     /* turn sas source statements on                               */
       MACROGEN    /* turn  MACROGENERATON ON                                     */
       SYMBOLGEN   /* turn  SYMBOLGENERATION ON                                   */
       NOTES       /* turn  NOTES ON                                              */
       NOOVP       /* never overstike                                             */
       CMDMAC      /* turn  CMDMAC command macros on                              */
       /* ERRORS=2    turn  ERRORS=2  max of two errors                           */
       MLOGIC      /* turn  MLOGIC    macro logic                                 */
       MPRINT      /* turn  MPRINT    macro statements                            */
       MRECALL     /* turn  MRECALL   always recall                               */
       MERROR      /* turn  MERROR    show macro errors                           */
       NOCENTER    /* turn  NOCENTER  I do not like centering                     */
       DETAILS     /* turn  DETAILS   show details in dir window                  */
       SERROR      /* turn  SERROR    show unresolved macro refs                  */
       NONUMBER    /* turn  NONUMBER  do not number pages                         */
       FULLSTIMER  /*   turn  FULLSTIMER  give me all space/time stats            */
       NODATE      /* turn  NODATE      suppress date                             */
       /*DSOPTIONS=NOTE2ERR                                                                              */
       /*ERRORCHECK=STRICT /*  syntax-check mode when an error occurs in a LIBNAME or FILENAME statement */
       DKRICOND=WARN      /*  variable is missing from input data during a DROP=, KEEP=, or RENAME=     */
       DKROCOND=WARN      /*  variable is missing from output data during a DROP=, KEEP=, or RENAME=     */
       /* NO$SYNTAXCHECK  be careful with this one */
     ;

    run;quit;

    %MEND UTLOPTS;

