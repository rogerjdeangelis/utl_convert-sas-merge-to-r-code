# utl_convert-sas-merge-to-r-code
Convert SAS merge to R code

    ```  Convert SAS merge to R code                                                                                                                                  ```
    ```                                                                                                                                                               ```
    ```    You are welcome here, even if Stackoverflow rejests your post                                                                                              ```
    ```                                                                                                                                                               ```
    ```    Inner self join in R                                                                                                                                       ```
    ```                                                                                                                                                               ```
    ```   INPUT (Two SAS datasets)                                                                                                                                    ```
    ```                                                                                                                                                               ```
    ```      sashelp.class(in=a keep=name sex age)                                                                                                                    ```
    ```      sashelp.class(in=b keep=name height weight)                                                                                                              ```
    ```                                                                                                                                                               ```
    ```    SAS WORKING CODE                                                                                                                                           ```
    ```    ================                                                                                                                                           ```
    ```        merge                                                                                                                                                  ```
    ```          sashelp.class(in=a keep=name sex age)                                                                                                                ```
    ```          sashelp.class(in=b keep=name height weight)                                                                                                          ```
    ```        ;                                                                                                                                                      ```
    ```        by name;                                                                                                                                               ```
    ```        If name='Alice' And sex='F' Then height=65;                                                                                                            ```
    ```        bmi=703 * weight / (height * height);                                                                                                                  ```
    ```                                                                                                                                                               ```
    ```                                                                                                                                                               ```
    ```    R WORKING CODE (IML/R)  (runs in R and SAS without any change)                                                                                             ```
    ```    ======================                                                                                                                                     ```
    ```        Select                                                                                                                                                 ```
    ```           l.name                                                                                                                                              ```
    ```          ,l.sex                                                                                                                                               ```
    ```          ,l.age                                                                                                                                               ```
    ```          ,r.weight                                                                                                                                            ```
    ```          ,703 * r.weight / (l.height * l.height) as bmi                                                                                                       ```
    ```          ,case when l.age=14 and l.sex="F" then 66 else r.height end as height                                                                                ```
    ```        from                                                                                                                                                   ```
    ```          have as l, have as r                                                                                                                                 ```
    ```        where                                                                                                                                                  ```
    ```          l.name = r.name                                                                                                                                      ```
    ```                                                                                                                                                               ```
    ```                                                                                                                                                               ```
    ```  see                                                                                                                                                          ```
    ```  https://stackoverflow.com/questions/47173341/convert-sas-code-to-r-code                                                                                      ```
    ```                                                                                                                                                               ```
    ```  *                              _                                                                                                                             ```
    ```   ___  __ _ ___    ___ ___   __| | ___                                                                                                                        ```
    ```  / __|/ _` / __|  / __/ _ \ / _` |/ _ \                                                                                                                       ```
    ```  \__ \ (_| \__ \ | (_| (_) | (_| |  __/                                                                                                                       ```
    ```  |___/\__,_|___/  \___\___/ \__,_|\___|                                                                                                                       ```
    ```                                                                                                                                                               ```
    ```  ;                                                                                                                                                            ```
    ```                                                                                                                                                               ```
    ```                                                                                                                                                               ```
    ```  options validvarname=upcase;                                                                                                                                 ```
    ```  libname sd1 "d:/sd1";                                                                                                                                        ```
    ```  data sd1.have;                                                                                                                                               ```
    ```    merge                                                                                                                                                      ```
    ```      sashelp.class(in=a keep=name sex age)                                                                                                                    ```
    ```      sashelp.class(in=b keep=name height weight)                                                                                                              ```
    ```    ;                                                                                                                                                          ```
    ```    by name;                                                                                                                                                   ```
    ```    If age=14 and sex='F' Then height=66;                                                                                                                      ```
    ```    bmi=703 * weight / (height * height);                                                                                                                      ```
    ```  run;quit;                                                                                                                                                    ```
    ```  /*                                                                                                                                                           ```
    ```  Up to 40 obs SD1.HAVE total obs=19                                                                                                                           ```
    ```                                                                                                                                                               ```
    ```  Obs    NAME       SEX    AGE    HEIGHT    WEIGHT      BMI                                                                                                    ```
    ```                                                                                                                                                               ```
    ```    1    Alfred      M      14     69.0      112.5    16.6115                                                                                                  ```
    ```    2    Alice       F      13     56.5       84.0    18.4986                                                                                                  ```
    ```    3    Barbara     F      13     65.3       98.0    16.1568                                                                                                  ```
    ```                                                                                                                                                               ```
    ```    4    Carol       F      14     66.0      102.5    16.5421  Height changed                                                                                  ```
    ```                                                                                                                                                               ```
    ```    5    Henry       M      14     63.5      102.5    17.8703                                                                                                  ```
    ```  ...                                                                                                                                                          ```
    ```  */                                                                                                                                                           ```
    ```                                                                                                                                                               ```
    ```  *____                                                                                                                                                        ```
    ```  |  _ \                                                                                                                                                       ```
    ```  | |_) |                                                                                                                                                      ```
    ```  |  _ <                                                                                                                                                       ```
    ```  |_| \_\                                                                                                                                                      ```
    ```                                                                                                                                                               ```
    ```  ;                                                                                                                                                            ```
    ```  %utl_submit_r64('                                                                                                                                            ```
    ```  source("C:/Program Files/R/R-3.3.2/etc/Rprofile.site", echo=T);                                                                                              ```
    ```  library(haven);                                                                                                                                              ```
    ```  library(sqldf);                                                                                                                                              ```
    ```  have<-read_sas("C:/Program Files/SASHome/SASFoundation/9.4/core/sashelp/class.sas7bdat");                                                                    ```
    ```  want<-sqldf("                                                                                                                                                ```
    ```         Select                                                                                                                                                ```
    ```            l.name                                                                                                                                             ```
    ```           ,l.sex                                                                                                                                              ```
    ```           ,l.age                                                                                                                                              ```
    ```           ,r.weight                                                                                                                                           ```
    ```           ,703 * r.weight / (l.height * l.height) as bmi                                                                                                      ```
    ```           ,case when l.age=14 and l.sex=''F'' then 66 else r.height end as height                                                                             ```
    ```         from                                                                                                                                                  ```
    ```           have as l, have as r                                                                                                                                ```
    ```         where                                                                                                                                                 ```
    ```           l.name = r.name                                                                                                                                     ```
    ```         ");                                                                                                                                                   ```
    ```  want;                                                                                                                                                        ```
    ```  ');                                                                                                                                                          ```
    ```                                                                                                                                                               ```
    ```  /*                                                                                                                                                           ```
    ```   R OUTPUT                                                                                                                                                    ```
    ```                                                                                                                                                               ```
    ```            Name Sex Age Weight      bmi height                                                                                                                ```
    ```      1   Alfred   M  14  112.5 16.61153   69.0                                                                                                                ```
    ```      2    Alice   F  13   84.0 18.49855   56.5                                                                                                                ```
    ```      3  Barbara   F  13   98.0 16.15679   65.3                                                                                                                ```
    ```                                                                                                                                                               ```
    ```      4    Carol   F  14  102.5 18.27090   66.0                                                                                                                ```
    ```                                                                                                                                                               ```
    ```      5    Henry   M  14  102.5 17.87030   63.5                                                                                                                ```
    ```    ...                                                                                                                                                        ```
    ```   */                                                                                                                                                          ```

