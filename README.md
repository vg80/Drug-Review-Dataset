#Drug Review Dataset

The drug Review dataset is made available recently on UCI machine Learning and also on Kaggle.

Load the libraries

```{r}
library(tidyverse)
```

Read the training data
```{r}
train<- read.csv("drugsComTrain_raw.csv", stringsAsFactors = FALSE)
glimpse(train)
```

Group Data by conditions and removing absurd and missing values of condition
```{r}
length(unique(train$condition))
Bycondition= train %>% group_by(condition) %>% filter(!grepl("^[0-9]", condition)) %>% filter(!condition=="") %>% summarise(number_of_drugs= n_distinct(drugName))
Bycondition %>% arrange(desc(number_of_drugs))
Bycondition %>% top_n(10, number_of_drugs) %>% 
  ggplot()+geom_bar(aes(x = reorder(condition, number_of_drugs,sum), y= number_of_drugs ), stat= "identity", fill="orange")+labs(x=NULL, y=NULL)+ggtitle("Number of drugs reviewed per condition") +theme(panel.grid.major = element_blank(), panel.grid.minor = element_blank(),
panel.background = element_blank(), axis.line = element_blank())+coord_flip()
```
Number of times each drug is reviewed and its average rating.
```{r}
length(unique(train$drugName))
Bydrug= train %>% group_by(drugName) %>% summarise(number_of_reviews= n_distinct(uniqueID), average_rating = mean(rating))
# plot of top reviewed drugs
Bydrug %>% top_n(10, number_of_reviews) %>% ggplot()+geom_bar(aes(x = reorder(drugName, number_of_reviews,sum), y= number_of_reviews), stat= "identity", fill="orange")+labs(x=NULL, y= NULL) + ggtitle("Number of reviews for each drug") + theme(panel.grid.major = element_blank(), panel.grid.minor = element_blank(),panel.background = element_blank(), axis.line = element_blank())+coord_flip()
  
```
Top rated drugs.(Overall average)
```{r}
Bydrug %>% top_n(10, average_rating) 

```
