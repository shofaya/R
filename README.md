# R

[LINK] https://public.tableau.com/app/sample-data/HollywoodsMostProfitableStories.csv

# R Task
Load data 
View(df)
#Load library: install.packages("tidyverse")
#Import library: library(tidyverse)
# Check data types: str(df)
# Check for missing values: colSums(is.na(df))
#Drop missing values: na.omit(df)
# check to make sure that the rows have been removed 
colSums(is.na(df))
#Check for duplicates: dim(df[duplicated(df$Film),])[1]  
#round off values to 2 places 
df$Profitability <- round(df$Profitability ,digit=2)
df$Worldwide.Gross <- round(df$Worldwide.Gross ,digit=2)
#View(df)
dim(df)
#Check for outliers using a boxplot: library(ggplot2)
#Create a boxplot that highlights the outliers   
ggplot(df, aes(x=Profitability, y=Worldwide.Gross)) + geom_boxplot(outlier.colour = "red", outlier.shape = 1)+ scale_x_continuous(labels = scales::comma)+coord_cartesian(ylim = c(0, 1000))
#Remove outliers in 'Profitability' 
Q1 <- quantile(df$Profitability, .25)
Q3 <- quantile(df$Profitability, .75)
IQR <- IQR(df$Profitability)
no_outliers <- subset(df, df$Profitability> (Q1 - 1.5*IQR) & df$Profitability< (Q3 + 1.5*IQR))
#check dim: dim(no_outliers)
# Remove outliers in 'Worldwide.Gross' 
Q1 <- quantile(no_outliers$Worldwide.Gross, .25)
Q3 <- quantile(no_outliers$Worldwide.Gross, .75)
IQR <- IQR(no_outliers$Worldwide.Gross)
  
df1 <- subset(no_outliers, no_outliers$Worldwide.Gross> (Q1 - 1.5*IQR) & no_outliers$Worldwide.Gross< (Q3 + 1.5*IQR))
  
dim(df1)
#Summary Statistics/Univariate Analysis: 
summary(df1)
  
#bivariate analysis 
  
#scatterplot 
ggplot(df1, aes(x=Lead.Studio, y=Rotten.Tomatoes..)) + geom_point()+ scale_y_continuous(labels = scales::comma)+coord_cartesian(ylim = c(0, 110))+theme(axis.text.x = element_text(angle = 90))
  
 #bar chart 
ggplot(df1, aes(x=Year)) + geom_bar()

#Export clean data 
write.csv(df1, "clean_df.csv") 

Import clean_df in Power BI.
In your power BI Dashboard, the client would like to see: 

The average Rotten Tomatoes ratings of each genre
The number of movies produced per year 
The audience score for each film  
The profitability per studio 
The worldwide gross per genre 


