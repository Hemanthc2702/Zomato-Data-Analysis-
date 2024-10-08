# Zomato-Data-Analysis-

# For that we should import libraries for anaylsis like numpy, pandas, seaborn, matplot libraries.. 
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt

# we should upload the data excel file in the from of file read_method using pandas..
dataframe = pd.read_csv("Zomato data .csv")
print(dataframe.head())

# First check data types in the file 'rate' coloumn convert them into float and remove the denominator.   
def handleRate(value):
    value = str(value).split('/')
    value = value[0];
    return float(value)
  
dataframe['rate'] = dataframe['rate'].apply(handleRate)
print(dataframe.head())

# Check if there any Null Value if it is there change into non-null values..
dataframe.info()

# Check the which category have more customer is visit to restaurents...
sns.countplot(x=dataframe['listed_in(type)'])
plt.xlabel('Type of restaurant')

# To get graph grouped the data listed_in(type) and votes sum this to, apply dictionary form to get vote and grouped data..
grouped_data = dataframe.groupby('listed_in(type)')['votes'].sum()
result = pd.DataFrame({'vote':grouped_data})
plt.plot(result, c="green",marker='o')
plt.xlabel("Type of restaurant", c="red",size=20)
plt.ylabel("Votes", c="red", size=20)

# To see the max_vote using max function and restaurant name..
max_votes = dataframe["votes"].max()
restaurant_with_max_votes = dataframe.loc[dataframe['votes']==max_votes, 'name']
print("Restaurant(s) with the maximum votes: ")
print(restaurant_with_max_votes)

# This show the max online order not accept are not by customer...
sns.countplot(x=dataframe['online_order'])

# this show the histplot graph using restaurant rating value..
plt.hist(dataframe['rate'],bins=5)
plt.title("Rating Distribution")
plt.show()

# This show prefer restaurant with maximum cost..
couple_data = dataframe['approx_cost(for two people)']
sns.countplot(x=couple_data)

# This graph show offline and online order lower rating to excellent rating..
plt.figure(figsize=(6,6))
sns.boxplot(x='online_order',y='rate',data=dataframe)

# This show heatmap and pivot table..
pivot_table = dataframe.pivot_table(index='listed_in(type)', columns='online_order',aggfunc='size',fill_value=0)
sns.heatmap(pivot_table,annot=True,cmap="YlGnBu",fmt='d')
plt.title('Heatmap')
plt.xlabel("Online Order")
plt.ylabel("Listed In (Type)")
plt.show()
