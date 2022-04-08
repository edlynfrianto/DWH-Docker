# Solution

## Introduction

For the following problem, I have decided to use 'pandas', a data processing library in Python running in a jupyter notebook environment. A docker-compose.yml have been build to define and run the application easily.

## Steps to run the program
1. Clone this repository or download it to your local machine
2. In your terminal, change directory to the 'solution' folder and run the docker-compose.yml file by:
```sh
docker-compose up
```
3. You should see something like the following output in your terminal/command prompt (I’ve omitted some of the output for brevity):
```sh
datascience-notebook-container | [I 11:37:37.993 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
datascience-notebook-container | [C 11:37:37.994 NotebookApp]
datascience-notebook-container |
datascience-notebook-container | Copy/paste this URL into your browser when you connect for the first time,
datascience-notebook-container | to login with a token:
datascience-notebook-container | http://localhost:8888/?token=123456789123456789123456789123456789
```
5. The last line is a URL that you need to copy and paste into your browser to access the new Jupyter stack. Please make sure that there are no application running in port 8888
6. Drill into the work directory and open the 'solution.ipynb' notebook
7. Run all the cells inside the notebook and you should be able to see the output for each questions!

## Explanation of each task

1. Visualize the complete historical table view of each tables in tabular format in stdout (hint: print your table)
> For this task, I make sure that each of the JSON files are sorted in UNIX timestamp first before loading it into the respective pandas df. A while loop is generated as I noticed all data ("cards", "accounts" and "saving_account") have similar ingestion structure. If the operation is 'c', we are appending it directly to the df. If the operation is 'u', we get the last row (last updated row since we have sorted the timestamp) where the global id matches, and update the key from the JSON file. Little context is given regarding the data file, but additional try and catch error might be needed to flag any erroneous data.

2. Visualize the complete historical table view of the denormalized joined table in stdout by joining these three tables (hint: the join key lies in the `resources` section, please read carefully)
> For this task, I notice that 'card' and 'saving_account' df have their own activity which makes it more complicated during the joining process as we need to make sure that each table update is reflected in the denormalised join table. 
> Another thing I notice is that in ts: 1578654000000, there are changes in both cards and saving_accounts df independently. Both changes must be reflected at the joined table.
> Last thing is the fact that when the card/ saving account is set to inactive, this must also be updated in the account df where their id is set to "" (ts: 1579078860000). Thus, if card/saving id is set to "", I directly set all the field as "" as I assume that the account df is the primary table and we need to prioritise this over other df
> From my code, we are assuming that, the account is the primary table, thus if primary account table is setting card_id as "", we are not any making further changes in the card_account after that timestamp. This is also to meet the constraint where each account can only 1 active card and salary account

3. From result from point no 2, discuss how many transactions has been made, when did each of them occur, and how much the value of each transaction?  
   Transaction is defined as activity which change the balance
> For this task, using the result from table 2, I see if there is any difference in value for both 'credit_used' and 'balance' field. Since the table from previous task is already sorted in ts, we find the difference by using .diff() as the next record is the next transaction. Then, I filter the table again to only include those records that have changes in value. As shown in the jupyter notebook, there are 8 transactions but 2 transaction happen in the same day which is on 1578654000000. The 'ts' column shows when each transaction occur, the value is shown in "diff_credit_used" and "diff_balance" respectively