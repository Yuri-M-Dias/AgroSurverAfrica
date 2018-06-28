---
title: "Agricultural Survey Dataset"
output: 
  html_document:
    keep_md: true
---

# Agricultural Survery Dataset

I am following @rtatman's tutorial on this dataset, just doing it with RMarkdown instead.

# Section 1


```r
library(tidyverse)
```

```
## ── Attaching packages ────────────────────────────────── tidyverse 1.2.1 ──
```

```
## ✔ ggplot2 2.2.1     ✔ purrr   0.2.5
## ✔ tibble  1.4.2     ✔ dplyr   0.7.5
## ✔ tidyr   0.8.1     ✔ stringr 1.3.1
## ✔ readr   1.1.1     ✔ forcats 0.3.0
```

```
## ── Conflicts ───────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
```

```r
library(data.table)
```

```
## 
## Attaching package: 'data.table'
```

```
## The following objects are masked from 'package:dplyr':
## 
##     between, first, last
```

```
## The following object is masked from 'package:purrr':
## 
##     transpose
```

I'll be using `data.table` instead of the normal Tibble, for learning purposes.


```r
musselData <- fread("data/histopaths.csv")
```

And get the dimensions

```r
dim(musselData)
```

```
## [1] 1800   79
```

# Section 2 - Piping


```r
columnNames <- musselData %>%
  names() %>%
  sort()
head(columnNames)
```

```
## [1] "abnormality"             "abnormality_description"
## [3] "bucephalus"              "ceroid"                 
## [5] "cestode_body"            "cestode_gill"
```


# Section 3


```r
musselData %>%
  select(starts_with('cestode')) %>%
  summary()
```

```
##   cestode_body     cestode_gill    cestode_mantle  
##  Min.   : 0.000   Min.   :0.0000   Min.   :0.0000  
##  1st Qu.: 0.000   1st Qu.:0.0000   1st Qu.:0.0000  
##  Median : 0.000   Median :0.0000   Median :0.0000  
##  Mean   : 0.223   Mean   :0.0588   Mean   :0.0098  
##  3rd Qu.: 0.000   3rd Qu.:0.0000   3rd Qu.:0.0000  
##  Max.   :15.000   Max.   :3.0000   Max.   :1.0000  
##  NA's   :1392     NA's   :1392     NA's   :1392
```


```r
musselData %>%
  select(-starts_with('cestode')) %>%
  names() %>%
  sort() %>%
  head(20) 
```

```
##  [1] "abnormality"                  "abnormality_description"     
##  [3] "bucephalus"                   "ceroid"                      
##  [5] "chlamydia"                    "ciliate_digestive_tract"     
##  [7] "ciliate_gut"                  "ciliate_large_gill"          
##  [9] "ciliate_small_gill"           "coastal_ecological_area"     
## [11] "condition_code"               "condition_code_description"  
## [13] "copepod_body"                 "copepod_gill"                
## [15] "copepod_gut_digestive_tubule" "dermo"                       
## [17] "dermo_description"            "dermo_infection_intensity"   
## [19] "dermo_numerical_value"        "diffuse_inflammation"
```

# Section 4


```r
musselData %>%
  filter(abnormality > 3)
```

```
##   abnormality abnormality_description bucephalus ceroid cestode_body
## 1           4  All follicles affected          0      0           NA
## 2           4  All follicles affected          0      0           NA
##   cestode_gill cestode_mantle chlamydia ciliate_digestive_tract
## 1           NA             NA         0                       0
## 2           NA             NA         0                       0
##   ciliate_gut ciliate_large_gill ciliate_small_gill
## 1           0                  0                  0
## 2           0                  0                  0
##   coastal_ecological_area condition_code condition_code_description
## 1        Alaska Southeast             NA                           
## 2        Alaska Southeast             NA                           
##   copepod_body copepod_gill copepod_gut_digestive_tubule dermo
## 1            0            0                            0      
## 2            0            0                            0      
##   dermo_description dermo_infection_intensity dermo_numerical_value
## 1                                                                NA
## 2                                                                NA
##   diffuse_inflammation diffuse_necrosis digestive_tubule_atrophy
## 1                    0                0                        1
## 2                    0                0                        2
##                                                                                                               digestive_tubule_atrophy_description
## 1 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 2                                                                                       Wall thickness averaging about one-half as thick as normal
##   edema empty_displacement_volume fiscal_year focal_inflammation
## 1    NA                        NA        2009                  0
## 2    NA                        NA        2009                  0
##   focal_necrosis full_displacement_volume general_location
## 1              0                       NA     Kachemak Bay
## 2              0                       NA     Kachemak Bay
##   gonad_subsample_wet_weight gonadal_index
## 1                         NA            S3
## 2                         NA            S2
##                                  gonadal_index_description hydra_gill
## 1                                   Gonad about half empty         NA
## 2 Gonadal Area reduced, follicles 1/3 full of ripe gametes         NA
##   latitude length longitude metacercaria multinucleated_sphere_x
## 1 59.42355    5.1  -151.311            0                      NA
## 2 59.42355    5.7  -151.311            0                      NA
##   multinucleated_sphere_x_description nematode nematopsis_body
## 1                                           NA               0
## 2                                           NA               0
##   nematopsis_gill nematopsis_mantle nemertine_gill neoplasm nst_sample_id
## 1               0                 0             NA        0  BA2009KBTBME
## 2               0                 0             NA        1  BA2009KBTBME
##   nst_site other_trematode_sporocyst_gill other_trematode_sporocyst_gut
## 1     KBTB                             NA                            NA
## 2     KBTB                             NA                            NA
##   pea_crab proctoeces protozoan_digestive_tubule protozoan_gut
## 1        0         NA                         NA            NA
## 2        0         NA                         NA            NA
##   pseudoklossia region rickettsia_digestive_tubule rickettsia_gut
## 1             0 Alaska                           0              0
## 2             0 Alaska                           0              0
##   sample_letter sample_number    sex                      source_file
## 1                           3 Female KachemakBay_Mussel_Histopath.csv
## 2                           4 Female KachemakBay_Mussel_Histopath.csv
##   species_name specific_location specific_region state_name station_letter
## 1                      Tutka Bay          Alaska     Alaska               
## 2                      Tutka Bay          Alaska     Alaska               
##                study_name trematode_metacercariae
## 1 Kachemak Bay Bioeffects                       0
## 2 Kachemak Bay Bioeffects                       0
##   trematode_metacercariae_description tumor unidentified_gonoduct_organism
## 1                          Uninfected    NA                              0
## 2                          Uninfected    NA                              0
##   unidentified_organism unusual_digestive_tubule wet_weight xenoma
## 1                     0                        0        3.3      0
## 2                     0                        0        3.9      0
```


```r
musselData %>%
  filter(coastal_ecological_area == 'Lake Michigan')
```

```
##     abnormality abnormality_description bucephalus ceroid cestode_body
## 1            NA                                 NA      0           NA
## 2            NA                                 NA      0           NA
## 3            NA                                 NA      0           NA
## 4            NA                                 NA      5           NA
## 5            NA                                 NA     15           NA
## 6            NA                                 NA      0           NA
## 7            NA                                 NA      3           NA
## 8            NA                                 NA      2           NA
## 9            NA                                 NA      0           NA
## 10           NA                                 NA      1           NA
## 11           NA                                 NA      0           NA
## 12           NA                                 NA      0           NA
## 13           NA                                 NA      0           NA
## 14           NA                                 NA      0           NA
## 15           NA                                 NA      0           NA
## 16           NA                                 NA      1           NA
## 17           NA                                 NA      2           NA
## 18           NA                                 NA      0           NA
## 19           NA                                 NA      0           NA
## 20           NA                                 NA      0           NA
## 21           NA                                 NA      0           NA
## 22           NA                                 NA      0           NA
## 23           NA                                 NA      0           NA
## 24           NA                                 NA      0           NA
## 25           NA                                 NA      0           NA
## 26           NA                                 NA      0           NA
## 27           NA                                 NA      0           NA
## 28           NA                                 NA      0           NA
## 29           NA                                 NA      0           NA
## 30           NA                                 NA      1           NA
## 31           NA                                 NA      0           NA
## 32           NA                                 NA     25           NA
## 33           NA                                 NA      0           NA
## 34           NA                                 NA      0           NA
## 35           NA                                 NA     26           NA
## 36           NA                                 NA      1           NA
## 37           NA                                 NA      2           NA
## 38           NA                                 NA      0           NA
## 39           NA                                 NA      0           NA
## 40           NA                                 NA      3           NA
## 41           NA                                 NA      0           NA
## 42           NA                                 NA      0           NA
## 43           NA                                 NA      0           NA
## 44           NA                                 NA      0           NA
## 45           NA                                 NA      0           NA
## 46           NA                                 NA      0           NA
## 47           NA                                 NA      0           NA
## 48           NA                                 NA      1           NA
## 49           NA                                 NA      0           NA
## 50           NA                                 NA      0           NA
## 51           NA                                 NA      0           NA
## 52           NA                                 NA     19           NA
## 53           NA                                 NA      0           NA
## 54           NA                                 NA      3           NA
## 55           NA                                 NA      8           NA
## 56           NA                                 NA      0           NA
## 57           NA                                 NA      0           NA
## 58           NA                                 NA      0           NA
## 59           NA                                 NA      0           NA
## 60           NA                                 NA      0           NA
## 61           NA                                 NA      0           NA
## 62           NA                                 NA      0           NA
## 63           NA                                 NA      0           NA
## 64           NA                                 NA      0           NA
## 65           NA                                 NA      0           NA
## 66           NA                                 NA      0           NA
## 67           NA                                 NA      0           NA
## 68           NA                                 NA      0           NA
## 69           NA                                 NA      0           NA
## 70           NA                                 NA      0           NA
## 71           NA                                 NA      0           NA
## 72           NA                                 NA      0           NA
## 73           NA                                 NA      0           NA
## 74           NA                                 NA      0           NA
## 75           NA                                 NA      0           NA
## 76           NA                                 NA      0           NA
## 77           NA                                 NA      0           NA
## 78           NA                                 NA      0           NA
## 79           NA                                 NA      0           NA
## 80           NA                                 NA      0           NA
## 81           NA                                 NA      0           NA
## 82           NA                                 NA      0           NA
## 83           NA                                 NA      0           NA
## 84           NA                                 NA      0           NA
## 85           NA                                 NA      0           NA
## 86           NA                                 NA      0           NA
## 87           NA                                 NA      0           NA
## 88           NA                                 NA      0           NA
## 89           NA                                 NA      0           NA
## 90           NA                                 NA      0           NA
## 91           NA                                 NA      0           NA
## 92           NA                                 NA      1           NA
## 93           NA                                 NA      0           NA
## 94           NA                                 NA      0           NA
## 95           NA                                 NA      0           NA
## 96           NA                                 NA      0           NA
## 97           NA                                 NA      0           NA
## 98           NA                                 NA      0           NA
## 99           NA                                 NA      0           NA
## 100          NA                                 NA      0           NA
## 101          NA                                 NA      9           NA
## 102          NA                                 NA      3           NA
## 103          NA                                 NA      7           NA
## 104          NA                                 NA      1           NA
## 105          NA                                 NA     31           NA
## 106          NA                                 NA      0           NA
## 107          NA                                 NA      1           NA
## 108          NA                                 NA      0           NA
## 109          NA                                 NA      0           NA
## 110          NA                                 NA      0           NA
## 111          NA                                 NA      0           NA
## 112          NA                                 NA      0           NA
## 113          NA                                 NA      0           NA
## 114          NA                                 NA      0           NA
## 115          NA                                 NA      0           NA
## 116          NA                                 NA      0           NA
## 117          NA                                 NA      0           NA
## 118          NA                                 NA      0           NA
## 119          NA                                 NA      0           NA
## 120          NA                                 NA      0           NA
## 121          NA                                 NA      1           NA
## 122          NA                                 NA      0           NA
## 123          NA                                 NA      2           NA
## 124          NA                                 NA      0           NA
## 125          NA                                 NA      0           NA
## 126          NA                                 NA      0           NA
## 127          NA                                 NA      0           NA
## 128          NA                                 NA      0           NA
## 129          NA                                 NA      0           NA
## 130          NA                                 NA      0           NA
## 131          NA                                 NA      0           NA
## 132          NA                                 NA      0           NA
## 133          NA                                 NA      0           NA
## 134          NA                                 NA      2           NA
## 135          NA                                 NA      0           NA
## 136          NA                                 NA     21           NA
## 137          NA                                 NA      7           NA
## 138          NA                                 NA      4           NA
## 139          NA                                 NA      8           NA
## 140          NA                                 NA      5           NA
## 141          NA                                 NA      1           NA
## 142          NA                                 NA      1           NA
## 143          NA                                 NA      0           NA
## 144          NA                                 NA      0           NA
## 145          NA                                 NA      0           NA
## 146          NA                                 NA      0           NA
## 147          NA                                 NA      0           NA
## 148          NA                                 NA      0           NA
## 149          NA                                 NA      0           NA
## 150          NA                                 NA      0           NA
## 151          NA                                 NA      0           NA
## 152          NA                                 NA      0           NA
## 153          NA                                 NA      0           NA
## 154          NA                                 NA      0           NA
## 155          NA                                 NA      0           NA
## 156          NA                                 NA      0           NA
## 157          NA                                 NA      0           NA
## 158          NA                                 NA      0           NA
## 159          NA                                 NA      0           NA
## 160          NA                                 NA      0           NA
## 161          NA                                 NA      0           NA
## 162          NA                                 NA      0           NA
## 163          NA                                 NA      0           NA
## 164          NA                                 NA      0           NA
## 165          NA                                 NA      0           NA
## 166          NA                                 NA     23           NA
## 167          NA                                 NA     16           NA
## 168          NA                                 NA      2           NA
## 169          NA                                 NA      4           NA
## 170          NA                                 NA      2           NA
## 171          NA                                 NA      3           NA
## 172          NA                                 NA      2           NA
## 173          NA                                 NA      0           NA
## 174          NA                                 NA      0           NA
## 175          NA                                 NA      0           NA
## 176          NA                                 NA      0           NA
## 177          NA                                 NA      0           NA
## 178          NA                                 NA      0           NA
## 179          NA                                 NA      0           NA
## 180          NA                                 NA      0           NA
## 181          NA                                 NA      0           NA
## 182          NA                                 NA      0           NA
## 183          NA                                 NA      0           NA
## 184          NA                                 NA      0           NA
## 185          NA                                 NA      0           NA
## 186          NA                                 NA      0           NA
## 187          NA                                 NA      0           NA
## 188          NA                                 NA      0           NA
## 189          NA                                 NA      0           NA
## 190          NA                                 NA      0           NA
## 191          NA                                 NA      0           NA
## 192          NA                                 NA      0           NA
## 193          NA                                 NA      0           NA
## 194          NA                                 NA      0           NA
## 195          NA                                 NA      0           NA
## 196          NA                                 NA      4           NA
## 197          NA                                 NA      3           NA
## 198          NA                                 NA     12           NA
## 199          NA                                 NA      4           NA
## 200          NA                                 NA      0           NA
## 201          NA                                 NA      0           NA
## 202          NA                                 NA      3           NA
## 203          NA                                 NA      1           NA
## 204          NA                                 NA      0           NA
## 205          NA                                 NA      0           NA
## 206          NA                                 NA      0           NA
## 207          NA                                 NA      0           NA
## 208          NA                                 NA      0           NA
## 209          NA                                 NA      0           NA
## 210          NA                                 NA      0           NA
## 211          NA                                 NA      4           NA
## 212          NA                                 NA      0           NA
## 213          NA                                 NA      0           NA
## 214          NA                                 NA      0           NA
## 215          NA                                 NA      0           NA
## 216          NA                                 NA      0           NA
## 217          NA                                 NA      0           NA
## 218          NA                                 NA      0           NA
## 219          NA                                 NA      0           NA
## 220          NA                                 NA      0           NA
## 221          NA                                 NA      0           NA
## 222          NA                                 NA      0           NA
## 223          NA                                 NA      0           NA
## 224          NA                                 NA      1           NA
## 225          NA                                 NA      1           NA
## 226          NA                                 NA      0           NA
## 227          NA                                 NA      0           NA
## 228          NA                                 NA      0           NA
## 229          NA                                 NA      0           NA
## 230          NA                                 NA      0           NA
## 231          NA                                 NA      0           NA
## 232          NA                                 NA      0           NA
## 233          NA                                 NA      0           NA
## 234          NA                                 NA      0           NA
## 235          NA                                 NA      0           NA
## 236          NA                                 NA      0           NA
## 237          NA                                 NA      0           NA
## 238          NA                                 NA      0           NA
## 239          NA                                 NA      3           NA
## 240          NA                                 NA      0           NA
## 241          NA                                 NA      0           NA
## 242          NA                                 NA      0           NA
## 243          NA                                 NA      0           NA
## 244          NA                                 NA      0           NA
## 245          NA                                 NA      0           NA
## 246          NA                                 NA      0           NA
## 247          NA                                 NA      0           NA
## 248          NA                                 NA      0           NA
## 249          NA                                 NA      0           NA
## 250          NA                                 NA      0           NA
## 251          NA                                 NA      0           NA
## 252          NA                                 NA      0           NA
## 253          NA                                 NA      0           NA
## 254          NA                                 NA      0           NA
## 255          NA                                 NA      0           NA
## 256          NA                                 NA      0           NA
## 257          NA                                 NA      0           NA
## 258          NA                                 NA      0           NA
## 259          NA                                 NA      1           NA
## 260          NA                                 NA      0           NA
##     cestode_gill cestode_mantle chlamydia ciliate_digestive_tract
## 1             NA             NA        NA                      NA
## 2             NA             NA        NA                      NA
## 3             NA             NA        NA                      NA
## 4             NA             NA        NA                      NA
## 5             NA             NA        NA                      NA
## 6             NA             NA        NA                      NA
## 7             NA             NA        NA                      NA
## 8             NA             NA        NA                      NA
## 9             NA             NA        NA                      NA
## 10            NA             NA        NA                      NA
## 11            NA             NA        NA                      NA
## 12            NA             NA        NA                      NA
## 13            NA             NA        NA                      NA
## 14            NA             NA        NA                      NA
## 15            NA             NA        NA                      NA
## 16            NA             NA        NA                      NA
## 17            NA             NA        NA                      NA
## 18            NA             NA        NA                      NA
## 19            NA             NA        NA                      NA
## 20            NA             NA        NA                      NA
## 21            NA             NA        NA                      NA
## 22            NA             NA        NA                      NA
## 23            NA             NA        NA                      NA
## 24            NA             NA        NA                      NA
## 25            NA             NA        NA                      NA
## 26            NA             NA        NA                      NA
## 27            NA             NA        NA                      NA
## 28            NA             NA        NA                      NA
## 29            NA             NA        NA                      NA
## 30            NA             NA        NA                      NA
## 31            NA             NA        NA                      NA
## 32            NA             NA        NA                      NA
## 33            NA             NA        NA                      NA
## 34            NA             NA        NA                      NA
## 35            NA             NA        NA                      NA
## 36            NA             NA        NA                      NA
## 37            NA             NA        NA                      NA
## 38            NA             NA        NA                      NA
## 39            NA             NA        NA                      NA
## 40            NA             NA        NA                      NA
## 41            NA             NA        NA                      NA
## 42            NA             NA        NA                      NA
## 43            NA             NA        NA                      NA
## 44            NA             NA        NA                      NA
## 45            NA             NA        NA                      NA
## 46            NA             NA        NA                      NA
## 47            NA             NA        NA                      NA
## 48            NA             NA        NA                      NA
## 49            NA             NA        NA                      NA
## 50            NA             NA        NA                      NA
## 51            NA             NA        NA                      NA
## 52            NA             NA        NA                      NA
## 53            NA             NA        NA                      NA
## 54            NA             NA        NA                      NA
## 55            NA             NA        NA                      NA
## 56            NA             NA        NA                      NA
## 57            NA             NA        NA                      NA
## 58            NA             NA        NA                      NA
## 59            NA             NA        NA                      NA
## 60            NA             NA        NA                      NA
## 61            NA             NA        NA                      NA
## 62            NA             NA        NA                      NA
## 63            NA             NA        NA                      NA
## 64            NA             NA        NA                      NA
## 65            NA             NA        NA                      NA
## 66            NA             NA        NA                      NA
## 67            NA             NA        NA                      NA
## 68            NA             NA        NA                      NA
## 69            NA             NA        NA                      NA
## 70            NA             NA        NA                      NA
## 71            NA             NA        NA                      NA
## 72            NA             NA        NA                      NA
## 73            NA             NA        NA                      NA
## 74            NA             NA        NA                      NA
## 75            NA             NA        NA                      NA
## 76            NA             NA        NA                      NA
## 77            NA             NA        NA                      NA
## 78            NA             NA        NA                      NA
## 79            NA             NA        NA                      NA
## 80            NA             NA        NA                      NA
## 81            NA             NA        NA                      NA
## 82            NA             NA        NA                      NA
## 83            NA             NA        NA                      NA
## 84            NA             NA        NA                      NA
## 85            NA             NA        NA                      NA
## 86            NA             NA        NA                      NA
## 87            NA             NA        NA                      NA
## 88            NA             NA        NA                      NA
## 89            NA             NA        NA                      NA
## 90            NA             NA        NA                      NA
## 91            NA             NA        NA                      NA
## 92            NA             NA        NA                      NA
## 93            NA             NA        NA                      NA
## 94            NA             NA        NA                      NA
## 95            NA             NA        NA                      NA
## 96            NA             NA        NA                      NA
## 97            NA             NA        NA                      NA
## 98            NA             NA        NA                      NA
## 99            NA             NA        NA                      NA
## 100           NA             NA        NA                      NA
## 101           NA             NA        NA                      NA
## 102           NA             NA        NA                      NA
## 103           NA             NA        NA                      NA
## 104           NA             NA        NA                      NA
## 105           NA             NA        NA                      NA
## 106           NA             NA        NA                      NA
## 107           NA             NA        NA                      NA
## 108           NA             NA        NA                      NA
## 109           NA             NA        NA                      NA
## 110           NA             NA        NA                      NA
## 111           NA             NA        NA                      NA
## 112           NA             NA        NA                      NA
## 113           NA             NA        NA                      NA
## 114           NA             NA        NA                      NA
## 115           NA             NA        NA                      NA
## 116           NA             NA        NA                      NA
## 117           NA             NA        NA                      NA
## 118           NA             NA        NA                      NA
## 119           NA             NA        NA                      NA
## 120           NA             NA        NA                      NA
## 121           NA             NA        NA                      NA
## 122           NA             NA        NA                      NA
## 123           NA             NA        NA                      NA
## 124           NA             NA        NA                      NA
## 125           NA             NA        NA                      NA
## 126           NA             NA        NA                      NA
## 127           NA             NA        NA                      NA
## 128           NA             NA        NA                      NA
## 129           NA             NA        NA                      NA
## 130           NA             NA        NA                      NA
## 131           NA             NA        NA                      NA
## 132           NA             NA        NA                      NA
## 133           NA             NA        NA                      NA
## 134           NA             NA        NA                      NA
## 135           NA             NA        NA                      NA
## 136           NA             NA        NA                      NA
## 137           NA             NA        NA                      NA
## 138           NA             NA        NA                      NA
## 139           NA             NA        NA                      NA
## 140           NA             NA        NA                      NA
## 141           NA             NA        NA                      NA
## 142           NA             NA        NA                      NA
## 143           NA             NA        NA                      NA
## 144           NA             NA        NA                      NA
## 145           NA             NA        NA                      NA
## 146           NA             NA        NA                      NA
## 147           NA             NA        NA                      NA
## 148           NA             NA        NA                      NA
## 149           NA             NA        NA                      NA
## 150           NA             NA        NA                      NA
## 151           NA             NA        NA                      NA
## 152           NA             NA        NA                      NA
## 153           NA             NA        NA                      NA
## 154           NA             NA        NA                      NA
## 155           NA             NA        NA                      NA
## 156           NA             NA        NA                      NA
## 157           NA             NA        NA                      NA
## 158           NA             NA        NA                      NA
## 159           NA             NA        NA                      NA
## 160           NA             NA        NA                      NA
## 161           NA             NA        NA                      NA
## 162           NA             NA        NA                      NA
## 163           NA             NA        NA                      NA
## 164           NA             NA        NA                      NA
## 165           NA             NA        NA                      NA
## 166           NA             NA        NA                      NA
## 167           NA             NA        NA                      NA
## 168           NA             NA        NA                      NA
## 169           NA             NA        NA                      NA
## 170           NA             NA        NA                      NA
## 171           NA             NA        NA                      NA
## 172           NA             NA        NA                      NA
## 173           NA             NA        NA                      NA
## 174           NA             NA        NA                      NA
## 175           NA             NA        NA                      NA
## 176           NA             NA        NA                      NA
## 177           NA             NA        NA                      NA
## 178           NA             NA        NA                      NA
## 179           NA             NA        NA                      NA
## 180           NA             NA        NA                      NA
## 181           NA             NA        NA                      NA
## 182           NA             NA        NA                      NA
## 183           NA             NA        NA                      NA
## 184           NA             NA        NA                      NA
## 185           NA             NA        NA                      NA
## 186           NA             NA        NA                      NA
## 187           NA             NA        NA                      NA
## 188           NA             NA        NA                      NA
## 189           NA             NA        NA                      NA
## 190           NA             NA        NA                      NA
## 191           NA             NA        NA                      NA
## 192           NA             NA        NA                      NA
## 193           NA             NA        NA                      NA
## 194           NA             NA        NA                      NA
## 195           NA             NA        NA                      NA
## 196           NA             NA        NA                      NA
## 197           NA             NA        NA                      NA
## 198           NA             NA        NA                      NA
## 199           NA             NA        NA                      NA
## 200           NA             NA        NA                      NA
## 201           NA             NA        NA                      NA
## 202           NA             NA        NA                      NA
## 203           NA             NA        NA                      NA
## 204           NA             NA        NA                      NA
## 205           NA             NA        NA                      NA
## 206           NA             NA        NA                      NA
## 207           NA             NA        NA                      NA
## 208           NA             NA        NA                      NA
## 209           NA             NA        NA                      NA
## 210           NA             NA        NA                      NA
## 211           NA             NA        NA                      NA
## 212           NA             NA        NA                      NA
## 213           NA             NA        NA                      NA
## 214           NA             NA        NA                      NA
## 215           NA             NA        NA                      NA
## 216           NA             NA        NA                      NA
## 217           NA             NA        NA                      NA
## 218           NA             NA        NA                      NA
## 219           NA             NA        NA                      NA
## 220           NA             NA        NA                      NA
## 221           NA             NA        NA                      NA
## 222           NA             NA        NA                      NA
## 223           NA             NA        NA                      NA
## 224           NA             NA        NA                      NA
## 225           NA             NA        NA                      NA
## 226           NA             NA        NA                      NA
## 227           NA             NA        NA                      NA
## 228           NA             NA        NA                      NA
## 229           NA             NA        NA                      NA
## 230           NA             NA        NA                      NA
## 231           NA             NA        NA                      NA
## 232           NA             NA        NA                      NA
## 233           NA             NA        NA                      NA
## 234           NA             NA        NA                      NA
## 235           NA             NA        NA                      NA
## 236           NA             NA        NA                      NA
## 237           NA             NA        NA                      NA
## 238           NA             NA        NA                      NA
## 239           NA             NA        NA                      NA
## 240           NA             NA        NA                      NA
## 241           NA             NA        NA                      NA
## 242           NA             NA        NA                      NA
## 243           NA             NA        NA                      NA
## 244           NA             NA        NA                      NA
## 245           NA             NA        NA                      NA
## 246           NA             NA        NA                      NA
## 247           NA             NA        NA                      NA
## 248           NA             NA        NA                      NA
## 249           NA             NA        NA                      NA
## 250           NA             NA        NA                      NA
## 251           NA             NA        NA                      NA
## 252           NA             NA        NA                      NA
## 253           NA             NA        NA                      NA
## 254           NA             NA        NA                      NA
## 255           NA             NA        NA                      NA
## 256           NA             NA        NA                      NA
## 257           NA             NA        NA                      NA
## 258           NA             NA        NA                      NA
## 259           NA             NA        NA                      NA
## 260           NA             NA        NA                      NA
##     ciliate_gut ciliate_large_gill ciliate_small_gill
## 1            NA                 NA                 NA
## 2            NA                 NA                 NA
## 3            NA                 NA                 NA
## 4            NA                 NA                 NA
## 5            NA                 NA                 NA
## 6            NA                 NA                 NA
## 7            NA                 NA                 NA
## 8            NA                 NA                 NA
## 9            NA                 NA                 NA
## 10           NA                 NA                 NA
## 11           NA                 NA                 NA
## 12           NA                 NA                 NA
## 13           NA                 NA                 NA
## 14           NA                 NA                 NA
## 15           NA                 NA                 NA
## 16           NA                 NA                 NA
## 17           NA                 NA                 NA
## 18           NA                 NA                 NA
## 19           NA                 NA                 NA
## 20           NA                 NA                 NA
## 21           NA                 NA                 NA
## 22           NA                 NA                 NA
## 23           NA                 NA                 NA
## 24           NA                 NA                 NA
## 25           NA                 NA                 NA
## 26           NA                 NA                 NA
## 27           NA                 NA                 NA
## 28           NA                 NA                 NA
## 29           NA                 NA                 NA
## 30           NA                 NA                 NA
## 31           NA                 NA                 NA
## 32           NA                 NA                 NA
## 33           NA                 NA                 NA
## 34           NA                 NA                 NA
## 35           NA                 NA                 NA
## 36           NA                 NA                 NA
## 37           NA                 NA                 NA
## 38           NA                 NA                 NA
## 39           NA                 NA                 NA
## 40           NA                 NA                 NA
## 41           NA                 NA                 NA
## 42           NA                 NA                 NA
## 43           NA                 NA                 NA
## 44           NA                 NA                 NA
## 45           NA                 NA                 NA
## 46           NA                 NA                 NA
## 47           NA                 NA                 NA
## 48           NA                 NA                 NA
## 49           NA                 NA                 NA
## 50           NA                 NA                 NA
## 51           NA                 NA                 NA
## 52           NA                 NA                 NA
## 53           NA                 NA                 NA
## 54           NA                 NA                 NA
## 55           NA                 NA                 NA
## 56           NA                 NA                 NA
## 57           NA                 NA                 NA
## 58           NA                 NA                 NA
## 59           NA                 NA                 NA
## 60           NA                 NA                 NA
## 61           NA                 NA                 NA
## 62           NA                 NA                 NA
## 63           NA                 NA                 NA
## 64           NA                 NA                 NA
## 65           NA                 NA                 NA
## 66           NA                 NA                 NA
## 67           NA                 NA                 NA
## 68           NA                 NA                 NA
## 69           NA                 NA                 NA
## 70           NA                 NA                 NA
## 71           NA                 NA                 NA
## 72           NA                 NA                 NA
## 73           NA                 NA                 NA
## 74           NA                 NA                 NA
## 75           NA                 NA                 NA
## 76           NA                 NA                 NA
## 77           NA                 NA                 NA
## 78           NA                 NA                 NA
## 79           NA                 NA                 NA
## 80           NA                 NA                 NA
## 81           NA                 NA                 NA
## 82           NA                 NA                 NA
## 83           NA                 NA                 NA
## 84           NA                 NA                 NA
## 85           NA                 NA                 NA
## 86           NA                 NA                 NA
## 87           NA                 NA                 NA
## 88           NA                 NA                 NA
## 89           NA                 NA                 NA
## 90           NA                 NA                 NA
## 91           NA                 NA                 NA
## 92           NA                 NA                 NA
## 93           NA                 NA                 NA
## 94           NA                 NA                 NA
## 95           NA                 NA                 NA
## 96           NA                 NA                 NA
## 97           NA                 NA                 NA
## 98           NA                 NA                 NA
## 99           NA                 NA                 NA
## 100          NA                 NA                 NA
## 101          NA                 NA                 NA
## 102          NA                 NA                 NA
## 103          NA                 NA                 NA
## 104          NA                 NA                 NA
## 105          NA                 NA                 NA
## 106          NA                 NA                 NA
## 107          NA                 NA                 NA
## 108          NA                 NA                 NA
## 109          NA                 NA                 NA
## 110          NA                 NA                 NA
## 111          NA                 NA                 NA
## 112          NA                 NA                 NA
## 113          NA                 NA                 NA
## 114          NA                 NA                 NA
## 115          NA                 NA                 NA
## 116          NA                 NA                 NA
## 117          NA                 NA                 NA
## 118          NA                 NA                 NA
## 119          NA                 NA                 NA
## 120          NA                 NA                 NA
## 121          NA                 NA                 NA
## 122          NA                 NA                 NA
## 123          NA                 NA                 NA
## 124          NA                 NA                 NA
## 125          NA                 NA                 NA
## 126          NA                 NA                 NA
## 127          NA                 NA                 NA
## 128          NA                 NA                 NA
## 129          NA                 NA                 NA
## 130          NA                 NA                 NA
## 131          NA                 NA                 NA
## 132          NA                 NA                 NA
## 133          NA                 NA                 NA
## 134          NA                 NA                 NA
## 135          NA                 NA                 NA
## 136          NA                 NA                 NA
## 137          NA                 NA                 NA
## 138          NA                 NA                 NA
## 139          NA                 NA                 NA
## 140          NA                 NA                 NA
## 141          NA                 NA                 NA
## 142          NA                 NA                 NA
## 143          NA                 NA                 NA
## 144          NA                 NA                 NA
## 145          NA                 NA                 NA
## 146          NA                 NA                 NA
## 147          NA                 NA                 NA
## 148          NA                 NA                 NA
## 149          NA                 NA                 NA
## 150          NA                 NA                 NA
## 151          NA                 NA                 NA
## 152          NA                 NA                 NA
## 153          NA                 NA                 NA
## 154          NA                 NA                 NA
## 155          NA                 NA                 NA
## 156          NA                 NA                 NA
## 157          NA                 NA                 NA
## 158          NA                 NA                 NA
## 159          NA                 NA                 NA
## 160          NA                 NA                 NA
## 161          NA                 NA                 NA
## 162          NA                 NA                 NA
## 163          NA                 NA                 NA
## 164          NA                 NA                 NA
## 165          NA                 NA                 NA
## 166          NA                 NA                 NA
## 167          NA                 NA                 NA
## 168          NA                 NA                 NA
## 169          NA                 NA                 NA
## 170          NA                 NA                 NA
## 171          NA                 NA                 NA
## 172          NA                 NA                 NA
## 173          NA                 NA                 NA
## 174          NA                 NA                 NA
## 175          NA                 NA                 NA
## 176          NA                 NA                 NA
## 177          NA                 NA                 NA
## 178          NA                 NA                 NA
## 179          NA                 NA                 NA
## 180          NA                 NA                 NA
## 181          NA                 NA                 NA
## 182          NA                 NA                 NA
## 183          NA                 NA                 NA
## 184          NA                 NA                 NA
## 185          NA                 NA                 NA
## 186          NA                 NA                 NA
## 187          NA                 NA                 NA
## 188          NA                 NA                 NA
## 189          NA                 NA                 NA
## 190          NA                 NA                 NA
## 191          NA                 NA                 NA
## 192          NA                 NA                 NA
## 193          NA                 NA                 NA
## 194          NA                 NA                 NA
## 195          NA                 NA                 NA
## 196          NA                 NA                 NA
## 197          NA                 NA                 NA
## 198          NA                 NA                 NA
## 199          NA                 NA                 NA
## 200          NA                 NA                 NA
## 201          NA                 NA                 NA
## 202          NA                 NA                 NA
## 203          NA                 NA                 NA
## 204          NA                 NA                 NA
## 205          NA                 NA                 NA
## 206          NA                 NA                 NA
## 207          NA                 NA                 NA
## 208          NA                 NA                 NA
## 209          NA                 NA                 NA
## 210          NA                 NA                 NA
## 211          NA                 NA                 NA
## 212          NA                 NA                 NA
## 213          NA                 NA                 NA
## 214          NA                 NA                 NA
## 215          NA                 NA                 NA
## 216          NA                 NA                 NA
## 217          NA                 NA                 NA
## 218          NA                 NA                 NA
## 219          NA                 NA                 NA
## 220          NA                 NA                 NA
## 221          NA                 NA                 NA
## 222          NA                 NA                 NA
## 223          NA                 NA                 NA
## 224          NA                 NA                 NA
## 225          NA                 NA                 NA
## 226          NA                 NA                 NA
## 227          NA                 NA                 NA
## 228          NA                 NA                 NA
## 229          NA                 NA                 NA
## 230          NA                 NA                 NA
## 231          NA                 NA                 NA
## 232          NA                 NA                 NA
## 233          NA                 NA                 NA
## 234          NA                 NA                 NA
## 235          NA                 NA                 NA
## 236          NA                 NA                 NA
## 237          NA                 NA                 NA
## 238          NA                 NA                 NA
## 239          NA                 NA                 NA
## 240          NA                 NA                 NA
## 241          NA                 NA                 NA
## 242          NA                 NA                 NA
## 243          NA                 NA                 NA
## 244          NA                 NA                 NA
## 245          NA                 NA                 NA
## 246          NA                 NA                 NA
## 247          NA                 NA                 NA
## 248          NA                 NA                 NA
## 249          NA                 NA                 NA
## 250          NA                 NA                 NA
## 251          NA                 NA                 NA
## 252          NA                 NA                 NA
## 253          NA                 NA                 NA
## 254          NA                 NA                 NA
## 255          NA                 NA                 NA
## 256          NA                 NA                 NA
## 257          NA                 NA                 NA
## 258          NA                 NA                 NA
## 259          NA                 NA                 NA
## 260          NA                 NA                 NA
##     coastal_ecological_area condition_code condition_code_description
## 1             Lake Michigan             NA                           
## 2             Lake Michigan             NA                           
## 3             Lake Michigan             NA                           
## 4             Lake Michigan             NA                           
## 5             Lake Michigan             NA                           
## 6             Lake Michigan             NA                           
## 7             Lake Michigan             NA                           
## 8             Lake Michigan             NA                           
## 9             Lake Michigan             NA                           
## 10            Lake Michigan             NA                           
## 11            Lake Michigan             NA                           
## 12            Lake Michigan             NA                           
## 13            Lake Michigan             NA                           
## 14            Lake Michigan             NA                           
## 15            Lake Michigan             NA                           
## 16            Lake Michigan             NA                           
## 17            Lake Michigan             NA                           
## 18            Lake Michigan             NA                           
## 19            Lake Michigan             NA                           
## 20            Lake Michigan             NA                           
## 21            Lake Michigan             NA                           
## 22            Lake Michigan             NA                           
## 23            Lake Michigan             NA                           
## 24            Lake Michigan             NA                           
## 25            Lake Michigan             NA                           
## 26            Lake Michigan             NA                           
## 27            Lake Michigan             NA                           
## 28            Lake Michigan             NA                           
## 29            Lake Michigan             NA                           
## 30            Lake Michigan             NA                           
## 31            Lake Michigan             NA                           
## 32            Lake Michigan             NA                           
## 33            Lake Michigan             NA                           
## 34            Lake Michigan             NA                           
## 35            Lake Michigan             NA                           
## 36            Lake Michigan             NA                           
## 37            Lake Michigan             NA                           
## 38            Lake Michigan             NA                           
## 39            Lake Michigan             NA                           
## 40            Lake Michigan             NA                           
## 41            Lake Michigan             NA                           
## 42            Lake Michigan             NA                           
## 43            Lake Michigan             NA                           
## 44            Lake Michigan             NA                           
## 45            Lake Michigan             NA                           
## 46            Lake Michigan             NA                           
## 47            Lake Michigan             NA                           
## 48            Lake Michigan             NA                           
## 49            Lake Michigan             NA                           
## 50            Lake Michigan             NA                           
## 51            Lake Michigan             NA                           
## 52            Lake Michigan             NA                           
## 53            Lake Michigan             NA                           
## 54            Lake Michigan             NA                           
## 55            Lake Michigan             NA                           
## 56            Lake Michigan             NA                           
## 57            Lake Michigan             NA                           
## 58            Lake Michigan             NA                           
## 59            Lake Michigan             NA                           
## 60            Lake Michigan             NA                           
## 61            Lake Michigan             NA                           
## 62            Lake Michigan             NA                           
## 63            Lake Michigan             NA                           
## 64            Lake Michigan             NA                           
## 65            Lake Michigan             NA                           
## 66            Lake Michigan             NA                           
## 67            Lake Michigan             NA                           
## 68            Lake Michigan             NA                           
## 69            Lake Michigan             NA                           
## 70            Lake Michigan             NA                           
## 71            Lake Michigan             NA                           
## 72            Lake Michigan             NA                           
## 73            Lake Michigan             NA                           
## 74            Lake Michigan             NA                           
## 75            Lake Michigan             NA                           
## 76            Lake Michigan             NA                           
## 77            Lake Michigan             NA                           
## 78            Lake Michigan             NA                           
## 79            Lake Michigan             NA                           
## 80            Lake Michigan             NA                           
## 81            Lake Michigan             NA                           
## 82            Lake Michigan             NA                           
## 83            Lake Michigan             NA                           
## 84            Lake Michigan             NA                           
## 85            Lake Michigan             NA                           
## 86            Lake Michigan             NA                           
## 87            Lake Michigan             NA                           
## 88            Lake Michigan             NA                           
## 89            Lake Michigan             NA                           
## 90            Lake Michigan             NA                           
## 91            Lake Michigan             NA                           
## 92            Lake Michigan             NA                           
## 93            Lake Michigan             NA                           
## 94            Lake Michigan             NA                           
## 95            Lake Michigan             NA                           
## 96            Lake Michigan             NA                           
## 97            Lake Michigan             NA                           
## 98            Lake Michigan             NA                           
## 99            Lake Michigan             NA                           
## 100           Lake Michigan             NA                           
## 101           Lake Michigan             NA                           
## 102           Lake Michigan             NA                           
## 103           Lake Michigan             NA                           
## 104           Lake Michigan             NA                           
## 105           Lake Michigan             NA                           
## 106           Lake Michigan             NA                           
## 107           Lake Michigan             NA                           
## 108           Lake Michigan             NA                           
## 109           Lake Michigan             NA                           
## 110           Lake Michigan             NA                           
## 111           Lake Michigan             NA                           
## 112           Lake Michigan             NA                           
## 113           Lake Michigan             NA                           
## 114           Lake Michigan             NA                           
## 115           Lake Michigan             NA                           
## 116           Lake Michigan             NA                           
## 117           Lake Michigan             NA                           
## 118           Lake Michigan             NA                           
## 119           Lake Michigan             NA                           
## 120           Lake Michigan             NA                           
## 121           Lake Michigan             NA                           
## 122           Lake Michigan             NA                           
## 123           Lake Michigan             NA                           
## 124           Lake Michigan             NA                           
## 125           Lake Michigan             NA                           
## 126           Lake Michigan             NA                           
## 127           Lake Michigan             NA                           
## 128           Lake Michigan             NA                           
## 129           Lake Michigan             NA                           
## 130           Lake Michigan             NA                           
## 131           Lake Michigan             NA                           
## 132           Lake Michigan             NA                           
## 133           Lake Michigan             NA                           
## 134           Lake Michigan             NA                           
## 135           Lake Michigan             NA                           
## 136           Lake Michigan             NA                           
## 137           Lake Michigan             NA                           
## 138           Lake Michigan             NA                           
## 139           Lake Michigan             NA                           
## 140           Lake Michigan             NA                           
## 141           Lake Michigan             NA                           
## 142           Lake Michigan             NA                           
## 143           Lake Michigan             NA                           
## 144           Lake Michigan             NA                           
## 145           Lake Michigan             NA                           
## 146           Lake Michigan             NA                           
## 147           Lake Michigan             NA                           
## 148           Lake Michigan             NA                           
## 149           Lake Michigan             NA                           
## 150           Lake Michigan             NA                           
## 151           Lake Michigan             NA                           
## 152           Lake Michigan             NA                           
## 153           Lake Michigan             NA                           
## 154           Lake Michigan             NA                           
## 155           Lake Michigan             NA                           
## 156           Lake Michigan             NA                           
## 157           Lake Michigan             NA                           
## 158           Lake Michigan             NA                           
## 159           Lake Michigan             NA                           
## 160           Lake Michigan             NA                           
## 161           Lake Michigan             NA                           
## 162           Lake Michigan             NA                           
## 163           Lake Michigan             NA                           
## 164           Lake Michigan             NA                           
## 165           Lake Michigan             NA                           
## 166           Lake Michigan             NA                           
## 167           Lake Michigan             NA                           
## 168           Lake Michigan             NA                           
## 169           Lake Michigan             NA                           
## 170           Lake Michigan             NA                           
## 171           Lake Michigan             NA                           
## 172           Lake Michigan             NA                           
## 173           Lake Michigan             NA                           
## 174           Lake Michigan             NA                           
## 175           Lake Michigan             NA                           
## 176           Lake Michigan             NA                           
## 177           Lake Michigan             NA                           
## 178           Lake Michigan             NA                           
## 179           Lake Michigan             NA                           
## 180           Lake Michigan             NA                           
## 181           Lake Michigan             NA                           
## 182           Lake Michigan             NA                           
## 183           Lake Michigan             NA                           
## 184           Lake Michigan             NA                           
## 185           Lake Michigan             NA                           
## 186           Lake Michigan             NA                           
## 187           Lake Michigan             NA                           
## 188           Lake Michigan             NA                           
## 189           Lake Michigan             NA                           
## 190           Lake Michigan             NA                           
## 191           Lake Michigan             NA                           
## 192           Lake Michigan             NA                           
## 193           Lake Michigan             NA                           
## 194           Lake Michigan             NA                           
## 195           Lake Michigan             NA                           
## 196           Lake Michigan             NA                           
## 197           Lake Michigan             NA                           
## 198           Lake Michigan             NA                           
## 199           Lake Michigan             NA                           
## 200           Lake Michigan             NA                           
## 201           Lake Michigan             NA                           
## 202           Lake Michigan             NA                           
## 203           Lake Michigan             NA                           
## 204           Lake Michigan             NA                           
## 205           Lake Michigan             NA                           
## 206           Lake Michigan             NA                           
## 207           Lake Michigan             NA                           
## 208           Lake Michigan             NA                           
## 209           Lake Michigan             NA                           
## 210           Lake Michigan             NA                           
## 211           Lake Michigan             NA                           
## 212           Lake Michigan             NA                           
## 213           Lake Michigan             NA                           
## 214           Lake Michigan             NA                           
## 215           Lake Michigan             NA                           
## 216           Lake Michigan             NA                           
## 217           Lake Michigan             NA                           
## 218           Lake Michigan             NA                           
## 219           Lake Michigan             NA                           
## 220           Lake Michigan             NA                           
## 221           Lake Michigan             NA                           
## 222           Lake Michigan             NA                           
## 223           Lake Michigan             NA                           
## 224           Lake Michigan             NA                           
## 225           Lake Michigan             NA                           
## 226           Lake Michigan             NA                           
## 227           Lake Michigan             NA                           
## 228           Lake Michigan             NA                           
## 229           Lake Michigan             NA                           
## 230           Lake Michigan             NA                           
## 231           Lake Michigan             NA                           
## 232           Lake Michigan             NA                           
## 233           Lake Michigan             NA                           
## 234           Lake Michigan             NA                           
## 235           Lake Michigan             NA                           
## 236           Lake Michigan             NA                           
## 237           Lake Michigan             NA                           
## 238           Lake Michigan             NA                           
## 239           Lake Michigan             NA                           
## 240           Lake Michigan             NA                           
## 241           Lake Michigan             NA                           
## 242           Lake Michigan             NA                           
## 243           Lake Michigan             NA                           
## 244           Lake Michigan             NA                           
## 245           Lake Michigan             NA                           
## 246           Lake Michigan             NA                           
## 247           Lake Michigan             NA                           
## 248           Lake Michigan             NA                           
## 249           Lake Michigan             NA                           
## 250           Lake Michigan             NA                           
## 251           Lake Michigan             NA                           
## 252           Lake Michigan             NA                           
## 253           Lake Michigan             NA                           
## 254           Lake Michigan             NA                           
## 255           Lake Michigan             NA                           
## 256           Lake Michigan             NA                           
## 257           Lake Michigan             NA                           
## 258           Lake Michigan             NA                           
## 259           Lake Michigan             NA                           
## 260           Lake Michigan             NA                           
##     copepod_body copepod_gill copepod_gut_digestive_tubule dermo
## 1             NA           NA                           NA      
## 2             NA           NA                           NA      
## 3             NA           NA                           NA      
## 4             NA           NA                           NA      
## 5             NA           NA                           NA      
## 6             NA           NA                           NA      
## 7             NA           NA                           NA      
## 8             NA           NA                           NA      
## 9             NA           NA                           NA      
## 10            NA           NA                           NA      
## 11            NA           NA                           NA      
## 12            NA           NA                           NA      
## 13            NA           NA                           NA      
## 14            NA           NA                           NA      
## 15            NA           NA                           NA      
## 16            NA           NA                           NA      
## 17            NA           NA                           NA      
## 18            NA           NA                           NA      
## 19            NA           NA                           NA      
## 20            NA           NA                           NA      
## 21            NA           NA                           NA      
## 22            NA           NA                           NA      
## 23            NA           NA                           NA      
## 24            NA           NA                           NA      
## 25            NA           NA                           NA      
## 26            NA           NA                           NA      
## 27            NA           NA                           NA      
## 28            NA           NA                           NA      
## 29            NA           NA                           NA      
## 30            NA           NA                           NA      
## 31            NA           NA                           NA      
## 32            NA           NA                           NA      
## 33            NA           NA                           NA      
## 34            NA           NA                           NA      
## 35            NA           NA                           NA      
## 36            NA           NA                           NA      
## 37            NA           NA                           NA      
## 38            NA           NA                           NA      
## 39            NA           NA                           NA      
## 40            NA           NA                           NA      
## 41            NA           NA                           NA      
## 42            NA           NA                           NA      
## 43            NA           NA                           NA      
## 44            NA           NA                           NA      
## 45            NA           NA                           NA      
## 46            NA           NA                           NA      
## 47            NA           NA                           NA      
## 48            NA           NA                           NA      
## 49            NA           NA                           NA      
## 50            NA           NA                           NA      
## 51            NA           NA                           NA      
## 52            NA           NA                           NA      
## 53            NA           NA                           NA      
## 54            NA           NA                           NA      
## 55            NA           NA                           NA      
## 56            NA           NA                           NA      
## 57            NA           NA                           NA      
## 58            NA           NA                           NA      
## 59            NA           NA                           NA      
## 60            NA           NA                           NA      
## 61            NA           NA                           NA      
## 62            NA           NA                           NA      
## 63            NA           NA                           NA      
## 64            NA           NA                           NA      
## 65            NA           NA                           NA      
## 66            NA           NA                           NA      
## 67            NA           NA                           NA      
## 68            NA           NA                           NA      
## 69            NA           NA                           NA      
## 70            NA           NA                           NA      
## 71            NA           NA                           NA      
## 72            NA           NA                           NA      
## 73            NA           NA                           NA      
## 74            NA           NA                           NA      
## 75            NA           NA                           NA      
## 76            NA           NA                           NA      
## 77            NA           NA                           NA      
## 78            NA           NA                           NA      
## 79            NA           NA                           NA      
## 80            NA           NA                           NA      
## 81            NA           NA                           NA      
## 82            NA           NA                           NA      
## 83            NA           NA                           NA      
## 84            NA           NA                           NA      
## 85            NA           NA                           NA      
## 86            NA           NA                           NA      
## 87            NA           NA                           NA      
## 88            NA           NA                           NA      
## 89            NA           NA                           NA      
## 90            NA           NA                           NA      
## 91            NA           NA                           NA      
## 92            NA           NA                           NA      
## 93            NA           NA                           NA      
## 94            NA           NA                           NA      
## 95            NA           NA                           NA      
## 96            NA           NA                           NA      
## 97            NA           NA                           NA      
## 98            NA           NA                           NA      
## 99            NA           NA                           NA      
## 100           NA           NA                           NA      
## 101           NA           NA                           NA      
## 102           NA           NA                           NA      
## 103           NA           NA                           NA      
## 104           NA           NA                           NA      
## 105           NA           NA                           NA      
## 106           NA           NA                           NA      
## 107           NA           NA                           NA      
## 108           NA           NA                           NA      
## 109           NA           NA                           NA      
## 110           NA           NA                           NA      
## 111           NA           NA                           NA      
## 112           NA           NA                           NA      
## 113           NA           NA                           NA      
## 114           NA           NA                           NA      
## 115           NA           NA                           NA      
## 116           NA           NA                           NA      
## 117           NA           NA                           NA      
## 118           NA           NA                           NA      
## 119           NA           NA                           NA      
## 120           NA           NA                           NA      
## 121           NA           NA                           NA      
## 122           NA           NA                           NA      
## 123           NA           NA                           NA      
## 124           NA           NA                           NA      
## 125           NA           NA                           NA      
## 126           NA           NA                           NA      
## 127           NA           NA                           NA      
## 128           NA           NA                           NA      
## 129           NA           NA                           NA      
## 130           NA           NA                           NA      
## 131           NA           NA                           NA      
## 132           NA           NA                           NA      
## 133           NA           NA                           NA      
## 134           NA           NA                           NA      
## 135           NA           NA                           NA      
## 136           NA           NA                           NA      
## 137           NA           NA                           NA      
## 138           NA           NA                           NA      
## 139           NA           NA                           NA      
## 140           NA           NA                           NA      
## 141           NA           NA                           NA      
## 142           NA           NA                           NA      
## 143           NA           NA                           NA      
## 144           NA           NA                           NA      
## 145           NA           NA                           NA      
## 146           NA           NA                           NA      
## 147           NA           NA                           NA      
## 148           NA           NA                           NA      
## 149           NA           NA                           NA      
## 150           NA           NA                           NA      
## 151           NA           NA                           NA      
## 152           NA           NA                           NA      
## 153           NA           NA                           NA      
## 154           NA           NA                           NA      
## 155           NA           NA                           NA      
## 156           NA           NA                           NA      
## 157           NA           NA                           NA      
## 158           NA           NA                           NA      
## 159           NA           NA                           NA      
## 160           NA           NA                           NA      
## 161           NA           NA                           NA      
## 162           NA           NA                           NA      
## 163           NA           NA                           NA      
## 164           NA           NA                           NA      
## 165           NA           NA                           NA      
## 166           NA           NA                           NA      
## 167           NA           NA                           NA      
## 168           NA           NA                           NA      
## 169           NA           NA                           NA      
## 170           NA           NA                           NA      
## 171           NA           NA                           NA      
## 172           NA           NA                           NA      
## 173           NA           NA                           NA      
## 174           NA           NA                           NA      
## 175           NA           NA                           NA      
## 176           NA           NA                           NA      
## 177           NA           NA                           NA      
## 178           NA           NA                           NA      
## 179           NA           NA                           NA      
## 180           NA           NA                           NA      
## 181           NA           NA                           NA      
## 182           NA           NA                           NA      
## 183           NA           NA                           NA      
## 184           NA           NA                           NA      
## 185           NA           NA                           NA      
## 186           NA           NA                           NA      
## 187           NA           NA                           NA      
## 188           NA           NA                           NA      
## 189           NA           NA                           NA      
## 190           NA           NA                           NA      
## 191           NA           NA                           NA      
## 192           NA           NA                           NA      
## 193           NA           NA                           NA      
## 194           NA           NA                           NA      
## 195           NA           NA                           NA      
## 196           NA           NA                           NA      
## 197           NA           NA                           NA      
## 198           NA           NA                           NA      
## 199           NA           NA                           NA      
## 200           NA           NA                           NA      
## 201           NA           NA                           NA      
## 202           NA           NA                           NA      
## 203           NA           NA                           NA      
## 204           NA           NA                           NA      
## 205           NA           NA                           NA      
## 206           NA           NA                           NA      
## 207           NA           NA                           NA      
## 208           NA           NA                           NA      
## 209           NA           NA                           NA      
## 210           NA           NA                           NA      
## 211           NA           NA                           NA      
## 212           NA           NA                           NA      
## 213           NA           NA                           NA      
## 214           NA           NA                           NA      
## 215           NA           NA                           NA      
## 216           NA           NA                           NA      
## 217           NA           NA                           NA      
## 218           NA           NA                           NA      
## 219           NA           NA                           NA      
## 220           NA           NA                           NA      
## 221           NA           NA                           NA      
## 222           NA           NA                           NA      
## 223           NA           NA                           NA      
## 224           NA           NA                           NA      
## 225           NA           NA                           NA      
## 226           NA           NA                           NA      
## 227           NA           NA                           NA      
## 228           NA           NA                           NA      
## 229           NA           NA                           NA      
## 230           NA           NA                           NA      
## 231           NA           NA                           NA      
## 232           NA           NA                           NA      
## 233           NA           NA                           NA      
## 234           NA           NA                           NA      
## 235           NA           NA                           NA      
## 236           NA           NA                           NA      
## 237           NA           NA                           NA      
## 238           NA           NA                           NA      
## 239           NA           NA                           NA      
## 240           NA           NA                           NA      
## 241           NA           NA                           NA      
## 242           NA           NA                           NA      
## 243           NA           NA                           NA      
## 244           NA           NA                           NA      
## 245           NA           NA                           NA      
## 246           NA           NA                           NA      
## 247           NA           NA                           NA      
## 248           NA           NA                           NA      
## 249           NA           NA                           NA      
## 250           NA           NA                           NA      
## 251           NA           NA                           NA      
## 252           NA           NA                           NA      
## 253           NA           NA                           NA      
## 254           NA           NA                           NA      
## 255           NA           NA                           NA      
## 256           NA           NA                           NA      
## 257           NA           NA                           NA      
## 258           NA           NA                           NA      
## 259           NA           NA                           NA      
## 260           NA           NA                           NA      
##     dermo_description dermo_infection_intensity dermo_numerical_value
## 1                                                                  NA
## 2                                                                  NA
## 3                                                                  NA
## 4                                                                  NA
## 5                                                                  NA
## 6                                                                  NA
## 7                                                                  NA
## 8                                                                  NA
## 9                                                                  NA
## 10                                                                 NA
## 11                                                                 NA
## 12                                                                 NA
## 13                                                                 NA
## 14                                                                 NA
## 15                                                                 NA
## 16                                                                 NA
## 17                                                                 NA
## 18                                                                 NA
## 19                                                                 NA
## 20                                                                 NA
## 21                                                                 NA
## 22                                                                 NA
## 23                                                                 NA
## 24                                                                 NA
## 25                                                                 NA
## 26                                                                 NA
## 27                                                                 NA
## 28                                                                 NA
## 29                                                                 NA
## 30                                                                 NA
## 31                                                                 NA
## 32                                                                 NA
## 33                                                                 NA
## 34                                                                 NA
## 35                                                                 NA
## 36                                                                 NA
## 37                                                                 NA
## 38                                                                 NA
## 39                                                                 NA
## 40                                                                 NA
## 41                                                                 NA
## 42                                                                 NA
## 43                                                                 NA
## 44                                                                 NA
## 45                                                                 NA
## 46                                                                 NA
## 47                                                                 NA
## 48                                                                 NA
## 49                                                                 NA
## 50                                                                 NA
## 51                                                                 NA
## 52                                                                 NA
## 53                                                                 NA
## 54                                                                 NA
## 55                                                                 NA
## 56                                                                 NA
## 57                                                                 NA
## 58                                                                 NA
## 59                                                                 NA
## 60                                                                 NA
## 61                                                                 NA
## 62                                                                 NA
## 63                                                                 NA
## 64                                                                 NA
## 65                                                                 NA
## 66                                                                 NA
## 67                                                                 NA
## 68                                                                 NA
## 69                                                                 NA
## 70                                                                 NA
## 71                                                                 NA
## 72                                                                 NA
## 73                                                                 NA
## 74                                                                 NA
## 75                                                                 NA
## 76                                                                 NA
## 77                                                                 NA
## 78                                                                 NA
## 79                                                                 NA
## 80                                                                 NA
## 81                                                                 NA
## 82                                                                 NA
## 83                                                                 NA
## 84                                                                 NA
## 85                                                                 NA
## 86                                                                 NA
## 87                                                                 NA
## 88                                                                 NA
## 89                                                                 NA
## 90                                                                 NA
## 91                                                                 NA
## 92                                                                 NA
## 93                                                                 NA
## 94                                                                 NA
## 95                                                                 NA
## 96                                                                 NA
## 97                                                                 NA
## 98                                                                 NA
## 99                                                                 NA
## 100                                                                NA
## 101                                                                NA
## 102                                                                NA
## 103                                                                NA
## 104                                                                NA
## 105                                                                NA
## 106                                                                NA
## 107                                                                NA
## 108                                                                NA
## 109                                                                NA
## 110                                                                NA
## 111                                                                NA
## 112                                                                NA
## 113                                                                NA
## 114                                                                NA
## 115                                                                NA
## 116                                                                NA
## 117                                                                NA
## 118                                                                NA
## 119                                                                NA
## 120                                                                NA
## 121                                                                NA
## 122                                                                NA
## 123                                                                NA
## 124                                                                NA
## 125                                                                NA
## 126                                                                NA
## 127                                                                NA
## 128                                                                NA
## 129                                                                NA
## 130                                                                NA
## 131                                                                NA
## 132                                                                NA
## 133                                                                NA
## 134                                                                NA
## 135                                                                NA
## 136                                                                NA
## 137                                                                NA
## 138                                                                NA
## 139                                                                NA
## 140                                                                NA
## 141                                                                NA
## 142                                                                NA
## 143                                                                NA
## 144                                                                NA
## 145                                                                NA
## 146                                                                NA
## 147                                                                NA
## 148                                                                NA
## 149                                                                NA
## 150                                                                NA
## 151                                                                NA
## 152                                                                NA
## 153                                                                NA
## 154                                                                NA
## 155                                                                NA
## 156                                                                NA
## 157                                                                NA
## 158                                                                NA
## 159                                                                NA
## 160                                                                NA
## 161                                                                NA
## 162                                                                NA
## 163                                                                NA
## 164                                                                NA
## 165                                                                NA
## 166                                                                NA
## 167                                                                NA
## 168                                                                NA
## 169                                                                NA
## 170                                                                NA
## 171                                                                NA
## 172                                                                NA
## 173                                                                NA
## 174                                                                NA
## 175                                                                NA
## 176                                                                NA
## 177                                                                NA
## 178                                                                NA
## 179                                                                NA
## 180                                                                NA
## 181                                                                NA
## 182                                                                NA
## 183                                                                NA
## 184                                                                NA
## 185                                                                NA
## 186                                                                NA
## 187                                                                NA
## 188                                                                NA
## 189                                                                NA
## 190                                                                NA
## 191                                                                NA
## 192                                                                NA
## 193                                                                NA
## 194                                                                NA
## 195                                                                NA
## 196                                                                NA
## 197                                                                NA
## 198                                                                NA
## 199                                                                NA
## 200                                                                NA
## 201                                                                NA
## 202                                                                NA
## 203                                                                NA
## 204                                                                NA
## 205                                                                NA
## 206                                                                NA
## 207                                                                NA
## 208                                                                NA
## 209                                                                NA
## 210                                                                NA
## 211                                                                NA
## 212                                                                NA
## 213                                                                NA
## 214                                                                NA
## 215                                                                NA
## 216                                                                NA
## 217                                                                NA
## 218                                                                NA
## 219                                                                NA
## 220                                                                NA
## 221                                                                NA
## 222                                                                NA
## 223                                                                NA
## 224                                                                NA
## 225                                                                NA
## 226                                                                NA
## 227                                                                NA
## 228                                                                NA
## 229                                                                NA
## 230                                                                NA
## 231                                                                NA
## 232                                                                NA
## 233                                                                NA
## 234                                                                NA
## 235                                                                NA
## 236                                                                NA
## 237                                                                NA
## 238                                                                NA
## 239                                                                NA
## 240                                                                NA
## 241                                                                NA
## 242                                                                NA
## 243                                                                NA
## 244                                                                NA
## 245                                                                NA
## 246                                                                NA
## 247                                                                NA
## 248                                                                NA
## 249                                                                NA
## 250                                                                NA
## 251                                                                NA
## 252                                                                NA
## 253                                                                NA
## 254                                                                NA
## 255                                                                NA
## 256                                                                NA
## 257                                                                NA
## 258                                                                NA
## 259                                                                NA
## 260                                                                NA
##     diffuse_inflammation diffuse_necrosis digestive_tubule_atrophy
## 1                      0                0                        1
## 2                      0                0                        2
## 3                      0                0                        2
## 4                      0                0                        2
## 5                      0                0                        2
## 6                      0                0                        3
## 7                      0                0                        2
## 8                      0                0                        2
## 9                      0                0                        3
## 10                     0                0                        1
## 11                     0                0                        2
## 12                     0                0                        2
## 13                     0                0                        1
## 14                     0                0                        3
## 15                     0                0                        1
## 16                     0                0                        1
## 17                     0                0                        2
## 18                     0                0                        1
## 19                     0                0                        2
## 20                     0                0                        3
## 21                     0                0                        1
## 22                     0                0                        3
## 23                     0                0                        2
## 24                     0                0                        1
## 25                     0                0                        2
## 26                     0                0                        2
## 27                     0                0                        1
## 28                     0                0                        2
## 29                     0                0                        1
## 30                     0                0                        1
## 31                     0                0                        0
## 32                     0                0                        0
## 33                     0                0                        0
## 34                     0                0                        1
## 35                     0                0                        3
## 36                     0                0                        1
## 37                     0                0                        2
## 38                     0                0                        2
## 39                     0                0                        2
## 40                     0                0                        2
## 41                     0                0                        1
## 42                     0                0                        1
## 43                     0                0                        2
## 44                     0                0                        1
## 45                     0                0                        0
## 46                     0                0                        1
## 47                     0                0                        1
## 48                     0                0                        1
## 49                     0                0                        1
## 50                     0                0                        1
## 51                     0                0                        2
## 52                     0                0                        1
## 53                     0                0                        1
## 54                     0                0                        1
## 55                     0                0                        1
## 56                     0                0                        2
## 57                     0                0                        2
## 58                     0                0                        3
## 59                     0                0                        3
## 60                     0                0                        2
## 61                     0                0                        1
## 62                     0                0                        1
## 63                     0                0                        1
## 64                     0                0                        1
## 65                     0                0                        2
## 66                     0                0                        2
## 67                     0                0                        2
## 68                     0                0                        2
## 69                     0                0                        2
## 70                     0                0                        1
## 71                     0                0                        0
## 72                     0                0                        1
## 73                     0                0                        1
## 74                     0                0                        1
## 75                     0                0                        1
## 76                     0                0                        0
## 77                     0                0                        0
## 78                     0                0                        0
## 79                     0                0                        1
## 80                     0                0                        0
## 81                     0                0                        2
## 82                     0                0                        2
## 83                     0                0                        2
## 84                     0                0                        2
## 85                     0                0                        2
## 86                     0                0                        1
## 87                     0                0                        2
## 88                     0                0                        1
## 89                     0                0                        2
## 90                     0                0                        1
## 91                     0                0                        1
## 92                     0                0                        0
## 93                     0                0                        1
## 94                     0                0                        1
## 95                     0                0                        0
## 96                     0                0                        1
## 97                     0                0                        2
## 98                     0                0                        1
## 99                     0                0                        1
## 100                    0                0                        1
## 101                    0                0                        2
## 102                    0                0                        1
## 103                    0                0                        2
## 104                    0                0                        1
## 105                    0                0                        3
## 106                    0                0                        2
## 107                    0                0                        3
## 108                    0                0                        3
## 109                    0                0                        3
## 110                    0                0                        3
## 111                    0                0                        2
## 112                    0                0                        2
## 113                    0                0                        1
## 114                    0                0                        3
## 115                    0                0                        2
## 116                    1                0                        1
## 117                    0                0                        2
## 118                    0                0                        1
## 119                    0                0                        1
## 120                    0                0                        1
## 121                    0                0                        2
## 122                    0                0                        1
## 123                    0                0                        2
## 124                    0                0                        2
## 125                    0                0                        1
## 126                    0                0                        0
## 127                    0                0                        1
## 128                    0                0                        1
## 129                    0                0                        1
## 130                    0                0                        0
## 131                    0                0                        3
## 132                    0                0                        2
## 133                    0                0                        2
## 134                    0                0                        1
## 135                    0                0                        3
## 136                    0                0                        2
## 137                    2                0                        1
## 138                    0                0                        1
## 139                    0                0                        0
## 140                    0                0                        3
## 141                    0                0                        3
## 142                    0                0                        3
## 143                    0                0                        2
## 144                    0                0                        2
## 145                    0                0                        2
## 146                    0                0                        2
## 147                    0                0                        1
## 148                    0                0                        1
## 149                    0                0                        3
## 150                    0                0                        3
## 151                    0                0                        1
## 152                    0                0                        1
## 153                    0                0                        1
## 154                    0                0                        1
## 155                    0                0                        2
## 156                    0                0                        1
## 157                    0                0                        1
## 158                    0                0                        2
## 159                    0                0                        1
## 160                    0                0                        2
## 161                    0                0                        2
## 162                    0                0                        2
## 163                    0                0                        0
## 164                    0                0                        0
## 165                    0                0                        1
## 166                    0                0                        2
## 167                    0                0                        1
## 168                    0                0                        2
## 169                    0                0                        2
## 170                    0                0                        2
## 171                    0                0                        1
## 172                    0                0                        3
## 173                    0                0                        3
## 174                    0                0                        3
## 175                    0                0                        2
## 176                    0                0                        1
## 177                    0                0                        2
## 178                    0                0                        2
## 179                    0                0                        2
## 180                    0                0                        2
## 181                    0                0                        1
## 182                    0                0                        1
## 183                    0                0                        2
## 184                    0                0                        1
## 185                    0                0                        3
## 186                    0                0                        2
## 187                    0                0                        1
## 188                    0                0                        2
## 189                    0                0                        1
## 190                    0                0                        1
## 191                    0                0                        3
## 192                    0                0                        2
## 193                    0                0                        3
## 194                    0                0                        2
## 195                    0                0                        0
## 196                    0                0                        1
## 197                    0                0                        2
## 198                    0                0                        1
## 199                    0                0                        1
## 200                    0                0                        3
## 201                    0                0                        2
## 202                    0                0                        2
## 203                    0                0                        1
## 204                    0                0                        2
## 205                    0                0                        3
## 206                    0                0                        2
## 207                    0                0                        2
## 208                    0                0                        2
## 209                    0                0                        2
## 210                    0                0                        2
## 211                    0                0                        3
## 212                    0                0                        2
## 213                    0                0                        2
## 214                    0                0                        3
## 215                    0                0                        3
## 216                    0                0                        1
## 217                    0                0                        1
## 218                    0                0                        0
## 219                    0                0                        0
## 220                    0                0                        1
## 221                    0                0                        2
## 222                    0                0                        1
## 223                    0                0                        2
## 224                    0                0                        0
## 225                    0                0                        1
## 226                    0                0                        1
## 227                    0                0                        1
## 228                    0                0                        1
## 229                    0                0                        1
## 230                    0                0                        1
## 231                    0                0                        0
## 232                    0                0                        2
## 233                    0                0                        2
## 234                    0                0                        1
## 235                    0                0                        3
## 236                    0                0                        1
## 237                    0                0                        3
## 238                    0                0                        2
## 239                    0                0                        2
## 240                    0                0                        2
## 241                    0                0                        1
## 242                    0                0                        1
## 243                    0                0                        1
## 244                    0                0                        1
## 245                    0                0                        1
## 246                    0                0                        0
## 247                    0                0                        1
## 248                    0                0                        3
## 249                    0                0                        2
## 250                    0                0                        3
## 251                    0                0                        2
## 252                    0                0                        2
## 253                    0                0                        1
## 254                    0                0                        1
## 255                    0                0                        3
## 256                    0                0                        2
## 257                    0                0                        2
## 258                    0                0                        3
## 259                    0                0                        2
## 260                    0                0                        2
##                                                                                                                 digestive_tubule_atrophy_description
## 1   Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 2                                                                                         Wall thickness averaging about one-half as thick as normal
## 3                                                                                         Wall thickness averaging about one-half as thick as normal
## 4                                                                                         Wall thickness averaging about one-half as thick as normal
## 5                                                                                         Wall thickness averaging about one-half as thick as normal
## 6                                 Wall thickness less than one-half of normal, most tubules walls significantly atrophied, some walls extremely thin
## 7                                                                                         Wall thickness averaging about one-half as thick as normal
## 8                                                                                         Wall thickness averaging about one-half as thick as normal
## 9                                 Wall thickness less than one-half of normal, most tubules walls significantly atrophied, some walls extremely thin
## 10  Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 11                                                                                        Wall thickness averaging about one-half as thick as normal
## 12                                                                                        Wall thickness averaging about one-half as thick as normal
## 13  Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 14                                Wall thickness less than one-half of normal, most tubules walls significantly atrophied, some walls extremely thin
## 15  Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 16  Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 17                                                                                        Wall thickness averaging about one-half as thick as normal
## 18  Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 19                                                                                        Wall thickness averaging about one-half as thick as normal
## 20                                Wall thickness less than one-half of normal, most tubules walls significantly atrophied, some walls extremely thin
## 21  Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 22                                Wall thickness less than one-half of normal, most tubules walls significantly atrophied, some walls extremely thin
## 23                                                                                        Wall thickness averaging about one-half as thick as normal
## 24  Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 25                                                                                        Wall thickness averaging about one-half as thick as normal
## 26                                                                                        Wall thickness averaging about one-half as thick as normal
## 27  Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 28                                                                                        Wall thickness averaging about one-half as thick as normal
## 29  Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 30  Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 31                                                 Normal wall thickness in most tubules, lumen nearly occluded, few tubules even slightly atrophied
## 32                                                 Normal wall thickness in most tubules, lumen nearly occluded, few tubules even slightly atrophied
## 33                                                 Normal wall thickness in most tubules, lumen nearly occluded, few tubules even slightly atrophied
## 34  Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 35                                Wall thickness less than one-half of normal, most tubules walls significantly atrophied, some walls extremely thin
## 36  Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 37                                                                                        Wall thickness averaging about one-half as thick as normal
## 38                                                                                        Wall thickness averaging about one-half as thick as normal
## 39                                                                                        Wall thickness averaging about one-half as thick as normal
## 40                                                                                        Wall thickness averaging about one-half as thick as normal
## 41  Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 42  Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 43                                                                                        Wall thickness averaging about one-half as thick as normal
## 44  Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 45                                                 Normal wall thickness in most tubules, lumen nearly occluded, few tubules even slightly atrophied
## 46  Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 47  Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 48  Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 49  Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 50  Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 51                                                                                        Wall thickness averaging about one-half as thick as normal
## 52  Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 53  Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 54  Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 55  Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 56                                                                                        Wall thickness averaging about one-half as thick as normal
## 57                                                                                        Wall thickness averaging about one-half as thick as normal
## 58                                Wall thickness less than one-half of normal, most tubules walls significantly atrophied, some walls extremely thin
## 59                                Wall thickness less than one-half of normal, most tubules walls significantly atrophied, some walls extremely thin
## 60                                                                                        Wall thickness averaging about one-half as thick as normal
## 61  Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 62  Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 63  Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 64  Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 65                                                                                        Wall thickness averaging about one-half as thick as normal
## 66                                                                                        Wall thickness averaging about one-half as thick as normal
## 67                                                                                        Wall thickness averaging about one-half as thick as normal
## 68                                                                                        Wall thickness averaging about one-half as thick as normal
## 69                                                                                        Wall thickness averaging about one-half as thick as normal
## 70  Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 71                                                 Normal wall thickness in most tubules, lumen nearly occluded, few tubules even slightly atrophied
## 72  Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 73  Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 74  Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 75  Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 76                                                 Normal wall thickness in most tubules, lumen nearly occluded, few tubules even slightly atrophied
## 77                                                 Normal wall thickness in most tubules, lumen nearly occluded, few tubules even slightly atrophied
## 78                                                 Normal wall thickness in most tubules, lumen nearly occluded, few tubules even slightly atrophied
## 79  Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 80                                                 Normal wall thickness in most tubules, lumen nearly occluded, few tubules even slightly atrophied
## 81                                                                                        Wall thickness averaging about one-half as thick as normal
## 82                                                                                        Wall thickness averaging about one-half as thick as normal
## 83                                                                                        Wall thickness averaging about one-half as thick as normal
## 84                                                                                        Wall thickness averaging about one-half as thick as normal
## 85                                                                                        Wall thickness averaging about one-half as thick as normal
## 86  Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 87                                                                                        Wall thickness averaging about one-half as thick as normal
## 88  Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 89                                                                                        Wall thickness averaging about one-half as thick as normal
## 90  Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 91  Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 92                                                 Normal wall thickness in most tubules, lumen nearly occluded, few tubules even slightly atrophied
## 93  Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 94  Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 95                                                 Normal wall thickness in most tubules, lumen nearly occluded, few tubules even slightly atrophied
## 96  Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 97                                                                                        Wall thickness averaging about one-half as thick as normal
## 98  Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 99  Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 100 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 101                                                                                       Wall thickness averaging about one-half as thick as normal
## 102 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 103                                                                                       Wall thickness averaging about one-half as thick as normal
## 104 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 105                               Wall thickness less than one-half of normal, most tubules walls significantly atrophied, some walls extremely thin
## 106                                                                                       Wall thickness averaging about one-half as thick as normal
## 107                               Wall thickness less than one-half of normal, most tubules walls significantly atrophied, some walls extremely thin
## 108                               Wall thickness less than one-half of normal, most tubules walls significantly atrophied, some walls extremely thin
## 109                               Wall thickness less than one-half of normal, most tubules walls significantly atrophied, some walls extremely thin
## 110                               Wall thickness less than one-half of normal, most tubules walls significantly atrophied, some walls extremely thin
## 111                                                                                       Wall thickness averaging about one-half as thick as normal
## 112                                                                                       Wall thickness averaging about one-half as thick as normal
## 113 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 114                               Wall thickness less than one-half of normal, most tubules walls significantly atrophied, some walls extremely thin
## 115                                                                                       Wall thickness averaging about one-half as thick as normal
## 116 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 117                                                                                       Wall thickness averaging about one-half as thick as normal
## 118 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 119 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 120 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 121                                                                                       Wall thickness averaging about one-half as thick as normal
## 122 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 123                                                                                       Wall thickness averaging about one-half as thick as normal
## 124                                                                                       Wall thickness averaging about one-half as thick as normal
## 125 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 126                                                Normal wall thickness in most tubules, lumen nearly occluded, few tubules even slightly atrophied
## 127 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 128 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 129 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 130                                                Normal wall thickness in most tubules, lumen nearly occluded, few tubules even slightly atrophied
## 131                               Wall thickness less than one-half of normal, most tubules walls significantly atrophied, some walls extremely thin
## 132                                                                                       Wall thickness averaging about one-half as thick as normal
## 133                                                                                       Wall thickness averaging about one-half as thick as normal
## 134 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 135                               Wall thickness less than one-half of normal, most tubules walls significantly atrophied, some walls extremely thin
## 136                                                                                       Wall thickness averaging about one-half as thick as normal
## 137 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 138 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 139                                                Normal wall thickness in most tubules, lumen nearly occluded, few tubules even slightly atrophied
## 140                               Wall thickness less than one-half of normal, most tubules walls significantly atrophied, some walls extremely thin
## 141                               Wall thickness less than one-half of normal, most tubules walls significantly atrophied, some walls extremely thin
## 142                               Wall thickness less than one-half of normal, most tubules walls significantly atrophied, some walls extremely thin
## 143                                                                                       Wall thickness averaging about one-half as thick as normal
## 144                                                                                       Wall thickness averaging about one-half as thick as normal
## 145                                                                                       Wall thickness averaging about one-half as thick as normal
## 146                                                                                       Wall thickness averaging about one-half as thick as normal
## 147 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 148 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 149                               Wall thickness less than one-half of normal, most tubules walls significantly atrophied, some walls extremely thin
## 150                               Wall thickness less than one-half of normal, most tubules walls significantly atrophied, some walls extremely thin
## 151 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 152 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 153 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 154 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 155                                                                                       Wall thickness averaging about one-half as thick as normal
## 156 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 157 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 158                                                                                       Wall thickness averaging about one-half as thick as normal
## 159 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 160                                                                                       Wall thickness averaging about one-half as thick as normal
## 161                                                                                       Wall thickness averaging about one-half as thick as normal
## 162                                                                                       Wall thickness averaging about one-half as thick as normal
## 163                                                Normal wall thickness in most tubules, lumen nearly occluded, few tubules even slightly atrophied
## 164                                                Normal wall thickness in most tubules, lumen nearly occluded, few tubules even slightly atrophied
## 165 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 166                                                                                       Wall thickness averaging about one-half as thick as normal
## 167 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 168                                                                                       Wall thickness averaging about one-half as thick as normal
## 169                                                                                       Wall thickness averaging about one-half as thick as normal
## 170                                                                                       Wall thickness averaging about one-half as thick as normal
## 171 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 172                               Wall thickness less than one-half of normal, most tubules walls significantly atrophied, some walls extremely thin
## 173                               Wall thickness less than one-half of normal, most tubules walls significantly atrophied, some walls extremely thin
## 174                               Wall thickness less than one-half of normal, most tubules walls significantly atrophied, some walls extremely thin
## 175                                                                                       Wall thickness averaging about one-half as thick as normal
## 176 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 177                                                                                       Wall thickness averaging about one-half as thick as normal
## 178                                                                                       Wall thickness averaging about one-half as thick as normal
## 179                                                                                       Wall thickness averaging about one-half as thick as normal
## 180                                                                                       Wall thickness averaging about one-half as thick as normal
## 181 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 182 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 183                                                                                       Wall thickness averaging about one-half as thick as normal
## 184 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 185                               Wall thickness less than one-half of normal, most tubules walls significantly atrophied, some walls extremely thin
## 186                                                                                       Wall thickness averaging about one-half as thick as normal
## 187 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 188                                                                                       Wall thickness averaging about one-half as thick as normal
## 189 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 190 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 191                               Wall thickness less than one-half of normal, most tubules walls significantly atrophied, some walls extremely thin
## 192                                                                                       Wall thickness averaging about one-half as thick as normal
## 193                               Wall thickness less than one-half of normal, most tubules walls significantly atrophied, some walls extremely thin
## 194                                                                                       Wall thickness averaging about one-half as thick as normal
## 195                                                Normal wall thickness in most tubules, lumen nearly occluded, few tubules even slightly atrophied
## 196 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 197                                                                                       Wall thickness averaging about one-half as thick as normal
## 198 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 199 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 200                               Wall thickness less than one-half of normal, most tubules walls significantly atrophied, some walls extremely thin
## 201                                                                                       Wall thickness averaging about one-half as thick as normal
## 202                                                                                       Wall thickness averaging about one-half as thick as normal
## 203 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 204                                                                                       Wall thickness averaging about one-half as thick as normal
## 205                               Wall thickness less than one-half of normal, most tubules walls significantly atrophied, some walls extremely thin
## 206                                                                                       Wall thickness averaging about one-half as thick as normal
## 207                                                                                       Wall thickness averaging about one-half as thick as normal
## 208                                                                                       Wall thickness averaging about one-half as thick as normal
## 209                                                                                       Wall thickness averaging about one-half as thick as normal
## 210                                                                                       Wall thickness averaging about one-half as thick as normal
## 211                               Wall thickness less than one-half of normal, most tubules walls significantly atrophied, some walls extremely thin
## 212                                                                                       Wall thickness averaging about one-half as thick as normal
## 213                                                                                       Wall thickness averaging about one-half as thick as normal
## 214                               Wall thickness less than one-half of normal, most tubules walls significantly atrophied, some walls extremely thin
## 215                               Wall thickness less than one-half of normal, most tubules walls significantly atrophied, some walls extremely thin
## 216 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 217 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 218                                                Normal wall thickness in most tubules, lumen nearly occluded, few tubules even slightly atrophied
## 219                                                Normal wall thickness in most tubules, lumen nearly occluded, few tubules even slightly atrophied
## 220 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 221                                                                                       Wall thickness averaging about one-half as thick as normal
## 222 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 223                                                                                       Wall thickness averaging about one-half as thick as normal
## 224                                                Normal wall thickness in most tubules, lumen nearly occluded, few tubules even slightly atrophied
## 225 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 226 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 227 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 228 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 229 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 230 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 231                                                Normal wall thickness in most tubules, lumen nearly occluded, few tubules even slightly atrophied
## 232                                                                                       Wall thickness averaging about one-half as thick as normal
## 233                                                                                       Wall thickness averaging about one-half as thick as normal
## 234 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 235                               Wall thickness less than one-half of normal, most tubules walls significantly atrophied, some walls extremely thin
## 236 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 237                               Wall thickness less than one-half of normal, most tubules walls significantly atrophied, some walls extremely thin
## 238                                                                                       Wall thickness averaging about one-half as thick as normal
## 239                                                                                       Wall thickness averaging about one-half as thick as normal
## 240                                                                                       Wall thickness averaging about one-half as thick as normal
## 241 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 242 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 243 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 244 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 245 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 246                                                Normal wall thickness in most tubules, lumen nearly occluded, few tubules even slightly atrophied
## 247 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 248                               Wall thickness less than one-half of normal, most tubules walls significantly atrophied, some walls extremely thin
## 249                                                                                       Wall thickness averaging about one-half as thick as normal
## 250                               Wall thickness less than one-half of normal, most tubules walls significantly atrophied, some walls extremely thin
## 251                                                                                       Wall thickness averaging about one-half as thick as normal
## 252                                                                                       Wall thickness averaging about one-half as thick as normal
## 253 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 254 Average wall thickness less than normal but greater then one-half normal thickness, most tubules showing some atrophy, some tubules still normal
## 255                               Wall thickness less than one-half of normal, most tubules walls significantly atrophied, some walls extremely thin
## 256                                                                                       Wall thickness averaging about one-half as thick as normal
## 257                                                                                       Wall thickness averaging about one-half as thick as normal
## 258                               Wall thickness less than one-half of normal, most tubules walls significantly atrophied, some walls extremely thin
## 259                                                                                       Wall thickness averaging about one-half as thick as normal
## 260                                                                                       Wall thickness averaging about one-half as thick as normal
##     edema empty_displacement_volume fiscal_year focal_inflammation
## 1      NA                        NA        1996                 NA
## 2      NA                        NA        1996                 NA
## 3      NA                        NA        1996                 NA
## 4      NA                        NA        1996                 NA
## 5      NA                        NA        1996                 NA
## 6      NA                        NA        1998                 NA
## 7      NA                        NA        1998                 NA
## 8      NA                        NA        1998                 NA
## 9      NA                        NA        1998                 NA
## 10     NA                        NA        1998                 NA
## 11     NA                        NA        2000                 NA
## 12     NA                        NA        2000                 NA
## 13     NA                        NA        2000                 NA
## 14     NA                        NA        2000                 NA
## 15     NA                        NA        2000                 NA
## 16     NA                        NA        2002                 NA
## 17     NA                        NA        2002                 NA
## 18     NA                        NA        2002                 NA
## 19     NA                        NA        2002                 NA
## 20     NA                        NA        2002                 NA
## 21     NA                        NA        2004                 NA
## 22     NA                        NA        2004                 NA
## 23     NA                        NA        2004                 NA
## 24     NA                        NA        2004                 NA
## 25     NA                        NA        2004                 NA
## 26     NA                        NA        2006                 NA
## 27     NA                        NA        2006                 NA
## 28     NA                        NA        2006                 NA
## 29     NA                        NA        2006                 NA
## 30     NA                        NA        2006                 NA
## 31     NA                        NA        2010                 NA
## 32     NA                        NA        2010                 NA
## 33     NA                        NA        2010                 NA
## 34     NA                        NA        2010                 NA
## 35     NA                        NA        2010                 NA
## 36     NA                        NA        1998                 NA
## 37     NA                        NA        1998                 NA
## 38     NA                        NA        1998                 NA
## 39     NA                        NA        1998                 NA
## 40     NA                        NA        1998                 NA
## 41     NA                        NA        2000                 NA
## 42     NA                        NA        2000                 NA
## 43     NA                        NA        2000                 NA
## 44     NA                        NA        2000                 NA
## 45     NA                        NA        2000                 NA
## 46     NA                        NA        2006                 NA
## 47     NA                        NA        2006                 NA
## 48     NA                        NA        2006                 NA
## 49     NA                        NA        2006                 NA
## 50     NA                        NA        2006                 NA
## 51     NA                        NA        1996                 NA
## 52     NA                        NA        1996                 NA
## 53     NA                        NA        1996                 NA
## 54     NA                        NA        1996                 NA
## 55     NA                        NA        1996                 NA
## 56     NA                        NA        1998                 NA
## 57     NA                        NA        1998                 NA
## 58     NA                        NA        1998                 NA
## 59     NA                        NA        1998                 NA
## 60     NA                        NA        1998                 NA
## 61     NA                        NA        2000                 NA
## 62     NA                        NA        2000                 NA
## 63     NA                        NA        2000                 NA
## 64     NA                        NA        2000                 NA
## 65     NA                        NA        2000                 NA
## 66     NA                        NA        2004                 NA
## 67     NA                        NA        2004                 NA
## 68     NA                        NA        2004                 NA
## 69     NA                        NA        2004                 NA
## 70     NA                        NA        2004                 NA
## 71     NA                        NA        2006                 NA
## 72     NA                        NA        2006                 NA
## 73     NA                        NA        2006                 NA
## 74     NA                        NA        2006                 NA
## 75     NA                        NA        2006                 NA
## 76     NA                        NA        2010                 NA
## 77     NA                        NA        2010                 NA
## 78     NA                        NA        2010                 NA
## 79     NA                        NA        2010                 NA
## 80     NA                        NA        2010                 NA
## 81     NA                        NA        2000                 NA
## 82     NA                        NA        2000                 NA
## 83     NA                        NA        2000                 NA
## 84     NA                        NA        2000                 NA
## 85     NA                        NA        2000                 NA
## 86     NA                        NA        2004                 NA
## 87     NA                        NA        2004                 NA
## 88     NA                        NA        2004                 NA
## 89     NA                        NA        2004                 NA
## 90     NA                        NA        2004                 NA
## 91     NA                        NA        2004                 NA
## 92     NA                        NA        2004                 NA
## 93     NA                        NA        2004                 NA
## 94     NA                        NA        2004                 NA
## 95     NA                        NA        2004                 NA
## 96     NA                        NA        2006                 NA
## 97     NA                        NA        2006                 NA
## 98     NA                        NA        2006                 NA
## 99     NA                        NA        2006                 NA
## 100    NA                        NA        2006                 NA
## 101    NA                        NA        1996                 NA
## 102    NA                        NA        1996                 NA
## 103    NA                        NA        1996                 NA
## 104    NA                        NA        1996                 NA
## 105    NA                        NA        1996                 NA
## 106    NA                        NA        1998                 NA
## 107    NA                        NA        1998                 NA
## 108    NA                        NA        1998                 NA
## 109    NA                        NA        1998                 NA
## 110    NA                        NA        1998                 NA
## 111    NA                        NA        2000                 NA
## 112    NA                        NA        2000                 NA
## 113    NA                        NA        2000                 NA
## 114    NA                        NA        2000                 NA
## 115    NA                        NA        2000                 NA
## 116    NA                        NA        2004                 NA
## 117    NA                        NA        2004                 NA
## 118    NA                        NA        2004                 NA
## 119    NA                        NA        2004                 NA
## 120    NA                        NA        2004                 NA
## 121    NA                        NA        2006                 NA
## 122    NA                        NA        2006                 NA
## 123    NA                        NA        2006                 NA
## 124    NA                        NA        2006                 NA
## 125    NA                        NA        2006                 NA
## 126    NA                        NA        2006                 NA
## 127    NA                        NA        2006                 NA
## 128    NA                        NA        2006                 NA
## 129    NA                        NA        2006                 NA
## 130    NA                        NA        2006                 NA
## 131    NA                        NA        2006                 NA
## 132    NA                        NA        2006                 NA
## 133    NA                        NA        2006                 NA
## 134    NA                        NA        2006                 NA
## 135    NA                        NA        2006                 NA
## 136    NA                        NA        1996                 NA
## 137    NA                        NA        1996                 NA
## 138    NA                        NA        1996                 NA
## 139    NA                        NA        1996                 NA
## 140    NA                        NA        1996                 NA
## 141    NA                        NA        1998                 NA
## 142    NA                        NA        1998                 NA
## 143    NA                        NA        1998                 NA
## 144    NA                        NA        1998                 NA
## 145    NA                        NA        1998                 NA
## 146    NA                        NA        2000                 NA
## 147    NA                        NA        2000                 NA
## 148    NA                        NA        2000                 NA
## 149    NA                        NA        2000                 NA
## 150    NA                        NA        2000                 NA
## 151    NA                        NA        2004                 NA
## 152    NA                        NA        2004                 NA
## 153    NA                        NA        2004                 NA
## 154    NA                        NA        2004                 NA
## 155    NA                        NA        2004                 NA
## 156    NA                        NA        2006                 NA
## 157    NA                        NA        2006                 NA
## 158    NA                        NA        2006                 NA
## 159    NA                        NA        2006                 NA
## 160    NA                        NA        2006                 NA
## 161    NA                        NA        2010                 NA
## 162    NA                        NA        2010                 NA
## 163    NA                        NA        2010                 NA
## 164    NA                        NA        2010                 NA
## 165    NA                        NA        2010                 NA
## 166    NA                        NA        1996                 NA
## 167    NA                        NA        1996                 NA
## 168    NA                        NA        1996                 NA
## 169    NA                        NA        1996                 NA
## 170    NA                        NA        1996                 NA
## 171    NA                        NA        1998                 NA
## 172    NA                        NA        1998                 NA
## 173    NA                        NA        1998                 NA
## 174    NA                        NA        1998                 NA
## 175    NA                        NA        1998                 NA
## 176    NA                        NA        2000                 NA
## 177    NA                        NA        2000                 NA
## 178    NA                        NA        2000                 NA
## 179    NA                        NA        2000                 NA
## 180    NA                        NA        2000                 NA
## 181    NA                        NA        2004                 NA
## 182    NA                        NA        2004                 NA
## 183    NA                        NA        2004                 NA
## 184    NA                        NA        2004                 NA
## 185    NA                        NA        2004                 NA
## 186    NA                        NA        2006                 NA
## 187    NA                        NA        2006                 NA
## 188    NA                        NA        2006                 NA
## 189    NA                        NA        2006                 NA
## 190    NA                        NA        2006                 NA
## 191    NA                        NA        2010                 NA
## 192    NA                        NA        2010                 NA
## 193    NA                        NA        2010                 NA
## 194    NA                        NA        2010                 NA
## 195    NA                        NA        2010                 NA
## 196    NA                        NA        1996                 NA
## 197    NA                        NA        1996                 NA
## 198    NA                        NA        1996                 NA
## 199    NA                        NA        1996                 NA
## 200    NA                        NA        1996                 NA
## 201    NA                        NA        1998                 NA
## 202    NA                        NA        1998                 NA
## 203    NA                        NA        1998                 NA
## 204    NA                        NA        1998                 NA
## 205    NA                        NA        1998                 NA
## 206    NA                        NA        2000                 NA
## 207    NA                        NA        2000                 NA
## 208    NA                        NA        2000                 NA
## 209    NA                        NA        2000                 NA
## 210    NA                        NA        2000                 NA
## 211    NA                        NA        2002                 NA
## 212    NA                        NA        2002                 NA
## 213    NA                        NA        2002                 NA
## 214    NA                        NA        2002                 NA
## 215    NA                        NA        2002                 NA
## 216    NA                        NA        2004                 NA
## 217    NA                        NA        2004                 NA
## 218    NA                        NA        2004                 NA
## 219    NA                        NA        2004                 NA
## 220    NA                        NA        2004                 NA
## 221    NA                        NA        2004                 NA
## 222    NA                        NA        2004                 NA
## 223    NA                        NA        2004                 NA
## 224    NA                        NA        2004                 NA
## 225    NA                        NA        2004                 NA
## 226    NA                        NA        2006                 NA
## 227    NA                        NA        2006                 NA
## 228    NA                        NA        2006                 NA
## 229    NA                        NA        2006                 NA
## 230    NA                        NA        2006                 NA
## 231    NA                        NA        2010                 NA
## 232    NA                        NA        2010                 NA
## 233    NA                        NA        2010                 NA
## 234    NA                        NA        2010                 NA
## 235    NA                        NA        2010                 NA
## 236    NA                        NA        2003                 NA
## 237    NA                        NA        2003                 NA
## 238    NA                        NA        2003                 NA
## 239    NA                        NA        2003                 NA
## 240    NA                        NA        2003                 NA
## 241    NA                        NA        2003                 NA
## 242    NA                        NA        2003                 NA
## 243    NA                        NA        2003                 NA
## 244    NA                        NA        2003                 NA
## 245    NA                        NA        2003                 NA
## 246    NA                        NA        2003                 NA
## 247    NA                        NA        2003                 NA
## 248    NA                        NA        2003                 NA
## 249    NA                        NA        2003                 NA
## 250    NA                        NA        2003                 NA
## 251    NA                        NA        2003                 NA
## 252    NA                        NA        2003                 NA
## 253    NA                        NA        2003                 NA
## 254    NA                        NA        2003                 NA
## 255    NA                        NA        2003                 NA
## 256    NA                        NA        2003                 NA
## 257    NA                        NA        2003                 NA
## 258    NA                        NA        2003                 NA
## 259    NA                        NA        2003                 NA
## 260    NA                        NA        2003                 NA
##     focal_necrosis full_displacement_volume general_location
## 1                0                       NA        Green Bay
## 2                0                       NA        Green Bay
## 3                0                       NA        Green Bay
## 4                0                       NA        Green Bay
## 5                0                       NA        Green Bay
## 6                0                       NA        Green Bay
## 7                0                       NA        Green Bay
## 8                0                       NA        Green Bay
## 9                0                       NA        Green Bay
## 10               0                       NA        Green Bay
## 11               0                       NA        Green Bay
## 12               0                       NA        Green Bay
## 13               0                       NA        Green Bay
## 14               0                       NA        Green Bay
## 15               0                       NA        Green Bay
## 16               0                       NA        Green Bay
## 17               0                       NA        Green Bay
## 18               0                       NA        Green Bay
## 19               0                       NA        Green Bay
## 20               0                       NA        Green Bay
## 21               0                       NA        Green Bay
## 22               0                       NA        Green Bay
## 23               0                       NA        Green Bay
## 24               0                       NA        Green Bay
## 25               0                       NA        Green Bay
## 26               0                       NA        Green Bay
## 27               0                       NA        Green Bay
## 28               0                       NA        Green Bay
## 29               0                       NA        Green Bay
## 30               0                       NA        Green Bay
## 31               0                       NA        Green Bay
## 32               0                       NA        Green Bay
## 33               0                       NA        Green Bay
## 34               0                       NA        Green Bay
## 35               0                       NA        Green Bay
## 36               0                       NA    Lake Michigan
## 37               0                       NA    Lake Michigan
## 38               0                       NA    Lake Michigan
## 39               0                       NA    Lake Michigan
## 40               0                       NA    Lake Michigan
## 41               0                       NA    Lake Michigan
## 42               0                       NA    Lake Michigan
## 43               0                       NA    Lake Michigan
## 44               0                       NA    Lake Michigan
## 45               0                       NA    Lake Michigan
## 46               0                       NA    Lake Michigan
## 47               0                       NA    Lake Michigan
## 48               0                       NA    Lake Michigan
## 49               0                       NA    Lake Michigan
## 50               0                       NA    Lake Michigan
## 51               0                       NA    Lake Michigan
## 52               0                       NA    Lake Michigan
## 53               0                       NA    Lake Michigan
## 54               0                       NA    Lake Michigan
## 55               0                       NA    Lake Michigan
## 56               0                       NA    Lake Michigan
## 57               0                       NA    Lake Michigan
## 58               0                       NA    Lake Michigan
## 59               0                       NA    Lake Michigan
## 60               0                       NA    Lake Michigan
## 61               0                       NA    Lake Michigan
## 62               0                       NA    Lake Michigan
## 63               0                       NA    Lake Michigan
## 64               0                       NA    Lake Michigan
## 65               0                       NA    Lake Michigan
## 66               0                       NA    Lake Michigan
## 67               0                       NA    Lake Michigan
## 68               0                       NA    Lake Michigan
## 69               0                       NA    Lake Michigan
## 70               0                       NA    Lake Michigan
## 71               0                       NA    Lake Michigan
## 72               0                       NA    Lake Michigan
## 73               0                       NA    Lake Michigan
## 74               0                       NA    Lake Michigan
## 75               0                       NA    Lake Michigan
## 76               0                       NA    Lake Michigan
## 77               0                       NA    Lake Michigan
## 78               0                       NA    Lake Michigan
## 79               0                       NA    Lake Michigan
## 80               0                       NA    Lake Michigan
## 81               0                       NA    Lake Michigan
## 82               0                       NA    Lake Michigan
## 83               0                       NA    Lake Michigan
## 84               0                       NA    Lake Michigan
## 85               0                       NA    Lake Michigan
## 86               0                       NA    Lake Michigan
## 87               0                       NA    Lake Michigan
## 88               0                       NA    Lake Michigan
## 89               0                       NA    Lake Michigan
## 90               0                       NA    Lake Michigan
## 91               0                       NA    Lake Michigan
## 92               0                       NA    Lake Michigan
## 93               0                       NA    Lake Michigan
## 94               0                       NA    Lake Michigan
## 95               0                       NA    Lake Michigan
## 96               0                       NA    Lake Michigan
## 97               0                       NA    Lake Michigan
## 98               0                       NA    Lake Michigan
## 99               0                       NA    Lake Michigan
## 100              0                       NA    Lake Michigan
## 101              0                       NA    Lake Michigan
## 102              0                       NA    Lake Michigan
## 103              0                       NA    Lake Michigan
## 104              0                       NA    Lake Michigan
## 105              0                       NA    Lake Michigan
## 106              0                       NA    Lake Michigan
## 107              0                       NA    Lake Michigan
## 108              0                       NA    Lake Michigan
## 109              0                       NA    Lake Michigan
## 110              0                       NA    Lake Michigan
## 111              0                       NA    Lake Michigan
## 112              2                       NA    Lake Michigan
## 113              0                       NA    Lake Michigan
## 114              0                       NA    Lake Michigan
## 115              0                       NA    Lake Michigan
## 116              0                       NA    Lake Michigan
## 117              0                       NA    Lake Michigan
## 118              0                       NA    Lake Michigan
## 119              0                       NA    Lake Michigan
## 120              0                       NA    Lake Michigan
## 121              0                       NA    Lake Michigan
## 122              0                       NA    Lake Michigan
## 123              0                       NA    Lake Michigan
## 124              0                       NA    Lake Michigan
## 125              0                       NA    Lake Michigan
## 126              0                       NA    Lake Michigan
## 127              0                       NA    Lake Michigan
## 128              0                       NA    Lake Michigan
## 129              0                       NA    Lake Michigan
## 130              0                       NA    Lake Michigan
## 131              0                       NA    Lake Michigan
## 132              0                       NA    Lake Michigan
## 133              0                       NA    Lake Michigan
## 134              0                       NA    Lake Michigan
## 135              0                       NA    Lake Michigan
## 136              0                       NA    Lake Michigan
## 137              0                       NA    Lake Michigan
## 138              0                       NA    Lake Michigan
## 139              0                       NA    Lake Michigan
## 140              0                       NA    Lake Michigan
## 141              0                       NA    Lake Michigan
## 142              0                       NA    Lake Michigan
## 143              0                       NA    Lake Michigan
## 144              0                       NA    Lake Michigan
## 145              0                       NA    Lake Michigan
## 146              0                       NA    Lake Michigan
## 147              0                       NA    Lake Michigan
## 148              0                       NA    Lake Michigan
## 149              0                       NA    Lake Michigan
## 150              0                       NA    Lake Michigan
## 151              0                       NA    Lake Michigan
## 152              0                       NA    Lake Michigan
## 153              0                       NA    Lake Michigan
## 154              0                       NA    Lake Michigan
## 155              0                       NA    Lake Michigan
## 156              0                       NA    Lake Michigan
## 157              0                       NA    Lake Michigan
## 158              0                       NA    Lake Michigan
## 159              0                       NA    Lake Michigan
## 160              0                       NA    Lake Michigan
## 161              0                       NA    Lake Michigan
## 162              0                       NA    Lake Michigan
## 163              0                       NA    Lake Michigan
## 164              0                       NA    Lake Michigan
## 165              0                       NA    Lake Michigan
## 166              0                       NA    Lake Michigan
## 167              0                       NA    Lake Michigan
## 168              0                       NA    Lake Michigan
## 169              0                       NA    Lake Michigan
## 170              0                       NA    Lake Michigan
## 171              0                       NA    Lake Michigan
## 172              0                       NA    Lake Michigan
## 173              0                       NA    Lake Michigan
## 174              0                       NA    Lake Michigan
## 175              0                       NA    Lake Michigan
## 176              0                       NA    Lake Michigan
## 177              0                       NA    Lake Michigan
## 178              0                       NA    Lake Michigan
## 179              0                       NA    Lake Michigan
## 180              0                       NA    Lake Michigan
## 181              0                       NA    Lake Michigan
## 182              0                       NA    Lake Michigan
## 183              0                       NA    Lake Michigan
## 184              0                       NA    Lake Michigan
## 185              0                       NA    Lake Michigan
## 186              0                       NA    Lake Michigan
## 187              0                       NA    Lake Michigan
## 188              0                       NA    Lake Michigan
## 189              0                       NA    Lake Michigan
## 190              0                       NA    Lake Michigan
## 191              0                       NA    Lake Michigan
## 192              0                       NA    Lake Michigan
## 193              0                       NA    Lake Michigan
## 194              0                       NA    Lake Michigan
## 195              0                       NA    Lake Michigan
## 196              0                       NA     Traverse Bay
## 197              0                       NA     Traverse Bay
## 198              0                       NA     Traverse Bay
## 199              0                       NA     Traverse Bay
## 200              0                       NA     Traverse Bay
## 201              0                       NA     Traverse Bay
## 202              0                       NA     Traverse Bay
## 203              0                       NA     Traverse Bay
## 204              0                       NA     Traverse Bay
## 205              0                       NA     Traverse Bay
## 206              0                       NA     Traverse Bay
## 207              0                       NA     Traverse Bay
## 208              0                       NA     Traverse Bay
## 209              0                       NA     Traverse Bay
## 210              0                       NA     Traverse Bay
## 211              0                       NA     Traverse Bay
## 212              0                       NA     Traverse Bay
## 213              0                       NA     Traverse Bay
## 214              0                       NA     Traverse Bay
## 215              0                       NA     Traverse Bay
## 216              0                       NA     Traverse Bay
## 217              0                       NA     Traverse Bay
## 218              0                       NA     Traverse Bay
## 219              0                       NA     Traverse Bay
## 220              0                       NA     Traverse Bay
## 221              0                       NA     Traverse Bay
## 222              0                       NA     Traverse Bay
## 223              0                       NA     Traverse Bay
## 224              0                       NA     Traverse Bay
## 225              0                       NA     Traverse Bay
## 226              0                       NA     Traverse Bay
## 227              0                       NA     Traverse Bay
## 228              0                       NA     Traverse Bay
## 229              0                       NA     Traverse Bay
## 230              0                       NA     Traverse Bay
## 231              0                       NA     Traverse Bay
## 232              0                       NA     Traverse Bay
## 233              0                       NA     Traverse Bay
## 234              0                       NA     Traverse Bay
## 235              0                       NA     Traverse Bay
## 236              0                       NA    Lake Michigan
## 237              0                       NA    Lake Michigan
## 238              0                       NA    Lake Michigan
## 239              0                       NA    Lake Michigan
## 240              0                       NA    Lake Michigan
## 241              0                       NA    Lake Michigan
## 242              0                       NA    Lake Michigan
## 243              0                       NA    Lake Michigan
## 244              0                       NA    Lake Michigan
## 245              0                       NA    Lake Michigan
## 246              0                       NA    Lake Michigan
## 247              0                       NA    Lake Michigan
## 248              0                       NA    Lake Michigan
## 249              0                       NA    Lake Michigan
## 250              0                       NA    Lake Michigan
## 251              0                       NA    Lake Michigan
## 252              0                       NA    Lake Michigan
## 253              0                       NA    Lake Michigan
## 254              0                       NA    Lake Michigan
## 255              0                       NA    Lake Michigan
## 256              0                       NA    Lake Michigan
## 257              0                       NA    Lake Michigan
## 258              0                       NA    Lake Michigan
## 259              0                       NA    Lake Michigan
## 260              0                       NA    Lake Michigan
##     gonad_subsample_wet_weight gonadal_index
## 1                           NA            D1
## 2                           NA            R0
## 3                           NA            R0
## 4                           NA            S2
## 5                           NA            D2
## 6                           NA            D1
## 7                           NA            S1
## 8                           NA            R0
## 9                           NA            S1
## 10                          NA            S1
## 11                          NA            D1
## 12                          NA            R0
## 13                          NA            D1
## 14                          NA            R0
## 15                          NA            D1
## 16                          NA            R0
## 17                          NA            R0
## 18                          NA            R0
## 19                          NA            R0
## 20                          NA            R0
## 21                          NA            R0
## 22                          NA            R0
## 23                          NA            R0
## 24                          NA            S1
## 25                          NA            R0
## 26                          NA            R0
## 27                          NA            S1
## 28                          NA            R0
## 29                          NA            R0
## 30                          NA            S1
## 31                          NA            D2
## 32                          NA            R0
## 33                          NA            D2
## 34                          NA            D4
## 35                          NA            R0
## 36                          NA            S3
## 37                          NA            R0
## 38                          NA            S1
## 39                          NA            S4
## 40                          NA            D1
## 41                          NA            R0
## 42                          NA            D2
## 43                          NA            R0
## 44                          NA            S2
## 45                          NA            S3
## 46                          NA            D4
## 47                          NA            D3
## 48                          NA            S2
## 49                          NA            D3
## 50                          NA            S3
## 51                          NA            S4
## 52                          NA            S4
## 53                          NA            R5
## 54                          NA            S4
## 55                          NA            S3
## 56                          NA            S3
## 57                          NA            D2
## 58                          NA            S2
## 59                          NA            S4
## 60                          NA            D1
## 61                          NA            S3
## 62                          NA            S4
## 63                          NA            S4
## 64                          NA            S1
## 65                          NA            D1
## 66                          NA            R0
## 67                          NA            S2
## 68                          NA            S1
## 69                          NA            S2
## 70                          NA            S2
## 71                          NA            S3
## 72                          NA            S3
## 73                          NA            D4
## 74                          NA            D4
## 75                          NA            D4
## 76                          NA            S1
## 77                          NA            D2
## 78                          NA            D2
## 79                          NA            R0
## 80                          NA            D2
## 81                          NA            R0
## 82                          NA            D1
## 83                          NA            R0
## 84                          NA            R0
## 85                          NA            S1
## 86                          NA            D3
## 87                          NA            S3
## 88                          NA            S3
## 89                          NA            S3
## 90                          NA            D2
## 91                          NA            S1
## 92                          NA            R0
## 93                          NA            S1
## 94                          NA            D3
## 95                          NA            D4
## 96                          NA            S3
## 97                          NA            D3
## 98                          NA            S3
## 99                          NA            S2
## 100                         NA            D3
## 101                         NA            S4
## 102                         NA            S2
## 103                         NA            S3
## 104                         NA            S3
## 105                         NA            S4
## 106                         NA            D2
## 107                         NA            D2
## 108                         NA            S1
## 109                         NA            S1
## 110                         NA            D1
## 111                         NA            D1
## 112                         NA            R0
## 113                         NA            R5
## 114                         NA            S2
## 115                         NA            R0
## 116                         NA            S1
## 117                         NA            S1
## 118                         NA            D3
## 119                         NA            R0
## 120                         NA            S1
## 121                         NA            S1
## 122                         NA            R0
## 123                         NA            R0
## 124                         NA            S1
## 125                         NA            S1
## 126                         NA            D4
## 127                         NA            S3
## 128                         NA            S3
## 129                         NA            D4
## 130                         NA            S3
## 131                         NA            S2
## 132                         NA            S2
## 133                         NA            R0
## 134                         NA            S4
## 135                         NA            R0
## 136                         NA            D2
## 137                         NA            D4
## 138                         NA            D3
## 139                         NA            R5
## 140                         NA            S2
## 141                         NA            S3
## 142                         NA            S4
## 143                         NA            S4
## 144                         NA            S1
## 145                         NA            S4
## 146                         NA            S4
## 147                         NA            R0
## 148                         NA            S3
## 149                         NA            S3
## 150                         NA            S1
## 151                         NA            S2
## 152                         NA            S1
## 153                         NA            S1
## 154                         NA            S1
## 155                         NA            R0
## 156                         NA            S4
## 157                         NA            S4
## 158                         NA            S3
## 159                         NA            S4
## 160                         NA            D4
## 161                         NA            S4
## 162                         NA            S3
## 163                         NA            R5
## 164                         NA            R5
## 165                         NA            S4
## 166                         NA            R0
## 167                         NA            S3
## 168                         NA            D2
## 169                         NA            R0
## 170                         NA            D1
## 171                         NA            R0
## 172                         NA            S1
## 173                         NA            R0
## 174                         NA            R0
## 175                         NA            D1
## 176                         NA            D1
## 177                         NA            R0
## 178                         NA            R0
## 179                         NA            R0
## 180                         NA            D1
## 181                         NA            S1
## 182                         NA            R0
## 183                         NA            R0
## 184                         NA            S1
## 185                         NA            R0
## 186                         NA            D3
## 187                         NA            S1
## 188                         NA            S3
## 189                         NA            S3
## 190                         NA            D3
## 191                         NA            D3
## 192                         NA            D3
## 193                         NA            D3
## 194                         NA            D2
## 195                         NA            D2
## 196                         NA            R0
## 197                         NA            D1
## 198                         NA            D1
## 199                         NA            R0
## 200                         NA            D1
## 201                         NA            D1
## 202                         NA            R0
## 203                         NA            R0
## 204                         NA            D1
## 205                         NA            S1
## 206                         NA            D1
## 207                         NA            S2
## 208                         NA            D1
## 209                         NA            R0
## 210                         NA            R0
## 211                         NA            R0
## 212                         NA            S2
## 213                         NA            D1
## 214                         NA            R0
## 215                         NA            R0
## 216                         NA            S3
## 217                         NA            D4
## 218                         NA            D4
## 219                         NA            S2
## 220                         NA            S3
## 221                         NA            S1
## 222                         NA            R0
## 223                         NA            S1
## 224                         NA            D2
## 225                         NA            S3
## 226                         NA            S2
## 227                         NA            S2
## 228                         NA            D1
## 229                         NA            R0
## 230                         NA            D3
## 231                         NA            D3
## 232                         NA            S1
## 233                         NA            R0
## 234                         NA            D2
## 235                         NA            D3
## 236                         NA            S3
## 237                         NA            D3
## 238                         NA            S2
## 239                         NA            S1
## 240                         NA            S2
## 241                         NA            R0
## 242                         NA            D1
## 243                         NA            R0
## 244                         NA            S2
## 245                         NA            R0
## 246                         NA            S3
## 247                         NA            R0
## 248                         NA            S1
## 249                         NA            S1
## 250                         NA            S2
## 251                         NA            S3
## 252                         NA            D4
## 253                         NA            S3
## 254                         NA            S1
## 255                         NA            R0
## 256                         NA            S1
## 257                         NA            R0
## 258                         NA            S1
## 259                         NA            S1
## 260                         NA            R0
##                                                  gonadal_index_description
## 1                         Gametogenesis has begun, no ripe gametes visible
## 2                                                                 Inactive
## 3                                                                 Inactive
## 4                 Gonadal Area reduced, follicles 1/3 full of ripe gametes
## 5                  Ripe gametes present, gonad developed to 1/3 final size
## 6                         Gametogenesis has begun, no ripe gametes visible
## 7                                             Only residual gametes remain
## 8                                                                 Inactive
## 9                                             Only residual gametes remain
## 10                                            Only residual gametes remain
## 11                        Gametogenesis has begun, no ripe gametes visible
## 12                                                                Inactive
## 13                        Gametogenesis has begun, no ripe gametes visible
## 14                                                                Inactive
## 15                        Gametogenesis has begun, no ripe gametes visible
## 16                                                                Inactive
## 17                                                                Inactive
## 18                                                                Inactive
## 19                                                                Inactive
## 20                                                                Inactive
## 21                                                                Inactive
## 22                                                                Inactive
## 23                                                                Inactive
## 24                                            Only residual gametes remain
## 25                                                                Inactive
## 26                                                                Inactive
## 27                                            Only residual gametes remain
## 28                                                                Inactive
## 29                                                                Inactive
## 30                                            Only residual gametes remain
## 31                 Ripe gametes present, gonad developed to 1/3 final size
## 32                                                                Inactive
## 33                 Ripe gametes present, gonad developed to 1/3 final size
## 34  Gametogenesis still progressing, follicles contain mainly ripe gametes
## 35                                                                Inactive
## 36                                                  Gonad about half empty
## 37                                                                Inactive
## 38                                            Only residual gametes remain
## 39                                               Active emission has begun
## 40                        Gametogenesis has begun, no ripe gametes visible
## 41                                                                Inactive
## 42                 Ripe gametes present, gonad developed to 1/3 final size
## 43                                                                Inactive
## 44                Gonadal Area reduced, follicles 1/3 full of ripe gametes
## 45                                                  Gonad about half empty
## 46  Gametogenesis still progressing, follicles contain mainly ripe gametes
## 47                                                 Gonad is half developed
## 48                Gonadal Area reduced, follicles 1/3 full of ripe gametes
## 49                                                 Gonad is half developed
## 50                                                  Gonad about half empty
## 51                                               Active emission has begun
## 52                                               Active emission has begun
## 53                                                        Gonad fully ripe
## 54                                               Active emission has begun
## 55                                                  Gonad about half empty
## 56                                                  Gonad about half empty
## 57                 Ripe gametes present, gonad developed to 1/3 final size
## 58                Gonadal Area reduced, follicles 1/3 full of ripe gametes
## 59                                               Active emission has begun
## 60                        Gametogenesis has begun, no ripe gametes visible
## 61                                                  Gonad about half empty
## 62                                               Active emission has begun
## 63                                               Active emission has begun
## 64                                            Only residual gametes remain
## 65                        Gametogenesis has begun, no ripe gametes visible
## 66                                                                Inactive
## 67                Gonadal Area reduced, follicles 1/3 full of ripe gametes
## 68                                            Only residual gametes remain
## 69                Gonadal Area reduced, follicles 1/3 full of ripe gametes
## 70                Gonadal Area reduced, follicles 1/3 full of ripe gametes
## 71                                                  Gonad about half empty
## 72                                                  Gonad about half empty
## 73  Gametogenesis still progressing, follicles contain mainly ripe gametes
## 74  Gametogenesis still progressing, follicles contain mainly ripe gametes
## 75  Gametogenesis still progressing, follicles contain mainly ripe gametes
## 76                                            Only residual gametes remain
## 77                 Ripe gametes present, gonad developed to 1/3 final size
## 78                 Ripe gametes present, gonad developed to 1/3 final size
## 79                                                                Inactive
## 80                 Ripe gametes present, gonad developed to 1/3 final size
## 81                                                                Inactive
## 82                        Gametogenesis has begun, no ripe gametes visible
## 83                                                                Inactive
## 84                                                                Inactive
## 85                                            Only residual gametes remain
## 86                                                 Gonad is half developed
## 87                                                  Gonad about half empty
## 88                                                  Gonad about half empty
## 89                                                  Gonad about half empty
## 90                 Ripe gametes present, gonad developed to 1/3 final size
## 91                                            Only residual gametes remain
## 92                                                                Inactive
## 93                                            Only residual gametes remain
## 94                                                 Gonad is half developed
## 95  Gametogenesis still progressing, follicles contain mainly ripe gametes
## 96                                                  Gonad about half empty
## 97                                                 Gonad is half developed
## 98                                                  Gonad about half empty
## 99                Gonadal Area reduced, follicles 1/3 full of ripe gametes
## 100                                                Gonad is half developed
## 101                                              Active emission has begun
## 102               Gonadal Area reduced, follicles 1/3 full of ripe gametes
## 103                                                 Gonad about half empty
## 104                                                 Gonad about half empty
## 105                                              Active emission has begun
## 106                Ripe gametes present, gonad developed to 1/3 final size
## 107                Ripe gametes present, gonad developed to 1/3 final size
## 108                                           Only residual gametes remain
## 109                                           Only residual gametes remain
## 110                       Gametogenesis has begun, no ripe gametes visible
## 111                       Gametogenesis has begun, no ripe gametes visible
## 112                                                               Inactive
## 113                                                       Gonad fully ripe
## 114               Gonadal Area reduced, follicles 1/3 full of ripe gametes
## 115                                                               Inactive
## 116                                           Only residual gametes remain
## 117                                           Only residual gametes remain
## 118                                                Gonad is half developed
## 119                                                               Inactive
## 120                                           Only residual gametes remain
## 121                                           Only residual gametes remain
## 122                                                               Inactive
## 123                                                               Inactive
## 124                                           Only residual gametes remain
## 125                                           Only residual gametes remain
## 126 Gametogenesis still progressing, follicles contain mainly ripe gametes
## 127                                                 Gonad about half empty
## 128                                                 Gonad about half empty
## 129 Gametogenesis still progressing, follicles contain mainly ripe gametes
## 130                                                 Gonad about half empty
## 131               Gonadal Area reduced, follicles 1/3 full of ripe gametes
## 132               Gonadal Area reduced, follicles 1/3 full of ripe gametes
## 133                                                               Inactive
## 134                                              Active emission has begun
## 135                                                               Inactive
## 136                Ripe gametes present, gonad developed to 1/3 final size
## 137 Gametogenesis still progressing, follicles contain mainly ripe gametes
## 138                                                Gonad is half developed
## 139                                                       Gonad fully ripe
## 140               Gonadal Area reduced, follicles 1/3 full of ripe gametes
## 141                                                 Gonad about half empty
## 142                                              Active emission has begun
## 143                                              Active emission has begun
## 144                                           Only residual gametes remain
## 145                                              Active emission has begun
## 146                                              Active emission has begun
## 147                                                               Inactive
## 148                                                 Gonad about half empty
## 149                                                 Gonad about half empty
## 150                                           Only residual gametes remain
## 151               Gonadal Area reduced, follicles 1/3 full of ripe gametes
## 152                                           Only residual gametes remain
## 153                                           Only residual gametes remain
## 154                                           Only residual gametes remain
## 155                                                               Inactive
## 156                                              Active emission has begun
## 157                                              Active emission has begun
## 158                                                 Gonad about half empty
## 159                                              Active emission has begun
## 160 Gametogenesis still progressing, follicles contain mainly ripe gametes
## 161                                              Active emission has begun
## 162                                                 Gonad about half empty
## 163                                                       Gonad fully ripe
## 164                                                       Gonad fully ripe
## 165                                              Active emission has begun
## 166                                                               Inactive
## 167                                                 Gonad about half empty
## 168                Ripe gametes present, gonad developed to 1/3 final size
## 169                                                               Inactive
## 170                       Gametogenesis has begun, no ripe gametes visible
## 171                                                               Inactive
## 172                                           Only residual gametes remain
## 173                                                               Inactive
## 174                                                               Inactive
## 175                       Gametogenesis has begun, no ripe gametes visible
## 176                       Gametogenesis has begun, no ripe gametes visible
## 177                                                               Inactive
## 178                                                               Inactive
## 179                                                               Inactive
## 180                       Gametogenesis has begun, no ripe gametes visible
## 181                                           Only residual gametes remain
## 182                                                               Inactive
## 183                                                               Inactive
## 184                                           Only residual gametes remain
## 185                                                               Inactive
## 186                                                Gonad is half developed
## 187                                           Only residual gametes remain
## 188                                                 Gonad about half empty
## 189                                                 Gonad about half empty
## 190                                                Gonad is half developed
## 191                                                Gonad is half developed
## 192                                                Gonad is half developed
## 193                                                Gonad is half developed
## 194                Ripe gametes present, gonad developed to 1/3 final size
## 195                Ripe gametes present, gonad developed to 1/3 final size
## 196                                                               Inactive
## 197                       Gametogenesis has begun, no ripe gametes visible
## 198                       Gametogenesis has begun, no ripe gametes visible
## 199                                                               Inactive
## 200                       Gametogenesis has begun, no ripe gametes visible
## 201                       Gametogenesis has begun, no ripe gametes visible
## 202                                                               Inactive
## 203                                                               Inactive
## 204                       Gametogenesis has begun, no ripe gametes visible
## 205                                           Only residual gametes remain
## 206                       Gametogenesis has begun, no ripe gametes visible
## 207               Gonadal Area reduced, follicles 1/3 full of ripe gametes
## 208                       Gametogenesis has begun, no ripe gametes visible
## 209                                                               Inactive
## 210                                                               Inactive
## 211                                                               Inactive
## 212               Gonadal Area reduced, follicles 1/3 full of ripe gametes
## 213                       Gametogenesis has begun, no ripe gametes visible
## 214                                                               Inactive
## 215                                                               Inactive
## 216                                                 Gonad about half empty
## 217 Gametogenesis still progressing, follicles contain mainly ripe gametes
## 218 Gametogenesis still progressing, follicles contain mainly ripe gametes
## 219               Gonadal Area reduced, follicles 1/3 full of ripe gametes
## 220                                                 Gonad about half empty
## 221                                           Only residual gametes remain
## 222                                                               Inactive
## 223                                           Only residual gametes remain
## 224                Ripe gametes present, gonad developed to 1/3 final size
## 225                                                 Gonad about half empty
## 226               Gonadal Area reduced, follicles 1/3 full of ripe gametes
## 227               Gonadal Area reduced, follicles 1/3 full of ripe gametes
## 228                       Gametogenesis has begun, no ripe gametes visible
## 229                                                               Inactive
## 230                                                Gonad is half developed
## 231                                                Gonad is half developed
## 232                                           Only residual gametes remain
## 233                                                               Inactive
## 234                Ripe gametes present, gonad developed to 1/3 final size
## 235                                                Gonad is half developed
## 236                                                 Gonad about half empty
## 237                                                Gonad is half developed
## 238               Gonadal Area reduced, follicles 1/3 full of ripe gametes
## 239                                           Only residual gametes remain
## 240               Gonadal Area reduced, follicles 1/3 full of ripe gametes
## 241                                                               Inactive
## 242                       Gametogenesis has begun, no ripe gametes visible
## 243                                                               Inactive
## 244               Gonadal Area reduced, follicles 1/3 full of ripe gametes
## 245                                                               Inactive
## 246                                                 Gonad about half empty
## 247                                                               Inactive
## 248                                           Only residual gametes remain
## 249                                           Only residual gametes remain
## 250               Gonadal Area reduced, follicles 1/3 full of ripe gametes
## 251                                                 Gonad about half empty
## 252 Gametogenesis still progressing, follicles contain mainly ripe gametes
## 253                                                 Gonad about half empty
## 254                                           Only residual gametes remain
## 255                                                               Inactive
## 256                                           Only residual gametes remain
## 257                                                               Inactive
## 258                                           Only residual gametes remain
## 259                                           Only residual gametes remain
## 260                                                               Inactive
##     hydra_gill latitude length longitude metacercaria
## 1           NA 44.63700    2.2 -87.80820           NA
## 2           NA 44.63700    2.3 -87.80820           NA
## 3           NA 44.63700    2.1 -87.80820           NA
## 4           NA 44.63700    2.2 -87.80820           NA
## 5           NA 44.63700    2.1 -87.80820           NA
## 6           NA 44.63700    2.5 -87.80820           NA
## 7           NA 44.63700    2.4 -87.80820           NA
## 8           NA 44.63700    2.3 -87.80820           NA
## 9           NA 44.63700    2.3 -87.80820           NA
## 10          NA 44.63700    2.4 -87.80820           NA
## 11          NA 44.63700    2.2 -87.80820           NA
## 12          NA 44.63700    2.3 -87.80820           NA
## 13          NA 44.63700    2.0 -87.80820           NA
## 14          NA 44.63700    2.2 -87.80820           NA
## 15          NA 44.63700    2.3 -87.80820           NA
## 16          NA 44.63700    2.2 -87.80820           NA
## 17          NA 44.63700    1.9 -87.80820           NA
## 18          NA 44.63700    2.1 -87.80820           NA
## 19          NA 44.63700    2.1 -87.80820           NA
## 20          NA 44.63700    1.9 -87.80820           NA
## 21          NA 44.63700    2.4 -87.80820           NA
## 22          NA 44.63700    2.1 -87.80820           NA
## 23          NA 44.63700    2.2 -87.80820           NA
## 24          NA 44.63700    2.0 -87.80820           NA
## 25          NA 44.63700    2.2 -87.80820           NA
## 26          NA 44.63700    2.3 -87.80820           NA
## 27          NA 44.63700    2.3 -87.80820           NA
## 28          NA 44.63700    2.1 -87.80820           NA
## 29          NA 44.63700    2.5 -87.80820           NA
## 30          NA 44.63700    2.2 -87.80820           NA
## 31          NA 44.63685    2.6 -87.80774           NA
## 32          NA 44.63685    2.2 -87.80774           NA
## 33          NA 44.63685    2.1 -87.80774           NA
## 34          NA 44.63685    2.0 -87.80774           NA
## 35          NA 44.63685    2.1 -87.80774           NA
## 36          NA 41.72720    2.3 -87.49500           NA
## 37          NA 41.72720    2.5 -87.49500           NA
## 38          NA 41.72720    2.5 -87.49500           NA
## 39          NA 41.72720    2.4 -87.49500           NA
## 40          NA 41.72720    2.6 -87.49500           NA
## 41          NA 41.72720    1.9 -87.49500           NA
## 42          NA 41.72720    2.0 -87.49500           NA
## 43          NA 41.72720    1.5 -87.49500           NA
## 44          NA 41.72720    1.5 -87.49500           NA
## 45          NA 41.72720    1.7 -87.49500           NA
## 46          NA 41.72720    2.3 -87.49500           NA
## 47          NA 41.72720    2.2 -87.49500           NA
## 48          NA 41.72720    2.3 -87.49500           NA
## 49          NA 41.72720    2.2 -87.49500           NA
## 50          NA 41.72720    2.3 -87.49500           NA
## 51          NA 42.77320    2.7 -86.21500           NA
## 52          NA 42.77320    2.8 -86.21500           NA
## 53          NA 42.77320    2.2 -86.21500           NA
## 54          NA 42.77320    2.3 -86.21500           NA
## 55          NA 42.77320    2.3 -86.21500           NA
## 56          NA 42.77320    2.5 -86.21500           NA
## 57          NA 42.77320    2.3 -86.21500           NA
## 58          NA 42.77320    2.7 -86.21500           NA
## 59          NA 42.77320    2.3 -86.21500           NA
## 60          NA 42.77320    2.4 -86.21500           NA
## 61          NA 42.77320    2.5 -86.21500           NA
## 62          NA 42.77320    2.5 -86.21500           NA
## 63          NA 42.77320    2.6 -86.21500           NA
## 64          NA 42.77320    2.7 -86.21500           NA
## 65          NA 42.77320    2.4 -86.21500           NA
## 66          NA 42.77320    2.6 -86.21500           NA
## 67          NA 42.77320    3.0 -86.21500           NA
## 68          NA 42.77320    2.9 -86.21500           NA
## 69          NA 42.77320    2.7 -86.21500           NA
## 70          NA 42.77320    2.6 -86.21500           NA
## 71          NA 42.77320    2.6 -86.21500           NA
## 72          NA 42.77320    1.9 -86.21500           NA
## 73          NA 42.77320    2.5 -86.21500           NA
## 74          NA 42.77320    2.5 -86.21500           NA
## 75          NA 42.77320    2.2 -86.21500           NA
## 76          NA 42.77392    2.8 -86.21409           NA
## 77          NA 42.77392    2.4 -86.21409           NA
## 78          NA 42.77392    2.3 -86.21409           NA
## 79          NA 42.77392    2.5 -86.21409           NA
## 80          NA 42.77392    2.1 -86.21409           NA
## 81          NA 41.69870    2.0 -87.50830           NA
## 82          NA 41.69870    2.1 -87.50830           NA
## 83          NA 41.69870    2.1 -87.50830           NA
## 84          NA 41.69870    2.2 -87.50830           NA
## 85          NA 41.69870    2.2 -87.50830           NA
## 86          NA 41.69870    1.9 -87.50830           NA
## 87          NA 41.69870    2.1 -87.50830           NA
## 88          NA 41.69870    1.7 -87.50830           NA
## 89          NA 41.69870    2.1 -87.50830           NA
## 90          NA 41.69870    2.1 -87.50830           NA
## 91          NA 41.69870    2.5 -87.50830           NA
## 92          NA 41.69870    2.3 -87.50830           NA
## 93          NA 41.69870    2.4 -87.50830           NA
## 94          NA 41.69870    1.8 -87.50830           NA
## 95          NA 41.69870    1.8 -87.50830           NA
## 96          NA 41.69870    2.4 -87.50830           NA
## 97          NA 41.69870    2.4 -87.50830           NA
## 98          NA 41.69870    2.5 -87.50830           NA
## 99          NA 41.69870    2.3 -87.50830           NA
## 100         NA 41.69870    2.3 -87.50830           NA
## 101         NA 43.03220    2.6 -87.89520           NA
## 102         NA 43.03220    2.9 -87.89520           NA
## 103         NA 43.03220    2.7 -87.89520           NA
## 104         NA 43.03220    2.5 -87.89520           NA
## 105         NA 43.03220    3.0 -87.89520           NA
## 106         NA 43.03220    2.7 -87.89520           NA
## 107         NA 43.03220    2.7 -87.89520           NA
## 108         NA 43.03220    2.9 -87.89520           NA
## 109         NA 43.03220    2.9 -87.89520           NA
## 110         NA 43.03220    3.2 -87.89520           NA
## 111         NA 43.03220    2.9 -87.89520           NA
## 112         NA 43.03220    2.5 -87.89520           NA
## 113         NA 43.03220    2.5 -87.89520           NA
## 114         NA 43.03220    2.5 -87.89520           NA
## 115         NA 43.03220    2.7 -87.89520           NA
## 116         NA 43.03220    2.7 -87.89520           NA
## 117         NA 43.03220    3.2 -87.89520           NA
## 118         NA 43.03220    3.1 -87.89520           NA
## 119         NA 43.03220    3.0 -87.89520           NA
## 120         NA 43.03220    2.8 -87.89520           NA
## 121         NA 43.03220    2.3 -87.89520           NA
## 122         NA 43.03220    2.7 -87.89520           NA
## 123         NA 43.03220    2.4 -87.89520           NA
## 124         NA 43.03220    2.3 -87.89520           NA
## 125         NA 43.03220    2.2 -87.89520           NA
## 126         NA 43.03220    2.9 -87.89520           NA
## 127         NA 43.03220    2.5 -87.89520           NA
## 128         NA 43.03220    2.5 -87.89520           NA
## 129         NA 43.03220    2.4 -87.89520           NA
## 130         NA 43.03220    2.4 -87.89520           NA
## 131         NA 43.03220    2.9 -87.89520           NA
## 132         NA 43.03220    2.4 -87.89520           NA
## 133         NA 43.03220    2.4 -87.89520           NA
## 134         NA 43.03220    2.6 -87.89520           NA
## 135         NA 43.03220    2.7 -87.89520           NA
## 136         NA 43.22820    2.4 -86.34690           NA
## 137         NA 43.22820    2.5 -86.34690           NA
## 138         NA 43.22820    2.2 -86.34690           NA
## 139         NA 43.22820    2.3 -86.34690           NA
## 140         NA 43.22820    2.8 -86.34690           NA
## 141         NA 43.22820    2.5 -86.34690           NA
## 142         NA 43.22820    2.5 -86.34690           NA
## 143         NA 43.22820    2.8 -86.34690           NA
## 144         NA 43.22820    2.4 -86.34690           NA
## 145         NA 43.22820    2.9 -86.34690           NA
## 146         NA 43.22820    2.5 -86.34690           NA
## 147         NA 43.22820    2.1 -86.34690           NA
## 148         NA 43.22820    1.9 -86.34690           NA
## 149         NA 43.22820    2.4 -86.34690           NA
## 150         NA 43.22820    2.6 -86.34690           NA
## 151         NA 43.22820    2.2 -86.34690           NA
## 152         NA 43.22820    2.0 -86.34690           NA
## 153         NA 43.22820    1.9 -86.34690           NA
## 154         NA 43.22820    2.5 -86.34690           NA
## 155         NA 43.22820    1.8 -86.34690           NA
## 156         NA 43.22820    1.8 -86.34690           NA
## 157         NA 43.22820    2.0 -86.34690           NA
## 158         NA 43.22820    2.2 -86.34690           NA
## 159         NA 43.22820    1.9 -86.34690           NA
## 160         NA 43.22820    1.8 -86.34690           NA
## 161         NA 43.22299    2.2 -86.34276           NA
## 162         NA 43.22299    2.1 -86.34276           NA
## 163         NA 43.22299    1.9 -86.34276           NA
## 164         NA 43.22299    2.0 -86.34276           NA
## 165         NA 43.22299    2.2 -86.34276           NA
## 166         NA 42.30470    2.0 -87.82730           NA
## 167         NA 42.30470    2.4 -87.82730           NA
## 168         NA 42.30470    1.7 -87.82730           NA
## 169         NA 42.30470    2.0 -87.82730           NA
## 170         NA 42.30470    2.0 -87.82730           NA
## 171         NA 42.30470    2.5 -87.82730           NA
## 172         NA 42.30470    2.5 -87.82730           NA
## 173         NA 42.30470    2.3 -87.82730           NA
## 174         NA 42.30470    2.4 -87.82730           NA
## 175         NA 42.30470    2.2 -87.82730           NA
## 176         NA 42.30470    2.2 -87.82730           NA
## 177         NA 42.30470    2.3 -87.82730           NA
## 178         NA 42.30470    2.4 -87.82730           NA
## 179         NA 42.30470    2.2 -87.82730           NA
## 180         NA 42.30470    2.3 -87.82730           NA
## 181         NA 42.30470    2.2 -87.82730           NA
## 182         NA 42.30470    2.0 -87.82730           NA
## 183         NA 42.30470    2.2 -87.82730           NA
## 184         NA 42.30470    2.0 -87.82730           NA
## 185         NA 42.30470    2.1 -87.82730           NA
## 186         NA 42.30470    2.3 -87.82730           NA
## 187         NA 42.30470    2.4 -87.82730           NA
## 188         NA 42.30470    2.3 -87.82730           NA
## 189         NA 42.30470    2.0 -87.82730           NA
## 190         NA 42.30470    2.0 -87.82730           NA
## 191         NA 42.30467    2.7 -87.82733           NA
## 192         NA 42.30467    2.4 -87.82733           NA
## 193         NA 42.30467    2.2 -87.82733           NA
## 194         NA 42.30467    2.2 -87.82733           NA
## 195         NA 42.30467    2.2 -87.82733           NA
## 196         NA 45.20570    1.9 -85.53680           NA
## 197         NA 45.20570    1.6 -85.53680           NA
## 198         NA 45.20570    1.8 -85.53680           NA
## 199         NA 45.20570    2.1 -85.53680           NA
## 200         NA 45.20570    1.6 -85.53680           NA
## 201         NA 45.20570    2.1 -85.53680           NA
## 202         NA 45.20570    2.2 -85.53680           NA
## 203         NA 45.20570    2.1 -85.53680           NA
## 204         NA 45.20570    1.9 -85.53680           NA
## 205         NA 45.20570    2.1 -85.53680           NA
## 206         NA 45.20570    1.9 -85.53680           NA
## 207         NA 45.20570    1.8 -85.53680           NA
## 208         NA 45.20570    2.0 -85.53680           NA
## 209         NA 45.20570    1.8 -85.53680           NA
## 210         NA 45.20570    2.0 -85.53680           NA
## 211         NA 45.20570    1.9 -85.53680           NA
## 212         NA 45.20570    2.0 -85.53680           NA
## 213         NA 45.20570    1.7 -85.53680           NA
## 214         NA 45.20570    2.0 -85.53680           NA
## 215         NA 45.20570    1.9 -85.53680           NA
## 216         NA 45.20570    1.6 -85.53680           NA
## 217         NA 45.20570    1.8 -85.53680           NA
## 218         NA 45.20570    1.9 -85.53680           NA
## 219         NA 45.20570    1.7 -85.53680           NA
## 220         NA 45.20570    1.9 -85.53680           NA
## 221         NA 45.20570    1.6 -85.53680           NA
## 222         NA 45.20570    1.4 -85.53680           NA
## 223         NA 45.20570    1.6 -85.53680           NA
## 224         NA 45.20570    1.5 -85.53680           NA
## 225         NA 45.20570    1.8 -85.53680           NA
## 226         NA 45.20570    2.1 -85.53680           NA
## 227         NA 45.20570    2.0 -85.53680           NA
## 228         NA 45.20570    1.8 -85.53680           NA
## 229         NA 45.20570    1.8 -85.53680           NA
## 230         NA 45.20570    2.0 -85.53680           NA
## 231         NA 45.21160    2.2 -85.54293           NA
## 232         NA 45.21160    2.1 -85.54293           NA
## 233         NA 45.21160    2.2 -85.54293           NA
## 234         NA 45.21160    2.0 -85.54293           NA
## 235         NA 45.21160    2.0 -85.54293           NA
## 236         NA 42.77320    2.2 -86.21500           NA
## 237         NA 42.77320    2.4 -86.21500           NA
## 238         NA 42.77320    2.6 -86.21500           NA
## 239         NA 42.77320    2.3 -86.21500           NA
## 240         NA 42.77320    2.4 -86.21500           NA
## 241         NA 41.69870    2.5 -87.50830           NA
## 242         NA 41.69870    2.3 -87.50830           NA
## 243         NA 41.69870    2.3 -87.50830           NA
## 244         NA 41.69870    2.3 -87.50830           NA
## 245         NA 41.69870    2.4 -87.50830           NA
## 246         NA 43.03220    2.6 -87.89520           NA
## 247         NA 43.03220    2.7 -87.89520           NA
## 248         NA 43.03220    2.6 -87.89520           NA
## 249         NA 43.03220    2.8 -87.89520           NA
## 250         NA 43.03220    2.4 -87.89520           NA
## 251         NA 43.22820    1.6 -86.34690           NA
## 252         NA 43.22820    1.7 -86.34690           NA
## 253         NA 43.22820    1.8 -86.34690           NA
## 254         NA 43.22820    1.5 -86.34690           NA
## 255         NA 43.22820    1.8 -86.34690           NA
## 256         NA 42.30470    2.1 -87.82730           NA
## 257         NA 42.30470    2.0 -87.82730           NA
## 258         NA 42.30470    2.1 -87.82730           NA
## 259         NA 42.30470    2.2 -87.82730           NA
## 260         NA 42.30470    2.2 -87.82730           NA
##     multinucleated_sphere_x multinucleated_sphere_x_description nematode
## 1                        NA                                            0
## 2                        NA                                            0
## 3                        NA                                            0
## 4                        NA                                            0
## 5                        NA                                            0
## 6                        NA                                            0
## 7                        NA                                            0
## 8                        NA                                            0
## 9                        NA                                            0
## 10                       NA                                            0
## 11                       NA                                            0
## 12                       NA                                            0
## 13                       NA                                            0
## 14                       NA                                            0
## 15                       NA                                            0
## 16                       NA                                            0
## 17                       NA                                            0
## 18                       NA                                            0
## 19                       NA                                            0
## 20                       NA                                            0
## 21                       NA                                            0
## 22                       NA                                            0
## 23                       NA                                            0
## 24                       NA                                            0
## 25                       NA                                            0
## 26                       NA                                            0
## 27                       NA                                            0
## 28                       NA                                            0
## 29                       NA                                            0
## 30                       NA                                            0
## 31                       NA                                            0
## 32                       NA                                            0
## 33                       NA                                            0
## 34                       NA                                            0
## 35                       NA                                            0
## 36                       NA                                            0
## 37                       NA                                            0
## 38                       NA                                            0
## 39                       NA                                            0
## 40                       NA                                            0
## 41                       NA                                            0
## 42                       NA                                            0
## 43                       NA                                            0
## 44                       NA                                            0
## 45                       NA                                            0
## 46                       NA                                            0
## 47                       NA                                            0
## 48                       NA                                            0
## 49                       NA                                            0
## 50                       NA                                            0
## 51                       NA                                            0
## 52                       NA                                            0
## 53                       NA                                            0
## 54                       NA                                            0
## 55                       NA                                            0
## 56                       NA                                            0
## 57                       NA                                            0
## 58                       NA                                            0
## 59                       NA                                            0
## 60                       NA                                            0
## 61                       NA                                            0
## 62                       NA                                            0
## 63                       NA                                            0
## 64                       NA                                            0
## 65                       NA                                            0
## 66                       NA                                            0
## 67                       NA                                            0
## 68                       NA                                            0
## 69                       NA                                            0
## 70                       NA                                            0
## 71                       NA                                            0
## 72                       NA                                            0
## 73                       NA                                            0
## 74                       NA                                            0
## 75                       NA                                            0
## 76                       NA                                            0
## 77                       NA                                            0
## 78                       NA                                            0
## 79                       NA                                            0
## 80                       NA                                            0
## 81                       NA                                            0
## 82                       NA                                            0
## 83                       NA                                            0
## 84                       NA                                            0
## 85                       NA                                            0
## 86                       NA                                            0
## 87                       NA                                            0
## 88                       NA                                            0
## 89                       NA                                            0
## 90                       NA                                            0
## 91                       NA                                            0
## 92                       NA                                            0
## 93                       NA                                            0
## 94                       NA                                            0
## 95                       NA                                            0
## 96                       NA                                            0
## 97                       NA                                            0
## 98                       NA                                            0
## 99                       NA                                            0
## 100                      NA                                            0
## 101                      NA                                            0
## 102                      NA                                            0
## 103                      NA                                            0
## 104                      NA                                            0
## 105                      NA                                            0
## 106                      NA                                            0
## 107                      NA                                            0
## 108                      NA                                            0
## 109                      NA                                            0
## 110                      NA                                            0
## 111                      NA                                            0
## 112                      NA                                            0
## 113                      NA                                            0
## 114                      NA                                            0
## 115                      NA                                            0
## 116                      NA                                            0
## 117                      NA                                            0
## 118                      NA                                            0
## 119                      NA                                            0
## 120                      NA                                            0
## 121                      NA                                            0
## 122                      NA                                            0
## 123                      NA                                            0
## 124                      NA                                            0
## 125                      NA                                            0
## 126                      NA                                            0
## 127                      NA                                            0
## 128                      NA                                            0
## 129                      NA                                            0
## 130                      NA                                            0
## 131                      NA                                            0
## 132                      NA                                            0
## 133                      NA                                            0
## 134                      NA                                            0
## 135                      NA                                            0
## 136                      NA                                            0
## 137                      NA                                            0
## 138                      NA                                            0
## 139                      NA                                            0
## 140                      NA                                            0
## 141                      NA                                            0
## 142                      NA                                            0
## 143                      NA                                            0
## 144                      NA                                            0
## 145                      NA                                            0
## 146                      NA                                            0
## 147                      NA                                            0
## 148                      NA                                            0
## 149                      NA                                            0
## 150                      NA                                            0
## 151                      NA                                            0
## 152                      NA                                            0
## 153                      NA                                            0
## 154                      NA                                            0
## 155                      NA                                            0
## 156                      NA                                            0
## 157                      NA                                            0
## 158                      NA                                            0
## 159                      NA                                            0
## 160                      NA                                            0
## 161                      NA                                            0
## 162                      NA                                            0
## 163                      NA                                            0
## 164                      NA                                            0
## 165                      NA                                            0
## 166                      NA                                            0
## 167                      NA                                            0
## 168                      NA                                            0
## 169                      NA                                            0
## 170                      NA                                            0
## 171                      NA                                            0
## 172                      NA                                            0
## 173                      NA                                            0
## 174                      NA                                            0
## 175                      NA                                            0
## 176                      NA                                            0
## 177                      NA                                            0
## 178                      NA                                            0
## 179                      NA                                            0
## 180                      NA                                            0
## 181                      NA                                            0
## 182                      NA                                            0
## 183                      NA                                            0
## 184                      NA                                            0
## 185                      NA                                            0
## 186                      NA                                            0
## 187                      NA                                            0
## 188                      NA                                            0
## 189                      NA                                            0
## 190                      NA                                            0
## 191                      NA                                            0
## 192                      NA                                            0
## 193                      NA                                            0
## 194                      NA                                            0
## 195                      NA                                            0
## 196                      NA                                            0
## 197                      NA                                            0
## 198                      NA                                            0
## 199                      NA                                            0
## 200                      NA                                            0
## 201                      NA                                            0
## 202                      NA                                            0
## 203                      NA                                            0
## 204                      NA                                            0
## 205                      NA                                            0
## 206                      NA                                            0
## 207                      NA                                            0
## 208                      NA                                            0
## 209                      NA                                            0
## 210                      NA                                            0
## 211                      NA                                            0
## 212                      NA                                            0
## 213                      NA                                            0
## 214                      NA                                            0
## 215                      NA                                            0
## 216                      NA                                            0
## 217                      NA                                            0
## 218                      NA                                            0
## 219                      NA                                            0
## 220                      NA                                            0
## 221                      NA                                            0
## 222                      NA                                            0
## 223                      NA                                            0
## 224                      NA                                            0
## 225                      NA                                            0
## 226                      NA                                            0
## 227                      NA                                            0
## 228                      NA                                            0
## 229                      NA                                            0
## 230                      NA                                            0
## 231                      NA                                            0
## 232                      NA                                            0
## 233                      NA                                            0
## 234                      NA                                            0
## 235                      NA                                            0
## 236                      NA                                            0
## 237                      NA                                            0
## 238                      NA                                            0
## 239                      NA                                            0
## 240                      NA                                            0
## 241                      NA                                            0
## 242                      NA                                            0
## 243                      NA                                            0
## 244                      NA                                            0
## 245                      NA                                            0
## 246                      NA                                            0
## 247                      NA                                            0
## 248                      NA                                            0
## 249                      NA                                            0
## 250                      NA                                            0
## 251                      NA                                            0
## 252                      NA                                            0
## 253                      NA                                            0
## 254                      NA                                            0
## 255                      NA                                            0
## 256                      NA                                            0
## 257                      NA                                            0
## 258                      NA                                            0
## 259                      NA                                            0
## 260                      NA                                            0
##     nematopsis_body nematopsis_gill nematopsis_mantle nemertine_gill
## 1                NA              NA                NA             NA
## 2                NA              NA                NA             NA
## 3                NA              NA                NA             NA
## 4                NA              NA                NA             NA
## 5                NA              NA                NA             NA
## 6                NA              NA                NA             NA
## 7                NA              NA                NA             NA
## 8                NA              NA                NA             NA
## 9                NA              NA                NA             NA
## 10               NA              NA                NA             NA
## 11               NA              NA                NA             NA
## 12               NA              NA                NA             NA
## 13               NA              NA                NA             NA
## 14               NA              NA                NA             NA
## 15               NA              NA                NA             NA
## 16               NA              NA                NA             NA
## 17               NA              NA                NA             NA
## 18               NA              NA                NA             NA
## 19               NA              NA                NA             NA
## 20               NA              NA                NA             NA
## 21               NA              NA                NA             NA
## 22               NA              NA                NA             NA
## 23               NA              NA                NA             NA
## 24               NA              NA                NA             NA
## 25               NA              NA                NA             NA
## 26               NA              NA                NA             NA
## 27               NA              NA                NA             NA
## 28               NA              NA                NA             NA
## 29               NA              NA                NA             NA
## 30               NA              NA                NA             NA
## 31               NA              NA                NA             NA
## 32               NA              NA                NA             NA
## 33               NA              NA                NA             NA
## 34               NA              NA                NA             NA
## 35               NA              NA                NA             NA
## 36               NA              NA                NA             NA
## 37               NA              NA                NA             NA
## 38               NA              NA                NA             NA
## 39               NA              NA                NA             NA
## 40               NA              NA                NA             NA
## 41               NA              NA                NA             NA
## 42               NA              NA                NA             NA
## 43               NA              NA                NA             NA
## 44               NA              NA                NA             NA
## 45               NA              NA                NA             NA
## 46               NA              NA                NA             NA
## 47               NA              NA                NA             NA
## 48               NA              NA                NA             NA
## 49               NA              NA                NA             NA
## 50               NA              NA                NA             NA
## 51               NA              NA                NA             NA
## 52               NA              NA                NA             NA
## 53               NA              NA                NA             NA
## 54               NA              NA                NA             NA
## 55               NA              NA                NA             NA
## 56               NA              NA                NA             NA
## 57               NA              NA                NA             NA
## 58               NA              NA                NA             NA
## 59               NA              NA                NA             NA
## 60               NA              NA                NA             NA
## 61               NA              NA                NA             NA
## 62               NA              NA                NA             NA
## 63               NA              NA                NA             NA
## 64               NA              NA                NA             NA
## 65               NA              NA                NA             NA
## 66               NA              NA                NA             NA
## 67               NA              NA                NA             NA
## 68               NA              NA                NA             NA
## 69               NA              NA                NA             NA
## 70               NA              NA                NA             NA
## 71               NA              NA                NA             NA
## 72               NA              NA                NA             NA
## 73               NA              NA                NA             NA
## 74               NA              NA                NA             NA
## 75               NA              NA                NA             NA
## 76               NA              NA                NA             NA
## 77               NA              NA                NA             NA
## 78               NA              NA                NA             NA
## 79               NA              NA                NA             NA
## 80               NA              NA                NA             NA
## 81               NA              NA                NA             NA
## 82               NA              NA                NA             NA
## 83               NA              NA                NA             NA
## 84               NA              NA                NA             NA
## 85               NA              NA                NA             NA
## 86               NA              NA                NA             NA
## 87               NA              NA                NA             NA
## 88               NA              NA                NA             NA
## 89               NA              NA                NA             NA
## 90               NA              NA                NA             NA
## 91               NA              NA                NA             NA
## 92               NA              NA                NA             NA
## 93               NA              NA                NA             NA
## 94               NA              NA                NA             NA
## 95               NA              NA                NA             NA
## 96               NA              NA                NA             NA
## 97               NA              NA                NA             NA
## 98               NA              NA                NA             NA
## 99               NA              NA                NA             NA
## 100              NA              NA                NA             NA
## 101              NA              NA                NA             NA
## 102              NA              NA                NA             NA
## 103              NA              NA                NA             NA
## 104              NA              NA                NA             NA
## 105              NA              NA                NA             NA
## 106              NA              NA                NA             NA
## 107              NA              NA                NA             NA
## 108              NA              NA                NA             NA
## 109              NA              NA                NA             NA
## 110              NA              NA                NA             NA
## 111              NA              NA                NA             NA
## 112              NA              NA                NA             NA
## 113              NA              NA                NA             NA
## 114              NA              NA                NA             NA
## 115              NA              NA                NA             NA
## 116              NA              NA                NA             NA
## 117              NA              NA                NA             NA
## 118              NA              NA                NA             NA
## 119              NA              NA                NA             NA
## 120              NA              NA                NA             NA
## 121              NA              NA                NA             NA
## 122              NA              NA                NA             NA
## 123              NA              NA                NA             NA
## 124              NA              NA                NA             NA
## 125              NA              NA                NA             NA
## 126              NA              NA                NA             NA
## 127              NA              NA                NA             NA
## 128              NA              NA                NA             NA
## 129              NA              NA                NA             NA
## 130              NA              NA                NA             NA
## 131              NA              NA                NA             NA
## 132              NA              NA                NA             NA
## 133              NA              NA                NA             NA
## 134              NA              NA                NA             NA
## 135              NA              NA                NA             NA
## 136              NA              NA                NA             NA
## 137              NA              NA                NA             NA
## 138              NA              NA                NA             NA
## 139              NA              NA                NA             NA
## 140              NA              NA                NA             NA
## 141              NA              NA                NA             NA
## 142              NA              NA                NA             NA
## 143              NA              NA                NA             NA
## 144              NA              NA                NA             NA
## 145              NA              NA                NA             NA
## 146              NA              NA                NA             NA
## 147              NA              NA                NA             NA
## 148              NA              NA                NA             NA
## 149              NA              NA                NA             NA
## 150              NA              NA                NA             NA
## 151              NA              NA                NA             NA
## 152              NA              NA                NA             NA
## 153              NA              NA                NA             NA
## 154              NA              NA                NA             NA
## 155              NA              NA                NA             NA
## 156              NA              NA                NA             NA
## 157              NA              NA                NA             NA
## 158              NA              NA                NA             NA
## 159              NA              NA                NA             NA
## 160              NA              NA                NA             NA
## 161              NA              NA                NA             NA
## 162              NA              NA                NA             NA
## 163              NA              NA                NA             NA
## 164              NA              NA                NA             NA
## 165              NA              NA                NA             NA
## 166              NA              NA                NA             NA
## 167              NA              NA                NA             NA
## 168              NA              NA                NA             NA
## 169              NA              NA                NA             NA
## 170              NA              NA                NA             NA
## 171              NA              NA                NA             NA
## 172              NA              NA                NA             NA
## 173              NA              NA                NA             NA
## 174              NA              NA                NA             NA
## 175              NA              NA                NA             NA
## 176              NA              NA                NA             NA
## 177              NA              NA                NA             NA
## 178              NA              NA                NA             NA
## 179              NA              NA                NA             NA
## 180              NA              NA                NA             NA
## 181              NA              NA                NA             NA
## 182              NA              NA                NA             NA
## 183              NA              NA                NA             NA
## 184              NA              NA                NA             NA
## 185              NA              NA                NA             NA
## 186              NA              NA                NA             NA
## 187              NA              NA                NA             NA
## 188              NA              NA                NA             NA
## 189              NA              NA                NA             NA
## 190              NA              NA                NA             NA
## 191              NA              NA                NA             NA
## 192              NA              NA                NA             NA
## 193              NA              NA                NA             NA
## 194              NA              NA                NA             NA
## 195              NA              NA                NA             NA
## 196              NA              NA                NA             NA
## 197              NA              NA                NA             NA
## 198              NA              NA                NA             NA
## 199              NA              NA                NA             NA
## 200              NA              NA                NA             NA
## 201              NA              NA                NA             NA
## 202              NA              NA                NA             NA
## 203              NA              NA                NA             NA
## 204              NA              NA                NA             NA
## 205              NA              NA                NA             NA
## 206              NA              NA                NA             NA
## 207              NA              NA                NA             NA
## 208              NA              NA                NA             NA
## 209              NA              NA                NA             NA
## 210              NA              NA                NA             NA
## 211              NA              NA                NA             NA
## 212              NA              NA                NA             NA
## 213              NA              NA                NA             NA
## 214              NA              NA                NA             NA
## 215              NA              NA                NA             NA
## 216              NA              NA                NA             NA
## 217              NA              NA                NA             NA
## 218              NA              NA                NA             NA
## 219              NA              NA                NA             NA
## 220              NA              NA                NA             NA
## 221              NA              NA                NA             NA
## 222              NA              NA                NA             NA
## 223              NA              NA                NA             NA
## 224              NA              NA                NA             NA
## 225              NA              NA                NA             NA
## 226              NA              NA                NA             NA
## 227              NA              NA                NA             NA
## 228              NA              NA                NA             NA
## 229              NA              NA                NA             NA
## 230              NA              NA                NA             NA
## 231              NA              NA                NA             NA
## 232              NA              NA                NA             NA
## 233              NA              NA                NA             NA
## 234              NA              NA                NA             NA
## 235              NA              NA                NA             NA
## 236              NA              NA                NA             NA
## 237              NA              NA                NA             NA
## 238              NA              NA                NA             NA
## 239              NA              NA                NA             NA
## 240              NA              NA                NA             NA
## 241              NA              NA                NA             NA
## 242              NA              NA                NA             NA
## 243              NA              NA                NA             NA
## 244              NA              NA                NA             NA
## 245              NA              NA                NA             NA
## 246              NA              NA                NA             NA
## 247              NA              NA                NA             NA
## 248              NA              NA                NA             NA
## 249              NA              NA                NA             NA
## 250              NA              NA                NA             NA
## 251              NA              NA                NA             NA
## 252              NA              NA                NA             NA
## 253              NA              NA                NA             NA
## 254              NA              NA                NA             NA
## 255              NA              NA                NA             NA
## 256              NA              NA                NA             NA
## 257              NA              NA                NA             NA
## 258              NA              NA                NA             NA
## 259              NA              NA                NA             NA
## 260              NA              NA                NA             NA
##     neoplasm nst_sample_id nst_site other_trematode_sporocyst_gill
## 1         NA  MW1996GBBSDS     GBBS                             NA
## 2         NA  MW1996GBBSDS     GBBS                             NA
## 3         NA  MW1996GBBSDS     GBBS                             NA
## 4         NA  MW1996GBBSDS     GBBS                             NA
## 5         NA  MW1996GBBSDS     GBBS                             NA
## 6         NA  MW1998GBBSDS     GBBS                             NA
## 7         NA  MW1998GBBSDS     GBBS                             NA
## 8         NA  MW1998GBBSDS     GBBS                             NA
## 9         NA  MW1998GBBSDS     GBBS                             NA
## 10        NA  MW1998GBBSDS     GBBS                             NA
## 11        NA  MW2000GBBSDS     GBBS                             NA
## 12        NA  MW2000GBBSDS     GBBS                             NA
## 13        NA  MW2000GBBSDS     GBBS                             NA
## 14        NA  MW2000GBBSDS     GBBS                             NA
## 15        NA  MW2000GBBSDS     GBBS                             NA
## 16        NA  MW2002GBBSDS     GBBS                             NA
## 17        NA  MW2002GBBSDS     GBBS                             NA
## 18        NA  MW2002GBBSDS     GBBS                             NA
## 19        NA  MW2002GBBSDS     GBBS                             NA
## 20        NA  MW2002GBBSDS     GBBS                             NA
## 21        NA  MW2004GBBSDS     GBBS                             NA
## 22        NA  MW2004GBBSDS     GBBS                             NA
## 23        NA  MW2004GBBSDS     GBBS                             NA
## 24        NA  MW2004GBBSDS     GBBS                             NA
## 25        NA  MW2004GBBSDS     GBBS                             NA
## 26        NA  MW2006GBBSDS     GBBS                             NA
## 27        NA  MW2006GBBSDS     GBBS                             NA
## 28        NA  MW2006GBBSDS     GBBS                             NA
## 29        NA  MW2006GBBSDS     GBBS                             NA
## 30        NA  MW2006GBBSDS     GBBS                             NA
## 31        NA MW2010GBBS_DS     GBBS                             NA
## 32        NA MW2010GBBS_DS     GBBS                             NA
## 33        NA MW2010GBBS_DS     GBBS                             NA
## 34        NA MW2010GBBS_DS     GBBS                             NA
## 35        NA MW2010GBBS_DS     GBBS                             NA
## 36        NA  MW1998LMCBDS     LMCB                             NA
## 37        NA  MW1998LMCBDS     LMCB                             NA
## 38        NA  MW1998LMCBDS     LMCB                             NA
## 39        NA  MW1998LMCBDS     LMCB                             NA
## 40        NA  MW1998LMCBDS     LMCB                             NA
## 41        NA  MW2000LMCBDS     LMCB                             NA
## 42        NA  MW2000LMCBDS     LMCB                             NA
## 43        NA  MW2000LMCBDS     LMCB                             NA
## 44        NA  MW2000LMCBDS     LMCB                             NA
## 45        NA  MW2000LMCBDS     LMCB                             NA
## 46        NA  MW2006LMCBDS     LMCB                             NA
## 47        NA  MW2006LMCBDS     LMCB                             NA
## 48        NA  MW2006LMCBDS     LMCB                             NA
## 49        NA  MW2006LMCBDS     LMCB                             NA
## 50        NA  MW2006LMCBDS     LMCB                             NA
## 51        NA  MW1996LMHBDS     LMHB                             NA
## 52        NA  MW1996LMHBDS     LMHB                             NA
## 53        NA  MW1996LMHBDS     LMHB                             NA
## 54        NA  MW1996LMHBDS     LMHB                             NA
## 55        NA  MW1996LMHBDS     LMHB                             NA
## 56        NA  MW1998LMHBDS     LMHB                             NA
## 57        NA  MW1998LMHBDS     LMHB                             NA
## 58        NA  MW1998LMHBDS     LMHB                             NA
## 59        NA  MW1998LMHBDS     LMHB                             NA
## 60        NA  MW1998LMHBDS     LMHB                             NA
## 61        NA  MW2000LMHBDS     LMHB                             NA
## 62        NA  MW2000LMHBDS     LMHB                             NA
## 63        NA  MW2000LMHBDS     LMHB                             NA
## 64        NA  MW2000LMHBDS     LMHB                             NA
## 65        NA  MW2000LMHBDS     LMHB                             NA
## 66        NA  MW2004LMHBDS     LMHB                             NA
## 67        NA  MW2004LMHBDS     LMHB                             NA
## 68        NA  MW2004LMHBDS     LMHB                             NA
## 69        NA  MW2004LMHBDS     LMHB                             NA
## 70        NA  MW2004LMHBDS     LMHB                             NA
## 71        NA  MW2006LMHBDS     LMHB                             NA
## 72        NA  MW2006LMHBDS     LMHB                             NA
## 73        NA  MW2006LMHBDS     LMHB                             NA
## 74        NA  MW2006LMHBDS     LMHB                             NA
## 75        NA  MW2006LMHBDS     LMHB                             NA
## 76        NA MW2010LMHB_DS     LMHB                             NA
## 77        NA MW2010LMHB_DS     LMHB                             NA
## 78        NA MW2010LMHB_DS     LMHB                             NA
## 79        NA MW2010LMHB_DS     LMHB                             NA
## 80        NA MW2010LMHB_DS     LMHB                             NA
## 81        NA  MW2000LMHMDS     LMHM                             NA
## 82        NA  MW2000LMHMDS     LMHM                             NA
## 83        NA  MW2000LMHMDS     LMHM                             NA
## 84        NA  MW2000LMHMDS     LMHM                             NA
## 85        NA  MW2000LMHMDS     LMHM                             NA
## 86        NA  MW2004LMHMDS     LMHM                             NA
## 87        NA  MW2004LMHMDS     LMHM                             NA
## 88        NA  MW2004LMHMDS     LMHM                             NA
## 89        NA  MW2004LMHMDS     LMHM                             NA
## 90        NA  MW2004LMHMDS     LMHM                             NA
## 91        NA  MW2004LMHMDS     LMHM                             NA
## 92        NA  MW2004LMHMDS     LMHM                             NA
## 93        NA  MW2004LMHMDS     LMHM                             NA
## 94        NA  MW2004LMHMDS     LMHM                             NA
## 95        NA  MW2004LMHMDS     LMHM                             NA
## 96        NA  MW2006LMHMDS     LMHM                             NA
## 97        NA  MW2006LMHMDS     LMHM                             NA
## 98        NA  MW2006LMHMDS     LMHM                             NA
## 99        NA  MW2006LMHMDS     LMHM                             NA
## 100       NA  MW2006LMHMDS     LMHM                             NA
## 101       NA  MW1996LMMBDS     LMMB                             NA
## 102       NA  MW1996LMMBDS     LMMB                             NA
## 103       NA  MW1996LMMBDS     LMMB                             NA
## 104       NA  MW1996LMMBDS     LMMB                             NA
## 105       NA  MW1996LMMBDS     LMMB                             NA
## 106       NA  MW1998LMMBDS     LMMB                             NA
## 107       NA  MW1998LMMBDS     LMMB                             NA
## 108       NA  MW1998LMMBDS     LMMB                             NA
## 109       NA  MW1998LMMBDS     LMMB                             NA
## 110       NA  MW1998LMMBDS     LMMB                             NA
## 111       NA  MW2000LMMBDS     LMMB                             NA
## 112       NA  MW2000LMMBDS     LMMB                             NA
## 113       NA  MW2000LMMBDS     LMMB                             NA
## 114       NA  MW2000LMMBDS     LMMB                             NA
## 115       NA  MW2000LMMBDS     LMMB                             NA
## 116       NA  MW2004LMMBDS     LMMB                             NA
## 117       NA  MW2004LMMBDS     LMMB                             NA
## 118       NA  MW2004LMMBDS     LMMB                             NA
## 119       NA  MW2004LMMBDS     LMMB                             NA
## 120       NA  MW2004LMMBDS     LMMB                             NA
## 121       NA  MW2006LMMBDS     LMMB                             NA
## 122       NA  MW2006LMMBDS     LMMB                             NA
## 123       NA  MW2006LMMBDS     LMMB                             NA
## 124       NA  MW2006LMMBDS     LMMB                             NA
## 125       NA  MW2006LMMBDS     LMMB                             NA
## 126       NA  MW2006LMMBDS     LMMB                             NA
## 127       NA  MW2006LMMBDS     LMMB                             NA
## 128       NA  MW2006LMMBDS     LMMB                             NA
## 129       NA  MW2006LMMBDS     LMMB                             NA
## 130       NA  MW2006LMMBDS     LMMB                             NA
## 131       NA  MW2006LMMBDS     LMMB                             NA
## 132       NA  MW2006LMMBDS     LMMB                             NA
## 133       NA  MW2006LMMBDS     LMMB                             NA
## 134       NA  MW2006LMMBDS     LMMB                             NA
## 135       NA  MW2006LMMBDS     LMMB                             NA
## 136       NA  MW1996LMMUDS     LMMU                             NA
## 137       NA  MW1996LMMUDS     LMMU                             NA
## 138       NA  MW1996LMMUDS     LMMU                             NA
## 139       NA  MW1996LMMUDS     LMMU                             NA
## 140       NA  MW1996LMMUDS     LMMU                             NA
## 141       NA  MW1998LMMUDS     LMMU                             NA
## 142       NA  MW1998LMMUDS     LMMU                             NA
## 143       NA  MW1998LMMUDS     LMMU                             NA
## 144       NA  MW1998LMMUDS     LMMU                             NA
## 145       NA  MW1998LMMUDS     LMMU                             NA
## 146       NA  MW2000LMMUDS     LMMU                             NA
## 147       NA  MW2000LMMUDS     LMMU                             NA
## 148       NA  MW2000LMMUDS     LMMU                             NA
## 149       NA  MW2000LMMUDS     LMMU                             NA
## 150       NA  MW2000LMMUDS     LMMU                             NA
## 151       NA  MW2004LMMUDS     LMMU                             NA
## 152       NA  MW2004LMMUDS     LMMU                             NA
## 153       NA  MW2004LMMUDS     LMMU                             NA
## 154       NA  MW2004LMMUDS     LMMU                             NA
## 155       NA  MW2004LMMUDS     LMMU                             NA
## 156       NA  MW2006LMMUDS     LMMU                             NA
## 157       NA  MW2006LMMUDS     LMMU                             NA
## 158       NA  MW2006LMMUDS     LMMU                             NA
## 159       NA  MW2006LMMUDS     LMMU                             NA
## 160       NA  MW2006LMMUDS     LMMU                             NA
## 161       NA MW2010LMMU_DS     LMMU                             NA
## 162       NA MW2010LMMU_DS     LMMU                             NA
## 163       NA MW2010LMMU_DS     LMMU                             NA
## 164       NA MW2010LMMU_DS     LMMU                             NA
## 165       NA MW2010LMMU_DS     LMMU                             NA
## 166       NA  MW1996LMNCDS     LMNC                             NA
## 167       NA  MW1996LMNCDS     LMNC                             NA
## 168       NA  MW1996LMNCDS     LMNC                             NA
## 169       NA  MW1996LMNCDS     LMNC                             NA
## 170       NA  MW1996LMNCDS     LMNC                             NA
## 171       NA  MW1998LMNCDS     LMNC                             NA
## 172       NA  MW1998LMNCDS     LMNC                             NA
## 173       NA  MW1998LMNCDS     LMNC                             NA
## 174       NA  MW1998LMNCDS     LMNC                             NA
## 175       NA  MW1998LMNCDS     LMNC                             NA
## 176       NA  MW2000LMNCDS     LMNC                             NA
## 177       NA  MW2000LMNCDS     LMNC                             NA
## 178       NA  MW2000LMNCDS     LMNC                             NA
## 179       NA  MW2000LMNCDS     LMNC                             NA
## 180       NA  MW2000LMNCDS     LMNC                             NA
## 181       NA  MW2004LMNCDS     LMNC                             NA
## 182       NA  MW2004LMNCDS     LMNC                             NA
## 183       NA  MW2004LMNCDS     LMNC                             NA
## 184       NA  MW2004LMNCDS     LMNC                             NA
## 185       NA  MW2004LMNCDS     LMNC                             NA
## 186       NA  MW2006LMNCDS     LMNC                             NA
## 187       NA  MW2006LMNCDS     LMNC                             NA
## 188       NA  MW2006LMNCDS     LMNC                             NA
## 189       NA  MW2006LMNCDS     LMNC                             NA
## 190       NA  MW2006LMNCDS     LMNC                             NA
## 191       NA MW2010LMNC_DS     LMNC                             NA
## 192       NA MW2010LMNC_DS     LMNC                             NA
## 193       NA MW2010LMNC_DS     LMNC                             NA
## 194       NA MW2010LMNC_DS     LMNC                             NA
## 195       NA MW2010LMNC_DS     LMNC                             NA
## 196       NA  MW1996TBLLDS     TBLL                             NA
## 197       NA  MW1996TBLLDS     TBLL                             NA
## 198       NA  MW1996TBLLDS     TBLL                             NA
## 199       NA  MW1996TBLLDS     TBLL                             NA
## 200       NA  MW1996TBLLDS     TBLL                             NA
## 201       NA  MW1998TBLLDS     TBLL                             NA
## 202       NA  MW1998TBLLDS     TBLL                             NA
## 203       NA  MW1998TBLLDS     TBLL                             NA
## 204       NA  MW1998TBLLDS     TBLL                             NA
## 205       NA  MW1998TBLLDS     TBLL                             NA
## 206       NA  MW2000TBLLDS     TBLL                             NA
## 207       NA  MW2000TBLLDS     TBLL                             NA
## 208       NA  MW2000TBLLDS     TBLL                             NA
## 209       NA  MW2000TBLLDS     TBLL                             NA
## 210       NA  MW2000TBLLDS     TBLL                             NA
## 211       NA  MW2002TBLLDS     TBLL                             NA
## 212       NA  MW2002TBLLDS     TBLL                             NA
## 213       NA  MW2002TBLLDS     TBLL                             NA
## 214       NA  MW2002TBLLDS     TBLL                             NA
## 215       NA  MW2002TBLLDS     TBLL                             NA
## 216       NA  MW2004TBLLDS     TBLL                             NA
## 217       NA  MW2004TBLLDS     TBLL                             NA
## 218       NA  MW2004TBLLDS     TBLL                             NA
## 219       NA  MW2004TBLLDS     TBLL                             NA
## 220       NA  MW2004TBLLDS     TBLL                             NA
## 221       NA  MW2004TBLLDS     TBLL                             NA
## 222       NA  MW2004TBLLDS     TBLL                             NA
## 223       NA  MW2004TBLLDS     TBLL                             NA
## 224       NA  MW2004TBLLDS     TBLL                             NA
## 225       NA  MW2004TBLLDS     TBLL                             NA
## 226       NA  MW2006TBLLDS     TBLL                             NA
## 227       NA  MW2006TBLLDS     TBLL                             NA
## 228       NA  MW2006TBLLDS     TBLL                             NA
## 229       NA  MW2006TBLLDS     TBLL                             NA
## 230       NA  MW2006TBLLDS     TBLL                             NA
## 231       NA MW2010TBLL_DS     TBLL                             NA
## 232       NA MW2010TBLL_DS     TBLL                             NA
## 233       NA MW2010TBLL_DS     TBLL                             NA
## 234       NA MW2010TBLL_DS     TBLL                             NA
## 235       NA MW2010TBLL_DS     TBLL                             NA
## 236       NA  SS2003LMHBDS     LMHB                             NA
## 237       NA  SS2003LMHBDS     LMHB                             NA
## 238       NA  SS2003LMHBDS     LMHB                             NA
## 239       NA  SS2003LMHBDS     LMHB                             NA
## 240       NA  SS2003LMHBDS     LMHB                             NA
## 241       NA  SS2003LMHMDS     LMHM                             NA
## 242       NA  SS2003LMHMDS     LMHM                             NA
## 243       NA  SS2003LMHMDS     LMHM                             NA
## 244       NA  SS2003LMHMDS     LMHM                             NA
## 245       NA  SS2003LMHMDS     LMHM                             NA
## 246       NA  SS2003LMMBDS     LMMB                             NA
## 247       NA  SS2003LMMBDS     LMMB                             NA
## 248       NA  SS2003LMMBDS     LMMB                             NA
## 249       NA  SS2003LMMBDS     LMMB                             NA
## 250       NA  SS2003LMMBDS     LMMB                             NA
## 251       NA  SS2003LMMUDS     LMMU                             NA
## 252       NA  SS2003LMMUDS     LMMU                             NA
## 253       NA  SS2003LMMUDS     LMMU                             NA
## 254       NA  SS2003LMMUDS     LMMU                             NA
## 255       NA  SS2003LMMUDS     LMMU                             NA
## 256       NA  SS2003LMNCDS     LMNC                             NA
## 257       NA  SS2003LMNCDS     LMNC                             NA
## 258       NA  SS2003LMNCDS     LMNC                             NA
## 259       NA  SS2003LMNCDS     LMNC                             NA
## 260       NA  SS2003LMNCDS     LMNC                             NA
##     other_trematode_sporocyst_gut pea_crab proctoeces
## 1                              NA       NA         NA
## 2                              NA       NA         NA
## 3                              NA       NA         NA
## 4                              NA       NA         NA
## 5                              NA       NA         NA
## 6                              NA       NA         NA
## 7                              NA       NA         NA
## 8                              NA       NA         NA
## 9                              NA       NA         NA
## 10                             NA       NA         NA
## 11                             NA       NA         NA
## 12                             NA       NA         NA
## 13                             NA       NA         NA
## 14                             NA       NA         NA
## 15                             NA       NA         NA
## 16                             NA       NA         NA
## 17                             NA       NA         NA
## 18                             NA       NA         NA
## 19                             NA       NA         NA
## 20                             NA       NA         NA
## 21                             NA       NA         NA
## 22                             NA       NA         NA
## 23                             NA       NA         NA
## 24                             NA       NA         NA
## 25                             NA       NA         NA
## 26                             NA       NA         NA
## 27                             NA       NA         NA
## 28                             NA       NA         NA
## 29                             NA       NA         NA
## 30                             NA       NA         NA
## 31                             NA       NA         NA
## 32                             NA       NA         NA
## 33                             NA       NA         NA
## 34                             NA       NA         NA
## 35                             NA       NA         NA
## 36                             NA       NA         NA
## 37                             NA       NA         NA
## 38                             NA       NA         NA
## 39                             NA       NA         NA
## 40                             NA       NA         NA
## 41                             NA       NA         NA
## 42                             NA       NA         NA
## 43                             NA       NA         NA
## 44                             NA       NA         NA
## 45                             NA       NA         NA
## 46                             NA       NA         NA
## 47                             NA       NA         NA
## 48                             NA       NA         NA
## 49                             NA       NA         NA
## 50                             NA       NA         NA
## 51                             NA       NA         NA
## 52                             NA       NA         NA
## 53                             NA       NA         NA
## 54                             NA       NA         NA
## 55                             NA       NA         NA
## 56                             NA       NA         NA
## 57                             NA       NA         NA
## 58                             NA       NA         NA
## 59                             NA       NA         NA
## 60                             NA       NA         NA
## 61                             NA       NA         NA
## 62                             NA       NA         NA
## 63                             NA       NA         NA
## 64                             NA       NA         NA
## 65                             NA       NA         NA
## 66                             NA       NA         NA
## 67                             NA       NA         NA
## 68                             NA       NA         NA
## 69                             NA       NA         NA
## 70                             NA       NA         NA
## 71                             NA       NA         NA
## 72                             NA       NA         NA
## 73                             NA       NA         NA
## 74                             NA       NA         NA
## 75                             NA       NA         NA
## 76                             NA       NA         NA
## 77                             NA       NA         NA
## 78                             NA       NA         NA
## 79                             NA       NA         NA
## 80                             NA       NA         NA
## 81                             NA       NA         NA
## 82                             NA       NA         NA
## 83                             NA       NA         NA
## 84                             NA       NA         NA
## 85                             NA       NA         NA
## 86                             NA       NA         NA
## 87                             NA       NA         NA
## 88                             NA       NA         NA
## 89                             NA       NA         NA
## 90                             NA       NA         NA
## 91                             NA       NA         NA
## 92                             NA       NA         NA
## 93                             NA       NA         NA
## 94                             NA       NA         NA
## 95                             NA       NA         NA
## 96                             NA       NA         NA
## 97                             NA       NA         NA
## 98                             NA       NA         NA
## 99                             NA       NA         NA
## 100                            NA       NA         NA
## 101                            NA       NA         NA
## 102                            NA       NA         NA
## 103                            NA       NA         NA
## 104                            NA       NA         NA
## 105                            NA       NA         NA
## 106                            NA       NA         NA
## 107                            NA       NA         NA
## 108                            NA       NA         NA
## 109                            NA       NA         NA
## 110                            NA       NA         NA
## 111                            NA       NA         NA
## 112                            NA       NA         NA
## 113                            NA       NA         NA
## 114                            NA       NA         NA
## 115                            NA       NA         NA
## 116                            NA       NA         NA
## 117                            NA       NA         NA
## 118                            NA       NA         NA
## 119                            NA       NA         NA
## 120                            NA       NA         NA
## 121                            NA       NA         NA
## 122                            NA       NA         NA
## 123                            NA       NA         NA
## 124                            NA       NA         NA
## 125                            NA       NA         NA
## 126                            NA       NA         NA
## 127                            NA       NA         NA
## 128                            NA       NA         NA
## 129                            NA       NA         NA
## 130                            NA       NA         NA
## 131                            NA       NA         NA
## 132                            NA       NA         NA
## 133                            NA       NA         NA
## 134                            NA       NA         NA
## 135                            NA       NA         NA
## 136                            NA       NA         NA
## 137                            NA       NA         NA
## 138                            NA       NA         NA
## 139                            NA       NA         NA
## 140                            NA       NA         NA
## 141                            NA       NA         NA
## 142                            NA       NA         NA
## 143                            NA       NA         NA
## 144                            NA       NA         NA
## 145                            NA       NA         NA
## 146                            NA       NA         NA
## 147                            NA       NA         NA
## 148                            NA       NA         NA
## 149                            NA       NA         NA
## 150                            NA       NA         NA
## 151                            NA       NA         NA
## 152                            NA       NA         NA
## 153                            NA       NA         NA
## 154                            NA       NA         NA
## 155                            NA       NA         NA
## 156                            NA       NA         NA
## 157                            NA       NA         NA
## 158                            NA       NA         NA
## 159                            NA       NA         NA
## 160                            NA       NA         NA
## 161                            NA       NA         NA
## 162                            NA       NA         NA
## 163                            NA       NA         NA
## 164                            NA       NA         NA
## 165                            NA       NA         NA
## 166                            NA       NA         NA
## 167                            NA       NA         NA
## 168                            NA       NA         NA
## 169                            NA       NA         NA
## 170                            NA       NA         NA
## 171                            NA       NA         NA
## 172                            NA       NA         NA
## 173                            NA       NA         NA
## 174                            NA       NA         NA
## 175                            NA       NA         NA
## 176                            NA       NA         NA
## 177                            NA       NA         NA
## 178                            NA       NA         NA
## 179                            NA       NA         NA
## 180                            NA       NA         NA
## 181                            NA       NA         NA
## 182                            NA       NA         NA
## 183                            NA       NA         NA
## 184                            NA       NA         NA
## 185                            NA       NA         NA
## 186                            NA       NA         NA
## 187                            NA       NA         NA
## 188                            NA       NA         NA
## 189                            NA       NA         NA
## 190                            NA       NA         NA
## 191                            NA       NA         NA
## 192                            NA       NA         NA
## 193                            NA       NA         NA
## 194                            NA       NA         NA
## 195                            NA       NA         NA
## 196                            NA       NA         NA
## 197                            NA       NA         NA
## 198                            NA       NA         NA
## 199                            NA       NA         NA
## 200                            NA       NA         NA
## 201                            NA       NA         NA
## 202                            NA       NA         NA
## 203                            NA       NA         NA
## 204                            NA       NA         NA
## 205                            NA       NA         NA
## 206                            NA       NA         NA
## 207                            NA       NA         NA
## 208                            NA       NA         NA
## 209                            NA       NA         NA
## 210                            NA       NA         NA
## 211                            NA       NA         NA
## 212                            NA       NA         NA
## 213                            NA       NA         NA
## 214                            NA       NA         NA
## 215                            NA       NA         NA
## 216                            NA       NA         NA
## 217                            NA       NA         NA
## 218                            NA       NA         NA
## 219                            NA       NA         NA
## 220                            NA       NA         NA
## 221                            NA       NA         NA
## 222                            NA       NA         NA
## 223                            NA       NA         NA
## 224                            NA       NA         NA
## 225                            NA       NA         NA
## 226                            NA       NA         NA
## 227                            NA       NA         NA
## 228                            NA       NA         NA
## 229                            NA       NA         NA
## 230                            NA       NA         NA
## 231                            NA       NA         NA
## 232                            NA       NA         NA
## 233                            NA       NA         NA
## 234                            NA       NA         NA
## 235                            NA       NA         NA
## 236                            NA       NA         NA
## 237                            NA       NA         NA
## 238                            NA       NA         NA
## 239                            NA       NA         NA
## 240                            NA       NA         NA
## 241                            NA       NA         NA
## 242                            NA       NA         NA
## 243                            NA       NA         NA
## 244                            NA       NA         NA
## 245                            NA       NA         NA
## 246                            NA       NA         NA
## 247                            NA       NA         NA
## 248                            NA       NA         NA
## 249                            NA       NA         NA
## 250                            NA       NA         NA
## 251                            NA       NA         NA
## 252                            NA       NA         NA
## 253                            NA       NA         NA
## 254                            NA       NA         NA
## 255                            NA       NA         NA
## 256                            NA       NA         NA
## 257                            NA       NA         NA
## 258                            NA       NA         NA
## 259                            NA       NA         NA
## 260                            NA       NA         NA
##     protozoan_digestive_tubule protozoan_gut pseudoklossia      region
## 1                           NA            NA            NA Great Lakes
## 2                           NA            NA            NA Great Lakes
## 3                           NA            NA            NA Great Lakes
## 4                           NA            NA            NA Great Lakes
## 5                           NA            NA            NA Great Lakes
## 6                           NA            NA            NA Great Lakes
## 7                           NA            NA            NA Great Lakes
## 8                           NA            NA            NA Great Lakes
## 9                           NA            NA            NA Great Lakes
## 10                          NA            NA            NA Great Lakes
## 11                          NA            NA            NA Great Lakes
## 12                          NA            NA            NA Great Lakes
## 13                          NA            NA            NA Great Lakes
## 14                          NA            NA            NA Great Lakes
## 15                          NA            NA            NA Great Lakes
## 16                          NA            NA            NA Great Lakes
## 17                          NA            NA            NA Great Lakes
## 18                          NA            NA            NA Great Lakes
## 19                          NA            NA            NA Great Lakes
## 20                          NA            NA            NA Great Lakes
## 21                          NA            NA            NA Great Lakes
## 22                          NA            NA            NA Great Lakes
## 23                          NA            NA            NA Great Lakes
## 24                          NA            NA            NA Great Lakes
## 25                          NA            NA            NA Great Lakes
## 26                          NA            NA            NA Great Lakes
## 27                          NA            NA            NA Great Lakes
## 28                          NA            NA            NA Great Lakes
## 29                          NA            NA            NA Great Lakes
## 30                          NA            NA            NA Great Lakes
## 31                          NA            NA            NA Great Lakes
## 32                          NA            NA            NA Great Lakes
## 33                          NA            NA            NA Great Lakes
## 34                          NA            NA            NA Great Lakes
## 35                          NA            NA            NA Great Lakes
## 36                          NA            NA            NA Great Lakes
## 37                          NA            NA            NA Great Lakes
## 38                          NA            NA            NA Great Lakes
## 39                          NA            NA            NA Great Lakes
## 40                          NA            NA            NA Great Lakes
## 41                          NA            NA            NA Great Lakes
## 42                          NA            NA            NA Great Lakes
## 43                          NA            NA            NA Great Lakes
## 44                          NA            NA            NA Great Lakes
## 45                          NA            NA            NA Great Lakes
## 46                          NA            NA            NA Great Lakes
## 47                          NA            NA            NA Great Lakes
## 48                          NA            NA            NA Great Lakes
## 49                          NA            NA            NA Great Lakes
## 50                          NA            NA            NA Great Lakes
## 51                          NA            NA            NA Great Lakes
## 52                          NA            NA            NA Great Lakes
## 53                          NA            NA            NA Great Lakes
## 54                          NA            NA            NA Great Lakes
## 55                          NA            NA            NA Great Lakes
## 56                          NA            NA            NA Great Lakes
## 57                          NA            NA            NA Great Lakes
## 58                          NA            NA            NA Great Lakes
## 59                          NA            NA            NA Great Lakes
## 60                          NA            NA            NA Great Lakes
## 61                          NA            NA            NA Great Lakes
## 62                          NA            NA            NA Great Lakes
## 63                          NA            NA            NA Great Lakes
## 64                          NA            NA            NA Great Lakes
## 65                          NA            NA            NA Great Lakes
## 66                          NA            NA            NA Great Lakes
## 67                          NA            NA            NA Great Lakes
## 68                          NA            NA            NA Great Lakes
## 69                          NA            NA            NA Great Lakes
## 70                          NA            NA            NA Great Lakes
## 71                          NA            NA            NA Great Lakes
## 72                          NA            NA            NA Great Lakes
## 73                          NA            NA            NA Great Lakes
## 74                          NA            NA            NA Great Lakes
## 75                          NA            NA            NA Great Lakes
## 76                          NA            NA            NA Great Lakes
## 77                          NA            NA            NA Great Lakes
## 78                          NA            NA            NA Great Lakes
## 79                          NA            NA            NA Great Lakes
## 80                          NA            NA            NA Great Lakes
## 81                          NA            NA            NA Great Lakes
## 82                          NA            NA            NA Great Lakes
## 83                          NA            NA            NA Great Lakes
## 84                          NA            NA            NA Great Lakes
## 85                          NA            NA            NA Great Lakes
## 86                          NA            NA            NA Great Lakes
## 87                          NA            NA            NA Great Lakes
## 88                          NA            NA            NA Great Lakes
## 89                          NA            NA            NA Great Lakes
## 90                          NA            NA            NA Great Lakes
## 91                          NA            NA            NA Great Lakes
## 92                          NA            NA            NA Great Lakes
## 93                          NA            NA            NA Great Lakes
## 94                          NA            NA            NA Great Lakes
## 95                          NA            NA            NA Great Lakes
## 96                          NA            NA            NA Great Lakes
## 97                          NA            NA            NA Great Lakes
## 98                          NA            NA            NA Great Lakes
## 99                          NA            NA            NA Great Lakes
## 100                         NA            NA            NA Great Lakes
## 101                         NA            NA            NA Great Lakes
## 102                         NA            NA            NA Great Lakes
## 103                         NA            NA            NA Great Lakes
## 104                         NA            NA            NA Great Lakes
## 105                         NA            NA            NA Great Lakes
## 106                         NA            NA            NA Great Lakes
## 107                         NA            NA            NA Great Lakes
## 108                         NA            NA            NA Great Lakes
## 109                         NA            NA            NA Great Lakes
## 110                         NA            NA            NA Great Lakes
## 111                         NA            NA            NA Great Lakes
## 112                         NA            NA            NA Great Lakes
## 113                         NA            NA            NA Great Lakes
## 114                         NA            NA            NA Great Lakes
## 115                         NA            NA            NA Great Lakes
## 116                         NA            NA            NA Great Lakes
## 117                         NA            NA            NA Great Lakes
## 118                         NA            NA            NA Great Lakes
## 119                         NA            NA            NA Great Lakes
## 120                         NA            NA            NA Great Lakes
## 121                         NA            NA            NA Great Lakes
## 122                         NA            NA            NA Great Lakes
## 123                         NA            NA            NA Great Lakes
## 124                         NA            NA            NA Great Lakes
## 125                         NA            NA            NA Great Lakes
## 126                         NA            NA            NA Great Lakes
## 127                         NA            NA            NA Great Lakes
## 128                         NA            NA            NA Great Lakes
## 129                         NA            NA            NA Great Lakes
## 130                         NA            NA            NA Great Lakes
## 131                         NA            NA            NA Great Lakes
## 132                         NA            NA            NA Great Lakes
## 133                         NA            NA            NA Great Lakes
## 134                         NA            NA            NA Great Lakes
## 135                         NA            NA            NA Great Lakes
## 136                         NA            NA            NA Great Lakes
## 137                         NA            NA            NA Great Lakes
## 138                         NA            NA            NA Great Lakes
## 139                         NA            NA            NA Great Lakes
## 140                         NA            NA            NA Great Lakes
## 141                         NA            NA            NA Great Lakes
## 142                         NA            NA            NA Great Lakes
## 143                         NA            NA            NA Great Lakes
## 144                         NA            NA            NA Great Lakes
## 145                         NA            NA            NA Great Lakes
## 146                         NA            NA            NA Great Lakes
## 147                         NA            NA            NA Great Lakes
## 148                         NA            NA            NA Great Lakes
## 149                         NA            NA            NA Great Lakes
## 150                         NA            NA            NA Great Lakes
## 151                         NA            NA            NA Great Lakes
## 152                         NA            NA            NA Great Lakes
## 153                         NA            NA            NA Great Lakes
## 154                         NA            NA            NA Great Lakes
## 155                         NA            NA            NA Great Lakes
## 156                         NA            NA            NA Great Lakes
## 157                         NA            NA            NA Great Lakes
## 158                         NA            NA            NA Great Lakes
## 159                         NA            NA            NA Great Lakes
## 160                         NA            NA            NA Great Lakes
## 161                         NA            NA            NA Great Lakes
## 162                         NA            NA            NA Great Lakes
## 163                         NA            NA            NA Great Lakes
## 164                         NA            NA            NA Great Lakes
## 165                         NA            NA            NA Great Lakes
## 166                         NA            NA            NA Great Lakes
## 167                         NA            NA            NA Great Lakes
## 168                         NA            NA            NA Great Lakes
## 169                         NA            NA            NA Great Lakes
## 170                         NA            NA            NA Great Lakes
## 171                         NA            NA            NA Great Lakes
## 172                         NA            NA            NA Great Lakes
## 173                         NA            NA            NA Great Lakes
## 174                         NA            NA            NA Great Lakes
## 175                         NA            NA            NA Great Lakes
## 176                         NA            NA            NA Great Lakes
## 177                         NA            NA            NA Great Lakes
## 178                         NA            NA            NA Great Lakes
## 179                         NA            NA            NA Great Lakes
## 180                         NA            NA            NA Great Lakes
## 181                         NA            NA            NA Great Lakes
## 182                         NA            NA            NA Great Lakes
## 183                         NA            NA            NA Great Lakes
## 184                         NA            NA            NA Great Lakes
## 185                         NA            NA            NA Great Lakes
## 186                         NA            NA            NA Great Lakes
## 187                         NA            NA            NA Great Lakes
## 188                         NA            NA            NA Great Lakes
## 189                         NA            NA            NA Great Lakes
## 190                         NA            NA            NA Great Lakes
## 191                         NA            NA            NA Great Lakes
## 192                         NA            NA            NA Great Lakes
## 193                         NA            NA            NA Great Lakes
## 194                         NA            NA            NA Great Lakes
## 195                         NA            NA            NA Great Lakes
## 196                         NA            NA            NA Great Lakes
## 197                         NA            NA            NA Great Lakes
## 198                         NA            NA            NA Great Lakes
## 199                         NA            NA            NA Great Lakes
## 200                         NA            NA            NA Great Lakes
## 201                         NA            NA            NA Great Lakes
## 202                         NA            NA            NA Great Lakes
## 203                         NA            NA            NA Great Lakes
## 204                         NA            NA            NA Great Lakes
## 205                         NA            NA            NA Great Lakes
## 206                         NA            NA            NA Great Lakes
## 207                         NA            NA            NA Great Lakes
## 208                         NA            NA            NA Great Lakes
## 209                         NA            NA            NA Great Lakes
## 210                         NA            NA            NA Great Lakes
## 211                         NA            NA            NA Great Lakes
## 212                         NA            NA            NA Great Lakes
## 213                         NA            NA            NA Great Lakes
## 214                         NA            NA            NA Great Lakes
## 215                         NA            NA            NA Great Lakes
## 216                         NA            NA            NA Great Lakes
## 217                         NA            NA            NA Great Lakes
## 218                         NA            NA            NA Great Lakes
## 219                         NA            NA            NA Great Lakes
## 220                         NA            NA            NA Great Lakes
## 221                         NA            NA            NA Great Lakes
## 222                         NA            NA            NA Great Lakes
## 223                         NA            NA            NA Great Lakes
## 224                         NA            NA            NA Great Lakes
## 225                         NA            NA            NA Great Lakes
## 226                         NA            NA            NA Great Lakes
## 227                         NA            NA            NA Great Lakes
## 228                         NA            NA            NA Great Lakes
## 229                         NA            NA            NA Great Lakes
## 230                         NA            NA            NA Great Lakes
## 231                         NA            NA            NA Great Lakes
## 232                         NA            NA            NA Great Lakes
## 233                         NA            NA            NA Great Lakes
## 234                         NA            NA            NA Great Lakes
## 235                         NA            NA            NA Great Lakes
## 236                         NA            NA            NA Great Lakes
## 237                         NA            NA            NA Great Lakes
## 238                         NA            NA            NA Great Lakes
## 239                         NA            NA            NA Great Lakes
## 240                         NA            NA            NA Great Lakes
## 241                         NA            NA            NA Great Lakes
## 242                         NA            NA            NA Great Lakes
## 243                         NA            NA            NA Great Lakes
## 244                         NA            NA            NA Great Lakes
## 245                         NA            NA            NA Great Lakes
## 246                         NA            NA            NA Great Lakes
## 247                         NA            NA            NA Great Lakes
## 248                         NA            NA            NA Great Lakes
## 249                         NA            NA            NA Great Lakes
## 250                         NA            NA            NA Great Lakes
## 251                         NA            NA            NA Great Lakes
## 252                         NA            NA            NA Great Lakes
## 253                         NA            NA            NA Great Lakes
## 254                         NA            NA            NA Great Lakes
## 255                         NA            NA            NA Great Lakes
## 256                         NA            NA            NA Great Lakes
## 257                         NA            NA            NA Great Lakes
## 258                         NA            NA            NA Great Lakes
## 259                         NA            NA            NA Great Lakes
## 260                         NA            NA            NA Great Lakes
##     rickettsia_digestive_tubule rickettsia_gut sample_letter sample_number
## 1                            NA             NA             a             1
## 2                            NA             NA             a             2
## 3                            NA             NA             a             3
## 4                            NA             NA             a             4
## 5                            NA             NA             a             5
## 6                            NA             NA             a             1
## 7                            NA             NA             a             2
## 8                            NA             NA             a             3
## 9                            NA             NA             a             4
## 10                           NA             NA             a             5
## 11                           NA             NA             a             1
## 12                           NA             NA             a             2
## 13                           NA             NA             a             3
## 14                           NA             NA             a             4
## 15                           NA             NA             a             5
## 16                           NA             NA             a             1
## 17                           NA             NA             a             2
## 18                           NA             NA             a             3
## 19                           NA             NA             a             4
## 20                           NA             NA             a             5
## 21                           NA             NA             a             1
## 22                           NA             NA             a             2
## 23                           NA             NA             a             3
## 24                           NA             NA             a             4
## 25                           NA             NA             a             5
## 26                           NA             NA             a             1
## 27                           NA             NA             a             2
## 28                           NA             NA             a             3
## 29                           NA             NA             a             4
## 30                           NA             NA             a             5
## 31                           NA             NA             a             1
## 32                           NA             NA             a             2
## 33                           NA             NA             a             3
## 34                           NA             NA             a             4
## 35                           NA             NA             a             5
## 36                           NA             NA             a             1
## 37                           NA             NA             a             2
## 38                           NA             NA             a             3
## 39                           NA             NA             a             4
## 40                           NA             NA             a             5
## 41                           NA             NA             a             1
## 42                           NA             NA             a             2
## 43                           NA             NA             a             3
## 44                           NA             NA             a             4
## 45                           NA             NA             a             5
## 46                           NA             NA             a             1
## 47                           NA             NA             a             2
## 48                           NA             NA             a             3
## 49                           NA             NA             a             4
## 50                           NA             NA             a             5
## 51                           NA             NA             a             1
## 52                           NA             NA             a             2
## 53                           NA             NA             a             3
## 54                           NA             NA             a             4
## 55                           NA             NA             a             5
## 56                           NA             NA             a             1
## 57                           NA             NA             a             2
## 58                           NA             NA             a             3
## 59                           NA             NA             a             4
## 60                           NA             NA             a             5
## 61                           NA             NA             a             1
## 62                           NA             NA             a             2
## 63                           NA             NA             a             3
## 64                           NA             NA             a             4
## 65                           NA             NA             a             5
## 66                           NA             NA             a             1
## 67                           NA             NA             a             2
## 68                           NA             NA             a             3
## 69                           NA             NA             a             4
## 70                           NA             NA             a             5
## 71                           NA             NA             a             1
## 72                           NA             NA             a             2
## 73                           NA             NA             a             3
## 74                           NA             NA             a             4
## 75                           NA             NA             a             5
## 76                           NA             NA             a             1
## 77                           NA             NA             a             2
## 78                           NA             NA             a             3
## 79                           NA             NA             a             4
## 80                           NA             NA             a             5
## 81                           NA             NA             a             1
## 82                           NA             NA             a             2
## 83                           NA             NA             a             3
## 84                           NA             NA             a             4
## 85                           NA             NA             a             5
## 86                           NA             NA             a             1
## 87                           NA             NA             a             2
## 88                           NA             NA             a             3
## 89                           NA             NA             a             4
## 90                           NA             NA             a             5
## 91                           NA             NA             b             1
## 92                           NA             NA             b             2
## 93                           NA             NA             b             3
## 94                           NA             NA             b             4
## 95                           NA             NA             b             5
## 96                           NA             NA             a             1
## 97                           NA             NA             a             2
## 98                           NA             NA             a             3
## 99                           NA             NA             a             4
## 100                          NA             NA             a             5
## 101                          NA             NA             a             1
## 102                          NA             NA             a             2
## 103                          NA             NA             a             3
## 104                          NA             NA             a             4
## 105                          NA             NA             a             5
## 106                          NA             NA             a             1
## 107                          NA             NA             a             2
## 108                          NA             NA             a             3
## 109                          NA             NA             a             4
## 110                          NA             NA             a             5
## 111                          NA             NA             a             1
## 112                          NA             NA             a             2
## 113                          NA             NA             a             3
## 114                          NA             NA             a             4
## 115                          NA             NA             a             5
## 116                          NA             NA             a             1
## 117                          NA             NA             a             2
## 118                          NA             NA             a             3
## 119                          NA             NA             a             4
## 120                          NA             NA             a             5
## 121                          NA             NA             a             1
## 122                          NA             NA             a             2
## 123                          NA             NA             a             3
## 124                          NA             NA             a             4
## 125                          NA             NA             a             5
## 126                          NA             NA             b             1
## 127                          NA             NA             b             2
## 128                          NA             NA             b             3
## 129                          NA             NA             b             4
## 130                          NA             NA             b             5
## 131                          NA             NA             c             1
## 132                          NA             NA             c             2
## 133                          NA             NA             c             3
## 134                          NA             NA             c             4
## 135                          NA             NA             c             5
## 136                          NA             NA             a             1
## 137                          NA             NA             a             2
## 138                          NA             NA             a             3
## 139                          NA             NA             a             4
## 140                          NA             NA             a             5
## 141                          NA             NA             a             1
## 142                          NA             NA             a             2
## 143                          NA             NA             a             3
## 144                          NA             NA             a             4
## 145                          NA             NA             a             5
## 146                          NA             NA             a             1
## 147                          NA             NA             a             2
## 148                          NA             NA             a             3
## 149                          NA             NA             a             4
## 150                          NA             NA             a             5
## 151                          NA             NA             a             1
## 152                          NA             NA             a             2
## 153                          NA             NA             a             3
## 154                          NA             NA             a             4
## 155                          NA             NA             a             5
## 156                          NA             NA             a             1
## 157                          NA             NA             a             2
## 158                          NA             NA             a             3
## 159                          NA             NA             a             4
## 160                          NA             NA             a             5
## 161                          NA             NA             a             1
## 162                          NA             NA             a             2
## 163                          NA             NA             a             3
## 164                          NA             NA             a             4
## 165                          NA             NA             a             5
## 166                          NA             NA             a             1
## 167                          NA             NA             a             2
## 168                          NA             NA             a             3
## 169                          NA             NA             a             4
## 170                          NA             NA             a             5
## 171                          NA             NA             a             1
## 172                          NA             NA             a             2
## 173                          NA             NA             a             3
## 174                          NA             NA             a             4
## 175                          NA             NA             a             5
## 176                          NA             NA             a             1
## 177                          NA             NA             a             2
## 178                          NA             NA             a             3
## 179                          NA             NA             a             4
## 180                          NA             NA             a             5
## 181                          NA             NA             a             1
## 182                          NA             NA             a             2
## 183                          NA             NA             a             3
## 184                          NA             NA             a             4
## 185                          NA             NA             a             5
## 186                          NA             NA             a             1
## 187                          NA             NA             a             2
## 188                          NA             NA             a             3
## 189                          NA             NA             a             4
## 190                          NA             NA             a             5
## 191                          NA             NA             a             1
## 192                          NA             NA             a             2
## 193                          NA             NA             a             3
## 194                          NA             NA             a             4
## 195                          NA             NA             a             5
## 196                          NA             NA             a             1
## 197                          NA             NA             a             2
## 198                          NA             NA             a             3
## 199                          NA             NA             a             4
## 200                          NA             NA             a             5
## 201                          NA             NA             a             1
## 202                          NA             NA             a             2
## 203                          NA             NA             a             3
## 204                          NA             NA             a             4
## 205                          NA             NA             a             5
## 206                          NA             NA             a             1
## 207                          NA             NA             a             2
## 208                          NA             NA             a             3
## 209                          NA             NA             a             4
## 210                          NA             NA             a             5
## 211                          NA             NA             a             1
## 212                          NA             NA             a             2
## 213                          NA             NA             a             3
## 214                          NA             NA             a             4
## 215                          NA             NA             a             5
## 216                          NA             NA             a             1
## 217                          NA             NA             a             2
## 218                          NA             NA             a             3
## 219                          NA             NA             a             4
## 220                          NA             NA             a             5
## 221                          NA             NA             b             1
## 222                          NA             NA             b             2
## 223                          NA             NA             b             3
## 224                          NA             NA             b             4
## 225                          NA             NA             b             5
## 226                          NA             NA             a             1
## 227                          NA             NA             a             2
## 228                          NA             NA             a             3
## 229                          NA             NA             a             4
## 230                          NA             NA             a             5
## 231                          NA             NA             a             1
## 232                          NA             NA             a             2
## 233                          NA             NA             a             3
## 234                          NA             NA             a             4
## 235                          NA             NA             a             5
## 236                          NA             NA             a             1
## 237                          NA             NA             a             2
## 238                          NA             NA             a             3
## 239                          NA             NA             a             4
## 240                          NA             NA             a             5
## 241                          NA             NA             a             1
## 242                          NA             NA             a             2
## 243                          NA             NA             a             3
## 244                          NA             NA             a             4
## 245                          NA             NA             a             5
## 246                          NA             NA             a             1
## 247                          NA             NA             a             2
## 248                          NA             NA             a             3
## 249                          NA             NA             a             4
## 250                          NA             NA             a             5
## 251                          NA             NA             a             1
## 252                          NA             NA             a             2
## 253                          NA             NA             a             3
## 254                          NA             NA             a             4
## 255                          NA             NA             a             5
## 256                          NA             NA             a             1
## 257                          NA             NA             a             2
## 258                          NA             NA             a             3
## 259                          NA             NA             a             4
## 260                          NA             NA             a             5
##         sex                                source_file
## 1    Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 2   Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 3   Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 4    Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 5      Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 6    Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 7      Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 8   Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 9      Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 10   Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 11   Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 12  Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 13   Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 14  Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 15   Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 16  Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 17  Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 18  Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 19  Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 20  Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 21  Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 22  Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 23  Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 24     Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 25  Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 26  Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 27   Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 28  Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 29  Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 30   Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 31   Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 32  Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 33   Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 34   Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 35  Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 36   Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 37  Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 38   Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 39     Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 40   Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 41  Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 42   Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 43  Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 44   Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 45   Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 46     Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 47     Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 48   Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 49     Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 50   Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 51     Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 52     Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 53     Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 54     Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 55   Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 56   Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 57   Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 58   Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 59     Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 60   Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 61   Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 62   Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 63     Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 64     Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 65   Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 66  Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 67   Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 68   Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 69   Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 70   Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 71   Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 72   Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 73     Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 74     Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 75     Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 76     Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 77   Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 78   Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 79  Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 80     Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 81  Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 82   Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 83  Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 84  Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 85     Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 86     Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 87   Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 88   Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 89   Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 90   Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 91   Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 92  Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 93   Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 94     Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 95     Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 96   Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 97     Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 98   Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 99   Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 100    Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 101    Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 102  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 103  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 104  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 105    Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 106    Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 107  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 108  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 109    Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 110  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 111  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 112 Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 113    Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 114  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 115 Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 116    Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 117    Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 118  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 119 Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 120    Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 121  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 122 Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 123 Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 124  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 125  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 126    Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 127  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 128  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 129    Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 130  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 131  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 132    Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 133 Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 134  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 135 Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 136    Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 137    Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 138    Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 139    Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 140  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 141  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 142  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 143    Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 144  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 145    Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 146  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 147 Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 148    Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 149    Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 150    Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 151  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 152    Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 153    Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 154  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 155 Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 156    Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 157  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 158  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 159  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 160    Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 161  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 162    Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 163    Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 164    Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 165    Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 166 Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 167    Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 168  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 169 Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 170  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 171 Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 172  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 173 Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 174 Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 175  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 176  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 177 Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 178 Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 179 Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 180  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 181  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 182 Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 183 Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 184  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 185 Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 186    Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 187    Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 188  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 189    Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 190    Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 191  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 192  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 193    Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 194  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 195    Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 196 Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 197  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 198  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 199 Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 200  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 201  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 202 Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 203 Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 204  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 205    Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 206  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 207    Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 208  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 209 Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 210 Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 211 Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 212    Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 213  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 214 Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 215 Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 216  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 217    Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 218    Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 219  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 220    Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 221  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 222 Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 223  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 224  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 225  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 226    Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 227    Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 228  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 229 Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 230    Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 231  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 232  Female MusselWatch_GreatLakes_Zebra_Histopath.csv
## 233 Unknown MusselWatch_GreatLakes_Zebra_Histopath.csv
## 234    Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 235    Male MusselWatch_GreatLakes_Zebra_Histopath.csv
## 236    Male GreatLakesSpecialStudy_Zebra_Histopath.csv
## 237    Male GreatLakesSpecialStudy_Zebra_Histopath.csv
## 238  Female GreatLakesSpecialStudy_Zebra_Histopath.csv
## 239  Female GreatLakesSpecialStudy_Zebra_Histopath.csv
## 240    Male GreatLakesSpecialStudy_Zebra_Histopath.csv
## 241 Unknown GreatLakesSpecialStudy_Zebra_Histopath.csv
## 242  Female GreatLakesSpecialStudy_Zebra_Histopath.csv
## 243 Unknown GreatLakesSpecialStudy_Zebra_Histopath.csv
## 244    Male GreatLakesSpecialStudy_Zebra_Histopath.csv
## 245 Unknown GreatLakesSpecialStudy_Zebra_Histopath.csv
## 246    Male GreatLakesSpecialStudy_Zebra_Histopath.csv
## 247 Unknown GreatLakesSpecialStudy_Zebra_Histopath.csv
## 248  Female GreatLakesSpecialStudy_Zebra_Histopath.csv
## 249  Female GreatLakesSpecialStudy_Zebra_Histopath.csv
## 250    Male GreatLakesSpecialStudy_Zebra_Histopath.csv
## 251  Female GreatLakesSpecialStudy_Zebra_Histopath.csv
## 252    Male GreatLakesSpecialStudy_Zebra_Histopath.csv
## 253    Male GreatLakesSpecialStudy_Zebra_Histopath.csv
## 254  Female GreatLakesSpecialStudy_Zebra_Histopath.csv
## 255 Unknown GreatLakesSpecialStudy_Zebra_Histopath.csv
## 256  Female GreatLakesSpecialStudy_Zebra_Histopath.csv
## 257 Unknown GreatLakesSpecialStudy_Zebra_Histopath.csv
## 258  Female GreatLakesSpecialStudy_Zebra_Histopath.csv
## 259  Female GreatLakesSpecialStudy_Zebra_Histopath.csv
## 260 Unknown GreatLakesSpecialStudy_Zebra_Histopath.csv
##             species_name   specific_location specific_region state_name
## 1      Dreissena species       Bayshore Park     Great Lakes  Wisconsin
## 2      Dreissena species       Bayshore Park     Great Lakes  Wisconsin
## 3      Dreissena species       Bayshore Park     Great Lakes  Wisconsin
## 4      Dreissena species       Bayshore Park     Great Lakes  Wisconsin
## 5      Dreissena species       Bayshore Park     Great Lakes  Wisconsin
## 6      Dreissena species       Bayshore Park     Great Lakes  Wisconsin
## 7      Dreissena species       Bayshore Park     Great Lakes  Wisconsin
## 8      Dreissena species       Bayshore Park     Great Lakes  Wisconsin
## 9      Dreissena species       Bayshore Park     Great Lakes  Wisconsin
## 10     Dreissena species       Bayshore Park     Great Lakes  Wisconsin
## 11  Dreissena polymorpha       Bayshore Park     Great Lakes  Wisconsin
## 12  Dreissena polymorpha       Bayshore Park     Great Lakes  Wisconsin
## 13  Dreissena polymorpha       Bayshore Park     Great Lakes  Wisconsin
## 14  Dreissena polymorpha       Bayshore Park     Great Lakes  Wisconsin
## 15  Dreissena polymorpha       Bayshore Park     Great Lakes  Wisconsin
## 16  Dreissena polymorpha       Bayshore Park     Great Lakes  Wisconsin
## 17  Dreissena polymorpha       Bayshore Park     Great Lakes  Wisconsin
## 18  Dreissena polymorpha       Bayshore Park     Great Lakes  Wisconsin
## 19  Dreissena polymorpha       Bayshore Park     Great Lakes  Wisconsin
## 20  Dreissena polymorpha       Bayshore Park     Great Lakes  Wisconsin
## 21  Dreissena polymorpha       Bayshore Park     Great Lakes  Wisconsin
## 22  Dreissena polymorpha       Bayshore Park     Great Lakes  Wisconsin
## 23  Dreissena polymorpha       Bayshore Park     Great Lakes  Wisconsin
## 24  Dreissena polymorpha       Bayshore Park     Great Lakes  Wisconsin
## 25  Dreissena polymorpha       Bayshore Park     Great Lakes  Wisconsin
## 26  Dreissena polymorpha       Bayshore Park     Great Lakes  Wisconsin
## 27  Dreissena polymorpha       Bayshore Park     Great Lakes  Wisconsin
## 28  Dreissena polymorpha       Bayshore Park     Great Lakes  Wisconsin
## 29  Dreissena polymorpha       Bayshore Park     Great Lakes  Wisconsin
## 30  Dreissena polymorpha       Bayshore Park     Great Lakes  Wisconsin
## 31    Dreissena bugensis       Bayshore Park     Great Lakes  Wisconsin
## 32    Dreissena bugensis       Bayshore Park     Great Lakes  Wisconsin
## 33    Dreissena bugensis       Bayshore Park     Great Lakes  Wisconsin
## 34    Dreissena bugensis       Bayshore Park     Great Lakes  Wisconsin
## 35    Dreissena bugensis       Bayshore Park     Great Lakes  Wisconsin
## 36     Dreissena species  Calumet Breakwater     Great Lakes    Indiana
## 37     Dreissena species  Calumet Breakwater     Great Lakes    Indiana
## 38     Dreissena species  Calumet Breakwater     Great Lakes    Indiana
## 39     Dreissena species  Calumet Breakwater     Great Lakes    Indiana
## 40     Dreissena species  Calumet Breakwater     Great Lakes    Indiana
## 41  Dreissena polymorpha  Calumet Breakwater     Great Lakes    Indiana
## 42  Dreissena polymorpha  Calumet Breakwater     Great Lakes    Indiana
## 43  Dreissena polymorpha  Calumet Breakwater     Great Lakes    Indiana
## 44  Dreissena polymorpha  Calumet Breakwater     Great Lakes    Indiana
## 45  Dreissena polymorpha  Calumet Breakwater     Great Lakes    Indiana
## 46    Dreissena bugensis  Calumet Breakwater     Great Lakes    Indiana
## 47    Dreissena bugensis  Calumet Breakwater     Great Lakes    Indiana
## 48    Dreissena bugensis  Calumet Breakwater     Great Lakes    Indiana
## 49    Dreissena bugensis  Calumet Breakwater     Great Lakes    Indiana
## 50    Dreissena bugensis  Calumet Breakwater     Great Lakes    Indiana
## 51     Dreissena species  Holland Breakwater     Great Lakes   Michigan
## 52     Dreissena species  Holland Breakwater     Great Lakes   Michigan
## 53     Dreissena species  Holland Breakwater     Great Lakes   Michigan
## 54     Dreissena species  Holland Breakwater     Great Lakes   Michigan
## 55     Dreissena species  Holland Breakwater     Great Lakes   Michigan
## 56     Dreissena species  Holland Breakwater     Great Lakes   Michigan
## 57     Dreissena species  Holland Breakwater     Great Lakes   Michigan
## 58     Dreissena species  Holland Breakwater     Great Lakes   Michigan
## 59     Dreissena species  Holland Breakwater     Great Lakes   Michigan
## 60     Dreissena species  Holland Breakwater     Great Lakes   Michigan
## 61  Dreissena polymorpha  Holland Breakwater     Great Lakes   Michigan
## 62  Dreissena polymorpha  Holland Breakwater     Great Lakes   Michigan
## 63  Dreissena polymorpha  Holland Breakwater     Great Lakes   Michigan
## 64  Dreissena polymorpha  Holland Breakwater     Great Lakes   Michigan
## 65  Dreissena polymorpha  Holland Breakwater     Great Lakes   Michigan
## 66  Dreissena polymorpha  Holland Breakwater     Great Lakes   Michigan
## 67  Dreissena polymorpha  Holland Breakwater     Great Lakes   Michigan
## 68  Dreissena polymorpha  Holland Breakwater     Great Lakes   Michigan
## 69  Dreissena polymorpha  Holland Breakwater     Great Lakes   Michigan
## 70  Dreissena polymorpha  Holland Breakwater     Great Lakes   Michigan
## 71    Dreissena bugensis  Holland Breakwater     Great Lakes   Michigan
## 72    Dreissena bugensis  Holland Breakwater     Great Lakes   Michigan
## 73    Dreissena bugensis  Holland Breakwater     Great Lakes   Michigan
## 74    Dreissena bugensis  Holland Breakwater     Great Lakes   Michigan
## 75    Dreissena bugensis  Holland Breakwater     Great Lakes   Michigan
## 76    Dreissena bugensis  Holland Breakwater     Great Lakes   Michigan
## 77    Dreissena bugensis  Holland Breakwater     Great Lakes   Michigan
## 78    Dreissena bugensis  Holland Breakwater     Great Lakes   Michigan
## 79    Dreissena bugensis  Holland Breakwater     Great Lakes   Michigan
## 80    Dreissena bugensis  Holland Breakwater     Great Lakes   Michigan
## 81  Dreissena polymorpha       Hammod Marina     Great Lakes    Indiana
## 82  Dreissena polymorpha       Hammod Marina     Great Lakes    Indiana
## 83  Dreissena polymorpha       Hammod Marina     Great Lakes    Indiana
## 84  Dreissena polymorpha       Hammod Marina     Great Lakes    Indiana
## 85  Dreissena polymorpha       Hammod Marina     Great Lakes    Indiana
## 86    Dreissena bugensis       Hammod Marina     Great Lakes    Indiana
## 87    Dreissena bugensis       Hammod Marina     Great Lakes    Indiana
## 88    Dreissena bugensis       Hammod Marina     Great Lakes    Indiana
## 89    Dreissena bugensis       Hammod Marina     Great Lakes    Indiana
## 90    Dreissena bugensis       Hammod Marina     Great Lakes    Indiana
## 91  Dreissena polymorpha       Hammod Marina     Great Lakes    Indiana
## 92  Dreissena polymorpha       Hammod Marina     Great Lakes    Indiana
## 93  Dreissena polymorpha       Hammod Marina     Great Lakes    Indiana
## 94  Dreissena polymorpha       Hammod Marina     Great Lakes    Indiana
## 95  Dreissena polymorpha       Hammod Marina     Great Lakes    Indiana
## 96    Dreissena bugensis       Hammod Marina     Great Lakes    Indiana
## 97    Dreissena bugensis       Hammod Marina     Great Lakes    Indiana
## 98    Dreissena bugensis       Hammod Marina     Great Lakes    Indiana
## 99    Dreissena bugensis       Hammod Marina     Great Lakes    Indiana
## 100   Dreissena bugensis       Hammod Marina     Great Lakes    Indiana
## 101    Dreissena species       Milwaukee Bay     Great Lakes  Wisconsin
## 102    Dreissena species       Milwaukee Bay     Great Lakes  Wisconsin
## 103    Dreissena species       Milwaukee Bay     Great Lakes  Wisconsin
## 104    Dreissena species       Milwaukee Bay     Great Lakes  Wisconsin
## 105    Dreissena species       Milwaukee Bay     Great Lakes  Wisconsin
## 106    Dreissena species       Milwaukee Bay     Great Lakes  Wisconsin
## 107    Dreissena species       Milwaukee Bay     Great Lakes  Wisconsin
## 108    Dreissena species       Milwaukee Bay     Great Lakes  Wisconsin
## 109    Dreissena species       Milwaukee Bay     Great Lakes  Wisconsin
## 110    Dreissena species       Milwaukee Bay     Great Lakes  Wisconsin
## 111 Dreissena polymorpha       Milwaukee Bay     Great Lakes  Wisconsin
## 112 Dreissena polymorpha       Milwaukee Bay     Great Lakes  Wisconsin
## 113 Dreissena polymorpha       Milwaukee Bay     Great Lakes  Wisconsin
## 114 Dreissena polymorpha       Milwaukee Bay     Great Lakes  Wisconsin
## 115 Dreissena polymorpha       Milwaukee Bay     Great Lakes  Wisconsin
## 116 Dreissena polymorpha       Milwaukee Bay     Great Lakes  Wisconsin
## 117 Dreissena polymorpha       Milwaukee Bay     Great Lakes  Wisconsin
## 118 Dreissena polymorpha       Milwaukee Bay     Great Lakes  Wisconsin
## 119 Dreissena polymorpha       Milwaukee Bay     Great Lakes  Wisconsin
## 120 Dreissena polymorpha       Milwaukee Bay     Great Lakes  Wisconsin
## 121    Dreissena species       Milwaukee Bay     Great Lakes  Wisconsin
## 122    Dreissena species       Milwaukee Bay     Great Lakes  Wisconsin
## 123    Dreissena species       Milwaukee Bay     Great Lakes  Wisconsin
## 124    Dreissena species       Milwaukee Bay     Great Lakes  Wisconsin
## 125    Dreissena species       Milwaukee Bay     Great Lakes  Wisconsin
## 126   Dreissena bugensis       Milwaukee Bay     Great Lakes  Wisconsin
## 127   Dreissena bugensis       Milwaukee Bay     Great Lakes  Wisconsin
## 128   Dreissena bugensis       Milwaukee Bay     Great Lakes  Wisconsin
## 129   Dreissena bugensis       Milwaukee Bay     Great Lakes  Wisconsin
## 130   Dreissena bugensis       Milwaukee Bay     Great Lakes  Wisconsin
## 131 Dreissena polymorpha       Milwaukee Bay     Great Lakes  Wisconsin
## 132 Dreissena polymorpha       Milwaukee Bay     Great Lakes  Wisconsin
## 133 Dreissena polymorpha       Milwaukee Bay     Great Lakes  Wisconsin
## 134 Dreissena polymorpha       Milwaukee Bay     Great Lakes  Wisconsin
## 135 Dreissena polymorpha       Milwaukee Bay     Great Lakes  Wisconsin
## 136    Dreissena species            Muskegon     Great Lakes   Michigan
## 137    Dreissena species            Muskegon     Great Lakes   Michigan
## 138    Dreissena species            Muskegon     Great Lakes   Michigan
## 139    Dreissena species            Muskegon     Great Lakes   Michigan
## 140    Dreissena species            Muskegon     Great Lakes   Michigan
## 141    Dreissena species            Muskegon     Great Lakes   Michigan
## 142    Dreissena species            Muskegon     Great Lakes   Michigan
## 143    Dreissena species            Muskegon     Great Lakes   Michigan
## 144    Dreissena species            Muskegon     Great Lakes   Michigan
## 145    Dreissena species            Muskegon     Great Lakes   Michigan
## 146 Dreissena polymorpha            Muskegon     Great Lakes   Michigan
## 147 Dreissena polymorpha            Muskegon     Great Lakes   Michigan
## 148 Dreissena polymorpha            Muskegon     Great Lakes   Michigan
## 149 Dreissena polymorpha            Muskegon     Great Lakes   Michigan
## 150 Dreissena polymorpha            Muskegon     Great Lakes   Michigan
## 151 Dreissena polymorpha            Muskegon     Great Lakes   Michigan
## 152 Dreissena polymorpha            Muskegon     Great Lakes   Michigan
## 153 Dreissena polymorpha            Muskegon     Great Lakes   Michigan
## 154 Dreissena polymorpha            Muskegon     Great Lakes   Michigan
## 155 Dreissena polymorpha            Muskegon     Great Lakes   Michigan
## 156   Dreissena bugensis            Muskegon     Great Lakes   Michigan
## 157   Dreissena bugensis            Muskegon     Great Lakes   Michigan
## 158   Dreissena bugensis            Muskegon     Great Lakes   Michigan
## 159   Dreissena bugensis            Muskegon     Great Lakes   Michigan
## 160   Dreissena bugensis            Muskegon     Great Lakes   Michigan
## 161   Dreissena bugensis            Muskegon     Great Lakes   Michigan
## 162   Dreissena bugensis            Muskegon     Great Lakes   Michigan
## 163   Dreissena bugensis            Muskegon     Great Lakes   Michigan
## 164   Dreissena bugensis            Muskegon     Great Lakes   Michigan
## 165   Dreissena bugensis            Muskegon     Great Lakes   Michigan
## 166    Dreissena species       North Chicago     Great Lakes   Illinois
## 167    Dreissena species       North Chicago     Great Lakes   Illinois
## 168    Dreissena species       North Chicago     Great Lakes   Illinois
## 169    Dreissena species       North Chicago     Great Lakes   Illinois
## 170    Dreissena species       North Chicago     Great Lakes   Illinois
## 171    Dreissena species       North Chicago     Great Lakes   Illinois
## 172    Dreissena species       North Chicago     Great Lakes   Illinois
## 173    Dreissena species       North Chicago     Great Lakes   Illinois
## 174    Dreissena species       North Chicago     Great Lakes   Illinois
## 175    Dreissena species       North Chicago     Great Lakes   Illinois
## 176 Dreissena polymorpha       North Chicago     Great Lakes   Illinois
## 177 Dreissena polymorpha       North Chicago     Great Lakes   Illinois
## 178 Dreissena polymorpha       North Chicago     Great Lakes   Illinois
## 179 Dreissena polymorpha       North Chicago     Great Lakes   Illinois
## 180 Dreissena polymorpha       North Chicago     Great Lakes   Illinois
## 181 Dreissena polymorpha       North Chicago     Great Lakes   Illinois
## 182 Dreissena polymorpha       North Chicago     Great Lakes   Illinois
## 183 Dreissena polymorpha       North Chicago     Great Lakes   Illinois
## 184 Dreissena polymorpha       North Chicago     Great Lakes   Illinois
## 185 Dreissena polymorpha       North Chicago     Great Lakes   Illinois
## 186   Dreissena bugensis       North Chicago     Great Lakes   Illinois
## 187   Dreissena bugensis       North Chicago     Great Lakes   Illinois
## 188   Dreissena bugensis       North Chicago     Great Lakes   Illinois
## 189   Dreissena bugensis       North Chicago     Great Lakes   Illinois
## 190   Dreissena bugensis       North Chicago     Great Lakes   Illinois
## 191   Dreissena bugensis       North Chicago     Great Lakes   Illinois
## 192   Dreissena bugensis       North Chicago     Great Lakes   Illinois
## 193   Dreissena bugensis       North Chicago     Great Lakes   Illinois
## 194   Dreissena bugensis       North Chicago     Great Lakes   Illinois
## 195   Dreissena bugensis       North Chicago     Great Lakes   Illinois
## 196    Dreissena species Leelanau State Park     Great Lakes   Michigan
## 197    Dreissena species Leelanau State Park     Great Lakes   Michigan
## 198    Dreissena species Leelanau State Park     Great Lakes   Michigan
## 199    Dreissena species Leelanau State Park     Great Lakes   Michigan
## 200    Dreissena species Leelanau State Park     Great Lakes   Michigan
## 201    Dreissena species Leelanau State Park     Great Lakes   Michigan
## 202    Dreissena species Leelanau State Park     Great Lakes   Michigan
## 203    Dreissena species Leelanau State Park     Great Lakes   Michigan
## 204    Dreissena species Leelanau State Park     Great Lakes   Michigan
## 205    Dreissena species Leelanau State Park     Great Lakes   Michigan
## 206 Dreissena polymorpha Leelanau State Park     Great Lakes   Michigan
## 207 Dreissena polymorpha Leelanau State Park     Great Lakes   Michigan
## 208 Dreissena polymorpha Leelanau State Park     Great Lakes   Michigan
## 209 Dreissena polymorpha Leelanau State Park     Great Lakes   Michigan
## 210 Dreissena polymorpha Leelanau State Park     Great Lakes   Michigan
## 211 Dreissena polymorpha Leelanau State Park     Great Lakes   Michigan
## 212 Dreissena polymorpha Leelanau State Park     Great Lakes   Michigan
## 213 Dreissena polymorpha Leelanau State Park     Great Lakes   Michigan
## 214 Dreissena polymorpha Leelanau State Park     Great Lakes   Michigan
## 215 Dreissena polymorpha Leelanau State Park     Great Lakes   Michigan
## 216   Dreissena bugensis Leelanau State Park     Great Lakes   Michigan
## 217   Dreissena bugensis Leelanau State Park     Great Lakes   Michigan
## 218   Dreissena bugensis Leelanau State Park     Great Lakes   Michigan
## 219   Dreissena bugensis Leelanau State Park     Great Lakes   Michigan
## 220   Dreissena bugensis Leelanau State Park     Great Lakes   Michigan
## 221 Dreissena polymorpha Leelanau State Park     Great Lakes   Michigan
## 222 Dreissena polymorpha Leelanau State Park     Great Lakes   Michigan
## 223 Dreissena polymorpha Leelanau State Park     Great Lakes   Michigan
## 224 Dreissena polymorpha Leelanau State Park     Great Lakes   Michigan
## 225 Dreissena polymorpha Leelanau State Park     Great Lakes   Michigan
## 226   Dreissena bugensis Leelanau State Park     Great Lakes   Michigan
## 227   Dreissena bugensis Leelanau State Park     Great Lakes   Michigan
## 228   Dreissena bugensis Leelanau State Park     Great Lakes   Michigan
## 229   Dreissena bugensis Leelanau State Park     Great Lakes   Michigan
## 230   Dreissena bugensis Leelanau State Park     Great Lakes   Michigan
## 231   Dreissena bugensis Leelanau State Park     Great Lakes   Michigan
## 232   Dreissena bugensis Leelanau State Park     Great Lakes   Michigan
## 233   Dreissena bugensis Leelanau State Park     Great Lakes   Michigan
## 234   Dreissena bugensis Leelanau State Park     Great Lakes   Michigan
## 235   Dreissena bugensis Leelanau State Park     Great Lakes   Michigan
## 236 Dreissena polymorpha  Holland Breakwater     Great Lakes   Michigan
## 237 Dreissena polymorpha  Holland Breakwater     Great Lakes   Michigan
## 238 Dreissena polymorpha  Holland Breakwater     Great Lakes   Michigan
## 239 Dreissena polymorpha  Holland Breakwater     Great Lakes   Michigan
## 240 Dreissena polymorpha  Holland Breakwater     Great Lakes   Michigan
## 241 Dreissena polymorpha       Hammod Marina     Great Lakes    Indiana
## 242 Dreissena polymorpha       Hammod Marina     Great Lakes    Indiana
## 243 Dreissena polymorpha       Hammod Marina     Great Lakes    Indiana
## 244 Dreissena polymorpha       Hammod Marina     Great Lakes    Indiana
## 245 Dreissena polymorpha       Hammod Marina     Great Lakes    Indiana
## 246 Dreissena polymorpha       Milwaukee Bay     Great Lakes  Wisconsin
## 247 Dreissena polymorpha       Milwaukee Bay     Great Lakes  Wisconsin
## 248 Dreissena polymorpha       Milwaukee Bay     Great Lakes  Wisconsin
## 249 Dreissena polymorpha       Milwaukee Bay     Great Lakes  Wisconsin
## 250 Dreissena polymorpha       Milwaukee Bay     Great Lakes  Wisconsin
## 251 Dreissena polymorpha            Muskegon     Great Lakes   Michigan
## 252 Dreissena polymorpha            Muskegon     Great Lakes   Michigan
## 253 Dreissena polymorpha            Muskegon     Great Lakes   Michigan
## 254 Dreissena polymorpha            Muskegon     Great Lakes   Michigan
## 255 Dreissena polymorpha            Muskegon     Great Lakes   Michigan
## 256 Dreissena polymorpha       North Chicago     Great Lakes   Illinois
## 257 Dreissena polymorpha       North Chicago     Great Lakes   Illinois
## 258 Dreissena polymorpha       North Chicago     Great Lakes   Illinois
## 259 Dreissena polymorpha       North Chicago     Great Lakes   Illinois
## 260 Dreissena polymorpha       North Chicago     Great Lakes   Illinois
##     station_letter                study_name trematode_metacercariae
## 1                               Mussel Watch                       0
## 2                               Mussel Watch                       0
## 3                               Mussel Watch                       0
## 4                               Mussel Watch                       0
## 5                               Mussel Watch                       0
## 6                               Mussel Watch                       0
## 7                               Mussel Watch                       0
## 8                               Mussel Watch                       0
## 9                               Mussel Watch                       0
## 10                              Mussel Watch                       0
## 11                              Mussel Watch                       0
## 12                              Mussel Watch                       0
## 13                              Mussel Watch                       0
## 14                              Mussel Watch                       0
## 15                              Mussel Watch                       0
## 16                              Mussel Watch                       0
## 17                              Mussel Watch                       0
## 18                              Mussel Watch                       0
## 19                              Mussel Watch                       0
## 20                              Mussel Watch                       0
## 21                              Mussel Watch                       0
## 22                              Mussel Watch                       0
## 23                              Mussel Watch                       0
## 24                              Mussel Watch                       0
## 25                              Mussel Watch                       0
## 26                              Mussel Watch                       0
## 27                              Mussel Watch                       0
## 28                              Mussel Watch                       0
## 29                              Mussel Watch                       0
## 30                              Mussel Watch                       0
## 31                              Mussel Watch                       0
## 32                              Mussel Watch                       0
## 33                              Mussel Watch                       0
## 34                              Mussel Watch                       0
## 35                              Mussel Watch                       0
## 36                              Mussel Watch                       0
## 37                              Mussel Watch                       0
## 38                              Mussel Watch                       0
## 39                              Mussel Watch                       0
## 40                              Mussel Watch                       0
## 41                              Mussel Watch                       0
## 42                              Mussel Watch                       0
## 43                              Mussel Watch                       0
## 44                              Mussel Watch                       0
## 45                              Mussel Watch                       0
## 46                              Mussel Watch                       0
## 47                              Mussel Watch                       0
## 48                              Mussel Watch                       0
## 49                              Mussel Watch                       0
## 50                              Mussel Watch                       0
## 51                              Mussel Watch                       0
## 52                              Mussel Watch                       0
## 53                              Mussel Watch                       0
## 54                              Mussel Watch                       0
## 55                              Mussel Watch                       0
## 56                              Mussel Watch                       0
## 57                              Mussel Watch                       0
## 58                              Mussel Watch                       0
## 59                              Mussel Watch                       0
## 60                              Mussel Watch                       0
## 61                              Mussel Watch                       0
## 62                              Mussel Watch                       0
## 63                              Mussel Watch                       0
## 64                              Mussel Watch                       0
## 65                              Mussel Watch                       0
## 66                              Mussel Watch                       0
## 67                              Mussel Watch                       0
## 68                              Mussel Watch                       0
## 69                              Mussel Watch                       0
## 70                              Mussel Watch                       0
## 71                              Mussel Watch                       0
## 72                              Mussel Watch                       0
## 73                              Mussel Watch                       0
## 74                              Mussel Watch                       0
## 75                              Mussel Watch                       0
## 76                              Mussel Watch                       0
## 77                              Mussel Watch                       0
## 78                              Mussel Watch                       0
## 79                              Mussel Watch                       0
## 80                              Mussel Watch                       0
## 81                              Mussel Watch                       0
## 82                              Mussel Watch                       0
## 83                              Mussel Watch                       0
## 84                              Mussel Watch                       0
## 85                              Mussel Watch                       0
## 86                              Mussel Watch                       0
## 87                              Mussel Watch                       0
## 88                              Mussel Watch                       0
## 89                              Mussel Watch                       0
## 90                              Mussel Watch                       0
## 91                              Mussel Watch                       0
## 92                              Mussel Watch                       0
## 93                              Mussel Watch                       0
## 94                              Mussel Watch                       0
## 95                              Mussel Watch                       0
## 96                              Mussel Watch                       0
## 97                              Mussel Watch                       0
## 98                              Mussel Watch                       0
## 99                              Mussel Watch                       0
## 100                             Mussel Watch                       0
## 101                             Mussel Watch                       0
## 102                             Mussel Watch                       0
## 103                             Mussel Watch                       0
## 104                             Mussel Watch                       0
## 105                             Mussel Watch                       0
## 106                             Mussel Watch                       0
## 107                             Mussel Watch                       0
## 108                             Mussel Watch                       0
## 109                             Mussel Watch                       0
## 110                             Mussel Watch                       0
## 111                             Mussel Watch                       0
## 112                             Mussel Watch                       8
## 113                             Mussel Watch                       0
## 114                             Mussel Watch                       0
## 115                             Mussel Watch                       0
## 116                             Mussel Watch                       0
## 117                             Mussel Watch                       0
## 118                             Mussel Watch                       0
## 119                             Mussel Watch                       0
## 120                             Mussel Watch                       0
## 121                             Mussel Watch                       0
## 122                             Mussel Watch                       0
## 123                             Mussel Watch                       0
## 124                             Mussel Watch                       0
## 125                             Mussel Watch                       0
## 126                             Mussel Watch                       0
## 127                             Mussel Watch                       0
## 128                             Mussel Watch                       0
## 129                             Mussel Watch                       0
## 130                             Mussel Watch                       0
## 131                             Mussel Watch                       0
## 132                             Mussel Watch                       0
## 133                             Mussel Watch                       0
## 134                             Mussel Watch                       0
## 135                             Mussel Watch                       0
## 136                             Mussel Watch                       0
## 137                             Mussel Watch                       0
## 138                             Mussel Watch                       0
## 139                             Mussel Watch                       0
## 140                             Mussel Watch                       0
## 141                             Mussel Watch                       0
## 142                             Mussel Watch                       0
## 143                             Mussel Watch                       0
## 144                             Mussel Watch                       0
## 145                             Mussel Watch                       0
## 146                             Mussel Watch                       0
## 147                             Mussel Watch                       0
## 148                             Mussel Watch                       0
## 149                             Mussel Watch                       0
## 150                             Mussel Watch                       0
## 151                             Mussel Watch                       0
## 152                             Mussel Watch                       0
## 153                             Mussel Watch                       0
## 154                             Mussel Watch                       0
## 155                             Mussel Watch                       0
## 156                             Mussel Watch                       0
## 157                             Mussel Watch                       0
## 158                             Mussel Watch                       0
## 159                             Mussel Watch                       0
## 160                             Mussel Watch                       0
## 161                             Mussel Watch                       0
## 162                             Mussel Watch                       0
## 163                             Mussel Watch                       0
## 164                             Mussel Watch                       0
## 165                             Mussel Watch                       0
## 166                             Mussel Watch                       0
## 167                             Mussel Watch                       0
## 168                             Mussel Watch                       0
## 169                             Mussel Watch                       0
## 170                             Mussel Watch                       0
## 171                             Mussel Watch                       0
## 172                             Mussel Watch                       0
## 173                             Mussel Watch                       0
## 174                             Mussel Watch                       0
## 175                             Mussel Watch                       0
## 176                             Mussel Watch                       0
## 177                             Mussel Watch                       0
## 178                             Mussel Watch                       0
## 179                             Mussel Watch                       0
## 180                             Mussel Watch                       0
## 181                             Mussel Watch                       0
## 182                             Mussel Watch                       0
## 183                             Mussel Watch                       0
## 184                             Mussel Watch                       0
## 185                             Mussel Watch                       0
## 186                             Mussel Watch                       0
## 187                             Mussel Watch                       0
## 188                             Mussel Watch                       0
## 189                             Mussel Watch                       0
## 190                             Mussel Watch                       0
## 191                             Mussel Watch                       0
## 192                             Mussel Watch                       0
## 193                             Mussel Watch                       0
## 194                             Mussel Watch                       0
## 195                             Mussel Watch                       0
## 196                             Mussel Watch                       0
## 197                             Mussel Watch                       0
## 198                             Mussel Watch                       0
## 199                             Mussel Watch                       0
## 200                             Mussel Watch                       0
## 201                             Mussel Watch                       0
## 202                             Mussel Watch                       0
## 203                             Mussel Watch                       0
## 204                             Mussel Watch                       0
## 205                             Mussel Watch                       0
## 206                             Mussel Watch                       0
## 207                             Mussel Watch                       0
## 208                             Mussel Watch                       0
## 209                             Mussel Watch                       0
## 210                             Mussel Watch                       0
## 211                             Mussel Watch                       0
## 212                             Mussel Watch                       0
## 213                             Mussel Watch                       0
## 214                             Mussel Watch                       0
## 215                             Mussel Watch                       0
## 216                             Mussel Watch                       0
## 217                             Mussel Watch                       0
## 218                             Mussel Watch                       0
## 219                             Mussel Watch                       0
## 220                             Mussel Watch                       0
## 221                             Mussel Watch                       0
## 222                             Mussel Watch                       0
## 223                             Mussel Watch                       0
## 224                             Mussel Watch                       0
## 225                             Mussel Watch                       0
## 226                             Mussel Watch                       0
## 227                             Mussel Watch                       0
## 228                             Mussel Watch                       0
## 229                             Mussel Watch                       0
## 230                             Mussel Watch                       0
## 231                             Mussel Watch                       0
## 232                             Mussel Watch                       0
## 233                             Mussel Watch                       0
## 234                             Mussel Watch                       0
## 235                             Mussel Watch                       0
## 236                Great Lakes Special Study                       0
## 237                Great Lakes Special Study                       0
## 238                Great Lakes Special Study                       0
## 239                Great Lakes Special Study                       0
## 240                Great Lakes Special Study                       0
## 241                Great Lakes Special Study                       0
## 242                Great Lakes Special Study                       0
## 243                Great Lakes Special Study                       0
## 244                Great Lakes Special Study                       0
## 245                Great Lakes Special Study                       0
## 246                Great Lakes Special Study                       0
## 247                Great Lakes Special Study                       0
## 248                Great Lakes Special Study                       0
## 249                Great Lakes Special Study                       0
## 250                Great Lakes Special Study                       0
## 251                Great Lakes Special Study                       0
## 252                Great Lakes Special Study                       0
## 253                Great Lakes Special Study                       0
## 254                Great Lakes Special Study                       0
## 255                Great Lakes Special Study                       0
## 256                Great Lakes Special Study                       0
## 257                Great Lakes Special Study                       0
## 258                Great Lakes Special Study                       0
## 259                Great Lakes Special Study                       0
## 260                Great Lakes Special Study                       0
##     trematode_metacercariae_description tumor
## 1                            Uninfected    NA
## 2                            Uninfected    NA
## 3                            Uninfected    NA
## 4                            Uninfected    NA
## 5                            Uninfected    NA
## 6                            Uninfected    NA
## 7                            Uninfected    NA
## 8                            Uninfected    NA
## 9                            Uninfected    NA
## 10                           Uninfected    NA
## 11                           Uninfected    NA
## 12                           Uninfected    NA
## 13                           Uninfected    NA
## 14                           Uninfected    NA
## 15                           Uninfected    NA
## 16                           Uninfected    NA
## 17                           Uninfected    NA
## 18                           Uninfected    NA
## 19                           Uninfected    NA
## 20                           Uninfected    NA
## 21                           Uninfected    NA
## 22                           Uninfected    NA
## 23                           Uninfected    NA
## 24                           Uninfected    NA
## 25                           Uninfected    NA
## 26                           Uninfected    NA
## 27                           Uninfected    NA
## 28                           Uninfected    NA
## 29                           Uninfected    NA
## 30                           Uninfected    NA
## 31                           Uninfected    NA
## 32                           Uninfected    NA
## 33                           Uninfected    NA
## 34                           Uninfected    NA
## 35                           Uninfected    NA
## 36                           Uninfected    NA
## 37                           Uninfected    NA
## 38                           Uninfected    NA
## 39                           Uninfected    NA
## 40                           Uninfected    NA
## 41                           Uninfected    NA
## 42                           Uninfected    NA
## 43                           Uninfected    NA
## 44                           Uninfected    NA
## 45                           Uninfected    NA
## 46                           Uninfected    NA
## 47                           Uninfected    NA
## 48                           Uninfected    NA
## 49                           Uninfected    NA
## 50                           Uninfected    NA
## 51                           Uninfected    NA
## 52                           Uninfected    NA
## 53                           Uninfected    NA
## 54                           Uninfected    NA
## 55                           Uninfected    NA
## 56                           Uninfected    NA
## 57                           Uninfected    NA
## 58                           Uninfected    NA
## 59                           Uninfected    NA
## 60                           Uninfected    NA
## 61                           Uninfected    NA
## 62                           Uninfected    NA
## 63                           Uninfected    NA
## 64                           Uninfected    NA
## 65                           Uninfected    NA
## 66                           Uninfected    NA
## 67                           Uninfected    NA
## 68                           Uninfected    NA
## 69                           Uninfected    NA
## 70                           Uninfected    NA
## 71                           Uninfected    NA
## 72                           Uninfected    NA
## 73                           Uninfected    NA
## 74                           Uninfected    NA
## 75                           Uninfected    NA
## 76                           Uninfected    NA
## 77                           Uninfected    NA
## 78                           Uninfected    NA
## 79                           Uninfected    NA
## 80                           Uninfected    NA
## 81                           Uninfected    NA
## 82                           Uninfected    NA
## 83                           Uninfected    NA
## 84                           Uninfected    NA
## 85                           Uninfected    NA
## 86                           Uninfected    NA
## 87                           Uninfected    NA
## 88                           Uninfected    NA
## 89                           Uninfected    NA
## 90                           Uninfected    NA
## 91                           Uninfected    NA
## 92                           Uninfected    NA
## 93                           Uninfected    NA
## 94                           Uninfected    NA
## 95                           Uninfected    NA
## 96                           Uninfected    NA
## 97                           Uninfected    NA
## 98                           Uninfected    NA
## 99                           Uninfected    NA
## 100                          Uninfected    NA
## 101                          Uninfected    NA
## 102                          Uninfected    NA
## 103                          Uninfected    NA
## 104                          Uninfected    NA
## 105                          Uninfected    NA
## 106                          Uninfected    NA
## 107                          Uninfected    NA
## 108                          Uninfected    NA
## 109                          Uninfected    NA
## 110                          Uninfected    NA
## 111                          Uninfected    NA
## 112                                        NA
## 113                          Uninfected    NA
## 114                          Uninfected    NA
## 115                          Uninfected    NA
## 116                          Uninfected    NA
## 117                          Uninfected    NA
## 118                          Uninfected    NA
## 119                          Uninfected    NA
## 120                          Uninfected    NA
## 121                          Uninfected    NA
## 122                          Uninfected    NA
## 123                          Uninfected    NA
## 124                          Uninfected    NA
## 125                          Uninfected    NA
## 126                          Uninfected    NA
## 127                          Uninfected    NA
## 128                          Uninfected    NA
## 129                          Uninfected    NA
## 130                          Uninfected    NA
## 131                          Uninfected    NA
## 132                          Uninfected    NA
## 133                          Uninfected    NA
## 134                          Uninfected    NA
## 135                          Uninfected    NA
## 136                          Uninfected    NA
## 137                          Uninfected    NA
## 138                          Uninfected    NA
## 139                          Uninfected    NA
## 140                          Uninfected    NA
## 141                          Uninfected    NA
## 142                          Uninfected    NA
## 143                          Uninfected    NA
## 144                          Uninfected    NA
## 145                          Uninfected    NA
## 146                          Uninfected    NA
## 147                          Uninfected    NA
## 148                          Uninfected    NA
## 149                          Uninfected    NA
## 150                          Uninfected    NA
## 151                          Uninfected    NA
## 152                          Uninfected    NA
## 153                          Uninfected    NA
## 154                          Uninfected    NA
## 155                          Uninfected    NA
## 156                          Uninfected    NA
## 157                          Uninfected    NA
## 158                          Uninfected    NA
## 159                          Uninfected    NA
## 160                          Uninfected    NA
## 161                          Uninfected    NA
## 162                          Uninfected    NA
## 163                          Uninfected    NA
## 164                          Uninfected    NA
## 165                          Uninfected    NA
## 166                          Uninfected    NA
## 167                          Uninfected    NA
## 168                          Uninfected    NA
## 169                          Uninfected    NA
## 170                          Uninfected    NA
## 171                          Uninfected    NA
## 172                          Uninfected    NA
## 173                          Uninfected    NA
## 174                          Uninfected    NA
## 175                          Uninfected    NA
## 176                          Uninfected    NA
## 177                          Uninfected    NA
## 178                          Uninfected    NA
## 179                          Uninfected    NA
## 180                          Uninfected    NA
## 181                          Uninfected    NA
## 182                          Uninfected    NA
## 183                          Uninfected    NA
## 184                          Uninfected    NA
## 185                          Uninfected    NA
## 186                          Uninfected    NA
## 187                          Uninfected    NA
## 188                          Uninfected    NA
## 189                          Uninfected    NA
## 190                          Uninfected    NA
## 191                          Uninfected    NA
## 192                          Uninfected    NA
## 193                          Uninfected    NA
## 194                          Uninfected    NA
## 195                          Uninfected    NA
## 196                          Uninfected    NA
## 197                          Uninfected    NA
## 198                          Uninfected    NA
## 199                          Uninfected    NA
## 200                          Uninfected    NA
## 201                          Uninfected    NA
## 202                          Uninfected    NA
## 203                          Uninfected    NA
## 204                          Uninfected    NA
## 205                          Uninfected    NA
## 206                          Uninfected    NA
## 207                          Uninfected    NA
## 208                          Uninfected    NA
## 209                          Uninfected    NA
## 210                          Uninfected    NA
## 211                          Uninfected    NA
## 212                          Uninfected    NA
## 213                          Uninfected    NA
## 214                          Uninfected    NA
## 215                          Uninfected    NA
## 216                          Uninfected    NA
## 217                          Uninfected    NA
## 218                          Uninfected    NA
## 219                          Uninfected    NA
## 220                          Uninfected    NA
## 221                          Uninfected    NA
## 222                          Uninfected    NA
## 223                          Uninfected    NA
## 224                          Uninfected    NA
## 225                          Uninfected    NA
## 226                          Uninfected    NA
## 227                          Uninfected    NA
## 228                          Uninfected    NA
## 229                          Uninfected    NA
## 230                          Uninfected    NA
## 231                          Uninfected    NA
## 232                          Uninfected    NA
## 233                          Uninfected    NA
## 234                          Uninfected    NA
## 235                          Uninfected    NA
## 236                          Uninfected    NA
## 237                          Uninfected    NA
## 238                          Uninfected    NA
## 239                          Uninfected    NA
## 240                          Uninfected    NA
## 241                          Uninfected    NA
## 242                          Uninfected    NA
## 243                          Uninfected    NA
## 244                          Uninfected    NA
## 245                          Uninfected    NA
## 246                          Uninfected    NA
## 247                          Uninfected    NA
## 248                          Uninfected    NA
## 249                          Uninfected    NA
## 250                          Uninfected    NA
## 251                          Uninfected    NA
## 252                          Uninfected    NA
## 253                          Uninfected    NA
## 254                          Uninfected    NA
## 255                          Uninfected    NA
## 256                          Uninfected    NA
## 257                          Uninfected    NA
## 258                          Uninfected    NA
## 259                          Uninfected    NA
## 260                          Uninfected    NA
##     unidentified_gonoduct_organism unidentified_organism
## 1                               NA                     0
## 2                               NA                     0
## 3                               NA                     0
## 4                               NA                     0
## 5                               NA                     0
## 6                               NA                     0
## 7                               NA                     0
## 8                               NA                     0
## 9                               NA                     0
## 10                              NA                     0
## 11                              NA                     0
## 12                              NA                     0
## 13                              NA                     0
## 14                              NA                     0
## 15                              NA                     0
## 16                              NA                     0
## 17                              NA                     0
## 18                              NA                     0
## 19                              NA                     0
## 20                              NA                     0
## 21                              NA                     0
## 22                              NA                     5
## 23                              NA                     1
## 24                              NA                     3
## 25                              NA                     0
## 26                              NA                     0
## 27                              NA                     0
## 28                              NA                     0
## 29                              NA                     0
## 30                              NA                     0
## 31                              NA                     0
## 32                              NA                     0
## 33                              NA                     0
## 34                              NA                     0
## 35                              NA                     0
## 36                              NA                     0
## 37                              NA                     0
## 38                              NA                     0
## 39                              NA                     0
## 40                              NA                     0
## 41                              NA                     0
## 42                              NA                     0
## 43                              NA                     0
## 44                              NA                     0
## 45                              NA                     0
## 46                              NA                     0
## 47                              NA                     0
## 48                              NA                     0
## 49                              NA                     0
## 50                              NA                     0
## 51                              NA                     0
## 52                              NA                     0
## 53                              NA                     0
## 54                              NA                     0
## 55                              NA                     0
## 56                              NA                     0
## 57                              NA                     0
## 58                              NA                     0
## 59                              NA                     0
## 60                              NA                     0
## 61                              NA                     0
## 62                              NA                     0
## 63                              NA                     0
## 64                              NA                     0
## 65                              NA                     0
## 66                              NA                     1
## 67                              NA                     0
## 68                              NA                     0
## 69                              NA                     0
## 70                              NA                     0
## 71                              NA                     0
## 72                              NA                     0
## 73                              NA                     0
## 74                              NA                     0
## 75                              NA                     0
## 76                              NA                     0
## 77                              NA                     0
## 78                              NA                     0
## 79                              NA                     0
## 80                              NA                     0
## 81                              NA                     0
## 82                              NA                     0
## 83                              NA                     0
## 84                              NA                     0
## 85                              NA                     0
## 86                              NA                     6
## 87                              NA                     4
## 88                              NA                     3
## 89                              NA                     1
## 90                              NA                     1
## 91                              NA                     2
## 92                              NA                     0
## 93                              NA                     8
## 94                              NA                     0
## 95                              NA                     0
## 96                              NA                     0
## 97                              NA                     0
## 98                              NA                     0
## 99                              NA                     0
## 100                             NA                     0
## 101                             NA                     0
## 102                             NA                     0
## 103                             NA                     0
## 104                             NA                     0
## 105                             NA                     0
## 106                             NA                     0
## 107                             NA                     0
## 108                             NA                     0
## 109                             NA                     0
## 110                             NA                     0
## 111                             NA                     0
## 112                             NA                     0
## 113                             NA                     0
## 114                             NA                     0
## 115                             NA                     0
## 116                             NA                     0
## 117                             NA                     2
## 118                             NA                     0
## 119                             NA                     0
## 120                             NA                     2
## 121                             NA                     0
## 122                             NA                     0
## 123                             NA                     0
## 124                             NA                     0
## 125                             NA                     0
## 126                             NA                     0
## 127                             NA                     0
## 128                             NA                     0
## 129                             NA                     0
## 130                             NA                     0
## 131                             NA                     0
## 132                             NA                     0
## 133                             NA                     0
## 134                             NA                     0
## 135                             NA                     0
## 136                             NA                     0
## 137                             NA                     0
## 138                             NA                     0
## 139                             NA                     0
## 140                             NA                     0
## 141                             NA                     0
## 142                             NA                     0
## 143                             NA                     0
## 144                             NA                     0
## 145                             NA                     0
## 146                             NA                     0
## 147                             NA                     0
## 148                             NA                     0
## 149                             NA                     0
## 150                             NA                     0
## 151                             NA                     2
## 152                             NA                     0
## 153                             NA                     0
## 154                             NA                     1
## 155                             NA                     0
## 156                             NA                     0
## 157                             NA                     0
## 158                             NA                     0
## 159                             NA                     0
## 160                             NA                     0
## 161                             NA                     0
## 162                             NA                     0
## 163                             NA                     0
## 164                             NA                     0
## 165                             NA                     0
## 166                             NA                     0
## 167                             NA                     0
## 168                             NA                     0
## 169                             NA                     0
## 170                             NA                     0
## 171                             NA                     0
## 172                             NA                     0
## 173                             NA                     0
## 174                             NA                     0
## 175                             NA                     0
## 176                             NA                     0
## 177                             NA                     0
## 178                             NA                     0
## 179                             NA                     0
## 180                             NA                     0
## 181                             NA                     0
## 182                             NA                     0
## 183                             NA                     0
## 184                             NA                     0
## 185                             NA                     0
## 186                             NA                     0
## 187                             NA                     0
## 188                             NA                     0
## 189                             NA                     0
## 190                             NA                     0
## 191                             NA                     0
## 192                             NA                     0
## 193                             NA                     0
## 194                             NA                     0
## 195                             NA                     0
## 196                             NA                     0
## 197                             NA                     0
## 198                             NA                     0
## 199                             NA                     0
## 200                             NA                     0
## 201                             NA                     0
## 202                             NA                     0
## 203                             NA                     1
## 204                             NA                     0
## 205                             NA                     0
## 206                             NA                     0
## 207                             NA                     0
## 208                             NA                     0
## 209                             NA                     0
## 210                             NA                     0
## 211                             NA                     0
## 212                             NA                     0
## 213                             NA                     0
## 214                             NA                     0
## 215                             NA                     0
## 216                             NA                     0
## 217                             NA                     0
## 218                             NA                     0
## 219                             NA                     0
## 220                             NA                     0
## 221                             NA                     2
## 222                             NA                     0
## 223                             NA                     0
## 224                             NA                     0
## 225                             NA                     0
## 226                             NA                     0
## 227                             NA                     0
## 228                             NA                     0
## 229                             NA                     0
## 230                             NA                     0
## 231                             NA                     0
## 232                             NA                     0
## 233                             NA                     0
## 234                             NA                     0
## 235                             NA                     0
## 236                             NA                     0
## 237                             NA                     0
## 238                             NA                     0
## 239                             NA                     0
## 240                             NA                     0
## 241                             NA                     0
## 242                             NA                     0
## 243                             NA                     0
## 244                             NA                     0
## 245                             NA                     0
## 246                             NA                     0
## 247                             NA                     0
## 248                             NA                     0
## 249                             NA                     0
## 250                             NA                     0
## 251                             NA                     0
## 252                             NA                     0
## 253                             NA                     0
## 254                             NA                     0
## 255                             NA                     0
## 256                             NA                     0
## 257                             NA                     0
## 258                             NA                     0
## 259                             NA                     0
## 260                             NA                     0
##     unusual_digestive_tubule wet_weight xenoma
## 1                          0         NA     NA
## 2                          0         NA     NA
## 3                          0         NA     NA
## 4                          0         NA     NA
## 5                          0         NA     NA
## 6                          0         NA     NA
## 7                          0         NA     NA
## 8                          0         NA     NA
## 9                          0         NA     NA
## 10                         0         NA     NA
## 11                         0        0.3     NA
## 12                         0        0.3     NA
## 13                         0        0.3     NA
## 14                         0        0.4     NA
## 15                         0        0.2     NA
## 16                         0        0.2     NA
## 17                         0        0.1     NA
## 18                         0        0.2     NA
## 19                         0        0.2     NA
## 20                         0        0.2     NA
## 21                         0        0.4     NA
## 22                         0        0.3     NA
## 23                         0        0.3     NA
## 24                         0        0.3     NA
## 25                         0        0.3     NA
## 26                         0        0.3     NA
## 27                         0        0.3     NA
## 28                         0        0.2     NA
## 29                         0        0.5     NA
## 30                         0        0.2     NA
## 31                         0        0.3     NA
## 32                         0        0.2     NA
## 33                         0        0.3     NA
## 34                         0        0.3     NA
## 35                         0        0.2     NA
## 36                         0         NA     NA
## 37                         0         NA     NA
## 38                         0         NA     NA
## 39                         0         NA     NA
## 40                         0         NA     NA
## 41                         0        0.2     NA
## 42                         0        0.2     NA
## 43                         0        0.1     NA
## 44                         0        0.1     NA
## 45                         0        0.1     NA
## 46                         0        0.4     NA
## 47                         0        0.4     NA
## 48                         0        0.5     NA
## 49                         0        0.4     NA
## 50                         0        0.4     NA
## 51                         0         NA     NA
## 52                         0         NA     NA
## 53                         0         NA     NA
## 54                         0         NA     NA
## 55                         1         NA     NA
## 56                         0         NA     NA
## 57                         0         NA     NA
## 58                         0         NA     NA
## 59                         0         NA     NA
## 60                         0         NA     NA
## 61                         0        0.5     NA
## 62                         0        0.4     NA
## 63                         0        0.6     NA
## 64                         0        0.4     NA
## 65                         0        0.5     NA
## 66                         0        0.7     NA
## 67                         0        0.7     NA
## 68                         0        0.7     NA
## 69                         0        0.5     NA
## 70                         0        0.6     NA
## 71                         0        0.4     NA
## 72                         0        0.3     NA
## 73                         0        0.5     NA
## 74                         0        0.5     NA
## 75                         0        0.3     NA
## 76                         0        0.7     NA
## 77                         0        0.9     NA
## 78                         0        0.3     NA
## 79                         0        0.6     NA
## 80                         0        0.4     NA
## 81                         0        0.2     NA
## 82                         0        0.2     NA
## 83                         0        0.2     NA
## 84                         0        0.1     NA
## 85                         0        0.3     NA
## 86                         0        0.2     NA
## 87                         0        0.4     NA
## 88                         0        0.2     NA
## 89                         0        0.4     NA
## 90                         0        0.3     NA
## 91                         0        0.5     NA
## 92                         0        0.2     NA
## 93                         0        0.4     NA
## 94                         0        0.2     NA
## 95                         0        0.2     NA
## 96                         0        0.5     NA
## 97                         0        0.5     NA
## 98                         0        0.4     NA
## 99                         0        0.3     NA
## 100                        0        0.4     NA
## 101                        0         NA     NA
## 102                        0         NA     NA
## 103                        0         NA     NA
## 104                        0         NA     NA
## 105                        0         NA     NA
## 106                        0         NA     NA
## 107                        1         NA     NA
## 108                        0         NA     NA
## 109                        0         NA     NA
## 110                        0         NA     NA
## 111                        0        0.8     NA
## 112                        0        0.5     NA
## 113                        0        0.6     NA
## 114                        0        0.5     NA
## 115                        0        0.7     NA
## 116                        0        0.8     NA
## 117                        0        1.2     NA
## 118                        0        1.0     NA
## 119                        0        1.4     NA
## 120                        0        0.9     NA
## 121                        0        0.4     NA
## 122                        0        0.7     NA
## 123                        0        0.3     NA
## 124                        0        0.4     NA
## 125                        0        0.3     NA
## 126                        0        1.0     NA
## 127                        0        0.7     NA
## 128                        0        0.5     NA
## 129                        0        0.6     NA
## 130                        0        0.5     NA
## 131                        0        0.9     NA
## 132                        0        0.6     NA
## 133                        0        0.5     NA
## 134                        0        0.5     NA
## 135                        0        0.7     NA
## 136                        0         NA     NA
## 137                        0         NA     NA
## 138                        0         NA     NA
## 139                        0         NA     NA
## 140                        0         NA     NA
## 141                        0         NA     NA
## 142                        0         NA     NA
## 143                        0         NA     NA
## 144                        0         NA     NA
## 145                        0         NA     NA
## 146                        0        0.6     NA
## 147                        0        0.5     NA
## 148                        0        0.3     NA
## 149                        0        0.5     NA
## 150                        0        0.8     NA
## 151                        0        0.4     NA
## 152                        0        0.4     NA
## 153                        0        0.3     NA
## 154                        0        0.5     NA
## 155                        0        0.3     NA
## 156                        0        0.2     NA
## 157                        0        0.2     NA
## 158                        0        0.2     NA
## 159                        0        0.3     NA
## 160                        0        0.2     NA
## 161                        0        0.4     NA
## 162                        0        0.5     NA
## 163                        0        0.4     NA
## 164                        0        0.4     NA
## 165                        0        0.4     NA
## 166                        0         NA     NA
## 167                        0         NA     NA
## 168                        0         NA     NA
## 169                        0         NA     NA
## 170                        0         NA     NA
## 171                        0         NA     NA
## 172                        0         NA     NA
## 173                        0         NA     NA
## 174                        1         NA     NA
## 175                        0         NA     NA
## 176                        0        0.4     NA
## 177                        0        0.3     NA
## 178                        0        0.4     NA
## 179                        0        0.4     NA
## 180                        0        0.2     NA
## 181                        0        0.3     NA
## 182                        0        0.2     NA
## 183                        0        0.2     NA
## 184                        0        0.2     NA
## 185                        0        0.2     NA
## 186                        0        0.4     NA
## 187                        0        0.4     NA
## 188                        0        0.3     NA
## 189                        0        0.2     NA
## 190                        0        0.3     NA
## 191                        0        0.5     NA
## 192                        0        0.5     NA
## 193                        0        0.3     NA
## 194                        0        0.4     NA
## 195                        0        0.4     NA
## 196                        0         NA     NA
## 197                        0         NA     NA
## 198                        0         NA     NA
## 199                        0         NA     NA
## 200                        0         NA     NA
## 201                        0         NA     NA
## 202                        0         NA     NA
## 203                        0         NA     NA
## 204                        0         NA     NA
## 205                        0         NA     NA
## 206                        0        0.2     NA
## 207                        0        0.2     NA
## 208                        0        0.2     NA
## 209                        0        0.1     NA
## 210                        0        0.2     NA
## 211                        0        0.2     NA
## 212                        0        0.2     NA
## 213                        0        0.2     NA
## 214                        0        0.3     NA
## 215                        0        0.2     NA
## 216                        0        0.1     NA
## 217                        0        0.2     NA
## 218                        0        0.2     NA
## 219                        0        0.2     NA
## 220                        0        0.2     NA
## 221                        0        0.1     NA
## 222                        0        0.1     NA
## 223                        0        0.1     NA
## 224                        0        0.1     NA
## 225                        0        0.1     NA
## 226                        0        0.3     NA
## 227                        0        0.3     NA
## 228                        0        0.3     NA
## 229                        0        0.3     NA
## 230                        0        0.3     NA
## 231                        0        0.2     NA
## 232                        0        0.2     NA
## 233                        0        0.4     NA
## 234                        0        0.3     NA
## 235                        0        0.2     NA
## 236                        0        0.7     NA
## 237                        0        0.4     NA
## 238                        0        0.6     NA
## 239                        0        0.5     NA
## 240                        0        0.4     NA
## 241                        0        0.5     NA
## 242                        0        0.4     NA
## 243                        0        0.3     NA
## 244                        0        0.4     NA
## 245                        0        0.3     NA
## 246                        0        0.8     NA
## 247                        0        0.7     NA
## 248                        0        0.4     NA
## 249                        0        0.7     NA
## 250                        0        0.5     NA
## 251                        0        0.1     NA
## 252                        0        0.2     NA
## 253                        0        0.2     NA
## 254                        0        0.1     NA
## 255                        0        0.2     NA
## 256                        0        0.3     NA
## 257                        0        0.2     NA
## 258                        0        0.2     NA
## 259                        0        0.3     NA
## 260                        0        0.2     NA
```


```r
plyrWay3 <- musselData %>%
  filter(sex == 'Male' & state_name == 'Mississippi')
tableWay <- musselData[sex == 'Male' & state_name == 'Mississippi']
all.equal(plyrWay3, tableWay)
```

```
## [1] "Attributes: < Component \"class\": Lengths (1, 2) differ (string compare on first 1) >"
## [2] "Attributes: < Component \"class\": 1 string mismatch >"
```

# Section 5


