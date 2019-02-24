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
ggplot(se, aes(x = park_name, y = visitors)) + 
  geom_boxplot() +
  theme(axis.text.x = element_text(angle = 45, hjust = 1))

## example line graph 
ggplot(se, aes(x = year, y = visitors, color = park_name)) +
   geom_line()
   
## example multiple in one graph 
ggplot(data = acadia, aes(x = year, y = visitors)) + 
  geom_point(color="purple") +
  geom_line(color="blue") +
  geom_smooth(color = "red") +
  labs(title = "Acadia National Park Visitation",
       y = "Visitation",
       x = "Year") +
  theme_bw()
##bar graph 
ggplot(data = visit_16, aes(x = state)) + 
  geom_bar()
  
##bar graph with different fills 
ggplot(data = visit_16, aes(x = state, y = visitors, fill = park_name)) + 
  geom_bar(stat = "identity")

## bar graph but separated out 
ggplot(data = visit_16, aes(x = state, y = visitors, fill = park_name)) + 
  geom_bar(stat = "identity", position = "dodge")

library(tidyverse)

## how to read data from online csv file 
gapminder <- readr::read_csv('https://raw.githubusercontent.com/OHI-Science/data-science-training/master/data/gapminder.csv') 
    
## use str() to explore structure of data 
str(gapminder)
## other commands to use for understanding data frame names(gapminder)
dim(gapminder)    # ?dim dimension
ncol(gapminder)   # ?ncol number of columns
nrow(gapminder)   # ?nrow number of rows

#statistical overview with summary() or skimr
summary(gapminder)
install.packages('skimr')
library(skimr) 
skim(gapminder)

gapminder$lifeExp ## just looking at the variable--have to always put in the $ sign 
head(gapminder$lifeExp) # shows first 6, tail() shows last 6
str(gapminder$lifeExp) # structure 
summary(gapminder$lifeExp) # summary stats like mean, median, min and max 

##using dplyr functions, there are 5 main functions:
## filter()= pick observations by values
## select()= pick variables by names 
## mutate()= generate new variables
##summarise()= collapse variables into a summary 
##arrange()= reorder the rows 
filter(gapminder, lifeExp < 29)
filter(gapminder, country == "Mexico")
filter(gapminder, country %in% c("Mexico", "Peru")) ## use the %in% when you have more than 1 
filter(gapminder, country == "Mexico", year == 2002)
sweden<-filter(gapminder,country=="Sweden")
mean(sweden$lifeExp)

##select multiple columns with a comma 
select(gapminder, year, lifeExp) 

# you can use - to deselect columns
select(gapminder, -continent, -lifeExp) 

##practice deleting columns for specific country 
gap_cambodia<- filter(gapminder, country == "Cambodia")
gap_cam2<- select(gap_cambodia, -continent, -lifeExp)
mean(gap_cam2$gdpPercap)

#note there is a new pipe operator %>% just for example you can do the following in less steps from the cambodia example previously
gap_cambodia  <- gapminder %>% 
filter(country == "Cambodia") %>%
select(-continent, -lifeExp) 

##create a new column by combining multiple variables 
gapminder %>% 
  mutate(index = 1:nrow(gapminder))
gapminder %>%
  mutate(gdp = pop * gdpPercap)
  
mutate(gapminder, gdp=pop*gdpPercap)

##exercise find max gdp for eqypt and vientnam 
  gapminder %>% 
  select(-continent, -lifeExp) %>% # not super necessary but to simplify
  filter(country == "Egypt") %>%
  mutate(gdp = pop * gdpPercap) %>%
  mutate(max_gdp = max(gdp))
  
##group 
gapminder %>%
  group_by(country) %>%
  mutate(gdp     = pop * gdpPercap,
         max_gdp = max(gdp)) %>%
  ungroup() # if you use group_by, also use ungroup() to save heartache later
  
##just looking at last 30 entries with tail 
  gapminder %>%
  group_by(country) %>%
  mutate(gdp     = pop * gdpPercap,
         max_gdp = max(gdp)) %>%
  ungroup() %>% 
  tail(30)
  
  
## summarize: We want to operate on a group, but actually collapse or distill the output from that group. The summarize() function will do that for us
gapminder %>%
  group_by(country) %>%
  mutate(gdp = pop * gdpPercap) %>%
  summarize(max_gdp = max(gdp)) %>%
  ungroup()
  
## arrange by 
gapminder %>%
  group_by(country) %>%
  mutate(gdp = pop * gdpPercap) %>%
  summarize(max_gdp = max(gdp)) %>%
  ungroup() %>%
  arrange(max_gdp)

## exercise 
gapminder %>%
  filter(continent == 'Asia') %>%
  group_by(country) %>%
  filter(lifeExp == max(lifeExp)) %>%
  arrange(lifeExp, desc(lifeExp)) 
  
