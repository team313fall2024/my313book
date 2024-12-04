---
title: "Online Data"
output: 
  html_document:
    toc: true
    toc_float: true
    toc_depth: 3
---

Data we have been analyzing in the class so far has been in a .csv format and mostly ready for analysis. But it won't always be so easy to access...

In this worksheet, we will:

-   Explore different sources to find datasets
-   Scrape data from the web "manually" and with code
-   Import datasets with a different format
-   Export datasets manipulated in R into csv files

## 1. Online data resource

Datasets posted online are sometimes easy to access and download but always look for a **data dictionary** or documentation to help you make sense of the variables.

### a. Data portal

Let's retrieve data from the [City of Austin Open Data Portal](https://data.austintexas.gov/) about the quality of the water in Austin. Search for Data with the keywords *watershed scores*. Then we can import the dataset directly to RStudio with API endpoint:


```r
library(tidyverse)
```

```
## Warning: package 'tidyverse' was built under R version 4.3.3
```

```
## Warning: package 'ggplot2' was built under R version 4.3.3
```

```
## Warning: package 'tibble' was built under R version 4.3.3
```

```
## Warning: package 'tidyr' was built under R version 4.3.3
```

```
## Warning: package 'readr' was built under R version 4.3.3
```

```
## Warning: package 'purrr' was built under R version 4.3.3
```

```
## Warning: package 'stringr' was built under R version 4.3.3
```

```
## Warning: package 'forcats' was built under R version 4.3.3
```

```
## Warning: package 'lubridate' was built under R version 4.3.3
```

```
## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
## ✔ dplyr     1.1.4     ✔ readr     2.1.5
## ✔ forcats   1.0.0     ✔ stringr   1.5.1
## ✔ ggplot2   3.5.1     ✔ tibble    3.2.1
## ✔ lubridate 1.9.3     ✔ tidyr     1.3.1
## ✔ purrr     1.0.2     
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors
```

```r
water_quality <- read_csv("https://data.austintexas.gov/resource/vk3r-6prc.csv")
```

```
## Rows: 141 Columns: 29
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr   (5): watershed_name, watershed_reach_id, index_source_type, created_by...
## dbl  (22): watershed_id, integrity_score_id, fiscal_year_of_observation, ind...
## dttm  (2): created_date, modified_date
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

------------------------------------------------------------------------

#### **Try it! Represent the distribution of `index_water_quality` and `problem_water_quality` separately. What is the difference between these two variables?**


```r
# Write and submit code here!
hist(water_quality$index_water_quality)
```

<img src="11_OnlineData_files/figure-html/unnamed-chunk-2-1.png" width="672" />

```r
hist(water_quality$problem_water_quality)
```

<img src="11_OnlineData_files/figure-html/unnamed-chunk-2-2.png" width="672" />

**Different way to assess water quality on different scales (100 is best or worst).**

------------------------------------------------------------------------

Be aware that if you get data directly from the web like that it might:

-   No longer be available at some point.
-   Be updated.

It might be a better idea to export and save the file locally.

### b. Downloading datasets

Only download files from trusted websites! To assess whether a website can be trusted or not, consider the following factors:

-   Check the source of the website: Websites of universities, government agencies, or well-known platforms (e.g., GitHub, Kaggle, and Data.gov...).
-   Look for HTTPS and security
-   Examine the quality of the website: Trustworthy websites often look polished and contain clearly written content, whereas scam sites may look poorly designed or filled with ads.

Also be aware that the dataset you download might not be in a usable format in R! See section 3.

### c. Websites with special permissions

Other times, you'll need approval to access the file, like [here](https://repository.niddk.nih.gov/studies/dpp/).

Depending on the source, before you can get the data you might need to:

-   Explain your purpose for using the data.
-   Explain what security measures you will follow to protect the data.
-   Get IRB (Instructional Review Board) approval before collecting data.
-   Sign a data use agreement.
-   Pay money.

Refer to the [Data Resources page](https://utexas.instructure.com/courses/1399995/pages/data-resources) for a list of common data resources.

## 2. Web scraping

### a. Manually from the web

If the data you want isn't already compiled in a file, you will need to pull the information yourself. Sometimes, it's as easy as a "manual scrape", and simply copy/paste the values into Excel. This is not always perfect but usually follow these steps:

-   Step 1: Select the data you want with your mouse and copy it.

-   Step 2: Open a blank Excel or Google Sheets file. Paste the selection into your file.

-   Step 3: Go to (Excel) File\>Save As\>Name it and choose the *.csv* format from the dropdown; or (Sheets) Name your file, then go to File\>Download\>csv

------------------------------------------------------------------------

#### **Try it! Manually scrape [the state table describing the legal abortion limits by state](https://en.wikipedia.org/wiki/Abortion_law_in_the_United_States_by_state). Create a .csv file and read it into R. Find in how many states abortion is banned.**


```r
# Write and submit code here!
wikipedia <- read_csv("wikipedia.csv")
```

```
## Rows: 50 Columns: 9
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (9): State, On-demand gestational limit, Waiting period, Mandatory ultra...
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
table(wikipedia$`Waiting period`)
```

```
## 
## N/A; abortion is banned here.                            No 
##                            13                             2 
##                      No [240]                       No[238] 
##                             1                             1 
##                          None                           Yes 
##                            24                             9
```

```r
wikipedia |> 
  filter(`On-demand gestational limit` == "Fertilization") |>
  count()
```

```
## # A tibble: 1 × 1
##       n
##   <int>
## 1    13
```

**Some challenges: extra row when copy/pasting, variable names long and with special characters.**

------------------------------------------------------------------------

### b. With code

There are many (paid) programs/software out there to scrape data from the web - some legal, some not. We will talk about scraping with code, for free with the *rvest* package. We can:

-   Read HTML source code from a website.
-   Break it into a nice structure.
-   Extract the specific elements we want to analyze.


```r
library(rvest)
```

```
## Warning: package 'rvest' was built under R version 4.3.3
```

Let's work with a simple example first, the [countries of the world](https://www.scrapethissite.com/pages/simple/) and read the HTML content of this page in R:


```r
read_html("https://www.scrapethissite.com/pages/simple/")
```

```
## {html_document}
## <html lang="en">
##  [1] <head>\n<meta http-equiv="Content-Type" content="text/html; charset=UTF- ...
##  [2] <body>\n    <nav id="site-nav"><div class="container">\n                 ...
##  [3] <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.3/jquery. ...
##  [4] <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/js/bootstra ...
##  [5] <script src="https://cdnjs.cloudflare.com/ajax/libs/pnotify/2.1.0/pnotif ...
##  [6] <link href="https://cdnjs.cloudflare.com/ajax/libs/pnotify/2.1.0/pnotify ...
##  [7] <script type="text/javascript">\n    \n    PNotify.prototype.options.sty ...
##  [8] <script type="text/javascript">\n    $("video").hover(function() {\n     ...
##  [9] <script>\n    (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r] ...
## [10] <script>\n  !function(f,b,e,v,n,t,s){if(f.fbq)return;n=f.fbq=function(){ ...
## [11] <noscript><img height="1" width="1" style="display:none" src="https://ww ...
## [12] <script type="text/javascript">\n    /* <![CDATA[ */\n    var google_con ...
## [13] <script type="text/javascript" src="//www.googleadservices.com/pagead/co ...
## [14] <noscript>\n    <div style="display:inline;">\n    <img height="1" width ...
## [15] <script async src="https://www.googletagmanager.com/gtag/js?id=AW-950945 ...
## [16] <script>\n   window.dataLayer = window.dataLayer || [];\n   function gta ...
```

We can select some elements of this page. Without a deep knowledge of HMTL, we can use a simple tool to differentiate between all the different elements:

1.  Download [Chrome's Selector Gadget extension](https://chrome.google.com/webstore/detail/selectorgadget/mhjhnkcfbdhnjickkkdbjoemdmbfginb), which lets you easily identify the HTML "tag" for a pattern of elements that you want to scrape.
2.  Open the webpage you want to scrape from and click on the "Developer Tools", the puzzle piece to the right of the address bar.
3.  Click on an element you want to scrape. If anything is highlighted yellow that you *don't* want, click it to remove it from your selection.

Once you see the **element tag** in the bar at the bottom, we can now scrape those with code:


```r
read_html("https://www.scrapethissite.com/pages/simple/") |>
  html_elements(".country-name") |>
  html_text(trim = TRUE)
```

```
##   [1] "Andorra"                                     
##   [2] "United Arab Emirates"                        
##   [3] "Afghanistan"                                 
##   [4] "Antigua and Barbuda"                         
##   [5] "Anguilla"                                    
##   [6] "Albania"                                     
##   [7] "Armenia"                                     
##   [8] "Angola"                                      
##   [9] "Antarctica"                                  
##  [10] "Argentina"                                   
##  [11] "American Samoa"                              
##  [12] "Austria"                                     
##  [13] "Australia"                                   
##  [14] "Aruba"                                       
##  [15] "Åland"                                       
##  [16] "Azerbaijan"                                  
##  [17] "Bosnia and Herzegovina"                      
##  [18] "Barbados"                                    
##  [19] "Bangladesh"                                  
##  [20] "Belgium"                                     
##  [21] "Burkina Faso"                                
##  [22] "Bulgaria"                                    
##  [23] "Bahrain"                                     
##  [24] "Burundi"                                     
##  [25] "Benin"                                       
##  [26] "Saint Barthélemy"                            
##  [27] "Bermuda"                                     
##  [28] "Brunei"                                      
##  [29] "Bolivia"                                     
##  [30] "Bonaire"                                     
##  [31] "Brazil"                                      
##  [32] "Bahamas"                                     
##  [33] "Bhutan"                                      
##  [34] "Bouvet Island"                               
##  [35] "Botswana"                                    
##  [36] "Belarus"                                     
##  [37] "Belize"                                      
##  [38] "Canada"                                      
##  [39] "Cocos [Keeling] Islands"                     
##  [40] "Democratic Republic of the Congo"            
##  [41] "Central African Republic"                    
##  [42] "Republic of the Congo"                       
##  [43] "Switzerland"                                 
##  [44] "Ivory Coast"                                 
##  [45] "Cook Islands"                                
##  [46] "Chile"                                       
##  [47] "Cameroon"                                    
##  [48] "China"                                       
##  [49] "Colombia"                                    
##  [50] "Costa Rica"                                  
##  [51] "Cuba"                                        
##  [52] "Cape Verde"                                  
##  [53] "Curacao"                                     
##  [54] "Christmas Island"                            
##  [55] "Cyprus"                                      
##  [56] "Czech Republic"                              
##  [57] "Germany"                                     
##  [58] "Djibouti"                                    
##  [59] "Denmark"                                     
##  [60] "Dominica"                                    
##  [61] "Dominican Republic"                          
##  [62] "Algeria"                                     
##  [63] "Ecuador"                                     
##  [64] "Estonia"                                     
##  [65] "Egypt"                                       
##  [66] "Western Sahara"                              
##  [67] "Eritrea"                                     
##  [68] "Spain"                                       
##  [69] "Ethiopia"                                    
##  [70] "Finland"                                     
##  [71] "Fiji"                                        
##  [72] "Falkland Islands"                            
##  [73] "Micronesia"                                  
##  [74] "Faroe Islands"                               
##  [75] "France"                                      
##  [76] "Gabon"                                       
##  [77] "United Kingdom"                              
##  [78] "Grenada"                                     
##  [79] "Georgia"                                     
##  [80] "French Guiana"                               
##  [81] "Guernsey"                                    
##  [82] "Ghana"                                       
##  [83] "Gibraltar"                                   
##  [84] "Greenland"                                   
##  [85] "Gambia"                                      
##  [86] "Guinea"                                      
##  [87] "Guadeloupe"                                  
##  [88] "Equatorial Guinea"                           
##  [89] "Greece"                                      
##  [90] "South Georgia and the South Sandwich Islands"
##  [91] "Guatemala"                                   
##  [92] "Guam"                                        
##  [93] "Guinea-Bissau"                               
##  [94] "Guyana"                                      
##  [95] "Hong Kong"                                   
##  [96] "Heard Island and McDonald Islands"           
##  [97] "Honduras"                                    
##  [98] "Croatia"                                     
##  [99] "Haiti"                                       
## [100] "Hungary"                                     
## [101] "Indonesia"                                   
## [102] "Ireland"                                     
## [103] "Israel"                                      
## [104] "Isle of Man"                                 
## [105] "India"                                       
## [106] "British Indian Ocean Territory"              
## [107] "Iraq"                                        
## [108] "Iran"                                        
## [109] "Iceland"                                     
## [110] "Italy"                                       
## [111] "Jersey"                                      
## [112] "Jamaica"                                     
## [113] "Jordan"                                      
## [114] "Japan"                                       
## [115] "Kenya"                                       
## [116] "Kyrgyzstan"                                  
## [117] "Cambodia"                                    
## [118] "Kiribati"                                    
## [119] "Comoros"                                     
## [120] "Saint Kitts and Nevis"                       
## [121] "North Korea"                                 
## [122] "South Korea"                                 
## [123] "Kuwait"                                      
## [124] "Cayman Islands"                              
## [125] "Kazakhstan"                                  
## [126] "Laos"                                        
## [127] "Lebanon"                                     
## [128] "Saint Lucia"                                 
## [129] "Liechtenstein"                               
## [130] "Sri Lanka"                                   
## [131] "Liberia"                                     
## [132] "Lesotho"                                     
## [133] "Lithuania"                                   
## [134] "Luxembourg"                                  
## [135] "Latvia"                                      
## [136] "Libya"                                       
## [137] "Morocco"                                     
## [138] "Monaco"                                      
## [139] "Moldova"                                     
## [140] "Montenegro"                                  
## [141] "Saint Martin"                                
## [142] "Madagascar"                                  
## [143] "Marshall Islands"                            
## [144] "Macedonia"                                   
## [145] "Mali"                                        
## [146] "Myanmar [Burma]"                             
## [147] "Mongolia"                                    
## [148] "Macao"                                       
## [149] "Northern Mariana Islands"                    
## [150] "Martinique"                                  
## [151] "Mauritania"                                  
## [152] "Montserrat"                                  
## [153] "Malta"                                       
## [154] "Mauritius"                                   
## [155] "Maldives"                                    
## [156] "Malawi"                                      
## [157] "Mexico"                                      
## [158] "Malaysia"                                    
## [159] "Mozambique"                                  
## [160] "Namibia"                                     
## [161] "New Caledonia"                               
## [162] "Niger"                                       
## [163] "Norfolk Island"                              
## [164] "Nigeria"                                     
## [165] "Nicaragua"                                   
## [166] "Netherlands"                                 
## [167] "Norway"                                      
## [168] "Nepal"                                       
## [169] "Nauru"                                       
## [170] "Niue"                                        
## [171] "New Zealand"                                 
## [172] "Oman"                                        
## [173] "Panama"                                      
## [174] "Peru"                                        
## [175] "French Polynesia"                            
## [176] "Papua New Guinea"                            
## [177] "Philippines"                                 
## [178] "Pakistan"                                    
## [179] "Poland"                                      
## [180] "Saint Pierre and Miquelon"                   
## [181] "Pitcairn Islands"                            
## [182] "Puerto Rico"                                 
## [183] "Palestine"                                   
## [184] "Portugal"                                    
## [185] "Palau"                                       
## [186] "Paraguay"                                    
## [187] "Qatar"                                       
## [188] "Réunion"                                     
## [189] "Romania"                                     
## [190] "Serbia"                                      
## [191] "Russia"                                      
## [192] "Rwanda"                                      
## [193] "Saudi Arabia"                                
## [194] "Solomon Islands"                             
## [195] "Seychelles"                                  
## [196] "Sudan"                                       
## [197] "Sweden"                                      
## [198] "Singapore"                                   
## [199] "Saint Helena"                                
## [200] "Slovenia"                                    
## [201] "Svalbard and Jan Mayen"                      
## [202] "Slovakia"                                    
## [203] "Sierra Leone"                                
## [204] "San Marino"                                  
## [205] "Senegal"                                     
## [206] "Somalia"                                     
## [207] "Suriname"                                    
## [208] "South Sudan"                                 
## [209] "São Tomé and Príncipe"                       
## [210] "El Salvador"                                 
## [211] "Sint Maarten"                                
## [212] "Syria"                                       
## [213] "Swaziland"                                   
## [214] "Turks and Caicos Islands"                    
## [215] "Chad"                                        
## [216] "French Southern Territories"                 
## [217] "Togo"                                        
## [218] "Thailand"                                    
## [219] "Tajikistan"                                  
## [220] "Tokelau"                                     
## [221] "East Timor"                                  
## [222] "Turkmenistan"                                
## [223] "Tunisia"                                     
## [224] "Tonga"                                       
## [225] "Turkey"                                      
## [226] "Trinidad and Tobago"                         
## [227] "Tuvalu"                                      
## [228] "Taiwan"                                      
## [229] "Tanzania"                                    
## [230] "Ukraine"                                     
## [231] "Uganda"                                      
## [232] "U.S. Minor Outlying Islands"                 
## [233] "United States"                               
## [234] "Uruguay"                                     
## [235] "Uzbekistan"                                  
## [236] "Vatican City"                                
## [237] "Saint Vincent and the Grenadines"            
## [238] "Venezuela"                                   
## [239] "British Virgin Islands"                      
## [240] "U.S. Virgin Islands"                         
## [241] "Vietnam"                                     
## [242] "Vanuatu"                                     
## [243] "Wallis and Futuna"                           
## [244] "Samoa"                                       
## [245] "Kosovo"                                      
## [246] "Yemen"                                       
## [247] "Mayotte"                                     
## [248] "South Africa"                                
## [249] "Zambia"                                      
## [250] "Zimbabwe"
```

Similarly, let's scrape the capitals:


```r
read_html("https://www.scrapethissite.com/pages/simple/") |>
  html_elements(".country-capital") |>
  html_text(trim = TRUE)
```

```
##   [1] "Andorra la Vella"    "Abu Dhabi"           "Kabul"              
##   [4] "St. John's"          "The Valley"          "Tirana"             
##   [7] "Yerevan"             "Luanda"              "None"               
##  [10] "Buenos Aires"        "Pago Pago"           "Vienna"             
##  [13] "Canberra"            "Oranjestad"          "Mariehamn"          
##  [16] "Baku"                "Sarajevo"            "Bridgetown"         
##  [19] "Dhaka"               "Brussels"            "Ouagadougou"        
##  [22] "Sofia"               "Manama"              "Bujumbura"          
##  [25] "Porto-Novo"          "Gustavia"            "Hamilton"           
##  [28] "Bandar Seri Begawan" "Sucre"               "Kralendijk"         
##  [31] "Brasília"            "Nassau"              "Thimphu"            
##  [34] "None"                "Gaborone"            "Minsk"              
##  [37] "Belmopan"            "Ottawa"              "West Island"        
##  [40] "Kinshasa"            "Bangui"              "Brazzaville"        
##  [43] "Bern"                "Yamoussoukro"        "Avarua"             
##  [46] "Santiago"            "Yaoundé"             "Beijing"            
##  [49] "Bogotá"              "San José"            "Havana"             
##  [52] "Praia"               "Willemstad"          "Flying Fish Cove"   
##  [55] "Nicosia"             "Prague"              "Berlin"             
##  [58] "Djibouti"            "Copenhagen"          "Roseau"             
##  [61] "Santo Domingo"       "Algiers"             "Quito"              
##  [64] "Tallinn"             "Cairo"               "Laâyoune / El Aaiún"
##  [67] "Asmara"              "Madrid"              "Addis Ababa"        
##  [70] "Helsinki"            "Suva"                "Stanley"            
##  [73] "Palikir"             "Tórshavn"            "Paris"              
##  [76] "Libreville"          "London"              "St. George's"       
##  [79] "Tbilisi"             "Cayenne"             "St Peter Port"      
##  [82] "Accra"               "Gibraltar"           "Nuuk"               
##  [85] "Bathurst"            "Conakry"             "Basse-Terre"        
##  [88] "Malabo"              "Athens"              "Grytviken"          
##  [91] "Guatemala City"      "Hagåtña"             "Bissau"             
##  [94] "Georgetown"          "Hong Kong"           "None"               
##  [97] "Tegucigalpa"         "Zagreb"              "Port-au-Prince"     
## [100] "Budapest"            "Jakarta"             "Dublin"             
## [103] "None"                "Douglas"             "New Delhi"          
## [106] "None"                "Baghdad"             "Tehran"             
## [109] "Reykjavik"           "Rome"                "Saint Helier"       
## [112] "Kingston"            "Amman"               "Tokyo"              
## [115] "Nairobi"             "Bishkek"             "Phnom Penh"         
## [118] "Tarawa"              "Moroni"              "Basseterre"         
## [121] "Pyongyang"           "Seoul"               "Kuwait City"        
## [124] "George Town"         "Astana"              "Vientiane"          
## [127] "Beirut"              "Castries"            "Vaduz"              
## [130] "Colombo"             "Monrovia"            "Maseru"             
## [133] "Vilnius"             "Luxembourg"          "Riga"               
## [136] "Tripoli"             "Rabat"               "Monaco"             
## [139] "Chişinău"            "Podgorica"           "Marigot"            
## [142] "Antananarivo"        "Majuro"              "Skopje"             
## [145] "Bamako"              "Naypyitaw"           "Ulan Bator"         
## [148] "Macao"               "Saipan"              "Fort-de-France"     
## [151] "Nouakchott"          "Plymouth"            "Valletta"           
## [154] "Port Louis"          "Malé"                "Lilongwe"           
## [157] "Mexico City"         "Kuala Lumpur"        "Maputo"             
## [160] "Windhoek"            "Noumea"              "Niamey"             
## [163] "Kingston"            "Abuja"               "Managua"            
## [166] "Amsterdam"           "Oslo"                "Kathmandu"          
## [169] "Yaren"               "Alofi"               "Wellington"         
## [172] "Muscat"              "Panama City"         "Lima"               
## [175] "Papeete"             "Port Moresby"        "Manila"             
## [178] "Islamabad"           "Warsaw"              "Saint-Pierre"       
## [181] "Adamstown"           "San Juan"            "None"               
## [184] "Lisbon"              "Melekeok"            "Asunción"           
## [187] "Doha"                "Saint-Denis"         "Bucharest"          
## [190] "Belgrade"            "Moscow"              "Kigali"             
## [193] "Riyadh"              "Honiara"             "Victoria"           
## [196] "Khartoum"            "Stockholm"           "Singapore"          
## [199] "Jamestown"           "Ljubljana"           "Longyearbyen"       
## [202] "Bratislava"          "Freetown"            "San Marino"         
## [205] "Dakar"               "Mogadishu"           "Paramaribo"         
## [208] "Juba"                "São Tomé"            "San Salvador"       
## [211] "Philipsburg"         "Damascus"            "Mbabane"            
## [214] "Cockburn Town"       "N'Djamena"           "Port-aux-Français"  
## [217] "Lomé"                "Bangkok"             "Dushanbe"           
## [220] "None"                "Dili"                "Ashgabat"           
## [223] "Tunis"               "Nuku'alofa"          "Ankara"             
## [226] "Port of Spain"       "Funafuti"            "Taipei"             
## [229] "Dodoma"              "Kiev"                "Kampala"            
## [232] "None"                "Washington"          "Montevideo"         
## [235] "Tashkent"            "Vatican City"        "Kingstown"          
## [238] "Caracas"             "Road Town"           "Charlotte Amalie"   
## [241] "Hanoi"               "Port Vila"           "Mata-Utu"           
## [244] "Apia"                "Pristina"            "Sanaa"              
## [247] "Mamoudzou"           "Pretoria"            "Lusaka"             
## [250] "Harare"
```

Now we can put all the information about the countries and their capitals in the same dataset:


```r
# Put it all in a dataframe
countries_data <- data.frame(
  "names" = read_html("https://www.scrapethissite.com/pages/simple/") |>
  html_elements(".country-name") |>
  html_text(trim = TRUE), 
  
  "capitals" = read_html("https://www.scrapethissite.com/pages/simple/") |>
  html_elements(".country-capital") |>
  html_text(trim = TRUE))
countries_data
```

```
##                                            names            capitals
## 1                                        Andorra    Andorra la Vella
## 2                           United Arab Emirates           Abu Dhabi
## 3                                    Afghanistan               Kabul
## 4                            Antigua and Barbuda          St. John's
## 5                                       Anguilla          The Valley
## 6                                        Albania              Tirana
## 7                                        Armenia             Yerevan
## 8                                         Angola              Luanda
## 9                                     Antarctica                None
## 10                                     Argentina        Buenos Aires
## 11                                American Samoa           Pago Pago
## 12                                       Austria              Vienna
## 13                                     Australia            Canberra
## 14                                         Aruba          Oranjestad
## 15                                         Åland           Mariehamn
## 16                                    Azerbaijan                Baku
## 17                        Bosnia and Herzegovina            Sarajevo
## 18                                      Barbados          Bridgetown
## 19                                    Bangladesh               Dhaka
## 20                                       Belgium            Brussels
## 21                                  Burkina Faso         Ouagadougou
## 22                                      Bulgaria               Sofia
## 23                                       Bahrain              Manama
## 24                                       Burundi           Bujumbura
## 25                                         Benin          Porto-Novo
## 26                              Saint Barthélemy            Gustavia
## 27                                       Bermuda            Hamilton
## 28                                        Brunei Bandar Seri Begawan
## 29                                       Bolivia               Sucre
## 30                                       Bonaire          Kralendijk
## 31                                        Brazil            Brasília
## 32                                       Bahamas              Nassau
## 33                                        Bhutan             Thimphu
## 34                                 Bouvet Island                None
## 35                                      Botswana            Gaborone
## 36                                       Belarus               Minsk
## 37                                        Belize            Belmopan
## 38                                        Canada              Ottawa
## 39                       Cocos [Keeling] Islands         West Island
## 40              Democratic Republic of the Congo            Kinshasa
## 41                      Central African Republic              Bangui
## 42                         Republic of the Congo         Brazzaville
## 43                                   Switzerland                Bern
## 44                                   Ivory Coast        Yamoussoukro
## 45                                  Cook Islands              Avarua
## 46                                         Chile            Santiago
## 47                                      Cameroon             Yaoundé
## 48                                         China             Beijing
## 49                                      Colombia              Bogotá
## 50                                    Costa Rica            San José
## 51                                          Cuba              Havana
## 52                                    Cape Verde               Praia
## 53                                       Curacao          Willemstad
## 54                              Christmas Island    Flying Fish Cove
## 55                                        Cyprus             Nicosia
## 56                                Czech Republic              Prague
## 57                                       Germany              Berlin
## 58                                      Djibouti            Djibouti
## 59                                       Denmark          Copenhagen
## 60                                      Dominica              Roseau
## 61                            Dominican Republic       Santo Domingo
## 62                                       Algeria             Algiers
## 63                                       Ecuador               Quito
## 64                                       Estonia             Tallinn
## 65                                         Egypt               Cairo
## 66                                Western Sahara Laâyoune / El Aaiún
## 67                                       Eritrea              Asmara
## 68                                         Spain              Madrid
## 69                                      Ethiopia         Addis Ababa
## 70                                       Finland            Helsinki
## 71                                          Fiji                Suva
## 72                              Falkland Islands             Stanley
## 73                                    Micronesia             Palikir
## 74                                 Faroe Islands            Tórshavn
## 75                                        France               Paris
## 76                                         Gabon          Libreville
## 77                                United Kingdom              London
## 78                                       Grenada        St. George's
## 79                                       Georgia             Tbilisi
## 80                                 French Guiana             Cayenne
## 81                                      Guernsey       St Peter Port
## 82                                         Ghana               Accra
## 83                                     Gibraltar           Gibraltar
## 84                                     Greenland                Nuuk
## 85                                        Gambia            Bathurst
## 86                                        Guinea             Conakry
## 87                                    Guadeloupe         Basse-Terre
## 88                             Equatorial Guinea              Malabo
## 89                                        Greece              Athens
## 90  South Georgia and the South Sandwich Islands           Grytviken
## 91                                     Guatemala      Guatemala City
## 92                                          Guam             Hagåtña
## 93                                 Guinea-Bissau              Bissau
## 94                                        Guyana          Georgetown
## 95                                     Hong Kong           Hong Kong
## 96             Heard Island and McDonald Islands                None
## 97                                      Honduras         Tegucigalpa
## 98                                       Croatia              Zagreb
## 99                                         Haiti      Port-au-Prince
## 100                                      Hungary            Budapest
## 101                                    Indonesia             Jakarta
## 102                                      Ireland              Dublin
## 103                                       Israel                None
## 104                                  Isle of Man             Douglas
## 105                                        India           New Delhi
## 106               British Indian Ocean Territory                None
## 107                                         Iraq             Baghdad
## 108                                         Iran              Tehran
## 109                                      Iceland           Reykjavik
## 110                                        Italy                Rome
## 111                                       Jersey        Saint Helier
## 112                                      Jamaica            Kingston
## 113                                       Jordan               Amman
## 114                                        Japan               Tokyo
## 115                                        Kenya             Nairobi
## 116                                   Kyrgyzstan             Bishkek
## 117                                     Cambodia          Phnom Penh
## 118                                     Kiribati              Tarawa
## 119                                      Comoros              Moroni
## 120                        Saint Kitts and Nevis          Basseterre
## 121                                  North Korea           Pyongyang
## 122                                  South Korea               Seoul
## 123                                       Kuwait         Kuwait City
## 124                               Cayman Islands         George Town
## 125                                   Kazakhstan              Astana
## 126                                         Laos           Vientiane
## 127                                      Lebanon              Beirut
## 128                                  Saint Lucia            Castries
## 129                                Liechtenstein               Vaduz
## 130                                    Sri Lanka             Colombo
## 131                                      Liberia            Monrovia
## 132                                      Lesotho              Maseru
## 133                                    Lithuania             Vilnius
## 134                                   Luxembourg          Luxembourg
## 135                                       Latvia                Riga
## 136                                        Libya             Tripoli
## 137                                      Morocco               Rabat
## 138                                       Monaco              Monaco
## 139                                      Moldova            Chişinău
## 140                                   Montenegro           Podgorica
## 141                                 Saint Martin             Marigot
## 142                                   Madagascar        Antananarivo
## 143                             Marshall Islands              Majuro
## 144                                    Macedonia              Skopje
## 145                                         Mali              Bamako
## 146                              Myanmar [Burma]           Naypyitaw
## 147                                     Mongolia          Ulan Bator
## 148                                        Macao               Macao
## 149                     Northern Mariana Islands              Saipan
## 150                                   Martinique      Fort-de-France
## 151                                   Mauritania          Nouakchott
## 152                                   Montserrat            Plymouth
## 153                                        Malta            Valletta
## 154                                    Mauritius          Port Louis
## 155                                     Maldives                Malé
## 156                                       Malawi            Lilongwe
## 157                                       Mexico         Mexico City
## 158                                     Malaysia        Kuala Lumpur
## 159                                   Mozambique              Maputo
## 160                                      Namibia            Windhoek
## 161                                New Caledonia              Noumea
## 162                                        Niger              Niamey
## 163                               Norfolk Island            Kingston
## 164                                      Nigeria               Abuja
## 165                                    Nicaragua             Managua
## 166                                  Netherlands           Amsterdam
## 167                                       Norway                Oslo
## 168                                        Nepal           Kathmandu
## 169                                        Nauru               Yaren
## 170                                         Niue               Alofi
## 171                                  New Zealand          Wellington
## 172                                         Oman              Muscat
## 173                                       Panama         Panama City
## 174                                         Peru                Lima
## 175                             French Polynesia             Papeete
## 176                             Papua New Guinea        Port Moresby
## 177                                  Philippines              Manila
## 178                                     Pakistan           Islamabad
## 179                                       Poland              Warsaw
## 180                    Saint Pierre and Miquelon        Saint-Pierre
## 181                             Pitcairn Islands           Adamstown
## 182                                  Puerto Rico            San Juan
## 183                                    Palestine                None
## 184                                     Portugal              Lisbon
## 185                                        Palau            Melekeok
## 186                                     Paraguay            Asunción
## 187                                        Qatar                Doha
## 188                                      Réunion         Saint-Denis
## 189                                      Romania           Bucharest
## 190                                       Serbia            Belgrade
## 191                                       Russia              Moscow
## 192                                       Rwanda              Kigali
## 193                                 Saudi Arabia              Riyadh
## 194                              Solomon Islands             Honiara
## 195                                   Seychelles            Victoria
## 196                                        Sudan            Khartoum
## 197                                       Sweden           Stockholm
## 198                                    Singapore           Singapore
## 199                                 Saint Helena           Jamestown
## 200                                     Slovenia           Ljubljana
## 201                       Svalbard and Jan Mayen        Longyearbyen
## 202                                     Slovakia          Bratislava
## 203                                 Sierra Leone            Freetown
## 204                                   San Marino          San Marino
## 205                                      Senegal               Dakar
## 206                                      Somalia           Mogadishu
## 207                                     Suriname          Paramaribo
## 208                                  South Sudan                Juba
## 209                        São Tomé and Príncipe            São Tomé
## 210                                  El Salvador        San Salvador
## 211                                 Sint Maarten         Philipsburg
## 212                                        Syria            Damascus
## 213                                    Swaziland             Mbabane
## 214                     Turks and Caicos Islands       Cockburn Town
## 215                                         Chad           N'Djamena
## 216                  French Southern Territories   Port-aux-Français
## 217                                         Togo                Lomé
## 218                                     Thailand             Bangkok
## 219                                   Tajikistan            Dushanbe
## 220                                      Tokelau                None
## 221                                   East Timor                Dili
## 222                                 Turkmenistan            Ashgabat
## 223                                      Tunisia               Tunis
## 224                                        Tonga          Nuku'alofa
## 225                                       Turkey              Ankara
## 226                          Trinidad and Tobago       Port of Spain
## 227                                       Tuvalu            Funafuti
## 228                                       Taiwan              Taipei
## 229                                     Tanzania              Dodoma
## 230                                      Ukraine                Kiev
## 231                                       Uganda             Kampala
## 232                  U.S. Minor Outlying Islands                None
## 233                                United States          Washington
## 234                                      Uruguay          Montevideo
## 235                                   Uzbekistan            Tashkent
## 236                                 Vatican City        Vatican City
## 237             Saint Vincent and the Grenadines           Kingstown
## 238                                    Venezuela             Caracas
## 239                       British Virgin Islands           Road Town
## 240                          U.S. Virgin Islands    Charlotte Amalie
## 241                                      Vietnam               Hanoi
## 242                                      Vanuatu           Port Vila
## 243                            Wallis and Futuna            Mata-Utu
## 244                                        Samoa                Apia
## 245                                       Kosovo            Pristina
## 246                                        Yemen               Sanaa
## 247                                      Mayotte           Mamoudzou
## 248                                 South Africa            Pretoria
## 249                                       Zambia              Lusaka
## 250                                     Zimbabwe              Harare
```

------------------------------------------------------------------------

#### **Try it! Scrape the population and area for each country then calculate the population density. Which countries have the highest population density in the world?**


```r
# Write and submit code here!
# Put it all in a dataframe
countries_data <- data.frame(
  "names" = read_html("https://www.scrapethissite.com/pages/simple/") |>
  html_elements(".country-name") |>
  html_text(trim = TRUE), 
  
  "population" = read_html("https://www.scrapethissite.com/pages/simple/") |>
  html_elements(".country-population") |>
  html_text(trim = TRUE), 
  
  "area" = read_html("https://www.scrapethissite.com/pages/simple/") |>
  html_elements(".country-area") |>
  html_text(trim = TRUE)) |>
  mutate(density = round(as.numeric(population)/as.numeric(area), 0))
countries_data |>
  slice_max(density, n = 10)
```

```
##           names population   area density
## 1        Monaco      32965   1.95   16905
## 2     Singapore    4701069  692.7    6787
## 3     Hong Kong    6898686 1092.0    6317
## 4     Gibraltar      27884    6.5    4290
## 5  Vatican City        921   0.44    2093
## 6  Sint Maarten      37429   21.0    1782
## 7         Macao     449198  254.0    1768
## 8      Maldives     395650  300.0    1319
## 9         Malta     403000  316.0    1275
## 10      Bermuda      65365   53.0    1233
```

**Write sentences here.**

------------------------------------------------------------------------

### c. More examples of scraping

Try scraping these:

1.  [Austin Date Ideas](https://mycurlyadventures.com/fun-austin-date-night-ideas/)
2.  [Travel destinations](https://www.forbes.com/sites/laurabegleybloom/2019/09/04/bucket-list-travel-the-top-50-places-in-the-world/?sh=248d064820cf)


```r
# Austin date ideas
read_html("https://mycurlyadventures.com/fun-austin-date-night-ideas/") |>
  html_elements(".adhesion_desktop , h3") |> 
  html_text()
```

```
##  [1] "Grab a Margarita with a View"                                             
##  [2] "Stroll Through Zilker Botanical Gardens"                                  
##  [3] "Book a Sunset River Cruise"                                               
##  [4] "Enjoy a Spa Day"                                                          
##  [5] "Learn to Give a Good Massage"                                             
##  [6] "Visit the Symphony"                                                       
##  [7] "Catch a Dance Show"                                                       
##  [8] "Visit NYC"                                                                
##  [9] "Go Sailing"                                                               
## [10] "See Austin from the Sky"                                                  
## [11] "Go Back in Time "                                                         
## [12] "Listen to Live Jazz"                                                      
## [13] "Visit the Opera "                                                         
## [14] "Check Out The Austin Aquarium"                                            
## [15] "Try a Chef’s Tasting"                                                     
## [16] "Or Go For a 20-course Omakase"                                            
## [17] "Cruise in a Cool Car"                                                     
## [18] "Relax in a Cave"                                                          
## [19] "Go for a Picnic"                                                          
## [20] "Go on a Wellness Retreat"                                                 
## [21] "Go for a Brunch or Dinner Cruise"                                         
## [22] "Take a Friday Moonlight Cruise"                                           
## [23] "Enjoy a Romantic Meal"                                                    
## [24] "Book More Spa Time"                                                       
## [25] "Plan a Staycation"                                                        
## [26] "Go on a Wine Tour"                                                        
## [27] "Visit the Umlauf Sculpture Garden & Museum"                               
## [28] "Plan a Day Trip/Weekend Getaway"                                          
## [29] "Escape the Box"                                                           
## [30] "Best Date Ideas: Things to Do in Austin on the Water"                     
## [31] "Best Date Ideas: Active Things to Do in Austin"                           
## [32] "Best Date Ideas: Fun Things to Do in Austin"                              
## [33] "Best Date Ideas: Romantic Things to Do in Austin for Art & History Lovers"
## [34] "Best Date Ideas: Things to Do for Foodies in Austin"                      
## [35] "Best Date Ideas: Unique Things to Do in Austin"                           
## [36] "Best Date Ideas: Free Things to Do in Austin"                             
## [37] "Where should I go on a date in Austin?"                                   
## [38] "Is Austin Texas good for dating?"
```

```r
# Travel destinations
read_html("https://www.forbes.com/sites/laurabegleybloom/2019/09/04/bucket-list-travel-the-top-50-places-in-the-world/?sh=248d064820cf") |>
  html_elements("strong") |>
  html_text()
```

```
##  [1] "1. Bali, Indonesia: "                                    
##  [2] "2. New Orleans:"                                         
##  [3] "3. Kerry, Ireland:"                                      
##  [4] "4. Marrakesh, Morocco: "                                 
##  [5] "5. Sydney:"                                              
##  [6] "6. The Maldives:"                                        
##  [7] "7. Paris, France:"                                       
##  [8] "8. Cape Town, South Africa:"                             
##  [9] "9. Dubai, U.A.E.:"                                       
## [10] "10. Bora Bora, French Polynesia:"                        
## [11] "11. New York: "                                          
## [12] "12. Dubrovnik, Croatia:"                                 
## [13] "13. Edinburgh, Scotland:"                                
## [14] "14. Rome, Italy:"                                        
## [15] "15. Paro Valley, Bhutan:"                                
## [16] "16. Jaipur, India:"                                      
## [17] "17. Waikato, New Zealand:"                               
## [18] "18. Havana, Cuba:"                                       
## [19] "19. Tokyo, Japan: "                                      
## [20] "20. Antarctica: "                                        
## [21] "21. Vancouver, Canada: "                                 
## [22] "22. Los Angeles:"                                        
## [23] "23. Kruger National Park, South Africa:"                 
## [24] "24. Santorini, Greece:"                                  
## [25] "25. Moscow, Russia:"                                     
## [26] "26. Singapore"                                           
## [27] "27. London, England:"                                    
## [28] "28. Rio de Janeiro, Brazil:"                             
## [29] "29. Petra, Jordan:"                                      
## [30] "30. Hong Kong:"                                          
## [31] "31. Barbados:"                                           
## [32] "32. Amsterdam:"                                          
## [33] "33. Santiago, Chile:"                                    
## [34] "34. Cairo, Egypt:"                                       
## [35] "35. Copenhagen, Denmark:"                                
## [36] "36. Seoul, Korea: "                                      
## [37] "37. Laucala Island Resort, Fiji:"                        
## [38] "38. Providencia, Colombia:"                              
## [39] "39. Machu Picchu, Peru:"                                 
## [40] "40. Virunga National Park, Democratic Republic of Congo:"
## [41] "41. Lisbon, Portugal:"                                   
## [42] "42. Hanoi, Vietnam:"                                     
## [43] "43. Hawaii:"                                             
## [44] "44. Ibiza, Spain:"                                       
## [45] "45. Beijing, China:"                                     
## [46] "46. Budapest, Hungary:"                                  
## [47] "47. Cinque Terre, Italy:"                                
## [48] "48. Buenos Aires, Argentina:"                            
## [49] "49. Las Vegas:"                                          
## [50] "50: Matterhorn, Switzerland:"
```

## 3. Datasets formats

Other researchers might use different software with their own file extensions. For common ones, there's likely already a package that can be used to import them into R.

Download the *tamu_scholarships.sas7bdat* file from this [GitHub](https://github.com/snehapnaik/SAS-TAMU-scholarship/blob/main/tamu_scholarships.sas7bdat). This file is from SAS (a popular statistical analysis software) and can be imported easily:


```r
# Submit this code only once:
#install.packages('sas7bdat')

# Upload library
library(sas7bdat)
```

```
## Warning: package 'sas7bdat' was built under R version 4.3.3
```

```r
# Import SAS dataset
tamu <- read.sas7bdat('tamu_scholarships.sas7bdat')
head(tamu)
```

```
##   student_id               Name amount1 amount2 amount3 amount4 amount5 amount6
## 1     120101        Lu, Patrick    5000     NaN     NaN     NaN     NaN     NaN
## 2     120102          Zhou, Tom    2250    1500     500     NaN     NaN     NaN
## 3     120103      Dawes, Wilson    1500     NaN     NaN     NaN     NaN     NaN
## 4     120104 Billington, Kareen    1500    1500    3750    4500    4750    5250
## 5     120105         Povey, Liz    2500    3000    1750    2750    5250    2250
## 6     120106      Hornsey, John    3750     500    2750     500     750    5000
##   amount7 amount8 amount9 amount10 fund1 fund2 fund3 fund4 fund5 fund6 fund7
## 1     NaN     NaN     NaN      NaN  1101                                    
## 2     NaN     NaN     NaN      NaN  1011  1190  1228                        
## 3     NaN     NaN     NaN      NaN  1130                                    
## 4    3750     NaN     NaN      NaN  1040  1125  CARE  1124  1085  1044  1091
## 5    3750     NaN     NaN      NaN  1053  1220  1185  1112  1056  1199  CARE
## 6     NaN     NaN     NaN      NaN  1012  1066  1229  1081  1121  1191      
##   fund8 fund9 fund10 major
## 1                     HIST
## 2                     PSYC
## 3                     GEOL
## 4                     BOTN
## 5                     ESCI
## 6                     FREN
```

------------------------------------------------------------------------

#### **Try it! Find the top 10 majors in terms of the number of students who got a scholarship.**


```r
# Write and submit code here!
```

**Write sentences here.**

------------------------------------------------------------------------

If you encounter a file with a weird extension, look up if there is an existing import function into R. If not, you might need to convert the file using another program before working with it in R.

## 4. Export into csv

Once we manipulate our dataset in R, we might be interested in exporting it into a `csv` file:


```r
# Write and submit code here!
write_csv(tamu, "tamu.csv")
```

------------------------------------------------------------------------

#### **Try it! Reshape the `tamu` dataset so that we can find the total amount of the scholarships for each major. Find the top 10 majors in terms of the amount of money awarded by the scholarships. Then export this dataset into a `csv` file.**


```r
# Write and submit code here!
```

**Write sentences here.**

------------------------------------------------------------------------
