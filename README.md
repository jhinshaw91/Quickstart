# Quickstart
learning to use
hello! 
hi
is this working?
30/3
library(tidyverse)
##put in data sets
ca <- read_csv("https://raw.githubusercontent.com/OHI-Science/data-science-training/master/data/ca.csv") 

#Acadia National Park
acadia <- read_csv("https://raw.githubusercontent.com/OHI-Science/data-science-training/master/data/acadia.csv")

#Southeast US National Parks
se <- read_csv("https://raw.githubusercontent.com/OHI-Science/data-science-training/master/data/se.csv")

#2016 Visitation for all Pacific West National Parks
visit_16 <- read_csv("https://raw.githubusercontent.com/OHI-Science/data-science-training/master/data/visit_16.csv")

#All Nationally designated sites in Massachusetts
mass <- read_csv("https://raw.githubusercontent.com/OHI-Science/data-science-training/master/data/mass.csv")
##example scatterplot
head(ca)
ggplot(data=ca)
ggplot(data = ca) +
    geom_point(aes(x = year, y = visitors, color = park_name))
## another example 
head(se)
ggplot(data = se) +
    geom_point(aes(x = year, y = visitors, color = state))+
     labs(x = "Year",
       y = "Visitation",
       title = "Southeast States National Park Visitation") +
  theme_light() +
  theme(legend.title = element_blank(),
        axis.text.x = element_text(angle = 45, hjust = 1, size = 14))
##faceting- where each graph is separated out (split one plot into multiple)
ggplot(data = se) +
    geom_point(aes(x = year, y = visitors, color=state)) +
    facet_wrap(~ state)
##geometric objects (bar, jitter, boxplot, etc)
ggplot(data = se) + 
  geom_jitter(aes(x = park_name, y = visitors, color = park_name), 
              width = 0.1, 
              alpha = 0.4) +
  coord_flip() +
  theme(legend.position = "none")
  
##example with box plot
> ggplot(se, aes(x = park_name, y = visitors)) + 
+     geom_boxplot() +
+     theme(axis.text.x = element_text(angle = 45, hjust = 1))

## example line graph 
ggplot(se, aes(x = year, y = visitors, color = park_name)) +
   geom_line()
   


    
