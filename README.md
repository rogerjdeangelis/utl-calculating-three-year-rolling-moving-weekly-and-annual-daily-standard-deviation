# utl-calculating-three-year-rolling-moving-weekly-and-annual-daily-standard-deviation
Calculating three year rolling moving weekly and annual daily standard deviation

    Calculating three year rolling weekly and annual daily standard deviation                                                          
                                                                                                                                       
    github                                                                                                                             
    https://tinyurl.com/y3xwfgjg                                                                                                       
    https://github.com/rogerjdeangelis/utl-calculating-three-year-rolling-moving-weekly-and-annual-daily-standard-deviation            
                                                                                                                                       
    SAS Forum                                                                                                                          
    https://tinyurl.com/y3xwfgjg                                                                                                       
    https://communities.sas.com/t5/SAS-Programming/Calculating-rolling-standard-deviation-over-the-past-12-months/m-p/585156           
                                                                                                                                       
                                                                                                                                       
      Assumptions                                                                                                                      
      -----------                                                                                                                      
                                                                                                                                       
          1. Window of rolling 365 days for each non leap year (2021, 2022, 2023)                                                      
             (assume 365 days in a year. ok for 2021-2023)                                                                             
                                                                                                                                       
          2. If the window has one or more missing values the rolling                                                                  
             standard deviation will be missing.                                                                                       
                                                                                                                                       
    *_                   _                                                                                                             
    (_)_ __  _ __  _   _| |_                                                                                                           
    | | '_ \| '_ \| | | | __|                                                                                                          
    | | | | | |_) | |_| | |_                                                                                                           
    |_|_| |_| .__/ \__,_|\__|                                                                                                          
            |_|                                                                                                                        
    ;                                                                                                                                  
                                                                                                                                       
    options validvarname=upcase;                                                                                                       
    libname sd1 "d:/sd1";                                                                                                              
    data sd1.have ;                                                                                                                    
      format day yymmdd10. ;                                                                                                           
      today="01JAN2021"D;                                                                                                              
      do day=today to today + 365*3;                                                                                                   
        return=int(100*uniform(1235));                                                                                                 
        if  not ( (today + 6 < day < today + 99) or day = (today + 109) ) then output;                                                 
      end;                                                                                                                             
      drop today;                                                                                                                      
    run;quit;                                                                                                                          
                                                                                                                                       
    * three years of data;                                                                                                             
                                                                                                                                       
    SD1.HAVE total obs=1,002                                                                                                           
                                                                                                                                       
                                                                                                                                       
          DAY        RETURN                                                                                                            
                                                                                                                                       
       2021-01-01      42                                                                                                              
       2021-01-02       5                                                                                                              
       2021-01-03      78                                                                                                              
       2021-01-04      35                                                                                                              
       2021-01-05      17                                                                                                              
       2021-01-06       5                                                                                                              
       2021-01-07      57                                                                                                              
       ...                 +--                                                                                                         
       ...                 | 3 months missing                                                                                          
       ...                 |                                                                                                           
       ..                  +--                                                                                                         
       2021-04-10      25                                                                                                              
       2021-04-11      82                                                                                                              
       2021-04-12      66                                                                                                              
       2021-04-13      98                                                                                                              
       2021-04-14       1                                                                                                              
       2021-04-15      94                                                                                                              
       2021-04-16      35                                                                                                              
       2021-04-17       7                                                                                                              
       2021-04-18      92                                                                                                              
       2021-04-19      10                                                                                                              
                           Missing Day                                                                                                 
       2021-04-21      84                                                                                                              
       2021-04-22      84                                                                                                              
       2021-04-23      24                                                                                                              
       2021-04-24      37                                                                                                              
      ...                                                                                                                              
                                                                                                                                       
    *            _               _                                                                                                     
      ___  _   _| |_ _ __  _   _| |_                                                                                                   
     / _ \| | | | __| '_ \| | | | __|                                                                                                  
    | (_) | |_| | |_| |_) | |_| | |_                                                                                                   
     \___/ \__,_|\__| .__/ \__,_|\__|                                                                                                  
                    |_|                                                                                                                
    ;                                                                                                                                  
                                                                                                                                       
    WORK.WANT obs = 1,003                                                                                                              
                                                                                                                                       
                                     StnDev    StnDev                                                                                  
     Obs       DAY        RETURN      WEEK     ANNUAL                                                                                  
                                                                                                                                       
       1    2021-01-01      42        .           .                                                                                    
       2    2021-01-02       5        .           .                                                                                    
       3    2021-01-03      78        .           .                                                                                    
       4    2021-01-04      35        .           .                                                                                    
       5    2021-01-05      17        .           .                                                                                    
       6    2021-01-06       5        .           .                                                                                    
       7    2021-01-07      57      27.3887       .  Stan Dev - Have 7 consecutive days                                                
                                                                                                                                       
       8    2021-01-08       .        .           .  +--                                                                               
       9    2021-01-09       .        .           .  |                                                                                 
      10    2021-01-10       .        .           .  |                                                                                 
      ....                                           | Filled in 3 months with missing values                                          
      97    2021-04-07       .        .           .  |                                                                                 
      98    2021-04-08       .        .           .  |                                                                                 
      99    2021-04-09       .        .           .  +--                                                                               
                                                                                                                                       
     100    2021-04-10      25        .           .                                                                                    
     101    2021-04-11      82        .           .                                                                                    
     102    2021-04-12      66        .           .                                                                                    
     103    2021-04-13      98        .           .                                                                                    
     104    2021-04-14       1        .           .                                                                                    
     105    2021-04-15      94        .           .                                                                                    
     106    2021-04-16      35      37.4153       .  Stan Dev - Have 7 consecutive days                                                
     107    2021-04-17       7      40.4957       .                                                                                    
     108    2021-04-18      92      41.7749       .                                                                                    
     109    2021-04-19      10      44.8235       .                                                                                    
     110    2021-04-20       .        .           .  Sop ultil we have 7 consecutive days                                              
     111    2021-04-21      84        .           .                                                                                    
     112    2021-04-22      84        .           .                                                                                    
     113    2021-04-23      24        .           .                                                                                    
     114    2021-04-24      37        .           .                                                                                    
     115    2021-04-25      11        .           .                                                                                    
     116    2021-04-26      67        .           .                                                                                    
     117    2021-04-27      47      28.7915       .  Stan Dev - Have 7 consecutive days                                                
     118    2021-04-28      61      25.4605       .                                                                                    
     119    2021-04-29      31      20.0226       .                                                                                    
     120    2021-04-30      41      18.7921       .                                                                                    
     121    2021-05-01      26      19.7303       .                                                                                    
     122    2021-05-02      90      22.4085       .                                                                                    
     123    2021-05-03      39      21.7442       .                                                                                    
     124    2021-05-04      93      27.6035       .                                                                                    
     125    2021-05-05      12      31.5851       .                                                                                    
     126    2021-05-06      98      35.6651       .                                                                                    
     ...                                                                                                                               
     ...                                             All have data                                                                     
     ...                                                                                                                               
     469    2022-04-14      78      25.4147       .                                                                                    
     470    2022-04-15      82      25.4147       .                                                                                    
     471    2022-04-16      83      19.8974       .                                                                                    
     472    2022-04-17       9      28.6473       .                                                                                    
     473    2022-04-18      41      29.3590       .                                                                                    
     474    2022-04-19      95      32.8728       .                                                                                    
                                                                                                                                       
     475    2022-04-20      59      30.0579     29.22  * 365 consecutive days                                                          
                                                       * 2021-4-21 to 20224-4-20                                                       
                                                                                                                                       
     476    2022-04-21      49      29.7809     29.16  * next 365 days                                                                 
     477    2022-04-22       4      34.3019     29.20                                                                                  
    ...                                                                                                                                
    ...                                                                                                                                
    1094    2023-12-30      56      21.6751     29.11                                                                                  
    1095    2023-12-31      76      28.9071     29.04                                                                                  
    1096    2024-01-01       3      30.7215     29.02                                                                                  
                                                                                                                                       
    *                                                                                                                                  
     _ __  _ __ ___   ___ ___  ___ ___                                                                                                 
    | '_ \| '__/ _ \ / __/ _ \/ __/ __|                                                                                                
    | |_) | | | (_) | (_|  __/\__ \__ \                                                                                                
    | .__/|_|  \___/ \___\___||___/___/                                                                                                
    |_|                                                                                                                                
    ;                                                                                                                                  
                                                                                                                                       
    %utl_submit_r64('                                                                                                                  
    library(tidyr);                                                                                                                    
    library(zoo);                                                                                                                      
    library(haven);                                                                                                                    
    library(RcppRoll);                                                                                                                 
    library(SASxport);                                                                                                                 
    have<-read_sas("d:/sd1/have.sas7bdat");                                                                                            
    want<- complete(have, DAY = full_seq(DAY, 1));                                                                                     
    want$WEEK <-c(rep(NA,6),roll_sd(want$RETURN, 7));                                                                                  
    want$ANNUAL <-c(rep(NA,364),roll_sd(want$RETURN, 365));                                                                            
    write.xport(want,file="d:/xpt/want.xpt");                                                                                          
    ');                                                                                                                                
                                                                                                                                       
    libname xpt xport "d:/xpt/want.xpt";                                                                                               
    data want ;                                                                                                                        
      format day yymmdd10.;                                                                                                            
      set xpt.want;                                                                                                                    
    run;quit;                                                                                                                          
    libname xpt clear;                                                                                                                 
                                                                                                                                       
    proc print data=want width=min;                                                                                                    
    run;quit;                                                                                                                          
                                                                                                                                       
