# utl-transpose-macro-eliminates-five-steps-transpose-sort-transpose-sort-merge
Transpose macro eliminates five steps transpose sort transpose sort merge   

    Transpose macro eliminates five steps transpose sort transpose sort merge                                                     
    and it is fast                                                                                                                
                                                                                                                                  
      * AUTHORS: Arthur Tabachneck, Xia Ke Shan, Robert Virgile and Joe Whitehurst                                                
      * CREATED: January 20, 2013                                                                                                 
      * MODIFIED: September 4, 2014                                                                                               
      * MODIFIED: September 15, 2017                                                                                              
                                                                                                                                  
                                                                                                                                  
    github                                                                                                                        
    https://tinyurl.com/y8sbkuez                                                                                                  
    https://github.com/rogerjdeangelis/utl-transpose-macro-eliminates-five-steps-transpose-sort-transpose-sort-merge              
                                                                                                                                  
    SAS Forum                                                                                                                     
    https://tinyurl.com/y9n2pauu                                                                                                  
    https://communities.sas.com/t5/New-SAS-User/Consolidating-Rows-by-Creating-Additional-Columns/m-p/530270                      
                                                                                                                                  
    macros                                                                                                                        
    https://tinyurl.com/y9nfugth                                                                                                  
    https://github.com/rogerjdeangelis/utl-macros-used-in-many-of-rogerjdeangelis-repositories                                    
                                                                                                                                  
                                                                                                                                  
    INPUT                                                                                                                         
    =====                                                                                                                         
                                                                                                                                  
    data have;                                                                                                                    
      input Policy ID Percent percent.;                                                                                           
    cards4;                                                                                                                       
    12345 12 5%                                                                                                                   
    12345 15 5%                                                                                                                   
    23467 23 6%                                                                                                                   
    23467 24 7%                                                                                                                   
    35678 30 5%                                                                                                                   
    ;;;;                                                                                                                          
    run;quit;                                                                                                                     
                                                                                                                                  
    WORK.HAVE total obs=5                                                                                                         
                                                                                                                                  
      POLICY    ID    PERCENT                                                                                                     
                                                                                                                                  
       12345    12      0.05                                                                                                      
       12345    15      0.05                                                                                                      
       23467    23      0.06                                                                                                      
       23467    24      0.07                                                                                                      
       35678    30      0.05                                                                                                      
                                                                                                                                  
    EXAMPLE OUTPUT                                                                                                                
    --------------                                                                                                                
                                                                                                                                  
     WORK.otal obs=3                                                                                                              
                                                                                                                                  
      POLICY    ID_1    PERCENT_1   ID_2    PERCENT_2                                                                             
                                                                                                                                  
       12345     12       0.05       15       0.05                                                                                
       23467     23       0.06       24       0.07                                                                                
       35678     30       0.05        .        .                                                                                  
                                                                                                                                  
    PROCESS                                                                                                                       
    =======                                                                                                                       
                                                                                                                                  
    %utl_transpose(data=have, out=want, by=policy,  delimiter=_,var=id percent);                                                  
                                                                                                                                  
    THATS ALL FOLKS!!!                                                                                                            
                                                                                                                                  
    POSTED SOLUTION                                                                                                               
    ---------------                                                                                                               
                                                                                                                                  
    proc transpose data=have out=want1(drop=_name_) prefix=ID;                                                                    
          by Policy policy=acx notsorted;                                                                                         
          var id;                                                                                                                 
    run;                                                                                                                          
    proc sort data=want1;                                                                                                         
          by policy;                                                                                                              
    run;                                                                                                                          
    proc transpose data=have out=want2(drop=_name_) prefix=percent;                                                               
          by Policy notsorted;                                                                                                    
          var percent;                                                                                                            
    run;                                                                                                                          
    proc sort data=want2;                                                                                                         
          by policy;                                                                                                              
    run;                                                                                                                          
    data wanted;                                                                                                                  
          merge want1 want2;                                                                                                      
          by policy;                                                                                                              
    run;                                                                                                                          
                                                                                                                                  
                                                                                                                                  
