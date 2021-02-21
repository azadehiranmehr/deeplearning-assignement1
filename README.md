# deeplearning-assignement1
Steps of my program are as follows repectivly:
1-	Load 3 CSV files to 3 Pandas Data frames (df1, df2, df3)
2-	Concatanate 3 different dataframe on rows (Axis=0)  , sort  merged dataframe based on symbol column and then store it in Bigdata
3-	Reorder columns of Bigdata dataframe based on the question 
4-	Save Bigdata dataframe to the disk
5-	Sort Bigdata dataframe based on MarketCap in descend order , Select first 15 rows
6-	Add new column “Sequence number” to first column .“Sequence number” is the index of each row in the old big data before sorting based on Marketcap
7-	Save new Sorted dataframe to the disk
8-	Request input from user to look up that value in dataframe colum
9-	Search  ‘Symbol’ column for user input, if user's input was found shows correspondent record otherwise shows "Symbol was not found"

Data structure used in this program:
             Pandas Dataframe( 2-D labeled data structure with columns of potentially different type)
Total Efficiency of program (Dominated Time Complexity): O(nlogn)

import pandas as pd
df1= pd.read_csv('https://raw.githubusercontent.com/gheniabla/datasets/master/companylist-1.csv')  O(n)
df2= pd.read_csv('https://raw.githubusercontent.com/gheniabla/datasets/master/companylist-2.csv')  O(n)
df3= pd.read_csv('https://raw.githubusercontent.com/gheniabla/datasets/master/companylist-3.csv')  O(n)

-----------------------------------------------------------------------------------------
bigdata = pd.concat([df1, df2,df3], ignore_index=True).sort_values(['Symbol']) O(1)
-----------------------------------------------------------------------------------------
bigdata.to_csv('merged.csv',index= False) O(n)
-----------------------------------------------------------------------------------------
new_index= ['Symbol','Name','LastSale','MarketCap','IPOyear','Sector','Industry'] O(1)
----------------------------------------------------------------------------------------
bigdata_sorted = bigdata.sort_values(['MarketCap'],ascending=False).head(15).reindex(columns = new_index) O(nlogn)
----------------------------------------------------------------------------------------
bigdata_sorted.reset_index(inplace=True) O(1)
bigdata_sorted = bigdata_sorted.rename(columns = {'index':'Sequence Number'}) O(1)
----------------------------------------------------------------------------------------
bigdata_sorted.to_csv('marketcap.csv', index= False) O(n)

----------------------------------------------------------------------------------------
symbol1 = input('please enter a stock symbol to search: ') O(1)
----------------------------------------------------------------------------------------
symbol1 = symbol1.upper();O(1)
----------------------------------------------------------------------------------------
dff = bigdata.loc[bigdata['Symbol']== symbol1] O(n)
----------------------------------------------------------------------------------------
if dff.empty: ) O(1)
    print("I don't finnd this symbol please enter another symbol ")) O(1)
else:
    dff = dff[new_index] ) O(1)
    print("Stockes with symbol  is: \n" ) ) O(1)
    print(dff) ) O(1)

Reasons:
Time  for reading and saving data frame depends on file size(n). the larger file have larger loading and saving time. so read_csv() and to_csv() both have time complexity O(n).
Based on my research Pandas indexing implementation is like hash table .
Like a dict, a DataFrame's index is backed by a hash table. Looking up rows or columns based on index is like looking up dict values based on a key. So due to its ability to perform fast hash lookups, all looking up by index (like loc, iloc), reordering the columns, reindexing, concatenate data frame on axis 0 are O(1)
In contrast, the values in a column are like values in a list and looking up rows in a column for a value is like doing linear search in a list. In our code when we search whole column for a value ( bigdata['Symbol']== symbol1 ) pandas has to loop through every value in the column to find the ones equal to symbol. So the type of search is linear with O(n) it is not binary search. bigdata.loc[bigdata['Symbol']== symbol1] O(n)
Sorted_values() Sorts whole dataframe based on a column using quick sort so its complexity is O(nlogn)


