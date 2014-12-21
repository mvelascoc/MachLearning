---
title: ???exploratory.md"
author: "Maria Velasco"
date: "20 Dec 2014"
output: html_document
---
**Data Analysis for Reproducing Qualitative Activity Recognition of Weight Lifting Exercises**
Maria Velasco, Dec 21 2014

Note
This document includes the exploratory data analysisi done for the Practical Machine Learning - Coursera Class Dec 2014 Project by mvelasco, published in https://github.com/mvelascoc/MachLearning

References
Data for this project comes from http://groupware.les.inf.puc-rio.br/har. 
Velloso, E.; Bulling, A.; Gellersen, H.; Ugulino, W.; Fuks, H. Qualitative Activity Recognition of Weight Lifting Exercises. Proceedings of 4th International Conference in Cooperation with SIGCHI (Augmented Human '13) . Stuttgart, Germany: ACM SIGCHI, 2013.

*Reviewing the data*
Data for the project includes 160 variables. In order to review them and find the significant ones, the following summaries and graphics were useful.

```r
seed(897)
```

```
## Error: no se pudo encontrar la funci'on "seed"
```

```r
traindata <- read.csv("pml-training.csv")
testdata <- read.csv("pml-testing.csv")

#reviewing types of data and selecting initial set of numeric and significant variables
summary(traindata)
```

```
##        X            user_name    raw_timestamp_part_1 raw_timestamp_part_2
##  Min.   :    1   adelmo  :3892   Min.   :1.32e+09     Min.   :   294      
##  1st Qu.: 4906   carlitos:3112   1st Qu.:1.32e+09     1st Qu.:252912      
##  Median : 9812   charles :3536   Median :1.32e+09     Median :496380      
##  Mean   : 9812   eurico  :3070   Mean   :1.32e+09     Mean   :500656      
##  3rd Qu.:14717   jeremy  :3402   3rd Qu.:1.32e+09     3rd Qu.:751891      
##  Max.   :19622   pedro   :2610   Max.   :1.32e+09     Max.   :998801      
##                                                                           
##           cvtd_timestamp  new_window    num_window    roll_belt    
##  28/11/2011 14:14: 1498   no :19216   Min.   :  1   Min.   :-28.9  
##  05/12/2011 11:24: 1497   yes:  406   1st Qu.:222   1st Qu.:  1.1  
##  30/11/2011 17:11: 1440               Median :424   Median :113.0  
##  05/12/2011 11:25: 1425               Mean   :431   Mean   : 64.4  
##  02/12/2011 14:57: 1380               3rd Qu.:644   3rd Qu.:123.0  
##  02/12/2011 13:34: 1375               Max.   :864   Max.   :162.0  
##  (Other)         :11007                                            
##    pitch_belt        yaw_belt      total_accel_belt kurtosis_roll_belt
##  Min.   :-55.80   Min.   :-180.0   Min.   : 0.0              :19216   
##  1st Qu.:  1.76   1st Qu.: -88.3   1st Qu.: 3.0     #DIV/0!  :   10   
##  Median :  5.28   Median : -13.0   Median :17.0     -1.908453:    2   
##  Mean   :  0.31   Mean   : -11.2   Mean   :11.3     -0.016850:    1   
##  3rd Qu.: 14.90   3rd Qu.:  12.9   3rd Qu.:18.0     -0.021024:    1   
##  Max.   : 60.30   Max.   : 179.0   Max.   :29.0     -0.025513:    1   
##                                                     (Other)  :  391   
##  kurtosis_picth_belt kurtosis_yaw_belt skewness_roll_belt
##           :19216            :19216              :19216   
##  #DIV/0!  :   32     #DIV/0!:  406     #DIV/0!  :    9   
##  47.000000:    4                       0.000000 :    4   
##  -0.150950:    3                       0.422463 :    2   
##  -0.684748:    3                       -0.003095:    1   
##  -1.750749:    3                       -0.010002:    1   
##  (Other)  :  361                       (Other)  :  389   
##  skewness_roll_belt.1 skewness_yaw_belt max_roll_belt   max_picth_belt 
##           :19216             :19216     Min.   :-94     Min.   : 3     
##  #DIV/0!  :   32      #DIV/0!:  406     1st Qu.:-88     1st Qu.: 5     
##  0.000000 :    4                        Median : -5     Median :18     
##  -2.156553:    3                        Mean   : -7     Mean   :13     
##  -3.072669:    3                        3rd Qu.: 18     3rd Qu.:19     
##  -6.324555:    3                        Max.   :180     Max.   :30     
##  (Other)  :  361                        NA's   :19216   NA's   :19216  
##   max_yaw_belt   min_roll_belt   min_pitch_belt   min_yaw_belt  
##         :19216   Min.   :-180    Min.   : 0             :19216  
##  -1.1   :   30   1st Qu.: -88    1st Qu.: 3      -1.1   :   30  
##  -1.4   :   29   Median :  -8    Median :16      -1.4   :   29  
##  -1.2   :   26   Mean   : -10    Mean   :11      -1.2   :   26  
##  -0.9   :   24   3rd Qu.:   9    3rd Qu.:17      -0.9   :   24  
##  -1.3   :   22   Max.   : 173    Max.   :23      -1.3   :   22  
##  (Other):  275   NA's   :19216   NA's   :19216   (Other):  275  
##  amplitude_roll_belt amplitude_pitch_belt amplitude_yaw_belt
##  Min.   :  0         Min.   : 0                  :19216     
##  1st Qu.:  0         1st Qu.: 1           #DIV/0!:   10     
##  Median :  1         Median : 1           0.00   :   12     
##  Mean   :  4         Mean   : 2           0.0000 :  384     
##  3rd Qu.:  2         3rd Qu.: 2                             
##  Max.   :360         Max.   :12                             
##  NA's   :19216       NA's   :19216                          
##  var_total_accel_belt avg_roll_belt   stddev_roll_belt var_roll_belt  
##  Min.   : 0           Min.   :-27     Min.   : 0       Min.   :  0    
##  1st Qu.: 0           1st Qu.:  1     1st Qu.: 0       1st Qu.:  0    
##  Median : 0           Median :116     Median : 0       Median :  0    
##  Mean   : 1           Mean   : 68     Mean   : 1       Mean   :  8    
##  3rd Qu.: 0           3rd Qu.:123     3rd Qu.: 1       3rd Qu.:  0    
##  Max.   :16           Max.   :157     Max.   :14       Max.   :201    
##  NA's   :19216        NA's   :19216   NA's   :19216    NA's   :19216  
##  avg_pitch_belt  stddev_pitch_belt var_pitch_belt   avg_yaw_belt  
##  Min.   :-51     Min.   :0         Min.   : 0      Min.   :-138   
##  1st Qu.:  2     1st Qu.:0         1st Qu.: 0      1st Qu.: -88   
##  Median :  5     Median :0         Median : 0      Median :  -7   
##  Mean   :  1     Mean   :1         Mean   : 1      Mean   :  -9   
##  3rd Qu.: 16     3rd Qu.:1         3rd Qu.: 0      3rd Qu.:  14   
##  Max.   : 60     Max.   :4         Max.   :16      Max.   : 174   
##  NA's   :19216   NA's   :19216     NA's   :19216   NA's   :19216  
##  stddev_yaw_belt  var_yaw_belt    gyros_belt_x      gyros_belt_y    
##  Min.   :  0     Min.   :    0   Min.   :-1.0400   Min.   :-0.6400  
##  1st Qu.:  0     1st Qu.:    0   1st Qu.:-0.0300   1st Qu.: 0.0000  
##  Median :  0     Median :    0   Median : 0.0300   Median : 0.0200  
##  Mean   :  1     Mean   :  107   Mean   :-0.0056   Mean   : 0.0396  
##  3rd Qu.:  1     3rd Qu.:    0   3rd Qu.: 0.1100   3rd Qu.: 0.1100  
##  Max.   :177     Max.   :31183   Max.   : 2.2200   Max.   : 0.6400  
##  NA's   :19216   NA's   :19216                                      
##   gyros_belt_z     accel_belt_x      accel_belt_y    accel_belt_z   
##  Min.   :-1.460   Min.   :-120.00   Min.   :-69.0   Min.   :-275.0  
##  1st Qu.:-0.200   1st Qu.: -21.00   1st Qu.:  3.0   1st Qu.:-162.0  
##  Median :-0.100   Median : -15.00   Median : 35.0   Median :-152.0  
##  Mean   :-0.131   Mean   :  -5.59   Mean   : 30.1   Mean   : -72.6  
##  3rd Qu.:-0.020   3rd Qu.:  -5.00   3rd Qu.: 61.0   3rd Qu.:  27.0  
##  Max.   : 1.620   Max.   :  85.00   Max.   :164.0   Max.   : 105.0  
##                                                                     
##  magnet_belt_x   magnet_belt_y magnet_belt_z     roll_arm     
##  Min.   :-52.0   Min.   :354   Min.   :-623   Min.   :-180.0  
##  1st Qu.:  9.0   1st Qu.:581   1st Qu.:-375   1st Qu.: -31.8  
##  Median : 35.0   Median :601   Median :-320   Median :   0.0  
##  Mean   : 55.6   Mean   :594   Mean   :-346   Mean   :  17.8  
##  3rd Qu.: 59.0   3rd Qu.:610   3rd Qu.:-306   3rd Qu.:  77.3  
##  Max.   :485.0   Max.   :673   Max.   : 293   Max.   : 180.0  
##                                                               
##    pitch_arm         yaw_arm        total_accel_arm var_accel_arm  
##  Min.   :-88.80   Min.   :-180.00   Min.   : 1.0    Min.   :  0    
##  1st Qu.:-25.90   1st Qu.: -43.10   1st Qu.:17.0    1st Qu.:  9    
##  Median :  0.00   Median :   0.00   Median :27.0    Median : 41    
##  Mean   : -4.61   Mean   :  -0.62   Mean   :25.5    Mean   : 53    
##  3rd Qu.: 11.20   3rd Qu.:  45.88   3rd Qu.:33.0    3rd Qu.: 76    
##  Max.   : 88.50   Max.   : 180.00   Max.   :66.0    Max.   :332    
##                                                     NA's   :19216  
##   avg_roll_arm   stddev_roll_arm  var_roll_arm   avg_pitch_arm  
##  Min.   :-167    Min.   :  0     Min.   :    0   Min.   :-82    
##  1st Qu.: -38    1st Qu.:  1     1st Qu.:    2   1st Qu.:-23    
##  Median :   0    Median :  6     Median :   33   Median :  0    
##  Mean   :  13    Mean   : 11     Mean   :  417   Mean   : -5    
##  3rd Qu.:  76    3rd Qu.: 15     3rd Qu.:  223   3rd Qu.:  8    
##  Max.   : 163    Max.   :162     Max.   :26232   Max.   : 76    
##  NA's   :19216   NA's   :19216   NA's   :19216   NA's   :19216  
##  stddev_pitch_arm var_pitch_arm    avg_yaw_arm    stddev_yaw_arm 
##  Min.   : 0       Min.   :   0    Min.   :-173    Min.   :  0    
##  1st Qu.: 2       1st Qu.:   3    1st Qu.: -29    1st Qu.:  3    
##  Median : 8       Median :  66    Median :   0    Median : 17    
##  Mean   :10       Mean   : 196    Mean   :   2    Mean   : 22    
##  3rd Qu.:16       3rd Qu.: 267    3rd Qu.:  38    3rd Qu.: 36    
##  Max.   :43       Max.   :1885    Max.   : 152    Max.   :177    
##  NA's   :19216    NA's   :19216   NA's   :19216   NA's   :19216  
##   var_yaw_arm     gyros_arm_x      gyros_arm_y      gyros_arm_z   
##  Min.   :    0   Min.   :-6.370   Min.   :-3.440   Min.   :-2.33  
##  1st Qu.:    7   1st Qu.:-1.330   1st Qu.:-0.800   1st Qu.:-0.07  
##  Median :  278   Median : 0.080   Median :-0.240   Median : 0.23  
##  Mean   : 1056   Mean   : 0.043   Mean   :-0.257   Mean   : 0.27  
##  3rd Qu.: 1295   3rd Qu.: 1.570   3rd Qu.: 0.140   3rd Qu.: 0.72  
##  Max.   :31345   Max.   : 4.870   Max.   : 2.840   Max.   : 3.02  
##  NA's   :19216                                                    
##   accel_arm_x      accel_arm_y      accel_arm_z      magnet_arm_x 
##  Min.   :-404.0   Min.   :-318.0   Min.   :-636.0   Min.   :-584  
##  1st Qu.:-242.0   1st Qu.: -54.0   1st Qu.:-143.0   1st Qu.:-300  
##  Median : -44.0   Median :  14.0   Median : -47.0   Median : 289  
##  Mean   : -60.2   Mean   :  32.6   Mean   : -71.2   Mean   : 192  
##  3rd Qu.:  84.0   3rd Qu.: 139.0   3rd Qu.:  23.0   3rd Qu.: 637  
##  Max.   : 437.0   Max.   : 308.0   Max.   : 292.0   Max.   : 782  
##                                                                   
##   magnet_arm_y   magnet_arm_z  kurtosis_roll_arm kurtosis_picth_arm
##  Min.   :-392   Min.   :-597           :19216            :19216    
##  1st Qu.:  -9   1st Qu.: 131   #DIV/0! :   78    #DIV/0! :   80    
##  Median : 202   Median : 444   -0.02438:    1    -0.00484:    1    
##  Mean   : 157   Mean   : 306   -0.04190:    1    -0.01311:    1    
##  3rd Qu.: 323   3rd Qu.: 545   -0.05051:    1    -0.02967:    1    
##  Max.   : 583   Max.   : 694   -0.05695:    1    -0.07394:    1    
##                                (Other) :  324    (Other) :  322    
##  kurtosis_yaw_arm skewness_roll_arm skewness_pitch_arm skewness_yaw_arm
##          :19216           :19216            :19216             :19216  
##  #DIV/0! :   11   #DIV/0! :   77    #DIV/0! :   80     #DIV/0! :   11  
##  0.55844 :    2   -0.00051:    1    -0.00184:    1     -1.62032:    2  
##  0.65132 :    2   -0.00696:    1    -0.01185:    1     0.55053 :    2  
##  -0.01548:    1   -0.01884:    1    -0.01247:    1     -0.00311:    1  
##  -0.01749:    1   -0.03359:    1    -0.02063:    1     -0.00562:    1  
##  (Other) :  389   (Other) :  325    (Other) :  322     (Other) :  389  
##   max_roll_arm   max_picth_arm    max_yaw_arm     min_roll_arm  
##  Min.   :-73     Min.   :-173    Min.   : 4      Min.   :-89    
##  1st Qu.:  0     1st Qu.:  -2    1st Qu.:29      1st Qu.:-42    
##  Median :  5     Median :  23    Median :34      Median :-22    
##  Mean   : 11     Mean   :  36    Mean   :35      Mean   :-21    
##  3rd Qu.: 27     3rd Qu.:  96    3rd Qu.:41      3rd Qu.:  0    
##  Max.   : 86     Max.   : 180    Max.   :65      Max.   : 66    
##  NA's   :19216   NA's   :19216   NA's   :19216   NA's   :19216  
##  min_pitch_arm    min_yaw_arm    amplitude_roll_arm amplitude_pitch_arm
##  Min.   :-180    Min.   : 1      Min.   :  0        Min.   :  0        
##  1st Qu.: -73    1st Qu.: 8      1st Qu.:  5        1st Qu.: 10        
##  Median : -34    Median :13      Median : 28        Median : 55        
##  Mean   : -34    Mean   :15      Mean   : 32        Mean   : 70        
##  3rd Qu.:   0    3rd Qu.:19      3rd Qu.: 51        3rd Qu.:115        
##  Max.   : 152    Max.   :38      Max.   :120        Max.   :360        
##  NA's   :19216   NA's   :19216   NA's   :19216      NA's   :19216      
##  amplitude_yaw_arm roll_dumbbell    pitch_dumbbell    yaw_dumbbell    
##  Min.   : 0        Min.   :-153.7   Min.   :-149.6   Min.   :-150.87  
##  1st Qu.:13        1st Qu.: -18.5   1st Qu.: -40.9   1st Qu.: -77.64  
##  Median :22        Median :  48.2   Median : -21.0   Median :  -3.32  
##  Mean   :21        Mean   :  23.8   Mean   : -10.8   Mean   :   1.67  
##  3rd Qu.:29        3rd Qu.:  67.6   3rd Qu.:  17.5   3rd Qu.:  79.64  
##  Max.   :52        Max.   : 153.6   Max.   : 149.4   Max.   : 154.95  
##  NA's   :19216                                                        
##  kurtosis_roll_dumbbell kurtosis_picth_dumbbell kurtosis_yaw_dumbbell
##         :19216                 :19216                  :19216        
##  #DIV/0!:    5          #DIV/0!:    2           #DIV/0!:  406        
##  -0.2583:    2          -0.5464:    2                                
##  -0.3705:    2          -0.9334:    2                                
##  -0.5855:    2          -2.0833:    2                                
##  -2.0851:    2          -2.0851:    2                                
##  (Other):  393          (Other):  396                                
##  skewness_roll_dumbbell skewness_pitch_dumbbell skewness_yaw_dumbbell
##         :19216                 :19216                  :19216        
##  #DIV/0!:    4          -0.2328:    2           #DIV/0!:  406        
##  -0.9324:    2          -0.3521:    2                                
##  0.1110 :    2          -0.7036:    2                                
##  1.0312 :    2          0.1090 :    2                                
##  -0.0082:    1          1.0326 :    2                                
##  (Other):  395          (Other):  396                                
##  max_roll_dumbbell max_picth_dumbbell max_yaw_dumbbell min_roll_dumbbell
##  Min.   :-70       Min.   :-113              :19216    Min.   :-150     
##  1st Qu.:-27       1st Qu.: -67       -0.6   :   20    1st Qu.: -60     
##  Median : 15       Median :  40       0.2    :   19    Median : -44     
##  Mean   : 14       Mean   :  33       -0.8   :   18    Mean   : -41     
##  3rd Qu.: 51       3rd Qu.: 133       -0.3   :   16    3rd Qu.: -25     
##  Max.   :137       Max.   : 155       -0.2   :   15    Max.   :  73     
##  NA's   :19216     NA's   :19216      (Other):  318    NA's   :19216    
##  min_pitch_dumbbell min_yaw_dumbbell amplitude_roll_dumbbell
##  Min.   :-147              :19216    Min.   :  0            
##  1st Qu.: -92       -0.6   :   20    1st Qu.: 15            
##  Median : -66       0.2    :   19    Median : 35            
##  Mean   : -33       -0.8   :   18    Mean   : 55            
##  3rd Qu.:  21       -0.3   :   16    3rd Qu.: 81            
##  Max.   : 121       -0.2   :   15    Max.   :256            
##  NA's   :19216      (Other):  318    NA's   :19216          
##  amplitude_pitch_dumbbell amplitude_yaw_dumbbell total_accel_dumbbell
##  Min.   :  0                     :19216          Min.   : 0.0        
##  1st Qu.: 17              #DIV/0!:    5          1st Qu.: 4.0        
##  Median : 42              0.00   :  401          Median :10.0        
##  Mean   : 66                                     Mean   :13.7        
##  3rd Qu.:100                                     3rd Qu.:19.0        
##  Max.   :274                                     Max.   :58.0        
##  NA's   :19216                                                       
##  var_accel_dumbbell avg_roll_dumbbell stddev_roll_dumbbell
##  Min.   :  0        Min.   :-129      Min.   :  0         
##  1st Qu.:  0        1st Qu.: -12      1st Qu.:  5         
##  Median :  1        Median :  48      Median : 12         
##  Mean   :  4        Mean   :  24      Mean   : 21         
##  3rd Qu.:  3        3rd Qu.:  64      3rd Qu.: 26         
##  Max.   :230        Max.   : 126      Max.   :124         
##  NA's   :19216      NA's   :19216     NA's   :19216       
##  var_roll_dumbbell avg_pitch_dumbbell stddev_pitch_dumbbell
##  Min.   :    0     Min.   :-71        Min.   : 0           
##  1st Qu.:   22     1st Qu.:-42        1st Qu.: 3           
##  Median :  149     Median :-20        Median : 8           
##  Mean   : 1020     Mean   :-12        Mean   :13           
##  3rd Qu.:  695     3rd Qu.: 13        3rd Qu.:19           
##  Max.   :15321     Max.   : 94        Max.   :83           
##  NA's   :19216     NA's   :19216      NA's   :19216        
##  var_pitch_dumbbell avg_yaw_dumbbell stddev_yaw_dumbbell var_yaw_dumbbell
##  Min.   :   0       Min.   :-118     Min.   :  0         Min.   :    0   
##  1st Qu.:  12       1st Qu.: -77     1st Qu.:  4         1st Qu.:   15   
##  Median :  65       Median :  -5     Median : 10         Median :  105   
##  Mean   : 350       Mean   :   0     Mean   : 17         Mean   :  590   
##  3rd Qu.: 370       3rd Qu.:  71     3rd Qu.: 25         3rd Qu.:  609   
##  Max.   :6836       Max.   : 135     Max.   :107         Max.   :11468   
##  NA's   :19216      NA's   :19216    NA's   :19216       NA's   :19216   
##  gyros_dumbbell_x  gyros_dumbbell_y gyros_dumbbell_z accel_dumbbell_x
##  Min.   :-204.00   Min.   :-2.10    Min.   : -2.4    Min.   :-419.0  
##  1st Qu.:  -0.03   1st Qu.:-0.14    1st Qu.: -0.3    1st Qu.: -50.0  
##  Median :   0.13   Median : 0.03    Median : -0.1    Median :  -8.0  
##  Mean   :   0.16   Mean   : 0.05    Mean   : -0.1    Mean   : -28.6  
##  3rd Qu.:   0.35   3rd Qu.: 0.21    3rd Qu.:  0.0    3rd Qu.:  11.0  
##  Max.   :   2.22   Max.   :52.00    Max.   :317.0    Max.   : 235.0  
##                                                                      
##  accel_dumbbell_y accel_dumbbell_z magnet_dumbbell_x magnet_dumbbell_y
##  Min.   :-189.0   Min.   :-334.0   Min.   :-643      Min.   :-3600    
##  1st Qu.:  -8.0   1st Qu.:-142.0   1st Qu.:-535      1st Qu.:  231    
##  Median :  41.5   Median :  -1.0   Median :-479      Median :  311    
##  Mean   :  52.6   Mean   : -38.3   Mean   :-328      Mean   :  221    
##  3rd Qu.: 111.0   3rd Qu.:  38.0   3rd Qu.:-304      3rd Qu.:  390    
##  Max.   : 315.0   Max.   : 318.0   Max.   : 592      Max.   :  633    
##                                                                       
##  magnet_dumbbell_z  roll_forearm     pitch_forearm     yaw_forearm    
##  Min.   :-262.0    Min.   :-180.00   Min.   :-72.50   Min.   :-180.0  
##  1st Qu.: -45.0    1st Qu.:  -0.74   1st Qu.:  0.00   1st Qu.: -68.6  
##  Median :  13.0    Median :  21.70   Median :  9.24   Median :   0.0  
##  Mean   :  46.1    Mean   :  33.83   Mean   : 10.71   Mean   :  19.2  
##  3rd Qu.:  95.0    3rd Qu.: 140.00   3rd Qu.: 28.40   3rd Qu.: 110.0  
##  Max.   : 452.0    Max.   : 180.00   Max.   : 89.80   Max.   : 180.0  
##                                                                       
##  kurtosis_roll_forearm kurtosis_picth_forearm kurtosis_yaw_forearm
##         :19216                :19216                 :19216       
##  #DIV/0!:   84         #DIV/0!:   85          #DIV/0!:  406       
##  -0.8079:    2         -0.0073:    1                              
##  -0.9169:    2         -0.0442:    1                              
##  -0.0227:    1         -0.0489:    1                              
##  -0.0359:    1         -0.0523:    1                              
##  (Other):  316         (Other):  317                              
##  skewness_roll_forearm skewness_pitch_forearm skewness_yaw_forearm
##         :19216                :19216                 :19216       
##  #DIV/0!:   83         #DIV/0!:   85          #DIV/0!:  406       
##  -0.1912:    2         0.0000 :    4                              
##  -0.4126:    2         -0.6992:    2                              
##  -0.0004:    1         -0.0113:    1                              
##  -0.0013:    1         -0.0131:    1                              
##  (Other):  317         (Other):  313                              
##  max_roll_forearm max_picth_forearm max_yaw_forearm min_roll_forearm
##  Min.   :-67      Min.   :-151             :19216   Min.   :-72     
##  1st Qu.:  0      1st Qu.:   0      #DIV/0!:   84   1st Qu.: -6     
##  Median : 27      Median : 113      -1.2   :   32   Median :  0     
##  Mean   : 24      Mean   :  81      -1.3   :   31   Mean   :  0     
##  3rd Qu.: 46      3rd Qu.: 175      -1.4   :   24   3rd Qu.: 12     
##  Max.   : 90      Max.   : 180      -1.5   :   24   Max.   : 62     
##  NA's   :19216    NA's   :19216     (Other):  211   NA's   :19216   
##  min_pitch_forearm min_yaw_forearm amplitude_roll_forearm
##  Min.   :-180             :19216   Min.   :  0           
##  1st Qu.:-175      #DIV/0!:   84   1st Qu.:  1           
##  Median : -61      -1.2   :   32   Median : 18           
##  Mean   : -58      -1.3   :   31   Mean   : 25           
##  3rd Qu.:   0      -1.4   :   24   3rd Qu.: 40           
##  Max.   : 167      -1.5   :   24   Max.   :126           
##  NA's   :19216     (Other):  211   NA's   :19216         
##  amplitude_pitch_forearm amplitude_yaw_forearm total_accel_forearm
##  Min.   :  0                    :19216         Min.   :  0.0      
##  1st Qu.:  2             #DIV/0!:   84         1st Qu.: 29.0      
##  Median : 84             0.00   :  322         Median : 36.0      
##  Mean   :139                                   Mean   : 34.7      
##  3rd Qu.:350                                   3rd Qu.: 41.0      
##  Max.   :360                                   Max.   :108.0      
##  NA's   :19216                                                    
##  var_accel_forearm avg_roll_forearm stddev_roll_forearm var_roll_forearm
##  Min.   :  0       Min.   :-177     Min.   :  0         Min.   :    0   
##  1st Qu.:  7       1st Qu.:  -1     1st Qu.:  0         1st Qu.:    0   
##  Median : 21       Median :  11     Median :  8         Median :   64   
##  Mean   : 34       Mean   :  33     Mean   : 42         Mean   : 5274   
##  3rd Qu.: 51       3rd Qu.: 107     3rd Qu.: 85         3rd Qu.: 7289   
##  Max.   :173       Max.   : 177     Max.   :179         Max.   :32102   
##  NA's   :19216     NA's   :19216    NA's   :19216       NA's   :19216   
##  avg_pitch_forearm stddev_pitch_forearm var_pitch_forearm avg_yaw_forearm
##  Min.   :-68       Min.   : 0           Min.   :   0      Min.   :-155   
##  1st Qu.:  0       1st Qu.: 0           1st Qu.:   0      1st Qu.: -26   
##  Median : 12       Median : 6           Median :  30      Median :   0   
##  Mean   : 12       Mean   : 8           Mean   : 140      Mean   :  18   
##  3rd Qu.: 28       3rd Qu.:13           3rd Qu.: 166      3rd Qu.:  86   
##  Max.   : 72       Max.   :48           Max.   :2280      Max.   : 169   
##  NA's   :19216     NA's   :19216        NA's   :19216     NA's   :19216  
##  stddev_yaw_forearm var_yaw_forearm gyros_forearm_x   gyros_forearm_y 
##  Min.   :  0        Min.   :    0   Min.   :-22.000   Min.   : -7.02  
##  1st Qu.:  1        1st Qu.:    0   1st Qu.: -0.220   1st Qu.: -1.46  
##  Median : 25        Median :  612   Median :  0.050   Median :  0.03  
##  Mean   : 45        Mean   : 4640   Mean   :  0.158   Mean   :  0.08  
##  3rd Qu.: 86        3rd Qu.: 7368   3rd Qu.:  0.560   3rd Qu.:  1.62  
##  Max.   :198        Max.   :39009   Max.   :  3.970   Max.   :311.00  
##  NA's   :19216      NA's   :19216                                     
##  gyros_forearm_z  accel_forearm_x  accel_forearm_y accel_forearm_z 
##  Min.   : -8.09   Min.   :-498.0   Min.   :-632    Min.   :-446.0  
##  1st Qu.: -0.18   1st Qu.:-178.0   1st Qu.:  57    1st Qu.:-182.0  
##  Median :  0.08   Median : -57.0   Median : 201    Median : -39.0  
##  Mean   :  0.15   Mean   : -61.7   Mean   : 164    Mean   : -55.3  
##  3rd Qu.:  0.49   3rd Qu.:  76.0   3rd Qu.: 312    3rd Qu.:  26.0  
##  Max.   :231.00   Max.   : 477.0   Max.   : 923    Max.   : 291.0  
##                                                                    
##  magnet_forearm_x magnet_forearm_y magnet_forearm_z classe  
##  Min.   :-1280    Min.   :-896     Min.   :-973     A:5580  
##  1st Qu.: -616    1st Qu.:   2     1st Qu.: 191     B:3797  
##  Median : -378    Median : 591     Median : 511     C:3422  
##  Mean   : -313    Mean   : 380     Mean   : 394     D:3216  
##  3rd Qu.:  -73    3rd Qu.: 737     3rd Qu.: 653     E:3607  
##  Max.   :  672    Max.   :1480     Max.   :1090             
## 
```

```r
numericdata <- subset(traindata, select=c("roll_belt","pitch_belt", "yaw_belt", "total_accel_belt", "gyros_belt_x", "gyros_belt_y", "gyros_belt_z", "accel_belt_x", "accel_belt_y", "accel_belt_z", "magnet_belt_x", "magnet_belt_y", "magnet_belt_z", "roll_arm", "pitch_arm", "yaw_arm", "total_accel_arm", "gyros_arm_x", "gyros_arm_y", "gyros_arm_z", "accel_arm_x", "accel_arm_y", "accel_arm_z", "magnet_arm_x", "magnet_arm_y", "magnet_arm_z", "roll_dumbbell", "pitch_dumbbell", "yaw_dumbbell", "total_accel_dumbbell", "gyros_dumbbell_x", "gyros_dumbbell_y", "gyros_dumbbell_z", "accel_dumbbell_x", "accel_dumbbell_y", "accel_dumbbell_z", "magnet_dumbbell_x", "magnet_dumbbell_y", "magnet_dumbbell_z", "roll_forearm", "pitch_forearm", "yaw_forearm", "total_accel_forearm", "gyros_forearm_x", "gyros_forearm_y", "gyros_forearm_z", "accel_forearm_x", "accel_forearm_y", "accel_forearm_z", "magnet_forearm_x", "magnet_forearm_y", "magnet_forearm_z"))

target <- subset(traindata, select=c("X", "classe"))

# checking if some data are similar... some variables can be eliminated. In particular
# total_accel_belt accel_belt_y gyros_dumbbell_z
M <- cor(numericdata)
diag(M) <- 0
which(M > 0.9, arr.ind=T)
```

```
##                  row col
## total_accel_belt   4   1
## accel_belt_y       9   1
## roll_belt          1   4
## accel_belt_y       9   4
## roll_belt          1   9
## total_accel_belt   4   9
## gyros_forearm_z   46  33
## gyros_dumbbell_z  33  46
```

```r
# Checking or principal components, the approach does not seem useful for this project
prComp <- prcomp(numericdata)
plot(prComp$x[,1],prComp$x[,2], col=traindata$classe, xlab="PC1", ylab="PC2")
```

![plot of chunk unnamed-chunk-1](figure/unnamed-chunk-11.png) 

```r
plot(prComp$x[,1], traindata$classe, xlab="PC1", ylab="Classe")
```

![plot of chunk unnamed-chunk-1](figure/unnamed-chunk-12.png) 

```r
plot(prComp$x[,2], traindata$classe, xlab="PC2", ylab="Classe")
```

![plot of chunk unnamed-chunk-1](figure/unnamed-chunk-13.png) 

```r
plot(prComp$x[,3], traindata$classe, xlab="PC3", ylab="Classe")
```

![plot of chunk unnamed-chunk-1](figure/unnamed-chunk-14.png) 

```r
#Still have too maney features, so
fit <- lm(as.numeric(target$classe) ~ ., data=numericdata)
summary(fit)
```

```
## 
## Call:
## lm(formula = as.numeric(target$classe) ~ ., data = numericdata)
## 
## Residuals:
##    Min     1Q Median     3Q    Max 
## -4.422 -0.693 -0.062  0.646  5.803 
## 
## Coefficients:
##                       Estimate Std. Error t value Pr(>|t|)    
## (Intercept)           8.26e+00   4.29e-01   19.25  < 2e-16 ***
## roll_belt             2.60e-02   2.00e-03   12.95  < 2e-16 ***
## pitch_belt            3.82e-02   1.93e-03   19.79  < 2e-16 ***
## yaw_belt             -3.69e-03   3.99e-04   -9.24  < 2e-16 ***
## total_accel_belt      5.12e-02   6.70e-03    7.64  2.3e-14 ***
## gyros_belt_x          2.11e-01   7.22e-02    2.92  0.00349 ** 
## gyros_belt_y         -5.85e-01   1.93e-01   -3.03  0.00248 ** 
## gyros_belt_z          2.00e-01   5.15e-02    3.88  0.00010 ***
## accel_belt_x         -6.17e-03   1.18e-03   -5.21  1.9e-07 ***
## accel_belt_y         -2.58e-02   1.16e-03  -22.20  < 2e-16 ***
## accel_belt_z          2.51e-03   1.11e-03    2.26  0.02373 *  
## magnet_belt_x        -2.54e-03   4.51e-04   -5.63  1.8e-08 ***
## magnet_belt_y        -1.39e-02   5.81e-04  -23.94  < 2e-16 ***
## magnet_belt_z         2.41e-03   3.00e-04    8.06  8.0e-16 ***
## roll_arm             -4.64e-05   1.47e-04   -0.31  0.75294    
## pitch_arm            -4.24e-03   3.39e-04  -12.52  < 2e-16 ***
## yaw_arm               5.26e-04   1.23e-04    4.28  1.8e-05 ***
## total_accel_arm       1.18e-02   1.09e-03   10.84  < 2e-16 ***
## gyros_arm_x           1.05e-01   1.13e-02    9.27  < 2e-16 ***
## gyros_arm_y           1.10e-01   2.76e-02    3.97  7.3e-05 ***
## gyros_arm_z          -7.67e-02   2.13e-02   -3.61  0.00031 ***
## accel_arm_x           5.76e-04   2.23e-04    2.59  0.00974 ** 
## accel_arm_y          -3.95e-03   4.94e-04   -7.99  1.4e-15 ***
## accel_arm_z           6.43e-03   2.82e-04   22.83  < 2e-16 ***
## magnet_arm_x         -4.10e-04   7.81e-05   -5.25  1.6e-07 ***
## magnet_arm_y         -1.41e-03   2.00e-04   -7.01  2.4e-12 ***
## magnet_arm_z         -8.54e-04   1.13e-04   -7.53  5.2e-14 ***
## roll_dumbbell         2.86e-03   2.06e-04   13.92  < 2e-16 ***
## pitch_dumbbell       -2.33e-03   4.42e-04   -5.28  1.3e-07 ***
## yaw_dumbbell         -5.18e-03   2.13e-04  -24.31  < 2e-16 ***
## total_accel_dumbbell  3.85e-02   2.16e-03   17.88  < 2e-16 ***
## gyros_dumbbell_x      2.02e-01   3.00e-02    6.75  1.5e-11 ***
## gyros_dumbbell_y      1.75e-01   1.87e-02    9.35  < 2e-16 ***
## gyros_dumbbell_z      6.88e-02   2.06e-02    3.35  0.00082 ***
## accel_dumbbell_x      7.16e-03   3.94e-04   18.15  < 2e-16 ***
## accel_dumbbell_y      3.25e-04   3.54e-04    0.92  0.35938    
## accel_dumbbell_z      8.82e-04   2.15e-04    4.10  4.1e-05 ***
## magnet_dumbbell_x    -3.57e-03   1.04e-04  -34.25  < 2e-16 ***
## magnet_dumbbell_y    -8.36e-04   8.71e-05   -9.60  < 2e-16 ***
## magnet_dumbbell_z     1.04e-02   1.58e-04   65.68  < 2e-16 ***
## roll_forearm          8.12e-04   8.76e-05    9.27  < 2e-16 ***
## pitch_forearm         1.13e-02   4.54e-04   24.92  < 2e-16 ***
## yaw_forearm          -3.51e-04   9.59e-05   -3.65  0.00026 ***
## total_accel_forearm   2.26e-02   9.88e-04   22.91  < 2e-16 ***
## gyros_forearm_x      -5.40e-02   2.05e-02   -2.64  0.00840 ** 
## gyros_forearm_y      -4.48e-03   6.30e-03   -0.71  0.47675    
## gyros_forearm_z       3.72e-02   1.86e-02    2.00  0.04536 *  
## accel_forearm_x       9.36e-04   1.63e-04    5.73  1.0e-08 ***
## accel_forearm_y       8.21e-04   1.14e-04    7.21  5.8e-13 ***
## accel_forearm_z      -6.09e-03   1.27e-04  -48.09  < 2e-16 ***
## magnet_forearm_x     -7.44e-04   6.91e-05  -10.77  < 2e-16 ***
## magnet_forearm_y     -5.33e-04   4.59e-05  -11.60  < 2e-16 ***
## magnet_forearm_z      1.70e-04   4.35e-05    3.91  9.5e-05 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 1.02 on 19569 degrees of freedom
## Multiple R-squared:  0.526,	Adjusted R-squared:  0.525 
## F-statistic:  418 on 52 and 19569 DF,  p-value: <2e-16
```

```r
#check some to eliminate

numericdata2 <- subset(traindata, select=c("roll_belt","pitch_belt", "yaw_belt",  "gyros_belt_x",  "accel_belt_x",   "magnet_belt_x", "magnet_belt_y", "magnet_belt_z",  "pitch_arm", "yaw_arm", "total_accel_arm", "gyros_arm_x", "gyros_arm_y", "gyros_arm_z",  "accel_arm_y", "accel_arm_z", "magnet_arm_x", "magnet_arm_y", "magnet_arm_z", "roll_dumbbell", "pitch_dumbbell", "yaw_dumbbell", "total_accel_dumbbell", "gyros_dumbbell_x", "gyros_dumbbell_y", "accel_dumbbell_x", "accel_dumbbell_z", "magnet_dumbbell_x", "magnet_dumbbell_y", "magnet_dumbbell_z", "roll_forearm", "pitch_forearm", "yaw_forearm", "total_accel_forearm", "gyros_forearm_x",  "accel_forearm_x", "accel_forearm_y", "accel_forearm_z", "magnet_forearm_x", "magnet_forearm_y", "magnet_forearm_z"))

fit2 <- lm(as.numeric(target$classe) ~ ., data=numericdata2)
summary(fit2)
```

```
## 
## Call:
## lm(formula = as.numeric(target$classe) ~ ., data = numericdata2)
## 
## Residuals:
##    Min     1Q Median     3Q    Max 
## -4.631 -0.702 -0.073  0.671  3.505 
## 
## Coefficients:
##                       Estimate Std. Error t value Pr(>|t|)    
## (Intercept)           9.90e+00   3.60e-01   27.46  < 2e-16 ***
## roll_belt             2.20e-02   9.96e-04   22.08  < 2e-16 ***
## pitch_belt            2.84e-02   1.89e-03   15.07  < 2e-16 ***
## yaw_belt             -3.66e-03   4.03e-04   -9.10  < 2e-16 ***
## gyros_belt_x          3.19e-01   6.96e-02    4.58  4.6e-06 ***
## accel_belt_x         -8.41e-03   1.16e-03   -7.25  4.3e-13 ***
## magnet_belt_x        -4.53e-03   4.06e-04  -11.16  < 2e-16 ***
## magnet_belt_y        -1.64e-02   4.55e-04  -36.09  < 2e-16 ***
## magnet_belt_z         3.77e-03   2.77e-04   13.60  < 2e-16 ***
## pitch_arm            -4.64e-03   3.40e-04  -13.63  < 2e-16 ***
## yaw_arm               6.25e-04   1.21e-04    5.16  2.6e-07 ***
## total_accel_arm       1.18e-02   1.10e-03   10.69  < 2e-16 ***
## gyros_arm_x           1.03e-01   1.11e-02    9.23  < 2e-16 ***
## gyros_arm_y           9.84e-02   2.75e-02    3.58  0.00034 ***
## gyros_arm_z          -1.07e-01   2.15e-02   -4.97  6.8e-07 ***
## accel_arm_y          -2.17e-03   4.53e-04   -4.79  1.7e-06 ***
## accel_arm_z           5.56e-03   2.74e-04   20.29  < 2e-16 ***
## magnet_arm_x         -2.73e-04   5.04e-05   -5.41  6.3e-08 ***
## magnet_arm_y         -1.94e-03   1.96e-04   -9.87  < 2e-16 ***
## magnet_arm_z         -6.78e-04   1.05e-04   -6.46  1.1e-10 ***
## roll_dumbbell         3.42e-03   1.62e-04   21.13  < 2e-16 ***
## pitch_dumbbell       -2.65e-03   4.45e-04   -5.97  2.5e-09 ***
## yaw_dumbbell         -5.12e-03   2.11e-04  -24.25  < 2e-16 ***
## total_accel_dumbbell  4.19e-02   1.89e-03   22.16  < 2e-16 ***
## gyros_dumbbell_x      5.95e-02   7.21e-03    8.25  < 2e-16 ***
## gyros_dumbbell_y      1.33e-01   1.79e-02    7.41  1.3e-13 ***
## accel_dumbbell_x      7.01e-03   3.80e-04   18.42  < 2e-16 ***
## accel_dumbbell_z      1.76e-04   2.02e-04    0.87  0.38377    
## magnet_dumbbell_x    -3.62e-03   9.03e-05  -40.13  < 2e-16 ***
## magnet_dumbbell_y    -8.85e-04   6.96e-05  -12.71  < 2e-16 ***
## magnet_dumbbell_z     1.12e-02   1.52e-04   73.18  < 2e-16 ***
## roll_forearm          8.31e-04   8.88e-05    9.36  < 2e-16 ***
## pitch_forearm         1.08e-02   4.51e-04   24.00  < 2e-16 ***
## yaw_forearm          -7.92e-05   9.50e-05   -0.83  0.40475    
## total_accel_forearm   2.15e-02   9.96e-04   21.60  < 2e-16 ***
## gyros_forearm_x      -7.43e-02   1.60e-02   -4.63  3.6e-06 ***
## accel_forearm_x       1.11e-03   1.63e-04    6.79  1.2e-11 ***
## accel_forearm_y       1.09e-03   1.14e-04    9.59  < 2e-16 ***
## accel_forearm_z      -6.40e-03   1.23e-04  -52.07  < 2e-16 ***
## magnet_forearm_x     -8.50e-04   6.87e-05  -12.36  < 2e-16 ***
## magnet_forearm_y     -6.62e-04   4.56e-05  -14.51  < 2e-16 ***
## magnet_forearm_z      1.61e-04   4.32e-05    3.72  0.00020 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 1.03 on 19580 degrees of freedom
## Multiple R-squared:  0.51,	Adjusted R-squared:  0.509 
## F-statistic:  497 on 41 and 19580 DF,  p-value: <2e-16
```

```r
#and verify there is no significant loss
#still to many features, but a model can be made

#caret
library(caret)
modFit <- train(as.numeric(target$classe) ~ ., method="lm", data=numericdata2)
modFit
```

```
## Linear Regression 
## 
## 19622 samples
##    40 predictor
## 
## No pre-processing
## Resampling: Bootstrapped (25 reps) 
## 
## Summary of sample sizes: 19622, 19622, 19622, 19622, 19622, 19622, ... 
## 
## Resampling results
## 
##   RMSE  Rsquared  RMSE SD  Rsquared SD
##   1     0.5       0.02     0.02       
## 
## 
```

```r
pred <- predict(modFit, numericdata2)
plot(target$classe, pred)
```

![plot of chunk unnamed-chunk-1](figure/unnamed-chunk-15.png) 

```r
# back to finding correct features (40 is too many)
# estimate variable importance starting from initial set

modFit <- train(as.numeric(target$classe) ~ ., method="lm", data=numericdata)
modFit
```

```
## Linear Regression 
## 
## 19622 samples
##    51 predictor
## 
## No pre-processing
## Resampling: Bootstrapped (25 reps) 
## 
## Summary of sample sizes: 19622, 19622, 19622, 19622, 19622, 19622, ... 
## 
## Resampling results
## 
##   RMSE  Rsquared  RMSE SD  Rsquared SD
##   1     0.5       0.06     0.05       
## 
## 
```

```r
importance <- varImp(modFit, scale=FALSE)
# summarize importance
print(importance)
```

```
## lm variable importance
## 
##   only 20 most important variables shown (out of 52)
## 
##                      Overall
## magnet_dumbbell_z      65.68
## accel_forearm_z        48.09
## magnet_dumbbell_x      34.25
## pitch_forearm          24.92
## yaw_dumbbell           24.31
## magnet_belt_y          23.94
## total_accel_forearm    22.91
## accel_arm_z            22.83
## accel_belt_y           22.20
## pitch_belt             19.79
## accel_dumbbell_x       18.15
## total_accel_dumbbell   17.88
## roll_dumbbell          13.92
## roll_belt              12.95
## pitch_arm              12.52
## magnet_forearm_y       11.60
## total_accel_arm        10.84
## magnet_forearm_x       10.77
## magnet_dumbbell_y       9.60
## gyros_dumbbell_y        9.35
```

```r
# plot importance
plot(importance)
```

![plot of chunk unnamed-chunk-1](figure/unnamed-chunk-16.png) 

```r
# I now take the first 12, since there apears to be a significant cut there

finaldata <- subset(traindata, select=c("classe", "magnet_dumbbell_z", "accel_forearm_z","magnet_dumbbell_x","pitch_forearm","yaw_dumbbell","magnet_belt_y","total_accel_forearm","accel_arm_z","accel_belt_y","pitch_belt","accel_dumbbell_x","total_accel_dumbbell"))

# Now, I check the best algorithm for the model.
#Since the testing data included does not have a target, I need to slice down the data for training and testing.

inTrain <- createDataPartition(y=target$class, p=0.75, list=FALSE)
```

```
## Warning: Name partially matched in data frame
```

```r
training <- finaldata[inTrain,]
testing <- finaldata[-inTrain,]

modFitOp1 <- train(as.numeric(classe) ~ ., method="lm", data=training)
modFitOp1
```

```
## Linear Regression 
## 
## 14718 samples
##    12 predictor
## 
## No pre-processing
## Resampling: Bootstrapped (25 reps) 
## 
## Summary of sample sizes: 14718, 14718, 14718, 14718, 14718, 14718, ... 
## 
## Resampling results
## 
##   RMSE  Rsquared  RMSE SD  Rsquared SD
##   1     0.3       0.008    0.006      
## 
## 
```

```r
#tc <- trainControl("repeatedcv", number=10, repeats=10, classProbs=TRUE, savePred=T) 
#modFitOp2 <- train(as.numeric(classe) ~ ., method="rf", trControl=tc, , data=training, preProc=c("center", "scale"), verbose=TRUE)

modFitOp2 <- train(as.numeric(classe) ~ ., method="nnet", data=training, verbose =TRUE)
```

```
## # weights:  15
## initial  value 107710.029322 
## final  value 78767.000000 
## converged
## # weights:  43
## initial  value 122067.475325 
## final  value 78767.000000 
## converged
## # weights:  71
## initial  value 103829.348905 
## final  value 78767.000000 
## converged
## # weights:  15
## initial  value 106886.289017 
## iter  10 value 78870.658258
## iter  20 value 78774.666492
## iter  30 value 78773.916989
## final  value 78773.906960 
## converged
## # weights:  43
## initial  value 112138.240553 
## iter  10 value 79007.980042
## iter  20 value 78775.516312
## iter  30 value 78772.071829
## iter  40 value 78771.326487
## iter  50 value 78770.872235
## iter  60 value 78770.838034
## final  value 78770.835394 
## converged
## # weights:  71
## initial  value 107476.420587 
## iter  10 value 78775.134416
## iter  20 value 78770.410925
## iter  30 value 78769.759504
## final  value 78769.715030 
## converged
## # weights:  15
## initial  value 117017.516174 
## iter  10 value 78845.505169
## iter  20 value 78767.909072
## final  value 78767.037257 
## converged
## # weights:  43
## initial  value 96573.468144 
## iter  10 value 78833.592952
## iter  20 value 78767.777544
## final  value 78767.046431 
## converged
## # weights:  71
## initial  value 128322.377712 
## iter  10 value 78881.858058
## iter  20 value 78768.343453
## final  value 78767.091997 
## converged
## # weights:  15
## initial  value 94029.392768 
## final  value 77582.000000 
## converged
## # weights:  43
## initial  value 105090.363234 
## final  value 77582.000000 
## converged
## # weights:  71
## initial  value 121847.396919 
## final  value 77582.000000 
## converged
## # weights:  15
## initial  value 96737.251173 
## iter  10 value 77591.940233
## iter  20 value 77588.900034
## final  value 77588.891860 
## converged
## # weights:  43
## initial  value 111259.817344 
## iter  10 value 77589.878751
## iter  20 value 77585.904893
## final  value 77585.831912 
## converged
## # weights:  71
## initial  value 109936.289435 
## iter  10 value 77590.456599
## iter  20 value 77585.230403
## iter  30 value 77584.993950
## iter  40 value 77584.821156
## final  value 77584.712239 
## converged
## # weights:  15
## initial  value 123036.655145 
## iter  10 value 77679.421700
## iter  20 value 77583.127255
## iter  30 value 77582.059773
## final  value 77582.057989 
## converged
## # weights:  43
## initial  value 102754.652844 
## iter  10 value 77705.001512
## iter  20 value 77583.428198
## iter  30 value 77582.080488
## final  value 77582.079180 
## converged
## # weights:  71
## initial  value 114568.224330 
## iter  10 value 77724.240780
## iter  20 value 77583.658128
## final  value 77582.081688 
## converged
## # weights:  15
## initial  value 95916.392902 
## final  value 77533.000000 
## converged
## # weights:  43
## initial  value 120880.139345 
## final  value 77533.000000 
## converged
## # weights:  71
## initial  value 117871.093116 
## final  value 77533.000000 
## converged
## # weights:  15
## initial  value 112636.395238 
## iter  10 value 77615.101301
## iter  20 value 77541.945584
## iter  30 value 77539.897824
## final  value 77539.893719 
## converged
## # weights:  43
## initial  value 119544.000301 
## iter  10 value 77654.682116
## iter  20 value 77540.171454
## iter  30 value 77536.858794
## final  value 77536.832440 
## converged
## # weights:  71
## initial  value 108406.640491 
## iter  10 value 78139.472589
## iter  20 value 77536.199403
## iter  30 value 77535.800838
## iter  40 value 77535.721467
## final  value 77535.713591 
## converged
## # weights:  15
## initial  value 118818.172717 
## iter  10 value 77577.543003
## iter  20 value 77533.516681
## final  value 77533.025483 
## converged
## # weights:  43
## initial  value 101511.517226 
## iter  10 value 77649.776881
## iter  20 value 77534.358548
## iter  30 value 77533.083262
## iter  30 value 77533.082738
## iter  30 value 77533.082695
## final  value 77533.082695 
## converged
## # weights:  71
## initial  value 102767.026074 
## iter  10 value 77690.928330
## iter  20 value 77534.837467
## iter  30 value 77533.061180
## final  value 77533.054058 
## converged
## # weights:  15
## initial  value 97550.156544 
## final  value 78560.000000 
## converged
## # weights:  43
## initial  value 100719.884622 
## final  value 78560.000000 
## converged
## # weights:  71
## initial  value 98931.628843 
## final  value 78560.000000 
## converged
## # weights:  15
## initial  value 105113.148650 
## iter  10 value 78569.694325
## iter  20 value 78566.906073
## final  value 78566.900333 
## converged
## # weights:  43
## initial  value 115943.536956 
## iter  10 value 78581.745463
## iter  20 value 78564.831244
## iter  30 value 78563.909212
## iter  40 value 78563.836285
## iter  40 value 78563.836176
## iter  40 value 78563.836176
## final  value 78563.836176 
## converged
## # weights:  71
## initial  value 101522.073815 
## iter  10 value 78576.977323
## iter  20 value 78563.083196
## iter  30 value 78562.750201
## iter  40 value 78562.717280
## final  value 78562.714001 
## converged
## # weights:  15
## initial  value 96449.805891 
## iter  10 value 78610.732419
## iter  20 value 78560.589704
## final  value 78560.038419 
## converged
## # weights:  43
## initial  value 113177.512087 
## final  value 78560.443426 
## converged
## # weights:  71
## initial  value 127433.405555 
## iter  10 value 78640.727168
## iter  20 value 78560.948876
## final  value 78560.087603 
## converged
## # weights:  15
## initial  value 96757.924743 
## final  value 78906.000000 
## converged
## # weights:  43
## initial  value 107446.312365 
## final  value 78906.000000 
## converged
## # weights:  71
## initial  value 109876.988582 
## final  value 78906.000000 
## converged
## # weights:  15
## initial  value 97030.659045 
## iter  10 value 78920.756106
## iter  20 value 78912.915064
## final  value 78912.906655 
## converged
## # weights:  43
## initial  value 118791.283705 
## iter  10 value 79257.486976
## iter  20 value 78913.269990
## iter  30 value 78909.880438
## iter  40 value 78909.840188
## iter  40 value 78909.839410
## iter  40 value 78909.838847
## final  value 78909.838847 
## converged
## # weights:  71
## initial  value 96011.619883 
## iter  10 value 79463.378714
## iter  20 value 78909.317382
## iter  30 value 78909.071996
## iter  40 value 78908.924357
## iter  50 value 78908.740057
## iter  60 value 78908.722001
## final  value 78908.716884 
## converged
## # weights:  15
## initial  value 106327.555066 
## iter  10 value 78958.544860
## iter  20 value 78906.609751
## final  value 78906.030529 
## converged
## # weights:  43
## initial  value 111341.467288 
## iter  10 value 78918.599172
## iter  20 value 78906.554376
## final  value 78906.525077 
## converged
## # weights:  71
## initial  value 119265.756706 
## iter  10 value 79063.362811
## iter  20 value 78907.831181
## iter  30 value 78906.079604
## final  value 78906.074978 
## converged
## # weights:  15
## initial  value 105056.132577 
## final  value 76658.000000 
## converged
## # weights:  43
## initial  value 97033.420179 
## final  value 76658.000000 
## converged
## # weights:  71
## initial  value 98066.184491 
## final  value 76658.000000 
## converged
## # weights:  15
## initial  value 106875.230103 
## iter  10 value 76786.139001
## iter  20 value 76664.896344
## final  value 76664.883343 
## converged
## # weights:  43
## initial  value 88885.438042 
## iter  10 value 76697.127395
## iter  20 value 76662.223524
## iter  30 value 76661.824974
## iter  30 value 76661.824291
## iter  30 value 76661.824276
## final  value 76661.824276 
## converged
## # weights:  71
## initial  value 103641.983928 
## iter  10 value 76928.361664
## iter  20 value 76665.644350
## iter  30 value 76661.379590
## iter  40 value 76661.074399
## iter  50 value 76660.859897
## iter  60 value 76660.790844
## iter  70 value 76660.710897
## final  value 76660.706954 
## converged
## # weights:  15
## initial  value 96776.191934 
## iter  10 value 76724.727493
## iter  20 value 76658.775376
## final  value 76658.050278 
## converged
## # weights:  43
## initial  value 94306.763702 
## iter  10 value 76720.252117
## iter  20 value 76658.728146
## final  value 76658.031302 
## converged
## # weights:  71
## initial  value 117265.807429 
## iter  10 value 76915.796546
## iter  20 value 76660.992077
## final  value 76658.119889 
## converged
## # weights:  15
## initial  value 99384.007744 
## final  value 78319.000000 
## converged
## # weights:  43
## initial  value 113521.315650 
## final  value 78319.000000 
## converged
## # weights:  71
## initial  value 131711.547134 
## final  value 78319.000000 
## converged
## # weights:  15
## initial  value 98460.094142 
## iter  10 value 78340.933557
## iter  20 value 78325.982128
## iter  30 value 78325.898142
## iter  30 value 78325.897894
## iter  30 value 78325.897525
## final  value 78325.897525 
## converged
## # weights:  43
## initial  value 94640.014556 
## iter  10 value 78340.472181
## iter  20 value 78323.404596
## iter  30 value 78323.130974
## iter  40 value 78322.851245
## final  value 78322.838677 
## converged
## # weights:  71
## initial  value 107333.718960 
## iter  10 value 78355.312408
## iter  20 value 78322.539952
## iter  30 value 78321.770111
## final  value 78321.713246 
## converged
## # weights:  15
## initial  value 118919.620184 
## iter  10 value 78407.817944
## iter  20 value 78320.026864
## iter  30 value 78319.053927
## final  value 78319.049753 
## converged
## # weights:  43
## initial  value 112529.215496 
## iter  10 value 78334.494549
## iter  20 value 78319.489889
## final  value 78319.434728 
## converged
## # weights:  71
## initial  value 120821.735456 
## iter  10 value 78562.111428
## iter  20 value 78321.819554
## iter  30 value 78319.052084
## iter  30 value 78319.051901
## iter  30 value 78319.051881
## final  value 78319.051881 
## converged
## # weights:  15
## initial  value 110036.673159 
## final  value 79799.000000 
## converged
## # weights:  43
## initial  value 115026.406633 
## final  value 79799.000000 
## converged
## # weights:  71
## initial  value 107145.026153 
## final  value 79799.000000 
## converged
## # weights:  15
## initial  value 114921.909686 
## iter  10 value 79807.542455
## final  value 79805.910836 
## converged
## # weights:  43
## initial  value 102136.636446 
## iter  10 value 79916.134690
## iter  20 value 79805.255206
## iter  30 value 79803.928974
## final  value 79803.904324 
## converged
## # weights:  71
## initial  value 122568.019535 
## iter  10 value 80000.669101
## iter  20 value 79801.924232
## iter  30 value 79801.731010
## final  value 79801.718235 
## converged
## # weights:  15
## initial  value 107909.567279 
## iter  10 value 79884.330435
## iter  20 value 79799.988986
## iter  30 value 79799.054006
## iter  30 value 79799.053229
## iter  30 value 79799.053207
## final  value 79799.053207 
## converged
## # weights:  43
## initial  value 114080.187393 
## iter  10 value 79913.704425
## iter  20 value 79800.331809
## final  value 79799.050325 
## converged
## # weights:  71
## initial  value 119185.722794 
## iter  10 value 79916.726790
## iter  20 value 79800.372432
## iter  30 value 79799.065152
## iter  30 value 79799.064993
## iter  30 value 79799.064967
## final  value 79799.064967 
## converged
## # weights:  15
## initial  value 103951.418989 
## final  value 77848.000000 
## converged
## # weights:  43
## initial  value 107797.214905 
## final  value 77848.000000 
## converged
## # weights:  71
## initial  value 89346.199349 
## final  value 77848.000000 
## converged
## # weights:  15
## initial  value 110438.216887 
## iter  10 value 78003.765908
## iter  20 value 77855.185568
## final  value 77854.889533 
## converged
## # weights:  43
## initial  value 129911.851459 
## iter  10 value 78007.015466
## iter  20 value 77852.537408
## iter  30 value 77851.898414
## iter  40 value 77851.836481
## final  value 77851.832245 
## converged
## # weights:  71
## initial  value 129306.998594 
## iter  10 value 77979.194718
## iter  20 value 77853.610670
## iter  30 value 77852.776735
## iter  40 value 77851.839868
## iter  50 value 77851.561316
## iter  60 value 77850.808400
## iter  70 value 77850.736013
## final  value 77850.710885 
## converged
## # weights:  15
## initial  value 119425.845887 
## iter  10 value 77934.743831
## iter  20 value 77849.003052
## final  value 77848.046253 
## converged
## # weights:  43
## initial  value 110377.079113 
## iter  10 value 77953.982266
## iter  20 value 77849.233065
## final  value 77848.045091 
## converged
## # weights:  71
## initial  value 106412.728234 
## iter  10 value 77858.352500
## iter  20 value 77848.697004
## final  value 77848.677130 
## converged
## # weights:  15
## initial  value 124798.748412 
## final  value 79416.000000 
## converged
## # weights:  43
## initial  value 116826.834210 
## final  value 79416.000000 
## converged
## # weights:  71
## initial  value 100992.832269 
## final  value 79416.000000 
## converged
## # weights:  15
## initial  value 109892.677908 
## iter  10 value 79425.052603
## iter  20 value 79422.922661
## final  value 79422.906093 
## converged
## # weights:  43
## initial  value 89929.034706 
## iter  10 value 79564.396706
## iter  20 value 79420.548066
## iter  30 value 79419.911206
## final  value 79419.838462 
## converged
## # weights:  71
## initial  value 124815.238148 
## iter  10 value 79438.838288
## iter  20 value 79421.356274
## iter  30 value 79419.542762
## final  value 79418.716732 
## converged
## # weights:  15
## initial  value 115090.970205 
## iter  10 value 79510.241405
## iter  20 value 79417.090712
## iter  30 value 79416.041622
## iter  30 value 79416.041026
## iter  30 value 79416.041009
## final  value 79416.041009 
## converged
## # weights:  43
## initial  value 128584.588813 
## iter  10 value 79488.733074
## iter  20 value 79416.850513
## final  value 79416.059249 
## converged
## # weights:  71
## initial  value 105079.898368 
## iter  10 value 79544.577093
## iter  20 value 79417.501330
## iter  30 value 79416.059691
## iter  30 value 79416.059125
## iter  30 value 79416.059089
## final  value 79416.059089 
## converged
## # weights:  15
## initial  value 111465.836249 
## final  value 78280.000000 
## converged
## # weights:  43
## initial  value 120448.666012 
## final  value 78280.000000 
## converged
## # weights:  71
## initial  value 113954.527139 
## final  value 78280.000000 
## converged
## # weights:  15
## initial  value 98846.691506 
## iter  10 value 78390.027981
## iter  20 value 78286.903750
## final  value 78286.896706 
## converged
## # weights:  43
## initial  value 120654.209958 
## iter  10 value 78297.492208
## iter  20 value 78285.824056
## iter  30 value 78285.391251
## iter  40 value 78284.499282
## iter  50 value 78283.867237
## final  value 78283.835895 
## converged
## # weights:  71
## initial  value 112266.759599 
## iter  10 value 78888.401112
## iter  20 value 78288.033500
## iter  30 value 78284.032971
## iter  40 value 78283.716262
## iter  50 value 78282.939441
## iter  60 value 78282.764639
## final  value 78282.712760 
## converged
## # weights:  15
## initial  value 113101.959147 
## iter  10 value 78393.616228
## iter  20 value 78281.315403
## iter  30 value 78280.074881
## final  value 78280.072659 
## converged
## # weights:  43
## initial  value 107269.408770 
## iter  10 value 78425.744993
## iter  20 value 78281.692776
## final  value 78280.063968 
## converged
## # weights:  71
## initial  value 91220.656954 
## iter  10 value 78362.793298
## iter  20 value 78280.974227
## final  value 78280.076672 
## converged
## # weights:  15
## initial  value 102758.238423 
## final  value 78881.000000 
## converged
## # weights:  43
## initial  value 100559.418102 
## final  value 78881.000000 
## converged
## # weights:  71
## initial  value 96557.429140 
## final  value 78881.000000 
## converged
## # weights:  15
## initial  value 107676.651324 
## iter  10 value 78963.839879
## iter  20 value 78889.637771
## iter  30 value 78889.340687
## final  value 78887.909114 
## converged
## # weights:  43
## initial  value 109080.399636 
## iter  10 value 78977.643532
## iter  20 value 78885.089368
## iter  30 value 78884.849273
## final  value 78884.841932 
## converged
## # weights:  71
## initial  value 91059.705050 
## iter  10 value 79053.871310
## iter  20 value 78885.322672
## iter  30 value 78883.790529
## iter  30 value 78883.790418
## final  value 78883.719899 
## converged
## # weights:  15
## initial  value 102284.525536 
## iter  10 value 78923.788676
## iter  20 value 78881.498946
## final  value 78881.032192 
## converged
## # weights:  43
## initial  value 108621.386170 
## iter  10 value 78954.986839
## iter  20 value 78881.862082
## final  value 78881.043714 
## converged
## # weights:  71
## initial  value 111882.360182 
## iter  10 value 79041.363847
## iter  20 value 78882.866425
## final  value 78881.081511 
## converged
## # weights:  15
## initial  value 106528.939409 
## final  value 78646.000000 
## converged
## # weights:  43
## initial  value 102887.289976 
## final  value 78646.000000 
## converged
## # weights:  71
## initial  value 108709.639126 
## final  value 78646.000000 
## converged
## # weights:  15
## initial  value 111896.816741 
## iter  10 value 78771.555443
## iter  20 value 78653.458367
## final  value 78652.899828 
## converged
## # weights:  43
## initial  value 118139.322062 
## iter  10 value 78711.793314
## iter  20 value 78654.775792
## iter  30 value 78652.468275
## iter  40 value 78649.873650
## final  value 78649.840488 
## converged
## # weights:  71
## initial  value 100878.160106 
## iter  10 value 78762.886260
## iter  20 value 78650.031014
## iter  30 value 78648.878524
## final  value 78648.713619 
## converged
## # weights:  15
## initial  value 108128.568062 
## iter  10 value 78723.391095
## iter  20 value 78646.896070
## iter  30 value 78646.048822
## final  value 78646.044613 
## converged
## # weights:  43
## initial  value 108243.119038 
## iter  10 value 78733.672115
## iter  20 value 78647.018561
## final  value 78646.039362 
## converged
## # weights:  71
## initial  value 118659.988418 
## iter  10 value 78659.013697
## iter  20 value 78646.737039
## final  value 78646.661593 
## converged
## # weights:  15
## initial  value 94612.420765 
## final  value 77650.000000 
## converged
## # weights:  43
## initial  value 115921.726059 
## final  value 77650.000000 
## converged
## # weights:  71
## initial  value 104425.163768 
## final  value 77650.000000 
## converged
## # weights:  15
## initial  value 119350.014409 
## iter  10 value 77698.760708
## iter  20 value 77656.889239
## iter  20 value 77656.889049
## iter  20 value 77656.888889
## final  value 77656.888889 
## converged
## # weights:  43
## initial  value 99488.433905 
## iter  10 value 77787.968274
## iter  20 value 77654.386840
## iter  30 value 77653.835259
## final  value 77653.829325 
## converged
## # weights:  71
## initial  value 111668.908144 
## iter  10 value 77666.850829
## iter  20 value 77654.153100
## iter  30 value 77653.858782
## iter  40 value 77652.795872
## iter  50 value 77652.712310
## iter  50 value 77652.711660
## iter  50 value 77652.711660
## final  value 77652.711660 
## converged
## # weights:  15
## initial  value 113132.288558 
## iter  10 value 77699.345087
## iter  20 value 77650.571759
## iter  30 value 77650.031075
## iter  30 value 77650.030626
## iter  30 value 77650.030614
## final  value 77650.030614 
## converged
## # weights:  43
## initial  value 88824.642378 
## iter  10 value 77691.458989
## iter  20 value 77650.487913
## final  value 77650.038084 
## converged
## # weights:  71
## initial  value 118511.782338 
## iter  10 value 77801.050763
## iter  20 value 77651.763573
## iter  30 value 77650.053249
## iter  30 value 77650.053221
## final  value 77650.053221 
## converged
## # weights:  15
## initial  value 111521.753703 
## final  value 78910.000000 
## converged
## # weights:  43
## initial  value 126229.671651 
## final  value 78910.000000 
## converged
## # weights:  71
## initial  value 95260.292432 
## final  value 78910.000000 
## converged
## # weights:  15
## initial  value 124366.340710 
## iter  10 value 78926.710297
## iter  20 value 78916.917989
## final  value 78916.899846 
## converged
## # weights:  43
## initial  value 124356.153952 
## iter  10 value 78950.313834
## iter  20 value 78914.835159
## iter  30 value 78913.899563
## iter  40 value 78913.838755
## final  value 78913.835051 
## converged
## # weights:  71
## initial  value 92916.067108 
## iter  10 value 79392.343761
## iter  20 value 78914.884495
## iter  30 value 78912.778488
## final  value 78912.715841 
## converged
## # weights:  15
## initial  value 100398.274841 
## iter  10 value 78948.909556
## iter  20 value 78910.450838
## final  value 78910.027297 
## converged
## # weights:  43
## initial  value 121492.892680 
## iter  10 value 78948.141678
## iter  20 value 78910.439744
## final  value 78910.017760 
## converged
## # weights:  71
## initial  value 97888.380374 
## iter  10 value 78998.207786
## iter  20 value 78911.035530
## final  value 78910.038629 
## converged
## # weights:  15
## initial  value 116844.200737 
## final  value 78083.000000 
## converged
## # weights:  43
## initial  value 121919.657923 
## final  value 78083.000000 
## converged
## # weights:  71
## initial  value 90360.243590 
## final  value 78083.000000 
## converged
## # weights:  15
## initial  value 97820.404212 
## iter  10 value 78197.831993
## iter  20 value 78090.269035
## final  value 78089.891328 
## converged
## # weights:  43
## initial  value 95433.394802 
## iter  10 value 78123.811053
## iter  20 value 78089.432546
## iter  30 value 78088.206204
## iter  40 value 78087.075506
## iter  50 value 78086.832216
## final  value 78086.829726 
## converged
## # weights:  71
## initial  value 98530.621722 
## iter  10 value 78331.861444
## iter  20 value 78085.949946
## iter  30 value 78085.751807
## final  value 78085.710693 
## converged
## # weights:  15
## initial  value 99773.817360 
## iter  10 value 78121.571083
## iter  20 value 78083.446744
## final  value 78083.049565 
## converged
## # weights:  43
## initial  value 95547.024523 
## iter  10 value 78194.602317
## iter  20 value 78084.295265
## final  value 78083.086826 
## converged
## # weights:  71
## initial  value 105347.791997 
## iter  10 value 78258.116301
## iter  20 value 78085.034681
## iter  30 value 78083.145967
## iter  30 value 78083.145550
## iter  30 value 78083.145492
## final  value 78083.145492 
## converged
## # weights:  15
## initial  value 120444.491918 
## final  value 77271.000000 
## converged
## # weights:  43
## initial  value 109770.638863 
## final  value 77271.000000 
## converged
## # weights:  71
## initial  value 110016.465147 
## final  value 77271.000000 
## converged
## # weights:  15
## initial  value 97418.880166 
## iter  10 value 77361.880121
## iter  20 value 77277.995872
## iter  30 value 77277.892873
## final  value 77277.883336 
## converged
## # weights:  43
## initial  value 109687.542337 
## iter  10 value 77294.034412
## iter  20 value 77276.468024
## iter  30 value 77275.307927
## iter  40 value 77274.853072
## final  value 77274.826365 
## converged
## # weights:  71
## initial  value 106640.348956 
## iter  10 value 77916.662876
## iter  20 value 77275.801602
## iter  30 value 77274.000409
## iter  40 value 77273.842059
## iter  50 value 77273.722772
## final  value 77273.713391 
## converged
## # weights:  15
## initial  value 125988.138930 
## iter  10 value 77329.586839
## iter  20 value 77271.680705
## final  value 77271.050946 
## converged
## # weights:  43
## initial  value 102745.142960 
## iter  10 value 77397.000638
## iter  20 value 77272.461598
## iter  30 value 77271.069868
## iter  30 value 77271.069673
## iter  30 value 77271.069645
## final  value 77271.069645 
## converged
## # weights:  71
## initial  value 117753.789570 
## iter  10 value 77442.860970
## iter  20 value 77272.999843
## iter  30 value 77271.086784
## final  value 77271.085447 
## converged
## # weights:  15
## initial  value 118777.294630 
## final  value 77907.000000 
## converged
## # weights:  43
## initial  value 103431.801956 
## final  value 77907.000000 
## converged
## # weights:  71
## initial  value 122008.629756 
## final  value 77907.000000 
## converged
## # weights:  15
## initial  value 121047.567837 
## iter  10 value 78026.022635
## iter  20 value 77914.090930
## final  value 77913.893863 
## converged
## # weights:  43
## initial  value 115642.713866 
## iter  10 value 78244.960864
## iter  20 value 77912.022448
## iter  30 value 77910.872710
## final  value 77910.832301 
## converged
## # weights:  71
## initial  value 104928.189310 
## iter  10 value 77949.097312
## iter  20 value 77911.344680
## iter  30 value 77910.108286
## iter  40 value 77909.729313
## final  value 77909.712953 
## converged
## # weights:  15
## initial  value 100088.569506 
## iter  10 value 78010.376056
## iter  20 value 77908.195327
## final  value 77907.040635 
## converged
## # weights:  43
## initial  value 100665.903176 
## iter  10 value 77974.777123
## iter  20 value 77907.791094
## final  value 77907.041426 
## converged
## # weights:  71
## initial  value 104940.394718 
## iter  10 value 78090.994054
## iter  20 value 77909.137591
## iter  30 value 77907.094396
## final  value 77907.090451 
## converged
## # weights:  15
## initial  value 116116.126207 
## final  value 77548.000000 
## converged
## # weights:  43
## initial  value 113200.031008 
## final  value 77548.000000 
## converged
## # weights:  71
## initial  value 95056.218123 
## final  value 77548.000000 
## converged
## # weights:  15
## initial  value 100825.274337 
## iter  10 value 77559.499937
## iter  20 value 77554.897628
## final  value 77554.890950 
## converged
## # weights:  43
## initial  value 104468.464961 
## iter  10 value 77556.174987
## iter  20 value 77552.558828
## iter  30 value 77551.865601
## iter  40 value 77551.831002
## iter  40 value 77551.830514
## iter  40 value 77551.830150
## final  value 77551.830150 
## converged
## # weights:  71
## initial  value 104539.284654 
## iter  10 value 78057.295079
## iter  20 value 77553.927072
## iter  30 value 77550.843212
## iter  40 value 77550.732640
## final  value 77550.710485 
## converged
## # weights:  15
## initial  value 115176.215940 
## iter  10 value 77597.127706
## iter  20 value 77548.570094
## final  value 77548.028224 
## converged
## # weights:  43
## initial  value 114390.740856 
## iter  10 value 77561.728211
## iter  20 value 77548.616921
## iter  20 value 77548.616779
## iter  20 value 77548.616532
## final  value 77548.616532 
## converged
## # weights:  71
## initial  value 94169.571498 
## iter  10 value 77614.453812
## iter  20 value 77548.783869
## iter  30 value 77548.044637
## iter  30 value 77548.044040
## iter  30 value 77548.043819
## final  value 77548.043819 
## converged
## # weights:  15
## initial  value 125303.470976 
## final  value 77354.000000 
## converged
## # weights:  43
## initial  value 100039.291197 
## final  value 77354.000000 
## converged
## # weights:  71
## initial  value 105505.890810 
## final  value 77354.000000 
## converged
## # weights:  15
## initial  value 110794.966754 
## iter  10 value 77443.359026
## iter  20 value 77360.918861
## final  value 77360.887159 
## converged
## # weights:  43
## initial  value 108330.232413 
## iter  10 value 77387.939927
## iter  20 value 77358.040206
## iter  30 value 77357.832204
## iter  30 value 77357.831622
## iter  30 value 77357.831450
## final  value 77357.831450 
## converged
## # weights:  71
## initial  value 107904.424809 
## iter  10 value 77461.739605
## iter  20 value 77360.645430
## iter  30 value 77356.794939
## final  value 77356.709607 
## converged
## # weights:  15
## initial  value 104720.553688 
## iter  10 value 77401.055239
## iter  20 value 77354.546022
## final  value 77354.027010 
## converged
## # weights:  43
## initial  value 120019.783351 
## iter  10 value 77429.244242
## iter  20 value 77354.874847
## final  value 77354.042434 
## converged
## # weights:  71
## initial  value 85237.575503 
## iter  10 value 77403.580622
## iter  20 value 77354.587477
## final  value 77354.048994 
## converged
## # weights:  15
## initial  value 118100.693321 
## final  value 78568.000000 
## converged
## # weights:  43
## initial  value 116873.585095 
## final  value 78568.000000 
## converged
## # weights:  71
## initial  value 119815.458441 
## final  value 78568.000000 
## converged
## # weights:  15
## initial  value 98871.930960 
## iter  10 value 78651.720346
## iter  20 value 78574.943047
## final  value 78574.899486 
## converged
## # weights:  43
## initial  value 95620.578979 
## iter  10 value 78583.609027
## iter  20 value 78573.237685
## iter  30 value 78571.912316
## iter  40 value 78571.836585
## iter  40 value 78571.835808
## iter  40 value 78571.835271
## final  value 78571.835271 
## converged
## # weights:  71
## initial  value 111919.596478 
## iter  10 value 78593.335415
## iter  20 value 78574.241581
## iter  30 value 78572.736358
## iter  40 value 78571.168644
## iter  50 value 78570.752843
## final  value 78570.715520 
## converged
## # weights:  15
## initial  value 99681.937387 
## iter  10 value 78642.149313
## iter  20 value 78568.857638
## final  value 78568.051828 
## converged
## # weights:  43
## initial  value 123476.675100 
## iter  10 value 78642.228641
## iter  20 value 78568.866433
## iter  30 value 78568.053467
## iter  30 value 78568.053467
## iter  30 value 78568.053467
## final  value 78568.053467 
## converged
## # weights:  71
## initial  value 113769.952232 
## iter  10 value 78729.622749
## iter  20 value 78569.881452
## iter  30 value 78568.070176
## final  value 78568.068104 
## converged
## # weights:  15
## initial  value 99815.273721 
## final  value 78309.000000 
## converged
## # weights:  43
## initial  value 114718.314941 
## final  value 78309.000000 
## converged
## # weights:  71
## initial  value 100886.183026 
## final  value 78309.000000 
## converged
## # weights:  15
## initial  value 109912.635985 
## iter  10 value 78452.731652
## iter  20 value 78317.610280
## iter  30 value 78315.923157
## final  value 78315.896874 
## converged
## # weights:  43
## initial  value 121613.928268 
## iter  10 value 78558.460648
## iter  20 value 78342.499705
## iter  30 value 78317.961343
## iter  40 value 78315.042264
## iter  50 value 78313.960467
## iter  60 value 78313.895642
## iter  60 value 78313.895062
## iter  60 value 78313.894531
## final  value 78313.894531 
## converged
## # weights:  71
## initial  value 129365.806434 
## iter  10 value 78427.098742
## iter  20 value 78312.713954
## iter  30 value 78312.135349
## iter  40 value 78311.718854
## final  value 78311.713207 
## converged
## # weights:  15
## initial  value 107531.818841 
## iter  10 value 78410.118658
## iter  20 value 78310.169394
## iter  30 value 78309.061210
## final  value 78309.056679 
## converged
## # weights:  43
## initial  value 122145.315331 
## iter  10 value 78421.517111
## iter  20 value 78310.308633
## iter  30 value 78309.080161
## final  value 78309.076856 
## converged
## # weights:  71
## initial  value 107388.149610 
## iter  10 value 78505.521017
## iter  20 value 78311.279146
## iter  30 value 78309.069787
## final  value 78309.056907 
## converged
## # weights:  15
## initial  value 100483.562750 
## final  value 77129.000000 
## converged
## # weights:  43
## initial  value 104837.089138 
## final  value 77129.000000 
## converged
## # weights:  71
## initial  value 113136.234443 
## final  value 77129.000000 
## converged
## # weights:  15
## initial  value 113620.064360 
## iter  10 value 77153.596312
## iter  20 value 77135.888048
## iter  20 value 77135.888003
## iter  20 value 77135.888003
## final  value 77135.888003 
## converged
## # weights:  43
## initial  value 116985.343150 
## iter  10 value 77242.994303
## iter  20 value 77134.842276
## iter  30 value 77132.893111
## iter  40 value 77132.839325
## final  value 77132.827060 
## converged
## # weights:  71
## initial  value 116257.322948 
## iter  10 value 77373.721903
## iter  20 value 77134.831467
## iter  30 value 77132.316381
## iter  40 value 77132.137264
## iter  50 value 77132.048308
## iter  60 value 77131.726530
## final  value 77131.708907 
## converged
## # weights:  15
## initial  value 110951.413820 
## iter  10 value 77216.283632
## iter  20 value 77130.010193
## iter  30 value 77129.038556
## iter  30 value 77129.038004
## iter  30 value 77129.037988
## final  value 77129.037988 
## converged
## # weights:  43
## initial  value 125141.657805 
## iter  10 value 77193.568110
## iter  20 value 77129.756739
## iter  30 value 77129.068372
## final  value 77129.067415 
## converged
## # weights:  71
## initial  value 105772.568192 
## iter  10 value 77279.413034
## iter  20 value 77130.755953
## final  value 77129.099108 
## converged
## # weights:  15
## initial  value 114526.373131 
## final  value 78423.000000 
## converged
## # weights:  43
## initial  value 121544.512753 
## final  value 78423.000000 
## converged
## # weights:  71
## initial  value 113996.207025 
## final  value 78423.000000 
## converged
## # weights:  15
## initial  value 116964.508273 
## iter  10 value 78499.346154
## iter  20 value 78430.082722
## final  value 78429.897696 
## converged
## # weights:  43
## initial  value 93124.025137 
## iter  10 value 78430.728737
## final  value 78427.898750 
## converged
## # weights:  71
## initial  value 101536.293475 
## iter  10 value 78436.420279
## iter  20 value 78426.459276
## iter  30 value 78425.768349
## final  value 78425.714545 
## converged
## # weights:  15
## initial  value 121199.070773 
## iter  10 value 78504.939249
## iter  20 value 78423.948906
## final  value 78423.045114 
## converged
## # weights:  43
## initial  value 117731.977132 
## iter  10 value 78520.457388
## iter  20 value 78424.135358
## final  value 78423.072066 
## converged
## # weights:  71
## initial  value 117652.481938 
## iter  10 value 78560.637182
## iter  20 value 78424.602629
## iter  30 value 78423.067552
## iter  30 value 78423.066994
## iter  30 value 78423.066800
## final  value 78423.066800 
## converged
## # weights:  15
## initial  value 103355.001698 
## final  value 78249.000000 
## converged
## # weights:  43
## initial  value 93901.486151 
## final  value 78249.000000 
## converged
## # weights:  71
## initial  value 122504.963407 
## final  value 78249.000000 
## converged
## # weights:  15
## initial  value 115876.059917 
## iter  10 value 78396.440742
## iter  20 value 78255.906271
## final  value 78255.901127 
## converged
## # weights:  43
## initial  value 105913.673576 
## iter  10 value 78623.095509
## iter  20 value 78253.714947
## iter  30 value 78252.903582
## iter  40 value 78252.841396
## iter  40 value 78252.840847
## iter  40 value 78252.840423
## final  value 78252.840423 
## converged
## # weights:  71
## initial  value 113746.692436 
## iter  10 value 78493.772273
## iter  20 value 78254.465214
## iter  30 value 78253.250447
## iter  40 value 78252.731123
## iter  50 value 78251.823498
## final  value 78251.720821 
## converged
## # weights:  15
## initial  value 112122.005343 
## iter  10 value 78299.692368
## iter  20 value 78249.589209
## final  value 78249.035728 
## converged
## # weights:  43
## initial  value 112693.342710 
## iter  10 value 78395.219745
## iter  20 value 78250.696275
## iter  30 value 78249.038545
## final  value 78249.035924 
## converged
## # weights:  71
## initial  value 94024.244121 
## iter  10 value 78254.798521
## iter  20 value 78249.709044
## final  value 78249.700431 
## converged
```

```
## Warning: There were missing values in resampled performance measures.
```

```
## # weights:  15
## initial  value 110489.154872 
## final  value 78120.000000 
## converged
```

```r
modFitOp2
```

```
## Neural Network 
## 
## 14718 samples
##    12 predictor
## 
## No pre-processing
## Resampling: Bootstrapped (25 reps) 
## 
## Summary of sample sizes: 14718, 14718, 14718, 14718, 14718, 14718, ... 
## 
## Resampling results across tuning parameters:
## 
##   size  decay  RMSE  Rsquared  RMSE SD  Rsquared SD
##   1     0e+00  2       NaN     0.02        NA      
##   1     1e-04  2     0.003     0.02     0.003      
##   1     1e-01  2     0.005     0.02     0.010      
##   3     0e+00  2       NaN     0.02        NA      
##   3     1e-04  2     0.002     0.02     0.003      
##   3     1e-01  2     0.012     0.02     0.011      
##   5     0e+00  2       NaN     0.02        NA      
##   5     1e-04  2     0.002     0.02     0.002      
##   5     1e-01  2     0.015     0.02     0.014      
## 
## RMSE was used to select the optimal model using  the smallest value.
## The final values used for the model were size = 1 and decay = 0.
```

```r
modFitOp3 <- train(as.numeric(classe) ~ ., method="gbm", data=training)
```

```
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0978             nan     0.1000    0.0601
##      2        2.0428             nan     0.1000    0.0547
##      3        1.9939             nan     0.1000    0.0499
##      4        1.9499             nan     0.1000    0.0432
##      5        1.9112             nan     0.1000    0.0379
##      6        1.8765             nan     0.1000    0.0341
##      7        1.8453             nan     0.1000    0.0296
##      8        1.8177             nan     0.1000    0.0270
##      9        1.7936             nan     0.1000    0.0229
##     10        1.7689             nan     0.1000    0.0249
##     20        1.5908             nan     0.1000    0.0150
##     40        1.3810             nan     0.1000    0.0070
##     60        1.2677             nan     0.1000    0.0042
##     80        1.1916             nan     0.1000    0.0026
##    100        1.1335             nan     0.1000    0.0024
##    120        1.0888             nan     0.1000    0.0013
##    140        1.0517             nan     0.1000    0.0014
##    150        1.0364             nan     0.1000    0.0009
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0504             nan     0.1000    0.1102
##      2        1.9631             nan     0.1000    0.0879
##      3        1.8928             nan     0.1000    0.0728
##      4        1.8350             nan     0.1000    0.0579
##      5        1.7867             nan     0.1000    0.0471
##      6        1.7389             nan     0.1000    0.0471
##      7        1.6994             nan     0.1000    0.0397
##      8        1.6606             nan     0.1000    0.0375
##      9        1.6224             nan     0.1000    0.0360
##     10        1.5890             nan     0.1000    0.0325
##     20        1.3635             nan     0.1000    0.0157
##     40        1.1427             nan     0.1000    0.0067
##     60        1.0154             nan     0.1000    0.0053
##     80        0.9326             nan     0.1000    0.0033
##    100        0.8661             nan     0.1000    0.0024
##    120        0.8143             nan     0.1000    0.0020
##    140        0.7738             nan     0.1000    0.0033
##    150        0.7568             nan     0.1000    0.0008
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0230             nan     0.1000    0.1334
##      2        1.9146             nan     0.1000    0.1055
##      3        1.8224             nan     0.1000    0.0920
##      4        1.7465             nan     0.1000    0.0741
##      5        1.6823             nan     0.1000    0.0637
##      6        1.6277             nan     0.1000    0.0557
##      7        1.5703             nan     0.1000    0.0568
##      8        1.5288             nan     0.1000    0.0409
##      9        1.4959             nan     0.1000    0.0309
##     10        1.4552             nan     0.1000    0.0403
##     20        1.2276             nan     0.1000    0.0149
##     40        0.9822             nan     0.1000    0.0086
##     60        0.8580             nan     0.1000    0.0024
##     80        0.7738             nan     0.1000    0.0016
##    100        0.7165             nan     0.1000    0.0024
##    120        0.6638             nan     0.1000    0.0017
##    140        0.6248             nan     0.1000    0.0012
##    150        0.6065             nan     0.1000    0.0008
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0959             nan     0.1000    0.0650
##      2        2.0396             nan     0.1000    0.0540
##      3        1.9893             nan     0.1000    0.0520
##      4        1.9454             nan     0.1000    0.0449
##      5        1.9048             nan     0.1000    0.0393
##      6        1.8699             nan     0.1000    0.0353
##      7        1.8376             nan     0.1000    0.0328
##      8        1.8096             nan     0.1000    0.0269
##      9        1.7835             nan     0.1000    0.0259
##     10        1.7608             nan     0.1000    0.0219
##     20        1.5804             nan     0.1000    0.0146
##     40        1.3708             nan     0.1000    0.0066
##     60        1.2557             nan     0.1000    0.0043
##     80        1.1820             nan     0.1000    0.0035
##    100        1.1253             nan     0.1000    0.0017
##    120        1.0808             nan     0.1000    0.0016
##    140        1.0446             nan     0.1000    0.0014
##    150        1.0286             nan     0.1000    0.0013
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0483             nan     0.1000    0.1117
##      2        1.9586             nan     0.1000    0.0888
##      3        1.8865             nan     0.1000    0.0710
##      4        1.8283             nan     0.1000    0.0571
##      5        1.7809             nan     0.1000    0.0454
##      6        1.7321             nan     0.1000    0.0475
##      7        1.6938             nan     0.1000    0.0365
##      8        1.6528             nan     0.1000    0.0404
##      9        1.6128             nan     0.1000    0.0394
##     10        1.5817             nan     0.1000    0.0294
##     20        1.3584             nan     0.1000    0.0117
##     40        1.1470             nan     0.1000    0.0058
##     60        1.0214             nan     0.1000    0.0052
##     80        0.9245             nan     0.1000    0.0038
##    100        0.8554             nan     0.1000    0.0013
##    120        0.8063             nan     0.1000    0.0006
##    140        0.7648             nan     0.1000    0.0017
##    150        0.7483             nan     0.1000    0.0008
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0231             nan     0.1000    0.1347
##      2        1.9106             nan     0.1000    0.1119
##      3        1.8169             nan     0.1000    0.0943
##      4        1.7396             nan     0.1000    0.0757
##      5        1.6730             nan     0.1000    0.0669
##      6        1.6147             nan     0.1000    0.0550
##      7        1.5591             nan     0.1000    0.0538
##      8        1.5181             nan     0.1000    0.0406
##      9        1.4746             nan     0.1000    0.0427
##     10        1.4377             nan     0.1000    0.0364
##     20        1.2142             nan     0.1000    0.0162
##     40        0.9816             nan     0.1000    0.0062
##     60        0.8512             nan     0.1000    0.0035
##     80        0.7727             nan     0.1000    0.0027
##    100        0.6961             nan     0.1000    0.0047
##    120        0.6466             nan     0.1000    0.0010
##    140        0.6114             nan     0.1000    0.0007
##    150        0.5938             nan     0.1000    0.0014
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.1093             nan     0.1000    0.0626
##      2        2.0549             nan     0.1000    0.0564
##      3        2.0045             nan     0.1000    0.0525
##      4        1.9605             nan     0.1000    0.0424
##      5        1.9209             nan     0.1000    0.0400
##      6        1.8863             nan     0.1000    0.0354
##      7        1.8543             nan     0.1000    0.0310
##      8        1.8281             nan     0.1000    0.0244
##      9        1.8003             nan     0.1000    0.0262
##     10        1.7761             nan     0.1000    0.0240
##     20        1.5917             nan     0.1000    0.0138
##     40        1.3783             nan     0.1000    0.0075
##     60        1.2658             nan     0.1000    0.0044
##     80        1.1913             nan     0.1000    0.0030
##    100        1.1358             nan     0.1000    0.0024
##    120        1.0925             nan     0.1000    0.0018
##    140        1.0566             nan     0.1000    0.0015
##    150        1.0424             nan     0.1000    0.0010
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0646             nan     0.1000    0.1065
##      2        1.9755             nan     0.1000    0.0872
##      3        1.9036             nan     0.1000    0.0721
##      4        1.8460             nan     0.1000    0.0575
##      5        1.7963             nan     0.1000    0.0506
##      6        1.7456             nan     0.1000    0.0490
##      7        1.7039             nan     0.1000    0.0411
##      8        1.6617             nan     0.1000    0.0399
##      9        1.6222             nan     0.1000    0.0386
##     10        1.5877             nan     0.1000    0.0334
##     20        1.3631             nan     0.1000    0.0154
##     40        1.1547             nan     0.1000    0.0071
##     60        1.0331             nan     0.1000    0.0080
##     80        0.9437             nan     0.1000    0.0021
##    100        0.8842             nan     0.1000    0.0019
##    120        0.8334             nan     0.1000    0.0019
##    140        0.7878             nan     0.1000    0.0023
##    150        0.7729             nan     0.1000    0.0010
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0361             nan     0.1000    0.1413
##      2        1.9254             nan     0.1000    0.1117
##      3        1.8294             nan     0.1000    0.0968
##      4        1.7505             nan     0.1000    0.0771
##      5        1.6860             nan     0.1000    0.0628
##      6        1.6273             nan     0.1000    0.0576
##      7        1.5712             nan     0.1000    0.0555
##      8        1.5274             nan     0.1000    0.0425
##      9        1.4845             nan     0.1000    0.0428
##     10        1.4497             nan     0.1000    0.0341
##     20        1.2257             nan     0.1000    0.0145
##     40        0.9894             nan     0.1000    0.0064
##     60        0.8736             nan     0.1000    0.0036
##     80        0.7849             nan     0.1000    0.0023
##    100        0.7218             nan     0.1000    0.0030
##    120        0.6663             nan     0.1000    0.0029
##    140        0.6280             nan     0.1000    0.0009
##    150        0.6095             nan     0.1000    0.0008
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.1413             nan     0.1000    0.0613
##      2        2.0811             nan     0.1000    0.0603
##      3        2.0319             nan     0.1000    0.0474
##      4        1.9839             nan     0.1000    0.0474
##      5        1.9452             nan     0.1000    0.0371
##      6        1.9076             nan     0.1000    0.0379
##      7        1.8755             nan     0.1000    0.0307
##      8        1.8445             nan     0.1000    0.0315
##      9        1.8194             nan     0.1000    0.0243
##     10        1.7949             nan     0.1000    0.0249
##     20        1.6156             nan     0.1000    0.0138
##     40        1.4113             nan     0.1000    0.0075
##     60        1.2987             nan     0.1000    0.0040
##     80        1.2190             nan     0.1000    0.0031
##    100        1.1590             nan     0.1000    0.0021
##    120        1.1107             nan     0.1000    0.0016
##    140        1.0717             nan     0.1000    0.0015
##    150        1.0553             nan     0.1000    0.0013
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0909             nan     0.1000    0.1128
##      2        1.9998             nan     0.1000    0.0906
##      3        1.9255             nan     0.1000    0.0740
##      4        1.8651             nan     0.1000    0.0576
##      5        1.8157             nan     0.1000    0.0486
##      6        1.7640             nan     0.1000    0.0505
##      7        1.7205             nan     0.1000    0.0412
##      8        1.6836             nan     0.1000    0.0365
##      9        1.6526             nan     0.1000    0.0304
##     10        1.6179             nan     0.1000    0.0331
##     20        1.3975             nan     0.1000    0.0136
##     40        1.1810             nan     0.1000    0.0068
##     60        1.0529             nan     0.1000    0.0038
##     80        0.9643             nan     0.1000    0.0023
##    100        0.8864             nan     0.1000    0.0010
##    120        0.8338             nan     0.1000    0.0022
##    140        0.7896             nan     0.1000    0.0038
##    150        0.7711             nan     0.1000    0.0010
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0663             nan     0.1000    0.1345
##      2        1.9536             nan     0.1000    0.1138
##      3        1.8594             nan     0.1000    0.0931
##      4        1.7806             nan     0.1000    0.0785
##      5        1.7133             nan     0.1000    0.0658
##      6        1.6509             nan     0.1000    0.0595
##      7        1.6030             nan     0.1000    0.0475
##      8        1.5577             nan     0.1000    0.0446
##      9        1.5189             nan     0.1000    0.0375
##     10        1.4806             nan     0.1000    0.0372
##     20        1.2530             nan     0.1000    0.0167
##     40        1.0056             nan     0.1000    0.0153
##     60        0.8793             nan     0.1000    0.0053
##     80        0.7999             nan     0.1000    0.0027
##    100        0.7420             nan     0.1000    0.0007
##    120        0.6831             nan     0.1000    0.0013
##    140        0.6338             nan     0.1000    0.0018
##    150        0.6179             nan     0.1000    0.0007
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.1342             nan     0.1000    0.0603
##      2        2.0755             nan     0.1000    0.0585
##      3        2.0253             nan     0.1000    0.0503
##      4        1.9784             nan     0.1000    0.0447
##      5        1.9384             nan     0.1000    0.0392
##      6        1.9016             nan     0.1000    0.0364
##      7        1.8697             nan     0.1000    0.0308
##      8        1.8407             nan     0.1000    0.0284
##      9        1.8140             nan     0.1000    0.0265
##     10        1.7890             nan     0.1000    0.0250
##     20        1.6047             nan     0.1000    0.0159
##     40        1.3973             nan     0.1000    0.0071
##     60        1.2847             nan     0.1000    0.0042
##     80        1.2071             nan     0.1000    0.0031
##    100        1.1496             nan     0.1000    0.0022
##    120        1.1037             nan     0.1000    0.0018
##    140        1.0664             nan     0.1000    0.0014
##    150        1.0496             nan     0.1000    0.0014
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0844             nan     0.1000    0.1128
##      2        1.9943             nan     0.1000    0.0900
##      3        1.9202             nan     0.1000    0.0732
##      4        1.8610             nan     0.1000    0.0592
##      5        1.8123             nan     0.1000    0.0480
##      6        1.7616             nan     0.1000    0.0478
##      7        1.7217             nan     0.1000    0.0394
##      8        1.6744             nan     0.1000    0.0462
##      9        1.6361             nan     0.1000    0.0382
##     10        1.6029             nan     0.1000    0.0317
##     20        1.3814             nan     0.1000    0.0141
##     40        1.1620             nan     0.1000    0.0064
##     60        1.0379             nan     0.1000    0.0071
##     80        0.9474             nan     0.1000    0.0061
##    100        0.8737             nan     0.1000    0.0018
##    120        0.8219             nan     0.1000    0.0010
##    140        0.7791             nan     0.1000    0.0021
##    150        0.7604             nan     0.1000    0.0009
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0579             nan     0.1000    0.1395
##      2        1.9439             nan     0.1000    0.1141
##      3        1.8496             nan     0.1000    0.0927
##      4        1.7677             nan     0.1000    0.0826
##      5        1.7005             nan     0.1000    0.0649
##      6        1.6469             nan     0.1000    0.0519
##      7        1.5913             nan     0.1000    0.0553
##      8        1.5466             nan     0.1000    0.0449
##      9        1.5029             nan     0.1000    0.0419
##     10        1.4685             nan     0.1000    0.0334
##     20        1.2566             nan     0.1000    0.0123
##     40        1.0073             nan     0.1000    0.0066
##     60        0.8841             nan     0.1000    0.0079
##     80        0.8014             nan     0.1000    0.0022
##    100        0.7262             nan     0.1000    0.0022
##    120        0.6804             nan     0.1000    0.0006
##    140        0.6386             nan     0.1000    0.0036
##    150        0.6216             nan     0.1000    0.0015
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.1001             nan     0.1000    0.0558
##      2        2.0382             nan     0.1000    0.0619
##      3        1.9883             nan     0.1000    0.0484
##      4        1.9425             nan     0.1000    0.0437
##      5        1.9031             nan     0.1000    0.0391
##      6        1.8671             nan     0.1000    0.0377
##      7        1.8371             nan     0.1000    0.0289
##      8        1.8059             nan     0.1000    0.0302
##      9        1.7811             nan     0.1000    0.0249
##     10        1.7583             nan     0.1000    0.0222
##     20        1.5801             nan     0.1000    0.0121
##     40        1.3716             nan     0.1000    0.0077
##     60        1.2611             nan     0.1000    0.0041
##     80        1.1864             nan     0.1000    0.0028
##    100        1.1311             nan     0.1000    0.0019
##    120        1.0874             nan     0.1000    0.0011
##    140        1.0524             nan     0.1000    0.0012
##    150        1.0375             nan     0.1000    0.0012
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0473             nan     0.1000    0.1097
##      2        1.9577             nan     0.1000    0.0928
##      3        1.8850             nan     0.1000    0.0729
##      4        1.8231             nan     0.1000    0.0612
##      5        1.7706             nan     0.1000    0.0512
##      6        1.7271             nan     0.1000    0.0416
##      7        1.6813             nan     0.1000    0.0439
##      8        1.6459             nan     0.1000    0.0337
##      9        1.6097             nan     0.1000    0.0352
##     10        1.5770             nan     0.1000    0.0325
##     20        1.3535             nan     0.1000    0.0156
##     40        1.1469             nan     0.1000    0.0051
##     60        1.0225             nan     0.1000    0.0042
##     80        0.9462             nan     0.1000    0.0029
##    100        0.8822             nan     0.1000    0.0018
##    120        0.8233             nan     0.1000    0.0013
##    140        0.7817             nan     0.1000    0.0010
##    150        0.7631             nan     0.1000    0.0008
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0208             nan     0.1000    0.1408
##      2        1.9090             nan     0.1000    0.1099
##      3        1.8147             nan     0.1000    0.0962
##      4        1.7366             nan     0.1000    0.0770
##      5        1.6759             nan     0.1000    0.0583
##      6        1.6110             nan     0.1000    0.0633
##      7        1.5599             nan     0.1000    0.0493
##      8        1.5185             nan     0.1000    0.0392
##      9        1.4758             nan     0.1000    0.0421
##     10        1.4437             nan     0.1000    0.0316
##     20        1.2288             nan     0.1000    0.0130
##     40        0.9890             nan     0.1000    0.0055
##     60        0.8628             nan     0.1000    0.0053
##     80        0.7842             nan     0.1000    0.0013
##    100        0.7194             nan     0.1000    0.0031
##    120        0.6715             nan     0.1000    0.0020
##    140        0.6312             nan     0.1000    0.0017
##    150        0.6135             nan     0.1000    0.0020
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.1028             nan     0.1000    0.0656
##      2        2.0471             nan     0.1000    0.0533
##      3        1.9927             nan     0.1000    0.0545
##      4        1.9499             nan     0.1000    0.0416
##      5        1.9061             nan     0.1000    0.0446
##      6        1.8714             nan     0.1000    0.0328
##      7        1.8371             nan     0.1000    0.0349
##      8        1.8091             nan     0.1000    0.0283
##      9        1.7822             nan     0.1000    0.0284
##     10        1.7597             nan     0.1000    0.0223
##     20        1.5765             nan     0.1000    0.0149
##     40        1.3696             nan     0.1000    0.0068
##     60        1.2556             nan     0.1000    0.0040
##     80        1.1776             nan     0.1000    0.0029
##    100        1.1195             nan     0.1000    0.0022
##    120        1.0732             nan     0.1000    0.0018
##    140        1.0364             nan     0.1000    0.0012
##    150        1.0202             nan     0.1000    0.0011
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0565             nan     0.1000    0.1169
##      2        1.9648             nan     0.1000    0.0913
##      3        1.8897             nan     0.1000    0.0729
##      4        1.8296             nan     0.1000    0.0604
##      5        1.7777             nan     0.1000    0.0534
##      6        1.7327             nan     0.1000    0.0447
##      7        1.6855             nan     0.1000    0.0461
##      8        1.6508             nan     0.1000    0.0339
##      9        1.6177             nan     0.1000    0.0314
##     10        1.5818             nan     0.1000    0.0353
##     20        1.3512             nan     0.1000    0.0160
##     40        1.1322             nan     0.1000    0.0103
##     60        1.0104             nan     0.1000    0.0050
##     80        0.9219             nan     0.1000    0.0050
##    100        0.8650             nan     0.1000    0.0025
##    120        0.8187             nan     0.1000    0.0018
##    140        0.7656             nan     0.1000    0.0024
##    150        0.7415             nan     0.1000    0.0013
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0315             nan     0.1000    0.1399
##      2        1.9166             nan     0.1000    0.1150
##      3        1.8208             nan     0.1000    0.0966
##      4        1.7411             nan     0.1000    0.0799
##      5        1.6734             nan     0.1000    0.0678
##      6        1.6162             nan     0.1000    0.0574
##      7        1.5696             nan     0.1000    0.0448
##      8        1.5202             nan     0.1000    0.0475
##      9        1.4833             nan     0.1000    0.0363
##     10        1.4540             nan     0.1000    0.0282
##     20        1.2191             nan     0.1000    0.0137
##     40        0.9735             nan     0.1000    0.0066
##     60        0.8504             nan     0.1000    0.0053
##     80        0.7685             nan     0.1000    0.0047
##    100        0.6978             nan     0.1000    0.0022
##    120        0.6503             nan     0.1000    0.0031
##    140        0.6051             nan     0.1000    0.0014
##    150        0.5880             nan     0.1000    0.0024
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.1099             nan     0.1000    0.0627
##      2        2.0531             nan     0.1000    0.0554
##      3        2.0042             nan     0.1000    0.0489
##      4        1.9592             nan     0.1000    0.0450
##      5        1.9195             nan     0.1000    0.0373
##      6        1.8841             nan     0.1000    0.0334
##      7        1.8522             nan     0.1000    0.0311
##      8        1.8239             nan     0.1000    0.0280
##      9        1.8006             nan     0.1000    0.0224
##     10        1.7757             nan     0.1000    0.0247
##     20        1.5938             nan     0.1000    0.0142
##     40        1.3844             nan     0.1000    0.0075
##     60        1.2723             nan     0.1000    0.0041
##     80        1.1977             nan     0.1000    0.0028
##    100        1.1429             nan     0.1000    0.0022
##    120        1.1006             nan     0.1000    0.0013
##    140        1.0648             nan     0.1000    0.0011
##    150        1.0500             nan     0.1000    0.0008
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0629             nan     0.1000    0.1116
##      2        1.9736             nan     0.1000    0.0867
##      3        1.9014             nan     0.1000    0.0716
##      4        1.8427             nan     0.1000    0.0574
##      5        1.7889             nan     0.1000    0.0540
##      6        1.7425             nan     0.1000    0.0443
##      7        1.7037             nan     0.1000    0.0379
##      8        1.6602             nan     0.1000    0.0414
##      9        1.6285             nan     0.1000    0.0311
##     10        1.5973             nan     0.1000    0.0290
##     20        1.3674             nan     0.1000    0.0153
##     40        1.1640             nan     0.1000    0.0064
##     60        1.0348             nan     0.1000    0.0072
##     80        0.9559             nan     0.1000    0.0020
##    100        0.8909             nan     0.1000    0.0015
##    120        0.8330             nan     0.1000    0.0004
##    140        0.7910             nan     0.1000    0.0027
##    150        0.7734             nan     0.1000    0.0013
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0375             nan     0.1000    0.1311
##      2        1.9266             nan     0.1000    0.1107
##      3        1.8314             nan     0.1000    0.0942
##      4        1.7532             nan     0.1000    0.0769
##      5        1.6868             nan     0.1000    0.0647
##      6        1.6328             nan     0.1000    0.0533
##      7        1.5769             nan     0.1000    0.0560
##      8        1.5350             nan     0.1000    0.0409
##      9        1.4923             nan     0.1000    0.0423
##     10        1.4564             nan     0.1000    0.0359
##     20        1.2388             nan     0.1000    0.0171
##     40        0.9991             nan     0.1000    0.0073
##     60        0.8850             nan     0.1000    0.0041
##     80        0.7964             nan     0.1000    0.0023
##    100        0.7249             nan     0.1000    0.0031
##    120        0.6852             nan     0.1000    0.0008
##    140        0.6459             nan     0.1000    0.0002
##    150        0.6311             nan     0.1000    0.0008
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.1020             nan     0.1000    0.0632
##      2        2.0448             nan     0.1000    0.0574
##      3        1.9952             nan     0.1000    0.0464
##      4        1.9497             nan     0.1000    0.0456
##      5        1.9101             nan     0.1000    0.0387
##      6        1.8739             nan     0.1000    0.0365
##      7        1.8419             nan     0.1000    0.0312
##      8        1.8127             nan     0.1000    0.0287
##      9        1.7893             nan     0.1000    0.0222
##     10        1.7651             nan     0.1000    0.0249
##     20        1.5841             nan     0.1000    0.0144
##     40        1.3775             nan     0.1000    0.0074
##     60        1.2655             nan     0.1000    0.0044
##     80        1.1897             nan     0.1000    0.0021
##    100        1.1311             nan     0.1000    0.0026
##    120        1.0853             nan     0.1000    0.0017
##    140        1.0481             nan     0.1000    0.0015
##    150        1.0314             nan     0.1000    0.0014
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0537             nan     0.1000    0.1092
##      2        1.9639             nan     0.1000    0.0916
##      3        1.8904             nan     0.1000    0.0740
##      4        1.8310             nan     0.1000    0.0598
##      5        1.7812             nan     0.1000    0.0486
##      6        1.7314             nan     0.1000    0.0501
##      7        1.6937             nan     0.1000    0.0384
##      8        1.6533             nan     0.1000    0.0394
##      9        1.6153             nan     0.1000    0.0354
##     10        1.5848             nan     0.1000    0.0284
##     20        1.3590             nan     0.1000    0.0133
##     40        1.1445             nan     0.1000    0.0080
##     60        1.0182             nan     0.1000    0.0046
##     80        0.9361             nan     0.1000    0.0037
##    100        0.8697             nan     0.1000    0.0009
##    120        0.8114             nan     0.1000    0.0045
##    140        0.7643             nan     0.1000    0.0014
##    150        0.7445             nan     0.1000    0.0007
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0289             nan     0.1000    0.1321
##      2        1.9148             nan     0.1000    0.1145
##      3        1.8217             nan     0.1000    0.0936
##      4        1.7422             nan     0.1000    0.0797
##      5        1.6770             nan     0.1000    0.0643
##      6        1.6224             nan     0.1000    0.0551
##      7        1.5653             nan     0.1000    0.0556
##      8        1.5219             nan     0.1000    0.0418
##      9        1.4795             nan     0.1000    0.0407
##     10        1.4440             nan     0.1000    0.0342
##     20        1.2258             nan     0.1000    0.0167
##     40        0.9907             nan     0.1000    0.0065
##     60        0.8570             nan     0.1000    0.0043
##     80        0.7735             nan     0.1000    0.0035
##    100        0.7109             nan     0.1000    0.0027
##    120        0.6601             nan     0.1000    0.0022
##    140        0.6212             nan     0.1000    0.0007
##    150        0.6030             nan     0.1000    0.0029
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.1153             nan     0.1000    0.0634
##      2        2.0583             nan     0.1000    0.0556
##      3        2.0083             nan     0.1000    0.0504
##      4        1.9639             nan     0.1000    0.0451
##      5        1.9245             nan     0.1000    0.0389
##      6        1.8886             nan     0.1000    0.0336
##      7        1.8571             nan     0.1000    0.0332
##      8        1.8279             nan     0.1000    0.0288
##      9        1.8026             nan     0.1000    0.0243
##     10        1.7801             nan     0.1000    0.0222
##     20        1.5998             nan     0.1000    0.0136
##     40        1.3940             nan     0.1000    0.0066
##     60        1.2811             nan     0.1000    0.0046
##     80        1.2051             nan     0.1000    0.0026
##    100        1.1481             nan     0.1000    0.0019
##    120        1.1036             nan     0.1000    0.0014
##    140        1.0676             nan     0.1000    0.0013
##    150        1.0517             nan     0.1000    0.0012
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0684             nan     0.1000    0.1135
##      2        1.9791             nan     0.1000    0.0899
##      3        1.9066             nan     0.1000    0.0704
##      4        1.8472             nan     0.1000    0.0589
##      5        1.7990             nan     0.1000    0.0478
##      6        1.7488             nan     0.1000    0.0480
##      7        1.7101             nan     0.1000    0.0374
##      8        1.6683             nan     0.1000    0.0426
##      9        1.6330             nan     0.1000    0.0345
##     10        1.6018             nan     0.1000    0.0314
##     20        1.3762             nan     0.1000    0.0158
##     40        1.1616             nan     0.1000    0.0084
##     60        1.0390             nan     0.1000    0.0050
##     80        0.9525             nan     0.1000    0.0025
##    100        0.8829             nan     0.1000    0.0013
##    120        0.8332             nan     0.1000    0.0033
##    140        0.7911             nan     0.1000    0.0008
##    150        0.7746             nan     0.1000    0.0018
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0414             nan     0.1000    0.1358
##      2        1.9310             nan     0.1000    0.1112
##      3        1.8367             nan     0.1000    0.0956
##      4        1.7593             nan     0.1000    0.0775
##      5        1.6918             nan     0.1000    0.0661
##      6        1.6355             nan     0.1000    0.0566
##      7        1.5905             nan     0.1000    0.0437
##      8        1.5387             nan     0.1000    0.0523
##      9        1.5049             nan     0.1000    0.0326
##     10        1.4644             nan     0.1000    0.0408
##     20        1.2414             nan     0.1000    0.0158
##     40        0.9866             nan     0.1000    0.0082
##     60        0.8572             nan     0.1000    0.0058
##     80        0.7767             nan     0.1000    0.0035
##    100        0.7173             nan     0.1000    0.0020
##    120        0.6699             nan     0.1000    0.0036
##    140        0.6336             nan     0.1000    0.0027
##    150        0.6186             nan     0.1000    0.0013
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.1224             nan     0.1000    0.0628
##      2        2.0661             nan     0.1000    0.0565
##      3        2.0157             nan     0.1000    0.0528
##      4        1.9704             nan     0.1000    0.0444
##      5        1.9296             nan     0.1000    0.0398
##      6        1.8944             nan     0.1000    0.0364
##      7        1.8633             nan     0.1000    0.0319
##      8        1.8348             nan     0.1000    0.0269
##      9        1.8090             nan     0.1000    0.0247
##     10        1.7863             nan     0.1000    0.0217
##     20        1.6035             nan     0.1000    0.0139
##     40        1.4007             nan     0.1000    0.0065
##     60        1.2872             nan     0.1000    0.0040
##     80        1.2082             nan     0.1000    0.0033
##    100        1.1479             nan     0.1000    0.0021
##    120        1.1013             nan     0.1000    0.0014
##    140        1.0631             nan     0.1000    0.0017
##    150        1.0474             nan     0.1000    0.0013
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0764             nan     0.1000    0.1107
##      2        1.9866             nan     0.1000    0.0893
##      3        1.9138             nan     0.1000    0.0714
##      4        1.8548             nan     0.1000    0.0596
##      5        1.8070             nan     0.1000    0.0479
##      6        1.7569             nan     0.1000    0.0499
##      7        1.7183             nan     0.1000    0.0390
##      8        1.6778             nan     0.1000    0.0406
##      9        1.6414             nan     0.1000    0.0341
##     10        1.6055             nan     0.1000    0.0346
##     20        1.3862             nan     0.1000    0.0172
##     40        1.1635             nan     0.1000    0.0070
##     60        1.0315             nan     0.1000    0.0048
##     80        0.9452             nan     0.1000    0.0027
##    100        0.8763             nan     0.1000    0.0028
##    120        0.8241             nan     0.1000    0.0011
##    140        0.7859             nan     0.1000    0.0025
##    150        0.7646             nan     0.1000    0.0007
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0478             nan     0.1000    0.1371
##      2        1.9386             nan     0.1000    0.1081
##      3        1.8433             nan     0.1000    0.0951
##      4        1.7645             nan     0.1000    0.0782
##      5        1.6985             nan     0.1000    0.0646
##      6        1.6448             nan     0.1000    0.0528
##      7        1.5898             nan     0.1000    0.0545
##      8        1.5492             nan     0.1000    0.0405
##      9        1.5112             nan     0.1000    0.0375
##     10        1.4805             nan     0.1000    0.0283
##     20        1.2512             nan     0.1000    0.0133
##     40        1.0058             nan     0.1000    0.0066
##     60        0.8744             nan     0.1000    0.0035
##     80        0.7791             nan     0.1000    0.0078
##    100        0.7133             nan     0.1000    0.0028
##    120        0.6630             nan     0.1000    0.0007
##    140        0.6198             nan     0.1000    0.0008
##    150        0.6031             nan     0.1000    0.0012
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.1250             nan     0.1000    0.0632
##      2        2.0672             nan     0.1000    0.0551
##      3        2.0152             nan     0.1000    0.0499
##      4        1.9693             nan     0.1000    0.0451
##      5        1.9288             nan     0.1000    0.0392
##      6        1.8918             nan     0.1000    0.0363
##      7        1.8600             nan     0.1000    0.0326
##      8        1.8309             nan     0.1000    0.0292
##      9        1.8055             nan     0.1000    0.0257
##     10        1.7821             nan     0.1000    0.0233
##     20        1.6005             nan     0.1000    0.0154
##     40        1.3897             nan     0.1000    0.0061
##     60        1.2766             nan     0.1000    0.0046
##     80        1.2006             nan     0.1000    0.0022
##    100        1.1418             nan     0.1000    0.0021
##    120        1.0961             nan     0.1000    0.0015
##    140        1.0578             nan     0.1000    0.0014
##    150        1.0420             nan     0.1000    0.0010
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0775             nan     0.1000    0.1111
##      2        1.9857             nan     0.1000    0.0942
##      3        1.9110             nan     0.1000    0.0732
##      4        1.8510             nan     0.1000    0.0629
##      5        1.8005             nan     0.1000    0.0489
##      6        1.7575             nan     0.1000    0.0419
##      7        1.7106             nan     0.1000    0.0461
##      8        1.6753             nan     0.1000    0.0344
##      9        1.6363             nan     0.1000    0.0380
##     10        1.6005             nan     0.1000    0.0358
##     20        1.3774             nan     0.1000    0.0141
##     40        1.1596             nan     0.1000    0.0072
##     60        1.0227             nan     0.1000    0.0048
##     80        0.9432             nan     0.1000    0.0018
##    100        0.8794             nan     0.1000    0.0013
##    120        0.8274             nan     0.1000    0.0010
##    140        0.7807             nan     0.1000    0.0012
##    150        0.7611             nan     0.1000    0.0021
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0525             nan     0.1000    0.1386
##      2        1.9371             nan     0.1000    0.1131
##      3        1.8410             nan     0.1000    0.0975
##      4        1.7620             nan     0.1000    0.0775
##      5        1.6951             nan     0.1000    0.0685
##      6        1.6382             nan     0.1000    0.0567
##      7        1.5816             nan     0.1000    0.0556
##      8        1.5366             nan     0.1000    0.0429
##      9        1.4991             nan     0.1000    0.0377
##     10        1.4602             nan     0.1000    0.0380
##     20        1.2354             nan     0.1000    0.0155
##     40        0.9817             nan     0.1000    0.0066
##     60        0.8542             nan     0.1000    0.0030
##     80        0.7779             nan     0.1000    0.0024
##    100        0.7143             nan     0.1000    0.0024
##    120        0.6725             nan     0.1000    0.0017
##    140        0.6364             nan     0.1000    0.0007
##    150        0.6163             nan     0.1000    0.0013
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.1104             nan     0.1000    0.0612
##      2        2.0551             nan     0.1000    0.0575
##      3        2.0044             nan     0.1000    0.0466
##      4        1.9601             nan     0.1000    0.0433
##      5        1.9202             nan     0.1000    0.0388
##      6        1.8850             nan     0.1000    0.0341
##      7        1.8531             nan     0.1000    0.0318
##      8        1.8259             nan     0.1000    0.0268
##      9        1.7998             nan     0.1000    0.0241
##     10        1.7771             nan     0.1000    0.0227
##     20        1.5963             nan     0.1000    0.0150
##     40        1.3869             nan     0.1000    0.0072
##     60        1.2718             nan     0.1000    0.0039
##     80        1.1941             nan     0.1000    0.0031
##    100        1.1360             nan     0.1000    0.0023
##    120        1.0888             nan     0.1000    0.0019
##    140        1.0509             nan     0.1000    0.0018
##    150        1.0347             nan     0.1000    0.0012
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0630             nan     0.1000    0.1079
##      2        1.9734             nan     0.1000    0.0899
##      3        1.9017             nan     0.1000    0.0714
##      4        1.8432             nan     0.1000    0.0586
##      5        1.7948             nan     0.1000    0.0489
##      6        1.7462             nan     0.1000    0.0482
##      7        1.7062             nan     0.1000    0.0388
##      8        1.6687             nan     0.1000    0.0361
##      9        1.6360             nan     0.1000    0.0311
##     10        1.6009             nan     0.1000    0.0345
##     20        1.3688             nan     0.1000    0.0157
##     40        1.1503             nan     0.1000    0.0103
##     60        1.0222             nan     0.1000    0.0044
##     80        0.9362             nan     0.1000    0.0029
##    100        0.8681             nan     0.1000    0.0021
##    120        0.8197             nan     0.1000    0.0026
##    140        0.7776             nan     0.1000    0.0008
##    150        0.7556             nan     0.1000    0.0018
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0354             nan     0.1000    0.1386
##      2        1.9243             nan     0.1000    0.1077
##      3        1.8297             nan     0.1000    0.0941
##      4        1.7532             nan     0.1000    0.0745
##      5        1.6874             nan     0.1000    0.0647
##      6        1.6261             nan     0.1000    0.0604
##      7        1.5772             nan     0.1000    0.0490
##      8        1.5322             nan     0.1000    0.0451
##      9        1.4960             nan     0.1000    0.0355
##     10        1.4569             nan     0.1000    0.0391
##     20        1.2336             nan     0.1000    0.0136
##     40        0.9902             nan     0.1000    0.0081
##     60        0.8557             nan     0.1000    0.0073
##     80        0.7798             nan     0.1000    0.0014
##    100        0.7145             nan     0.1000    0.0044
##    120        0.6700             nan     0.1000    0.0014
##    140        0.6310             nan     0.1000    0.0031
##    150        0.6136             nan     0.1000    0.0019
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.1297             nan     0.1000    0.0612
##      2        2.0722             nan     0.1000    0.0590
##      3        2.0219             nan     0.1000    0.0514
##      4        1.9760             nan     0.1000    0.0456
##      5        1.9364             nan     0.1000    0.0404
##      6        1.8997             nan     0.1000    0.0356
##      7        1.8680             nan     0.1000    0.0319
##      8        1.8392             nan     0.1000    0.0289
##      9        1.8140             nan     0.1000    0.0251
##     10        1.7911             nan     0.1000    0.0229
##     20        1.6165             nan     0.1000    0.0160
##     40        1.4113             nan     0.1000    0.0075
##     60        1.2976             nan     0.1000    0.0040
##     80        1.2203             nan     0.1000    0.0029
##    100        1.1617             nan     0.1000    0.0019
##    120        1.1145             nan     0.1000    0.0015
##    140        1.0780             nan     0.1000    0.0015
##    150        1.0620             nan     0.1000    0.0010
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0804             nan     0.1000    0.1093
##      2        1.9903             nan     0.1000    0.0885
##      3        1.9171             nan     0.1000    0.0725
##      4        1.8576             nan     0.1000    0.0576
##      5        1.8087             nan     0.1000    0.0466
##      6        1.7702             nan     0.1000    0.0361
##      7        1.7260             nan     0.1000    0.0442
##      8        1.6838             nan     0.1000    0.0415
##      9        1.6518             nan     0.1000    0.0302
##     10        1.6169             nan     0.1000    0.0344
##     20        1.3945             nan     0.1000    0.0142
##     40        1.1683             nan     0.1000    0.0070
##     60        1.0482             nan     0.1000    0.0039
##     80        0.9580             nan     0.1000    0.0021
##    100        0.8866             nan     0.1000    0.0060
##    120        0.8309             nan     0.1000    0.0009
##    140        0.7890             nan     0.1000    0.0018
##    150        0.7671             nan     0.1000    0.0004
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0575             nan     0.1000    0.1338
##      2        1.9455             nan     0.1000    0.1123
##      3        1.8517             nan     0.1000    0.0897
##      4        1.7730             nan     0.1000    0.0778
##      5        1.7078             nan     0.1000    0.0657
##      6        1.6513             nan     0.1000    0.0563
##      7        1.6048             nan     0.1000    0.0458
##      8        1.5564             nan     0.1000    0.0477
##      9        1.5192             nan     0.1000    0.0357
##     10        1.4807             nan     0.1000    0.0375
##     20        1.2568             nan     0.1000    0.0151
##     40        1.0107             nan     0.1000    0.0061
##     60        0.8800             nan     0.1000    0.0039
##     80        0.7939             nan     0.1000    0.0025
##    100        0.7260             nan     0.1000    0.0029
##    120        0.6700             nan     0.1000    0.0025
##    140        0.6297             nan     0.1000    0.0019
##    150        0.6149             nan     0.1000    0.0010
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.1276             nan     0.1000    0.0647
##      2        2.0738             nan     0.1000    0.0511
##      3        2.0192             nan     0.1000    0.0520
##      4        1.9752             nan     0.1000    0.0422
##      5        1.9339             nan     0.1000    0.0413
##      6        1.8986             nan     0.1000    0.0356
##      7        1.8660             nan     0.1000    0.0331
##      8        1.8382             nan     0.1000    0.0282
##      9        1.8123             nan     0.1000    0.0258
##     10        1.7886             nan     0.1000    0.0223
##     20        1.6075             nan     0.1000    0.0134
##     40        1.4001             nan     0.1000    0.0071
##     60        1.2858             nan     0.1000    0.0044
##     80        1.2084             nan     0.1000    0.0028
##    100        1.1500             nan     0.1000    0.0020
##    120        1.1027             nan     0.1000    0.0024
##    140        1.0647             nan     0.1000    0.0016
##    150        1.0478             nan     0.1000    0.0012
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0807             nan     0.1000    0.1124
##      2        1.9898             nan     0.1000    0.0895
##      3        1.9159             nan     0.1000    0.0744
##      4        1.8565             nan     0.1000    0.0574
##      5        1.8077             nan     0.1000    0.0496
##      6        1.7640             nan     0.1000    0.0429
##      7        1.7201             nan     0.1000    0.0414
##      8        1.6769             nan     0.1000    0.0414
##      9        1.6435             nan     0.1000    0.0319
##     10        1.6073             nan     0.1000    0.0341
##     20        1.3830             nan     0.1000    0.0152
##     40        1.1657             nan     0.1000    0.0067
##     60        1.0308             nan     0.1000    0.0038
##     80        0.9382             nan     0.1000    0.0030
##    100        0.8688             nan     0.1000    0.0015
##    120        0.8205             nan     0.1000    0.0010
##    140        0.7804             nan     0.1000    0.0007
##    150        0.7584             nan     0.1000    0.0015
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0560             nan     0.1000    0.1334
##      2        1.9439             nan     0.1000    0.1109
##      3        1.8482             nan     0.1000    0.0930
##      4        1.7708             nan     0.1000    0.0765
##      5        1.7045             nan     0.1000    0.0673
##      6        1.6481             nan     0.1000    0.0564
##      7        1.6022             nan     0.1000    0.0445
##      8        1.5512             nan     0.1000    0.0508
##      9        1.5168             nan     0.1000    0.0336
##     10        1.4772             nan     0.1000    0.0378
##     20        1.2407             nan     0.1000    0.0165
##     40        0.9970             nan     0.1000    0.0072
##     60        0.8591             nan     0.1000    0.0048
##     80        0.7833             nan     0.1000    0.0016
##    100        0.7141             nan     0.1000    0.0017
##    120        0.6670             nan     0.1000    0.0025
##    140        0.6306             nan     0.1000    0.0019
##    150        0.6115             nan     0.1000    0.0006
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.1093             nan     0.1000    0.0607
##      2        2.0551             nan     0.1000    0.0533
##      3        2.0063             nan     0.1000    0.0496
##      4        1.9624             nan     0.1000    0.0426
##      5        1.9239             nan     0.1000    0.0393
##      6        1.8898             nan     0.1000    0.0331
##      7        1.8596             nan     0.1000    0.0311
##      8        1.8325             nan     0.1000    0.0274
##      9        1.8079             nan     0.1000    0.0243
##     10        1.7859             nan     0.1000    0.0215
##     20        1.6101             nan     0.1000    0.0145
##     40        1.4060             nan     0.1000    0.0068
##     60        1.2921             nan     0.1000    0.0040
##     80        1.2143             nan     0.1000    0.0023
##    100        1.1559             nan     0.1000    0.0022
##    120        1.1096             nan     0.1000    0.0019
##    140        1.0724             nan     0.1000    0.0016
##    150        1.0561             nan     0.1000    0.0013
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0635             nan     0.1000    0.1067
##      2        1.9764             nan     0.1000    0.0850
##      3        1.9063             nan     0.1000    0.0694
##      4        1.8490             nan     0.1000    0.0550
##      5        1.8027             nan     0.1000    0.0465
##      6        1.7620             nan     0.1000    0.0409
##      7        1.7185             nan     0.1000    0.0416
##      8        1.6802             nan     0.1000    0.0362
##      9        1.6431             nan     0.1000    0.0359
##     10        1.6112             nan     0.1000    0.0317
##     20        1.3913             nan     0.1000    0.0148
##     40        1.1652             nan     0.1000    0.0078
##     60        1.0423             nan     0.1000    0.0039
##     80        0.9485             nan     0.1000    0.0024
##    100        0.8815             nan     0.1000    0.0049
##    120        0.8316             nan     0.1000    0.0016
##    140        0.7913             nan     0.1000    0.0008
##    150        0.7737             nan     0.1000    0.0007
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0400             nan     0.1000    0.1306
##      2        1.9304             nan     0.1000    0.1098
##      3        1.8381             nan     0.1000    0.0920
##      4        1.7632             nan     0.1000    0.0749
##      5        1.6981             nan     0.1000    0.0627
##      6        1.6454             nan     0.1000    0.0514
##      7        1.6029             nan     0.1000    0.0419
##      8        1.5632             nan     0.1000    0.0393
##      9        1.5180             nan     0.1000    0.0447
##     10        1.4856             nan     0.1000    0.0320
##     20        1.2513             nan     0.1000    0.0173
##     40        0.9937             nan     0.1000    0.0120
##     60        0.8566             nan     0.1000    0.0051
##     80        0.7755             nan     0.1000    0.0032
##    100        0.7093             nan     0.1000    0.0020
##    120        0.6563             nan     0.1000    0.0022
##    140        0.6188             nan     0.1000    0.0022
##    150        0.5971             nan     0.1000    0.0013
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0944             nan     0.1000    0.0611
##      2        2.0389             nan     0.1000    0.0565
##      3        1.9900             nan     0.1000    0.0485
##      4        1.9460             nan     0.1000    0.0433
##      5        1.9073             nan     0.1000    0.0380
##      6        1.8723             nan     0.1000    0.0346
##      7        1.8419             nan     0.1000    0.0307
##      8        1.8146             nan     0.1000    0.0274
##      9        1.7895             nan     0.1000    0.0233
##     10        1.7669             nan     0.1000    0.0222
##     20        1.5870             nan     0.1000    0.0147
##     40        1.3833             nan     0.1000    0.0070
##     60        1.2740             nan     0.1000    0.0040
##     80        1.1988             nan     0.1000    0.0025
##    100        1.1416             nan     0.1000    0.0024
##    120        1.0971             nan     0.1000    0.0018
##    140        1.0620             nan     0.1000    0.0009
##    150        1.0465             nan     0.1000    0.0015
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0483             nan     0.1000    0.1090
##      2        1.9608             nan     0.1000    0.0856
##      3        1.8892             nan     0.1000    0.0704
##      4        1.8318             nan     0.1000    0.0573
##      5        1.7847             nan     0.1000    0.0467
##      6        1.7426             nan     0.1000    0.0400
##      7        1.6970             nan     0.1000    0.0439
##      8        1.6538             nan     0.1000    0.0430
##      9        1.6189             nan     0.1000    0.0344
##     10        1.5886             nan     0.1000    0.0299
##     20        1.3683             nan     0.1000    0.0141
##     40        1.1555             nan     0.1000    0.0103
##     60        1.0326             nan     0.1000    0.0040
##     80        0.9538             nan     0.1000    0.0031
##    100        0.8923             nan     0.1000    0.0021
##    120        0.8322             nan     0.1000    0.0010
##    140        0.7879             nan     0.1000    0.0043
##    150        0.7690             nan     0.1000    0.0033
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0234             nan     0.1000    0.1292
##      2        1.9140             nan     0.1000    0.1075
##      3        1.8216             nan     0.1000    0.0904
##      4        1.7451             nan     0.1000    0.0766
##      5        1.6801             nan     0.1000    0.0664
##      6        1.6263             nan     0.1000    0.0533
##      7        1.5713             nan     0.1000    0.0547
##      8        1.5319             nan     0.1000    0.0378
##      9        1.4948             nan     0.1000    0.0364
##     10        1.4551             nan     0.1000    0.0386
##     20        1.2365             nan     0.1000    0.0149
##     40        1.0001             nan     0.1000    0.0063
##     60        0.8637             nan     0.1000    0.0028
##     80        0.7798             nan     0.1000    0.0054
##    100        0.7213             nan     0.1000    0.0023
##    120        0.6711             nan     0.1000    0.0011
##    140        0.6282             nan     0.1000    0.0013
##    150        0.6092             nan     0.1000    0.0016
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.1262             nan     0.1000    0.0683
##      2        2.0715             nan     0.1000    0.0564
##      3        2.0189             nan     0.1000    0.0523
##      4        1.9749             nan     0.1000    0.0431
##      5        1.9321             nan     0.1000    0.0413
##      6        1.8973             nan     0.1000    0.0331
##      7        1.8634             nan     0.1000    0.0344
##      8        1.8362             nan     0.1000    0.0267
##      9        1.8096             nan     0.1000    0.0275
##     10        1.7874             nan     0.1000    0.0214
##     20        1.6053             nan     0.1000    0.0134
##     40        1.3986             nan     0.1000    0.0076
##     60        1.2862             nan     0.1000    0.0038
##     80        1.2103             nan     0.1000    0.0030
##    100        1.1524             nan     0.1000    0.0024
##    120        1.1063             nan     0.1000    0.0016
##    140        1.0686             nan     0.1000    0.0014
##    150        1.0529             nan     0.1000    0.0013
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0816             nan     0.1000    0.1135
##      2        1.9905             nan     0.1000    0.0951
##      3        1.9167             nan     0.1000    0.0734
##      4        1.8568             nan     0.1000    0.0602
##      5        1.8071             nan     0.1000    0.0480
##      6        1.7643             nan     0.1000    0.0393
##      7        1.7161             nan     0.1000    0.0479
##      8        1.6810             nan     0.1000    0.0348
##      9        1.6401             nan     0.1000    0.0392
##     10        1.6061             nan     0.1000    0.0333
##     20        1.3818             nan     0.1000    0.0157
##     40        1.1681             nan     0.1000    0.0075
##     60        1.0390             nan     0.1000    0.0032
##     80        0.9518             nan     0.1000    0.0030
##    100        0.8878             nan     0.1000    0.0019
##    120        0.8230             nan     0.1000    0.0022
##    140        0.7757             nan     0.1000    0.0010
##    150        0.7571             nan     0.1000    0.0007
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0543             nan     0.1000    0.1368
##      2        1.9402             nan     0.1000    0.1127
##      3        1.8456             nan     0.1000    0.0945
##      4        1.7662             nan     0.1000    0.0781
##      5        1.6984             nan     0.1000    0.0684
##      6        1.6409             nan     0.1000    0.0568
##      7        1.5927             nan     0.1000    0.0482
##      8        1.5534             nan     0.1000    0.0377
##      9        1.5065             nan     0.1000    0.0467
##     10        1.4674             nan     0.1000    0.0378
##     20        1.2406             nan     0.1000    0.0159
##     40        0.9892             nan     0.1000    0.0144
##     60        0.8575             nan     0.1000    0.0078
##     80        0.7685             nan     0.1000    0.0025
##    100        0.7117             nan     0.1000    0.0023
##    120        0.6660             nan     0.1000    0.0009
##    140        0.6281             nan     0.1000    0.0018
##    150        0.6046             nan     0.1000    0.0011
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.1206             nan     0.1000    0.0612
##      2        2.0655             nan     0.1000    0.0564
##      3        2.0153             nan     0.1000    0.0492
##      4        1.9713             nan     0.1000    0.0442
##      5        1.9312             nan     0.1000    0.0392
##      6        1.8961             nan     0.1000    0.0342
##      7        1.8639             nan     0.1000    0.0319
##      8        1.8358             nan     0.1000    0.0280
##      9        1.8108             nan     0.1000    0.0252
##     10        1.7872             nan     0.1000    0.0233
##     20        1.6064             nan     0.1000    0.0149
##     40        1.3938             nan     0.1000    0.0081
##     60        1.2766             nan     0.1000    0.0035
##     80        1.1967             nan     0.1000    0.0031
##    100        1.1354             nan     0.1000    0.0020
##    120        1.0871             nan     0.1000    0.0016
##    140        1.0494             nan     0.1000    0.0013
##    150        1.0324             nan     0.1000    0.0008
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0746             nan     0.1000    0.1092
##      2        1.9852             nan     0.1000    0.0884
##      3        1.9126             nan     0.1000    0.0736
##      4        1.8542             nan     0.1000    0.0568
##      5        1.8060             nan     0.1000    0.0458
##      6        1.7568             nan     0.1000    0.0468
##      7        1.7170             nan     0.1000    0.0403
##      8        1.6770             nan     0.1000    0.0393
##      9        1.6432             nan     0.1000    0.0326
##     10        1.6078             nan     0.1000    0.0357
##     20        1.3802             nan     0.1000    0.0156
##     40        1.1566             nan     0.1000    0.0054
##     60        1.0202             nan     0.1000    0.0040
##     80        0.9342             nan     0.1000    0.0021
##    100        0.8588             nan     0.1000    0.0012
##    120        0.8073             nan     0.1000    0.0023
##    140        0.7651             nan     0.1000    0.0018
##    150        0.7500             nan     0.1000    0.0019
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0487             nan     0.1000    0.1386
##      2        1.9362             nan     0.1000    0.1099
##      3        1.8423             nan     0.1000    0.0937
##      4        1.7643             nan     0.1000    0.0752
##      5        1.6995             nan     0.1000    0.0631
##      6        1.6381             nan     0.1000    0.0622
##      7        1.5877             nan     0.1000    0.0504
##      8        1.5456             nan     0.1000    0.0412
##      9        1.5080             nan     0.1000    0.0370
##     10        1.4685             nan     0.1000    0.0392
##     20        1.2395             nan     0.1000    0.0170
##     40        1.0061             nan     0.1000    0.0071
##     60        0.8751             nan     0.1000    0.0046
##     80        0.7831             nan     0.1000    0.0033
##    100        0.7139             nan     0.1000    0.0019
##    120        0.6651             nan     0.1000    0.0019
##    140        0.6205             nan     0.1000    0.0030
##    150        0.6021             nan     0.1000    0.0003
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.1128             nan     0.1000    0.0596
##      2        2.0534             nan     0.1000    0.0585
##      3        2.0057             nan     0.1000    0.0482
##      4        1.9573             nan     0.1000    0.0495
##      5        1.9191             nan     0.1000    0.0380
##      6        1.8818             nan     0.1000    0.0354
##      7        1.8507             nan     0.1000    0.0308
##      8        1.8214             nan     0.1000    0.0297
##      9        1.7971             nan     0.1000    0.0235
##     10        1.7734             nan     0.1000    0.0221
##     20        1.5961             nan     0.1000    0.0137
##     40        1.3940             nan     0.1000    0.0069
##     60        1.2842             nan     0.1000    0.0042
##     80        1.2079             nan     0.1000    0.0031
##    100        1.1505             nan     0.1000    0.0019
##    120        1.1047             nan     0.1000    0.0016
##    140        1.0678             nan     0.1000    0.0013
##    150        1.0505             nan     0.1000    0.0014
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0620             nan     0.1000    0.1117
##      2        1.9721             nan     0.1000    0.0917
##      3        1.8999             nan     0.1000    0.0714
##      4        1.8366             nan     0.1000    0.0621
##      5        1.7845             nan     0.1000    0.0527
##      6        1.7414             nan     0.1000    0.0448
##      7        1.7025             nan     0.1000    0.0371
##      8        1.6637             nan     0.1000    0.0392
##      9        1.6269             nan     0.1000    0.0365
##     10        1.5969             nan     0.1000    0.0287
##     20        1.3778             nan     0.1000    0.0154
##     40        1.1692             nan     0.1000    0.0090
##     60        1.0425             nan     0.1000    0.0043
##     80        0.9472             nan     0.1000    0.0061
##    100        0.8777             nan     0.1000    0.0040
##    120        0.8311             nan     0.1000    0.0016
##    140        0.7905             nan     0.1000    0.0005
##    150        0.7755             nan     0.1000    0.0016
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0355             nan     0.1000    0.1376
##      2        1.9235             nan     0.1000    0.1122
##      3        1.8308             nan     0.1000    0.0920
##      4        1.7550             nan     0.1000    0.0753
##      5        1.6855             nan     0.1000    0.0680
##      6        1.6276             nan     0.1000    0.0586
##      7        1.5785             nan     0.1000    0.0492
##      8        1.5371             nan     0.1000    0.0399
##      9        1.5037             nan     0.1000    0.0335
##     10        1.4655             nan     0.1000    0.0376
##     20        1.2545             nan     0.1000    0.0133
##     40        1.0153             nan     0.1000    0.0071
##     60        0.8855             nan     0.1000    0.0039
##     80        0.7960             nan     0.1000    0.0028
##    100        0.7325             nan     0.1000    0.0024
##    120        0.6823             nan     0.1000    0.0015
##    140        0.6398             nan     0.1000    0.0036
##    150        0.6220             nan     0.1000    0.0015
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.1033             nan     0.1000    0.0624
##      2        2.0454             nan     0.1000    0.0583
##      3        1.9977             nan     0.1000    0.0471
##      4        1.9498             nan     0.1000    0.0467
##      5        1.9108             nan     0.1000    0.0367
##      6        1.8735             nan     0.1000    0.0376
##      7        1.8431             nan     0.1000    0.0312
##      8        1.8135             nan     0.1000    0.0284
##      9        1.7886             nan     0.1000    0.0250
##     10        1.7654             nan     0.1000    0.0229
##     20        1.5884             nan     0.1000    0.0136
##     40        1.3824             nan     0.1000    0.0076
##     60        1.2723             nan     0.1000    0.0034
##     80        1.1991             nan     0.1000    0.0027
##    100        1.1439             nan     0.1000    0.0019
##    120        1.1005             nan     0.1000    0.0021
##    140        1.0652             nan     0.1000    0.0010
##    150        1.0489             nan     0.1000    0.0011
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0558             nan     0.1000    0.1105
##      2        1.9652             nan     0.1000    0.0923
##      3        1.8924             nan     0.1000    0.0758
##      4        1.8322             nan     0.1000    0.0579
##      5        1.7833             nan     0.1000    0.0487
##      6        1.7324             nan     0.1000    0.0518
##      7        1.6940             nan     0.1000    0.0374
##      8        1.6539             nan     0.1000    0.0409
##      9        1.6178             nan     0.1000    0.0361
##     10        1.5870             nan     0.1000    0.0299
##     20        1.3693             nan     0.1000    0.0158
##     40        1.1596             nan     0.1000    0.0050
##     60        1.0359             nan     0.1000    0.0057
##     80        0.9455             nan     0.1000    0.0064
##    100        0.8794             nan     0.1000    0.0015
##    120        0.8300             nan     0.1000    0.0022
##    140        0.7805             nan     0.1000    0.0012
##    150        0.7618             nan     0.1000    0.0010
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0300             nan     0.1000    0.1359
##      2        1.9190             nan     0.1000    0.1114
##      3        1.8235             nan     0.1000    0.0942
##      4        1.7453             nan     0.1000    0.0782
##      5        1.6817             nan     0.1000    0.0619
##      6        1.6185             nan     0.1000    0.0641
##      7        1.5685             nan     0.1000    0.0496
##      8        1.5288             nan     0.1000    0.0394
##      9        1.4861             nan     0.1000    0.0418
##     10        1.4524             nan     0.1000    0.0326
##     20        1.2319             nan     0.1000    0.0140
##     40        1.0028             nan     0.1000    0.0073
##     60        0.8723             nan     0.1000    0.0047
##     80        0.7910             nan     0.1000    0.0029
##    100        0.7287             nan     0.1000    0.0018
##    120        0.6762             nan     0.1000    0.0007
##    140        0.6383             nan     0.1000    0.0006
##    150        0.6172             nan     0.1000    0.0027
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.1030             nan     0.1000    0.0561
##      2        2.0461             nan     0.1000    0.0568
##      3        1.9995             nan     0.1000    0.0442
##      4        1.9544             nan     0.1000    0.0448
##      5        1.9174             nan     0.1000    0.0363
##      6        1.8809             nan     0.1000    0.0369
##      7        1.8520             nan     0.1000    0.0285
##      8        1.8228             nan     0.1000    0.0290
##      9        1.7992             nan     0.1000    0.0222
##     10        1.7758             nan     0.1000    0.0223
##     20        1.5972             nan     0.1000    0.0135
##     40        1.3955             nan     0.1000    0.0074
##     60        1.2860             nan     0.1000    0.0039
##     80        1.2100             nan     0.1000    0.0026
##    100        1.1543             nan     0.1000    0.0017
##    120        1.1089             nan     0.1000    0.0019
##    140        1.0726             nan     0.1000    0.0016
##    150        1.0570             nan     0.1000    0.0013
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0529             nan     0.1000    0.1091
##      2        1.9669             nan     0.1000    0.0866
##      3        1.8962             nan     0.1000    0.0704
##      4        1.8394             nan     0.1000    0.0555
##      5        1.7928             nan     0.1000    0.0471
##      6        1.7420             nan     0.1000    0.0488
##      7        1.7037             nan     0.1000    0.0372
##      8        1.6702             nan     0.1000    0.0326
##      9        1.6343             nan     0.1000    0.0349
##     10        1.5994             nan     0.1000    0.0346
##     20        1.3777             nan     0.1000    0.0168
##     40        1.1699             nan     0.1000    0.0072
##     60        1.0518             nan     0.1000    0.0050
##     80        0.9543             nan     0.1000    0.0024
##    100        0.8885             nan     0.1000    0.0037
##    120        0.8330             nan     0.1000    0.0033
##    140        0.7913             nan     0.1000    0.0009
##    150        0.7705             nan     0.1000    0.0036
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0283             nan     0.1000    0.1282
##      2        1.9214             nan     0.1000    0.1040
##      3        1.8293             nan     0.1000    0.0929
##      4        1.7553             nan     0.1000    0.0747
##      5        1.6938             nan     0.1000    0.0595
##      6        1.6302             nan     0.1000    0.0613
##      7        1.5858             nan     0.1000    0.0439
##      8        1.5387             nan     0.1000    0.0459
##      9        1.4987             nan     0.1000    0.0390
##     10        1.4663             nan     0.1000    0.0320
##     20        1.2542             nan     0.1000    0.0136
##     40        1.0220             nan     0.1000    0.0064
##     60        0.8767             nan     0.1000    0.0042
##     80        0.7825             nan     0.1000    0.0032
##    100        0.7230             nan     0.1000    0.0024
##    120        0.6718             nan     0.1000    0.0019
##    140        0.6352             nan     0.1000    0.0023
##    150        0.6137             nan     0.1000    0.0025
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.1132             nan     0.1000    0.0640
##      2        2.0589             nan     0.1000    0.0559
##      3        2.0090             nan     0.1000    0.0505
##      4        1.9653             nan     0.1000    0.0433
##      5        1.9255             nan     0.1000    0.0400
##      6        1.8913             nan     0.1000    0.0339
##      7        1.8597             nan     0.1000    0.0310
##      8        1.8319             nan     0.1000    0.0273
##      9        1.8067             nan     0.1000    0.0257
##     10        1.7850             nan     0.1000    0.0203
##     20        1.6063             nan     0.1000    0.0138
##     40        1.3956             nan     0.1000    0.0078
##     60        1.2835             nan     0.1000    0.0044
##     80        1.2063             nan     0.1000    0.0028
##    100        1.1482             nan     0.1000    0.0022
##    120        1.1023             nan     0.1000    0.0017
##    140        1.0655             nan     0.1000    0.0009
##    150        1.0491             nan     0.1000    0.0014
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0676             nan     0.1000    0.1088
##      2        1.9800             nan     0.1000    0.0878
##      3        1.9088             nan     0.1000    0.0752
##      4        1.8509             nan     0.1000    0.0596
##      5        1.8027             nan     0.1000    0.0477
##      6        1.7545             nan     0.1000    0.0471
##      7        1.7135             nan     0.1000    0.0412
##      8        1.6727             nan     0.1000    0.0405
##      9        1.6393             nan     0.1000    0.0317
##     10        1.6036             nan     0.1000    0.0341
##     20        1.3784             nan     0.1000    0.0158
##     40        1.1604             nan     0.1000    0.0089
##     60        1.0364             nan     0.1000    0.0041
##     80        0.9467             nan     0.1000    0.0036
##    100        0.8754             nan     0.1000    0.0025
##    120        0.8263             nan     0.1000    0.0021
##    140        0.7804             nan     0.1000    0.0010
##    150        0.7618             nan     0.1000    0.0013
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0408             nan     0.1000    0.1371
##      2        1.9313             nan     0.1000    0.1119
##      3        1.8397             nan     0.1000    0.0953
##      4        1.7632             nan     0.1000    0.0759
##      5        1.6974             nan     0.1000    0.0665
##      6        1.6449             nan     0.1000    0.0524
##      7        1.5889             nan     0.1000    0.0556
##      8        1.5448             nan     0.1000    0.0447
##      9        1.5080             nan     0.1000    0.0362
##     10        1.4764             nan     0.1000    0.0305
##     20        1.2421             nan     0.1000    0.0136
##     40        1.0007             nan     0.1000    0.0052
##     60        0.8680             nan     0.1000    0.0036
##     80        0.7794             nan     0.1000    0.0021
##    100        0.7180             nan     0.1000    0.0022
##    120        0.6699             nan     0.1000    0.0013
##    140        0.6292             nan     0.1000    0.0028
##    150        0.6097             nan     0.1000    0.0013
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.1010             nan     0.1000    0.0631
##      2        2.0439             nan     0.1000    0.0568
##      3        1.9928             nan     0.1000    0.0511
##      4        1.9485             nan     0.1000    0.0430
##      5        1.9087             nan     0.1000    0.0402
##      6        1.8722             nan     0.1000    0.0355
##      7        1.8397             nan     0.1000    0.0342
##      8        1.8114             nan     0.1000    0.0286
##      9        1.7857             nan     0.1000    0.0244
##     10        1.7623             nan     0.1000    0.0230
##     20        1.5782             nan     0.1000    0.0138
##     40        1.3690             nan     0.1000    0.0076
##     60        1.2550             nan     0.1000    0.0036
##     80        1.1791             nan     0.1000    0.0027
##    100        1.1221             nan     0.1000    0.0020
##    120        1.0767             nan     0.1000    0.0015
##    140        1.0409             nan     0.1000    0.0015
##    150        1.0259             nan     0.1000    0.0009
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0545             nan     0.1000    0.1128
##      2        1.9636             nan     0.1000    0.0893
##      3        1.8904             nan     0.1000    0.0727
##      4        1.8310             nan     0.1000    0.0600
##      5        1.7824             nan     0.1000    0.0473
##      6        1.7288             nan     0.1000    0.0534
##      7        1.6887             nan     0.1000    0.0397
##      8        1.6467             nan     0.1000    0.0415
##      9        1.6095             nan     0.1000    0.0369
##     10        1.5791             nan     0.1000    0.0311
##     20        1.3507             nan     0.1000    0.0177
##     40        1.1363             nan     0.1000    0.0082
##     60        1.0075             nan     0.1000    0.0068
##     80        0.9186             nan     0.1000    0.0030
##    100        0.8559             nan     0.1000    0.0013
##    120        0.8038             nan     0.1000    0.0014
##    140        0.7655             nan     0.1000    0.0002
##    150        0.7485             nan     0.1000    0.0006
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0254             nan     0.1000    0.1343
##      2        1.9127             nan     0.1000    0.1134
##      3        1.8198             nan     0.1000    0.0898
##      4        1.7367             nan     0.1000    0.0841
##      5        1.6715             nan     0.1000    0.0623
##      6        1.6065             nan     0.1000    0.0645
##      7        1.5571             nan     0.1000    0.0497
##      8        1.5175             nan     0.1000    0.0382
##      9        1.4788             nan     0.1000    0.0368
##     10        1.4387             nan     0.1000    0.0405
##     20        1.2212             nan     0.1000    0.0136
##     40        0.9841             nan     0.1000    0.0072
##     60        0.8575             nan     0.1000    0.0033
##     80        0.7795             nan     0.1000    0.0020
##    100        0.7087             nan     0.1000    0.0031
##    120        0.6543             nan     0.1000    0.0012
##    140        0.6132             nan     0.1000    0.0010
##    150        0.5942             nan     0.1000    0.0020
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.1012             nan     0.1000    0.0612
##      2        2.0438             nan     0.1000    0.0589
##      3        1.9946             nan     0.1000    0.0482
##      4        1.9489             nan     0.1000    0.0455
##      5        1.9101             nan     0.1000    0.0398
##      6        1.8729             nan     0.1000    0.0359
##      7        1.8422             nan     0.1000    0.0322
##      8        1.8132             nan     0.1000    0.0299
##      9        1.7887             nan     0.1000    0.0263
##     10        1.7655             nan     0.1000    0.0225
##     20        1.5867             nan     0.1000    0.0153
##     40        1.3838             nan     0.1000    0.0082
##     60        1.2739             nan     0.1000    0.0038
##     80        1.1975             nan     0.1000    0.0030
##    100        1.1410             nan     0.1000    0.0017
##    120        1.0965             nan     0.1000    0.0016
##    140        1.0606             nan     0.1000    0.0016
##    150        1.0452             nan     0.1000    0.0008
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0535             nan     0.1000    0.1114
##      2        1.9629             nan     0.1000    0.0913
##      3        1.8905             nan     0.1000    0.0736
##      4        1.8305             nan     0.1000    0.0594
##      5        1.7826             nan     0.1000    0.0478
##      6        1.7344             nan     0.1000    0.0454
##      7        1.6975             nan     0.1000    0.0345
##      8        1.6544             nan     0.1000    0.0418
##      9        1.6240             nan     0.1000    0.0291
##     10        1.5895             nan     0.1000    0.0334
##     20        1.3715             nan     0.1000    0.0166
##     40        1.1620             nan     0.1000    0.0073
##     60        1.0463             nan     0.1000    0.0034
##     80        0.9587             nan     0.1000    0.0019
##    100        0.8843             nan     0.1000    0.0043
##    120        0.8309             nan     0.1000    0.0021
##    140        0.7870             nan     0.1000    0.0015
##    150        0.7665             nan     0.1000    0.0018
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0292             nan     0.1000    0.1362
##      2        1.9169             nan     0.1000    0.1117
##      3        1.8240             nan     0.1000    0.0946
##      4        1.7464             nan     0.1000    0.0759
##      5        1.6796             nan     0.1000    0.0655
##      6        1.6252             nan     0.1000    0.0535
##      7        1.5696             nan     0.1000    0.0549
##      8        1.5266             nan     0.1000    0.0429
##      9        1.4913             nan     0.1000    0.0346
##     10        1.4521             nan     0.1000    0.0382
##     20        1.2366             nan     0.1000    0.0135
##     40        1.0028             nan     0.1000    0.0080
##     60        0.8685             nan     0.1000    0.0068
##     80        0.7859             nan     0.1000    0.0030
##    100        0.7211             nan     0.1000    0.0025
##    120        0.6684             nan     0.1000    0.0017
##    140        0.6312             nan     0.1000    0.0014
##    150        0.6153             nan     0.1000    0.0015
## 
## Iter   TrainDeviance   ValidDeviance   StepSize   Improve
##      1        2.0402             nan     0.1000    0.1332
##      2        1.9293             nan     0.1000    0.1088
##      3        1.8339             nan     0.1000    0.0954
##      4        1.7571             nan     0.1000    0.0754
##      5        1.6950             nan     0.1000    0.0621
##      6        1.6368             nan     0.1000    0.0599
##      7        1.5804             nan     0.1000    0.0537
##      8        1.5349             nan     0.1000    0.0443
##      9        1.4955             nan     0.1000    0.0394
##     10        1.4628             nan     0.1000    0.0319
##     20        1.2445             nan     0.1000    0.0140
##     40        1.0070             nan     0.1000    0.0065
##     60        0.8781             nan     0.1000    0.0080
##     80        0.7838             nan     0.1000    0.0023
##    100        0.7226             nan     0.1000    0.0045
##    120        0.6702             nan     0.1000    0.0026
##    140        0.6304             nan     0.1000    0.0016
##    150        0.6132             nan     0.1000    0.0006
```

```r
modFitOp3
```

```
## Stochastic Gradient Boosting 
## 
## 14718 samples
##    12 predictor
## 
## No pre-processing
## Resampling: Bootstrapped (25 reps) 
## 
## Summary of sample sizes: 14718, 14718, 14718, 14718, 14718, 14718, ... 
## 
## Resampling results across tuning parameters:
## 
##   interaction.depth  n.trees  RMSE  Rsquared  RMSE SD  Rsquared SD
##   1                   50      1.2   0.4       0.008    0.008      
##   1                  100      1.1   0.5       0.008    0.007      
##   1                  150      1.0   0.5       0.009    0.008      
##   2                   50      1.1   0.5       0.009    0.009      
##   2                  100      1.0   0.6       0.011    0.010      
##   2                  150      0.9   0.6       0.009    0.007      
##   3                   50      1.0   0.6       0.010    0.009      
##   3                  100      0.9   0.7       0.009    0.007      
##   3                  150      0.8   0.7       0.009    0.007      
## 
## Tuning parameter 'shrinkage' was held constant at a value of 0.1
## RMSE was used to select the optimal model using  the smallest value.
## The final values used for the model were n.trees = 150,
##  interaction.depth = 3 and shrinkage = 0.1.
```

```r
plot(testing$classe, pred1)
```

```
## Error: error in evaluating the argument 'y' in selecting a method for function 'plot': Error: objeto 'pred1' no encontrado
```

```r
plot(testing$classe, pred2)
```

```
## Error: error in evaluating the argument 'y' in selecting a method for function 'plot': Error: objeto 'pred2' no encontrado
```

```r
plot(testing$classe, pred3)
```

```
## Error: error in evaluating the argument 'y' in selecting a method for function 'plot': Error: objeto 'pred3' no encontrado
```

```r
predicted1 <- predict(modFitOp1, training)
divide1 = cut(predicted1,5)
levels(divide1)[1] <- "A"
levels(divide1)[2] <- "B"
levels(divide1)[3] <- "C"
levels(divide1)[4] <- "D"
levels(divide1)[5] <- "E"

confusionMat1 <- confusionMatrix(divide1, training$classe)
confusionMat1
```

```
## Confusion Matrix and Statistics
## 
##           Reference
## Prediction    A    B    C    D    E
##          A 1403  188    0   47    0
##          B 2567 2050 1779 1198 1167
##          C  214  610  788 1080 1346
##          D    1    0    0   85  188
##          E    0    0    0    2    5
## 
## Overall Statistics
##                                         
##                Accuracy : 0.294         
##                  95% CI : (0.287, 0.302)
##     No Information Rate : 0.284         
##     P-Value [Acc > NIR] : 0.00401       
##                                         
##                   Kappa : 0.12          
##  Mcnemar's Test P-Value : NA            
## 
## Statistics by Class:
## 
##                      Class: A Class: B Class: C Class: D Class: E
## Sensitivity            0.3352    0.720   0.3070  0.03524 0.001848
## Specificity            0.9777    0.435   0.7325  0.98464 0.999833
## Pos Pred Value         0.8565    0.234   0.1951  0.31022 0.714286
## Neg Pred Value         0.7873    0.866   0.8334  0.83890 0.816396
## Prevalence             0.2843    0.194   0.1744  0.16388 0.183857
## Detection Rate         0.0953    0.139   0.0535  0.00578 0.000340
## Detection Prevalence   0.1113    0.595   0.2744  0.01862 0.000476
## Balanced Accuracy      0.6565    0.577   0.5198  0.50994 0.500841
```

```r
predicted2 <- predict(modFitOp2, training)
divide2 = cut(predicted2,5)
levels(divide2)[1] <- "A"
levels(divide2)[2] <- "B"
levels(divide2)[3] <- "C"
levels(divide2)[4] <- "D"
levels(divide2)[5] <- "E"

confusionMat2 <- confusionMatrix(divide2, training$classe)
confusionMat2
```

```
## Confusion Matrix and Statistics
## 
##           Reference
## Prediction    A    B    C    D    E
##          A    0    0    0    0    0
##          B    0    0    0    0    0
##          C 4185 2848 2567 2412 2706
##          D    0    0    0    0    0
##          E    0    0    0    0    0
## 
## Overall Statistics
##                                         
##                Accuracy : 0.174         
##                  95% CI : (0.168, 0.181)
##     No Information Rate : 0.284         
##     P-Value [Acc > NIR] : 1             
##                                         
##                   Kappa : 0             
##  Mcnemar's Test P-Value : NA            
## 
## Statistics by Class:
## 
##                      Class: A Class: B Class: C Class: D Class: E
## Sensitivity             0.000    0.000    1.000    0.000    0.000
## Specificity             1.000    1.000    0.000    1.000    1.000
## Pos Pred Value            NaN      NaN    0.174      NaN      NaN
## Neg Pred Value          0.716    0.806      NaN    0.836    0.816
## Prevalence              0.284    0.194    0.174    0.164    0.184
## Detection Rate          0.000    0.000    0.174    0.000    0.000
## Detection Prevalence    0.000    0.000    1.000    0.000    0.000
## Balanced Accuracy       0.500    0.500    0.500    0.500    0.500
```

```r
predicted3 <- predict(modFitOp3, training)
divide3 = cut(predicted3,5)
levels(divide3)[1] <- "A"
levels(divide3)[2] <- "B"
levels(divide3)[3] <- "C"
levels(divide3)[4] <- "D"
levels(divide3)[5] <- "E"

confusionMat3 <- confusionMatrix(divide3, training$classe)
confusionMat3
```

```
## Confusion Matrix and Statistics
## 
##           Reference
## Prediction    A    B    C    D    E
##          A 2309  173    0    0    0
##          B 1699 1531  365   28   26
##          C  176 1123 2164 1890  992
##          D    1   21   38  481 1184
##          E    0    0    0   13  504
## 
## Overall Statistics
##                                         
##                Accuracy : 0.475         
##                  95% CI : (0.467, 0.483)
##     No Information Rate : 0.284         
##     P-Value [Acc > NIR] : <2e-16        
##                                         
##                   Kappa : 0.346         
##  Mcnemar's Test P-Value : NA            
## 
## Statistics by Class:
## 
##                      Class: A Class: B Class: C Class: D Class: E
## Sensitivity             0.552    0.538    0.843   0.1994   0.1863
## Specificity             0.984    0.822    0.656   0.8989   0.9989
## Pos Pred Value          0.930    0.420    0.341   0.2788   0.9749
## Neg Pred Value          0.847    0.881    0.952   0.8514   0.8449
## Prevalence              0.284    0.194    0.174   0.1639   0.1839
## Detection Rate          0.157    0.104    0.147   0.0327   0.0342
## Detection Prevalence    0.169    0.248    0.431   0.1172   0.0351
## Balanced Accuracy       0.768    0.680    0.749   0.5492   0.5926
```
