Tidyverse is a collection of R packages designed for data science. They
all share common grammar and data structures.

The core packages are:

-   **ggplot2**
-   **dplyr**
-   tidyr
-   readr
-   purrr
-   tibble
-   **stringr**
-   forcats

### Setting up tidyverse

First install tidyverse: `install.packages("tidyverse")`

Then load tidyverse: `library(tidyverse)`

### Loading the Data

First install the Data Package: `install.packages("titanic")`

Then load the Data: `library(titanic)`

### Inspecting the Data

    str(titanic_train)

    ## 'data.frame':    891 obs. of  12 variables:
    ##  $ PassengerId: int  1 2 3 4 5 6 7 8 9 10 ...
    ##  $ Survived   : int  0 1 1 1 0 0 0 0 1 1 ...
    ##  $ Pclass     : int  3 1 3 1 3 3 1 3 3 2 ...
    ##  $ Name       : chr  "Braund, Mr. Owen Harris" "Cumings, Mrs. John Bradley (Florence Briggs Thayer)" "Heikkinen, Miss. Laina" "Futrelle, Mrs. Jacques Heath (Lily May Peel)" ...
    ##  $ Sex        : chr  "male" "female" "female" "female" ...
    ##  $ Age        : num  22 38 26 35 35 NA 54 2 27 14 ...
    ##  $ SibSp      : int  1 1 0 1 0 0 0 3 0 1 ...
    ##  $ Parch      : int  0 0 0 0 0 0 0 1 2 0 ...
    ##  $ Ticket     : chr  "A/5 21171" "PC 17599" "STON/O2. 3101282" "113803" ...
    ##  $ Fare       : num  7.25 71.28 7.92 53.1 8.05 ...
    ##  $ Cabin      : chr  "" "C85" "" "C123" ...
    ##  $ Embarked   : chr  "S" "C" "S" "S" ...

### A Question You Might Ask:

#### How much did people pay?

    ggplot(titanic_train, aes(Fare)) + 
      geom_histogram(binwidth = 10, fill = "cornflowerblue") + 
      geom_rug() + 
      ggtitle("Distribution of Fares on the Titanic") + 
      xlab("Fare (1912 pounds)") + ylab("Num Passengers")

![](L5tidyverse_files/figure-markdown_strict/unnamed-chunk-2-1.png)

#### What about for each of the different classes?

    ggplot(titanic_train, aes(Fare, color=factor(Pclass))) + 
      geom_density() + 
      facet_wrap(~ Pclass) + 
      theme(legend.position = "none") + 
      xlab("Fare (1912 pounds)")

![](L5tidyverse_files/figure-markdown_strict/unnamed-chunk-3-1.png)

#### Let's go back and see how the medians compare

    first_class_mean = median(select(filter(titanic_train, Pclass == 1), Fare)[[1]])

    ## Warning: package 'bindrcpp' was built under R version 3.4.4

    second_class_mean = median(select(filter(titanic_train, Pclass == 2), Fare)[[1]])
    third_class_mean = median(select(filter(titanic_train, Pclass == 3), Fare)[[1]])

    ggplot(titanic_train, aes(Fare)) + geom_histogram(binwidth = 10) + 
      geom_vline(aes(xintercept = first_class_mean, color = "First")) + 
      geom_vline(aes(xintercept = second_class_mean, color = "Second")) + 
      geom_vline(aes(xintercept =third_class_mean, color = "Third")) + 
      scale_color_manual(name="Class", values = c("First" = "darkgreen", "Second" = "blue", "Third" = "red")) + 
      ggtitle("Distribution of Fares on the Titanic") + 
      xlab("Fare (1912 pounds)") + ylab("Num Passengers")

![](L5tidyverse_files/figure-markdown_strict/unnamed-chunk-4-1.png)

### **YOUR TURN:**

#### Create a density plot of `Fare` with a rug plot for each `Sex`

    ggplot(titanic_train, aes(Fare, color=factor(Sex))) + 
      geom_density() + 
      facet_wrap(~ Sex) + 
      geom_rug() +
      theme(legend.position = "none") + 
      xlab("Fare (1912 pounds)")

![](L5tidyverse_files/figure-markdown_strict/unnamed-chunk-5-1.png)

### Another Question You Might Ask:

#### Is there a relationship between a passenger's title and their survival rate?

    names = select(titanic_train, "Name")[[1]]
    mr = str_detect(names, "Mr.")
    mrs = str_detect(names, "Mrs.")
    miss = str_detect(names, "Miss.")
    master = str_detect(names, "Master")
    leftovers = filter(titanic_train, !mr & !mrs & !miss & !master)$Name
    leftovers

    ##  [1] "Uruchurtu, Don. Manuel E"                                
    ##  [2] "Byles, Rev. Thomas Roussel Davids"                       
    ##  [3] "Bateman, Rev. Robert James"                              
    ##  [4] "Minahan, Dr. William Edward"                             
    ##  [5] "Carter, Rev. Ernest Courtenay"                           
    ##  [6] "Moraweck, Dr. Ernest"                                    
    ##  [7] "Aubart, Mme. Leontine Pauline"                           
    ##  [8] "Pain, Dr. Alfred"                                        
    ##  [9] "Reynaldo, Ms. Encarnacion"                               
    ## [10] "Peuchen, Major. Arthur Godfrey"                          
    ## [11] "Butt, Major. Archibald Willingham"                       
    ## [12] "Kirkland, Rev. Charles Leonard"                          
    ## [13] "Stahelin-Maeglin, Dr. Max"                               
    ## [14] "Sagesser, Mlle. Emma"                                    
    ## [15] "Simonius-Blumer, Col. Oberst Alfons"                     
    ## [16] "Frauenthal, Dr. Henry William"                           
    ## [17] "Weir, Col. John"                                         
    ## [18] "Crosby, Capt. Edward Gifford"                            
    ## [19] "Rothes, the Countess. of (Lucy Noel Martha Dyer-Edwards)"
    ## [20] "Brewe, Dr. Arthur Jackson"                               
    ## [21] "Leader, Dr. Alice (Farnham)"                             
    ## [22] "Reuchlin, Jonkheer. John George"                         
    ## [23] "Harper, Rev. John"                                       
    ## [24] "Montvila, Rev. Juozas"

#### The Pipe Operator `%>%`

To get the survival outcome of only the "Mr"s using `dplyr`, we have two
option:

1.  Nested Functions: `select(filter(titanic_train, mr)`
2.  The Pipe Operator:
    `titanic_train %>% select("Survived") %>% filter(mr)`

<!-- -->

    mr_outcomes = select(filter(titanic_train, mr), "Survived")[[1]]
    mrs_outcomes = select(filter(titanic_train, mrs), "Survived")[[1]]
    miss_outcomes = select(filter(titanic_train, miss), "Survived")[[1]]
    master_outcomes = select(filter(titanic_train, master), "Survived")[[1]]

    mr_survival = sum(mr_outcomes)/length(mr_outcomes)
    mrs_survival = sum(mrs_outcomes)/length(mrs_outcomes)
    miss_survival = sum(miss_outcomes)/length(miss_outcomes)
    master_survival = sum(master_outcomes)/length(master_outcomes)

    common_titles = c("Mr.", "Mrs.", "Miss", "Master")
    title_data = data.frame("Title" = common_titles, "prop" = c(mr_survival, mrs_survival, miss_survival, master_survival))
    ggplot(title_data, aes(x=Title, y=prop, fill = common_titles)) + 
      geom_bar(stat="identity") + 
      ylab("Survival Rate") + 
      ggtitle("Survival Rate By Title") + 
      scale_fill_manual(values = c("skyblue", "pink", "skyblue", "pink")) +
      theme(legend.position = "None")

![](L5tidyverse_files/figure-markdown_strict/unnamed-chunk-9-1.png)
