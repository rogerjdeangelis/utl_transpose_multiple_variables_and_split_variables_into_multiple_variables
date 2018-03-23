# utl_transpose_multiple_variables_and_split_variables_into_multiple_variables
Transpose multiple variables and split variables into multiple variables.
    Transpose multiple variables and split variables into mutiple variables

    see github
    https://tinyurl.com/ybpjo86q
    https://github.com/rogerjdeangelis/utl_transpose_multiple_variables_and_split_variables_into_multiple_variables

    for gather macro
    Alea Iacta
    https://github.com/clindocu

    see
    https://tinyurl.com/ybpjo86q
    https://communities.sas.com/t5/Base-SAS-Programming/Macro-to-Parse-Multiple-Variables-Multiple-Times/m-p/447957


    INPUT
    =====

     WORK.HAVE total obs=3

      ID            JAN17                     FEB17                     MAR17

      1     I-2-0-056-0029-22-561     C-4-0-056-0029-22-561     C-4-1-056-0029-22-561
      2     P-4-23-056-0029-22-561    P-4-23-056-0016-22-561    B-1-23-056-0016-22-561
      3     P-4-33-007-0140-03-071    N-4-33-007-0140-03-071    N-4-33-007-0140-03-071

     EXAMPLE OF WHAT WE WANT

      WORK.HAVFIX total obs=9

       ID     VAR     VAR1    VAR2    VAR3    VAR4    VAR5    VAR6    VAR7

       1     JAN17     I       2       0      056     0029     22     561
       2     JAN17     P       4       23     056     0029     22     561
       3     JAN17     P       4       33     007     0140     03     071
      ....


    PROCESS (All the code)
    ======================

        %utl_gather(have,var,val,id,havXpo,valformat=$40.);

        /*
        WORK.HAVXPO total obs=9
         ID     VAR              VAL
         1     JAN17    I-2-0-056-0029-22-561
         2     JAN17    P-4-23-056-0029-22-561
         3     JAN17    P-4-33-007-0140-03-071
        */

        data havFix;

           * make a file we can use an input statement with;
           if _n_=0 then do;
             %let rc=%sysfunc(dosubl('
                data _null_;
                  set havXpo;
                  file "d:/txt/stack.txt";
                  put id '-'  var '-' val;
                run;quit;
             '));
           end;
           * split variables;
           infile "d:/txt/stack.txt" dlm='-';
           input id$ var$ (var1-var7) ($);
        run;quit;

    OUTPUT
    ======

       WORK.HAVFIX total obs=9

        ID     VAR     VAR1    VAR2    VAR3    VAR4    VAR5    VAR6    VAR7

        1     JAN17     I       2       0      056     0029     22     561
        1     FEB17     C       4       0      056     0029     22     561
        1     MAR17     C       4       1      056     0029     22     561
        2     JAN17     P       4       23     056     0029     22     561
        2     FEB17     P       4       23     056     0016     22     561
        2     MAR17     B       1       23     056     0016     22     561
        3     JAN17     P       4       33     007     0140     03     071
        3     FEB17     N       4       33     007     0140     03     071
        3     MAR17     N       4       33     007     0140     03     071

    *                _               _       _
     _ __ ___   __ _| | _____     __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \   / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/  | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|   \__,_|\__,_|\__\__,_|

    ;

    data have;
      retain id;
      informat Jan17 Feb17 Mar17 $40.;
      input ID$ Jan17 Feb17 Mar17;
    cards4;
    1 I-2-0-056-0029-22-561 C-4-0-056-0029-22-561 C-4-1-056-0029-22-561
    2 P-4-23-056-0029-22-561 P-4-23-056-0016-22-561 B-1-23-056-0016-22-561
    3 P-4-33-007-0140-03-071 N-4-33-007-0140-03-071 N-4-33-007-0140-03-071
    ;;;;
    run;quit;

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;

    %utl_gather(have,var,val,id,havXpo,valformat=$40.);

    data havFix;
       if _n_=0 then do;
         %let rc=%sysfunc(dosubl('
            data _null_;
              set havXpo;
              file "d:/txt/stack.txt";
              put id '-'  var '-' val;
            run;quit;
         '));
       end;
       infile "d:/txt/stack.txt" dlm='-';
       input id$ var$ (var1-var7) ($);
    run;quit;



