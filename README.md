# utl-lagging-variables-without-renaming-the-variables
Lagging variables without renaming the variables
    Lagging variables without renaming the variables                                                          
                                                                                                              
    Solution uses a temporary array                                                                           
                                                                                                              
    github                                                                                                    
    https://tinyurl.com/y6ctslls                                                                              
    https://github.com/rogerjdeangelis/utl-lagging-variables-without-renaming-the-variables                   
                                                                                                              
    Inspired by                                                                                               
    https://tinyurl.com/y2n46va5                                                                              
    https://communities.sas.com/t5/SAS-Programming/Interaction-of-LAG-and-OUTPUT-in-DATA/m-p/545354           
                                                                                                              
    For an excellent explantion of FIFO ques                                                                  
    freelance Reinhard                                                                                        
    https://communities.sas.com/t5/user/viewprofilepage/user-id/32733                                         
                                                                                                              
    *                _     _                                                                                  
     _ __  _ __ ___ | |__ | | ___ _ __ ___                                                                    
    | '_ \| '__/ _ \| '_ \| |/ _ \ '_ ` _ \                                                                   
    | |_) | | | (_) | |_) | |  __/ | | | | |                                                                  
    | .__/|_|  \___/|_.__/|_|\___|_| |_| |_|                                                                  
    |_|                                                                                                       
    ;                                                                                                         
                                                                                                              
                                                                                                              
    data have;                                                                                                
      do i=1 to 15;                                                                                           
        val1=int(10*uniform(1233));                                                                           
        val2=val1;                                                                                            
        val3=val1;                                                                                            
        if mod(i,3) ne 0 and i ne 1 then val1=.;                                                              
        if mod(i,4) ne 0 and i ne 1 then val2=.;                                                              
        if mod(i,5) ne 0 and i ne 1 then val3=.;                                                              
        output;                                                                                               
      end;                                                                                                    
    run;quit;                                                                                                 
                                                                                                              
    data want;                                                                                                
        set have;                                                                                             
        if val1=. then val1=lag(val1);                                                                        
        if val2=. then val2=lag(val1);                                                                        
        if val3=. then val3=lag(val1);                                                                        
    run;quit;                                                                                                 
                                                                                                              
                                                                                                              
    ======                                                                                                    
    YEILDS                                                                                                    
    ======                                                                                                    
                                                                                                              
                                                                                                              
    Up to 40 obs WORK.WANT total obs=15                                                                       
                                                                                                              
    Obs     I    VAL1    VAL2    VAL3                                                                         
                                                                                                              
      1     1      0       0       0                                                                          
      2     2      .       .       .                                                                          
      3     3      9       .       .                                                                          
      4     4      .       8       9                                                                          
      5     5      .       9       3                                                                          
      6     6      1       .       .                                                                          
      7     7      .       1       1                                                                          
      8     8      .       5       .                                                                          
      9     9      3       .       .                                                                          
     10    10      .       3       0                                                                          
     11    11      .       .       3                                                                          
     12    12      9       9       .                                                                          
     13    13      .       .       9                                                                          
     14    14      .       .       .                                                                          
     15    15      0       .       0                                                                          
                                                                                                              
                                                                                                              
    ==========                                                                                                
    BUT I WANT                                                                                                
    ==========                                                                                                
                                                                                                              
    Up to 40 obs WORK.WANT total obs=15                                                                       
                                                                                                              
    Obs    I    VAL1    VAL2    VAL3                                                                          
                                                                                                              
      1    4      0       0       0                                                                           
      2    4      0       0       0                                                                           
      3    4      9       0       0                                                                           
      4    4      9       8       0                                                                           
      5    4      9       8       3                                                                           
      6    4      1       8       3                                                                           
      7    4      1       8       3                                                                           
      8    4      1       5       3                                                                           
      9    4      3       5       3                                                                           
     10    4      3       5       0                                                                           
     11    4      3       5       0                                                                           
     12    4      9       9       0                                                                           
     13    4      9       9       0                                                                           
     14    4      9       9       0                                                                           
     15    4      0       9       0                                                                           
                                                                                                              
                                                                                                              
    *          _       _   _                                                                                  
     ___  ___ | |_   _| |_(_) ___  _ __                                                                       
    / __|/ _ \| | | | | __| |/ _ \| '_ \                                                                      
    \__ \ (_) | | |_| | |_| | (_) | | | |                                                                     
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|                                                                     
                                                                                                              
    ;                                                                                                         
                                                                                                              
    data want;                                                                                                
        set have;                                                                                             
        array vals val1 val2 val3;                                                                            
        array fils[3] _temporary_ ;                                                                           
        do i=1 to 3;                                                                                          
           if vals[i] ne . then fils[i]=vals[i];                                                              
           vals[i]=fils[i];                                                                                   
        end;                                                                                                  
    run;quit;                                                                                                 
                                                                                                              
    *            _               _                                                                            
      ___  _   _| |_ _ __  _   _| |_                                                                          
     / _ \| | | | __| '_ \| | | | __|                                                                         
    | (_) | |_| | |_| |_) | |_| | |_                                                                          
     \___/ \__,_|\__| .__/ \__,_|\__|                                                                         
                    |_|                                                                                       
    ;                                                                                                         
                                                                                                              
    Up to 40 obs WORK.WANT total obs=15                                                                       
                                                                                                              
    Obs    I    VAL1    VAL2    VAL3                                                                          
                                                                                                              
      1    4      0       0       0                                                                           
      2    4      0       0       0                                                                           
      3    4      9       0       0                                                                           
      4    4      9       8       0                                                                           
      5    4      9       8       3                                                                           
      6    4      1       8       3                                                                           
      7    4      1       8       3                                                                           
      8    4      1       5       3                                                                           
      9    4      3       5       3                                                                           
     10    4      3       5       0                                                                           
     11    4      3       5       0                                                                           
     12    4      9       9       0                                                                           
     13    4      9       9       0                                                                           
     14    4      9       9       0                                                                           
     15    4      0       9       0                                                                           
                                                                                                              
