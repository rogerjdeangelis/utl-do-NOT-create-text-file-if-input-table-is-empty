# utl-do-NOT-create-text-file-if-input-table-is-empty
Do NOT create txt file if input table is empty

    Do NOT create txt file if input table is empty

            Two Test cases
                a. Input table is empty
                b. Input table has data

    Not as simple as you might think.

    github
    https://github.com/rogerjdeangelis/utl-do-NOT-create-text-file-if-input-table-is-empty

    Related to
    https://tinyurl.com/52w2rrf2
    https://communities.sas.com/t5/SAS-Programming/Conditionally-create-a-file-in-a-data-statement/m-p/724490

    Original Solution by SAS Jedi  (I made minor changes)
    https://communities.sas.com/t5/user/viewprofilepage/user-id/13728

    *_                   _
    (_)_ __  _ __  _   _| |_
    | | '_ \| '_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|                     _           _        _     _
      __ _      ___ _ __ ___  _ __ | |_ _   _  | |_ __ _| |__ | | ___
     / _` |    / _ \ '_ ` _ \| '_ \| __| | | | | __/ _` | '_ \| |/ _ \
    | (_| |_  |  __/ | | | | | |_) | |_| |_| | | || (_| | |_) | |  __/
     \__,_(_)  \___|_| |_| |_| .__/ \__|\__, |  \__\__,_|_.__/|_|\___|
                             |_|        |___/
    ;
    data empty;
       stop;
       set sashelp.class(obs=1);
    run;

    *_        _        _     _                 _ _   _           _       _
    | |__    | |_ __ _| |__ | | ___  __      _(_) |_| |__     __| | __ _| |_ __ _
    | '_ \   | __/ _` | '_ \| |/ _ \ \ \ /\ / / | __| '_ \   / _` |/ _` | __/ _` |
    | |_) |  | || (_| | |_) | |  __/  \ V  V /| | |_| | | | | (_| | (_| | || (_| |
    |_.__(_)  \__\__,_|_.__/|_|\___|   \_/\_/ |_|\__|_| |_|  \__,_|\__,_|\__\__,_|

    ;
    data has_date;
       stop;
       set sashelp.class(obs=1);
    run;

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __  ___
    / __|/ _ \| | | | | __| |/ _ \| '_ \/ __|
    \__ \ (_) | | |_| | |_| | (_) | | | \__ \
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|___/
                                    _           _        _     _
      __ _      ___ _ __ ___  _ __ | |_ _   _  | |_ __ _| |__ | | ___
     / _` |    / _ \ '_ ` _ \| '_ \| __| | | | | __/ _` | '_ \| |/ _ \
    | (_| |_  |  __/ | | | | | |_) | |_| |_| | | || (_| | |_) | |  __/
     \__,_(_)  \___|_| |_| |_| .__/ \__|\__, |  \__\__,_|_.__/|_|\___|
                             |_|        |___/
    ;
    * delete file if it exists;
    %utlfkil(d:/txt/want.txt);

    data empty;
       stop;
       set sashelp.class(obs=1);
    run;

    DATA _NULL_;

        /* nobs is available at datastep compilation time */

        /* If there are observations, proceed*/
        if nobs>0 then stop;

        fileloc="d:/txt/want.txt";
        SET Work.EMPTY nobs=nobs;

        file x filevar=fileloc; /* file loc only exists if nobs > 0 */

        IF _N_ = 1 THEN DO;
            put 'First Age'; /* These are the column headers. */
        END;
        put Name ' ' Age;

    RUN;

    * Verify that the file does not exist;

    filename mytest 'd:\txt\want.txt';
    %let fref=mytest;

    %macro test;
     %if %sysfunc(fexist(&fref)) %then
      %put The file identified by the fileref &fref exists.;
     %else
     %put %sysfunc(sysmsg());
    %mend test;

    %test

    *            _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| '_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    ;

    WARNING: Physical file does not exist, d:\txt\want.txt.

    *_        _        _     _                 _ _   _           _       _
    | |__    | |_ __ _| |__ | | ___  __      _(_) |_| |__     __| | __ _| |_ __ _
    | '_ \   | __/ _` | '_ \| |/ _ \ \ \ /\ / / | __| '_ \   / _` |/ _` | __/ _` |
    | |_) |  | || (_| | |_) | |  __/  \ V  V /| | |_| | | | | (_| | (_| | || (_| |
    |_.__(_)  \__\__,_|_.__/|_|\___|   \_/\_/ |_|\__|_| |_|  \__,_|\__,_|\__\__,_|

    ;

    %utlfkil(d:/txt/want.txt);

    data has_data;
       set sashelp.class(obs=1);
    run;

    DATA _NULL_;

        /* nobs is available at datastep compilation time */

        /* If there are observations, proceed*/
        if nobs>0 then fileloc="d:/txt/want.txt";


        SET Work.HAS_DATA nobs=nobs;

        file x filevar=fileloc; /* file loc only exists if nobs > 0 */

        IF _N_ = 1 THEN DO;
            put 'First Age'; /* These are the column headers. */
        END;

        put Name ' ' Age;

    RUN;

    * Verify that the file does not exist;

    filename mytest 'd:\txt\want.txt';
    %let fref=mytest;

    %macro test;

     %if %sysfunc(fexist(&fref)) %then %do;
      %let pth=%sysfunc(pathname(fref));
      %put The file &pth exists.;
     %end;

     %else %do;
      %put %sysfunc(sysmsg());
     %end;

    %mend test;

    %test
    *            _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| '_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    ;

    The file d:\txt\test.txt exists.
