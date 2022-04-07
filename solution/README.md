# Solution

## Introduction

For the following problem, I have decided to use 'pandas', a data processing library in Python running in a jupyter notebook environment. A docker-compose.yml have been configured to define and run the application easily.

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
5. The last line is a URL that we need to copy and paste into our browser to access the new Jupyter stack
6. Drill into the work directory and open the 'solution.ipynb' notebook
7. Run all the cells inside the notebook and you should be able to see the output for each questions!

## Explanation of each task

1. Visualize the complete historical table view of each tables in tabular format in stdout (hint: print your table)
> For this task, I make sure that each of the JSON files are sorted in UNIX timestamp first before loading it into the respective pandas df. A while loop is generated as I noticed all data ("cards", "accounts" and "saving_account") have similar ingestion structure. If the operation is 'c', we are appending it directly to the df. If the operation is 'u', we get the last row (last updated row since we have sorted the timestamp) where the global id matches, and update the key from the JSON file. Little context is given regarding the data file, but additional try and catch error might be needed to flag any erroneous data

2. Visualize the complete historical table view of the denormalized joined table in stdout by joining these three tables (hint: the join key lies in the `resources` section, please read carefully)
> For this task, I merge 2 tables at a time by the timestamp and respective primary ID. So, for account and cards df, i join by timestamp and card_id. Then, I sort the joined table in ascending order of the timestamp column. To fill the data that only exist in certain table, I make sure that i fill the NA cell with the row above it (since the row above it is the last updated information for that ID).
3. From result from point no 2, discuss how many transactions has been made, when did each of them occur, and how much the value of each transaction?  
   Transaction is defined as activity which change the balance of the savings account or credit used of the card


