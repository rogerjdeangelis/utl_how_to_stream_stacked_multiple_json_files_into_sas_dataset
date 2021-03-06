# utl_how_to_stream_stacked_multiple_json_files_into_sas_dataset
How to stream stacked multiple json files into sas dataset.  Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.


    How to stream stacked multiple json files into sas dataset

    github  (Do not use the readme.md to copy and paste use the .sas file)
    https://tinyurl.com/y923phgr
    https://github.com/rogerjdeangelis/utl_how_to_stream_stacked_multiple_json_files_into_sas_dataset

    stackoverflow
    https://tinyurl.com/y8v4xwea
    https://stackoverflow.com/questions/50430510/how-to-parse-a-file-with-stacked-multiple-jsons-in-r

    SymbolixAU profile
    https://stackoverflow.com/users/5977215/symbolixau


    INPUT (Three back to back nested json files)
    =============================================

    NOTE CODE IS NESTED
                                                                          NESTED
                                                              ----------------------------------
    {"ID":"12345","Timestamp":"20140101", "Usefulness":"Yes","Code":[{"event1":"A","result":"1"}]}
    {"ID":"1A35B","Timestamp":"20140102", "Usefulness":"No","Code":[{"event1":"B","result":"1"}]}
    {"ID":"AA356","Timestamp":"20140103", "Usefulness":"No","Code":[{"event1":"B","result":"0"}]}


    EXAMPLE OUTPUT

    WORK.WANTWPS total obs=3

      ID      TIMESTAMP    USEFULNESS    EVENT1    RESULT

     12345    20140101        Yes          A         1
     1A35B    20140102        No           B         1
     AA356    20140103        No           B         0

    Internal list structure in R

    data.frame':   3 obs. of  4 variables:
     $ ID        : chr  "12345" "1A35B" "AA356"
     $ Timestamp : chr  "20140101" "20140102" "20140103"
     $ Usefulness: chr  "Yes" "No" "No"

     NESTING
     $ Code      :List of 3
      ..$ :'data.frame':    1 obs. of  2 variables:
      .. ..$ event1: chr "A"
      .. ..$ result: chr "1"
      ..$ :'data.frame':    1 obs. of  2 variables:
      .. ..$ event1: chr "B"
      .. ..$ result: chr "1"
      ..$ :'data.frame':    1 obs. of  2 variables:
      .. ..$ event1: chr "B"



    PROCESS  (WORKING CODE ONE LINER)
    =================================

       want<-jsonlite::stream_in(file(
          "d:/json/utl_how_to_parse_a_file_with_stacked_multiple_jsons_in_r.json"));


    OUTPUT
    ======

    WORK.WANTWPS total obs=3

      ID      TIMESTAMP    USEFULNESS    EVENT1    RESULT

     12345    20140101        Yes          A         1
     1A35B    20140102        No           B         1
     AA356    20140103        No           B         0

    *                _               _       _
     _ __ ___   __ _| | _____     __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \   / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/  | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|   \__,_|\__,_|\__\__,_|

    ;

    data _null_;
     file "d:/json/utl_how_to_parse_a_file_with_stacked_multiple_jsons_in_r.json";
     input;
     put _infile_;
    cards4;
    {"ID":"12345","Timestamp":"20140101", "Usefulness":"Yes","Code":[{"event1":"A","result":"1"}]}
    {"ID":"1A35B","Timestamp":"20140102", "Usefulness":"No","Code":[{"event1":"B","result":"1"}]}
    {"ID":"AA356","Timestamp":"20140103", "Usefulness":"No","Code":[{"event1":"B","result":"0"}]}
    ;;;;
    run;quit;

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;


    %utl_submit_wps64('
    libname wrk  sas7bdat "%sysfunc(pathname(work))";
    options set=R_HOME "C:/Program Files/R/R-3.3.2";
    proc r;
    submit;
    source("C:/Program Files/R/R-3.3.2/etc/Rprofile.site", echo=T);
    library(jsonlite);
    want<-jsonlite::stream_in(file(
    "d:/json/utl_how_to_parse_a_file_with_stacked_multiple_jsons_in_r.json"));
    str(want);
    cde<-do.call(rbind, lapply(want$Code, data.frame));
    want<-cbind(want[,1:nrow(want)],cde);
    endsubmit;
    import r=want data=wrk.wantwps;
    run;quit;
    ');


