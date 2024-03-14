# Goals of the exercise. 

* Learn data manipulation using Pandas dataframe 
* Access AWS resources programatically using boto3.

# Checklist of assignment Steps

**Due Date: February 24, 11:59 PM**

- Accept the assignment at github classroom
- Update the colab badge in the [mlb.ipynb](mlb.ipynb) notebook  and [setup-colab2s3.ipynb](setup-colab2s3.ipynb) notebook with your username as what you did in previous two homeworks.
- Ensure you have AWS Educate access through the classroom.
- The data is in the [data](data) folder. Look at all the csv files. Download them.
- Follow the steps in this [tutorial](https://brightspace.vanderbilt.edu/d2l/le/content/342996/viewContent/2255467/View) to create S3 bucket and import the downloaded files in the previous step into S3. Note you will need to follow the steps from the previous assigment to start the AWS lab, get to the AWS console and then follow the instructions described in the tutorial.
- Finish the queries and save the response in the mlb.ipynb notebook. Save the results to show the execution results.
- Copy the assignment github url and submit to  brightspace.
- Delete your S3 bucket from AWS when you are sure that you are done.
- Shut down the lab

# Prior Reading

* https://pandas.pydata.org/docs/user_guide/10min.html
* https://medium.com/swlh/the-mastery-of-pandas-i-50156db42125.  This article is also available as pdf in our teams general channel. See the note from Feb 12.
* https://pandas.pydata.org/docs/
* [Pandas Tutorial Video](https://www.youtube.com/watch?v=vmEHCJofslg)
* Go through the notebook [setup-colab2s3.ipynb](setup-colab2s3.ipynb) to learn how to access S3 bucket from Google Colab using Boto3.
* About Boto3  https://medium.com/@luiscelismx/starting-with-aws-boto3-6a5e8c70a1ca - note get the latest version of boto3


# AWS CLI Credentials.

Note for this assignment you will be accessing AWS S3 storage programmatically using the Boto3 Python API. For this reason you will need to obtain the credentials. The region name will also be shown there. Make sure that you do not commit these credentials in the notebook and do not save it. Copy it temporarily, work on the code and delete the credential before checking it in. 

See the image below - follow these steps to get your credentials that will need to be added to the colab notebook in the right place to test things.

![follow these steps to get your credentials that will need to be added to the colab notebook in the right place to test things](awscredentialhowto.png)


# Assignment Description (100 points)

In this assignment you are using the Pandas library to query a dataset that contains batting and pitching statistics from 1871 onwards, plus fielding statistics, standings, team stats, managerial records, post-season data, and more. See [readme2012.txt](data/readme2012.txt) for details about this MLB data.

## Software Required and initial steps

**Pandas** is an open source, BSD-licensed library providing high-performance, easy-to-use data structures and data analysis tools for the Python programming language.

Install pandas using the following commands: (add '!' before the command when running on Google colab.)

- Option 1: (preferred)
  ```sh
  # python3-pip has been installed on colab, so you can skip this step
  sudo apt-get install python3-pip

  pip3 install pandas
  ```
- Option 2:
  ```sh
  sudo apt-get install -y python3-pandas
  ```

**Boto3** is the Amazon Web Services (AWS) SDK for Python. It enables Python developers to create, configure, and manage AWS services, such as EC2 and S3. Boto3 provides an easy to use, object-oriented API, as well as low-level access to AWS services.

Install boto3 using the following commands: (add '!' before the command when running on Google colab.)
```sh
pip3 install boto3
```

## Repository Setup
Repositories will be created for each student. You should see yours at https://github.com/vu-topics-in-big-data-2021/homework-2-<GITHUB USERNAME\>. Clone the repository to your home directory on the cluster using:
```sh
git clone https://github.com/vu-topics-in-big-data-2022/programming-assignment-2-pandas-<GITHUB USERNAME>.git
```
I may push updates to this homework assignment in the future. To setup an upstream repo, do the following:
```sh
git remote add upstream https://github.com/vu-topics-in-big-data-2022/programming-assignment-2-pandas.git
```
To pull updates do the following: 
```sh
# this will create a new branch: upstream/main
git fetch upstream

# then merge your current brach(origin/main) with upstream/main
git merge upstream/main --allow-unrelated-histories
```
You will need to resolve conflicts if they occur. An easy way to resolve conficts is [Visual Code](https://code.visualstudio.com/Download). To accept all incoming changes, go to visual code then press cmd+shift+p(for Mac) or ctrl+shift+p(for Windows), type "merge" in the search bar and select "Merge Conflict: Accept All Incoming".

## Dataset Preparation

* Download the entire dataset foler [Lahman2012 Baseball Dataset (CSV)](https://vanderbilt365.sharepoint.com/sites/TopicsInBigData/Shared%20Documents/General/Datasets/Lahman2012%20Baseball%20Dataset%20(CSV)) from Microsoft Teams.
* Go through the notebook [setup-colab2s3.ipynb](setup-colab2s3.ipynb) to ensure that your credential are setup correctly. It assumes that you have created an S3 bucket called vandy-bigdata. And you have uploaded all csv files from the data folder there.


## Pandas Assignment Part:

### Step 1 Implement all the Pandas queries identified in mlb.ipynb. 


The queries are - 

1. The number of all stars from 1937 to 1983 (using AllstarFull.csv).
2. The playerid of the player with the most hit by pitch (using the Batting.csv).
3. The players with the stolen base percentage (100% * SB/(SB+CS)) equals to 80%. Return a list of playid. (using Batting.csv).
4. The top 4 players with the most Gold Glove awards since 2000. Return a list of playerid. (using AwardsPlayers.csv)
5. All salaries earned by jamesch02(playerid) (using Salaries.csv).
6. All players who played for Vanderbilt University and ranked them in descending order according to the "yearMin" field (using Schools.csv and SchoolsPlayers.csv). Return playerids.
7. All players who played in universities in Tennessee. Return a dataframe with columns: schoolName and playerID, and without index.
8. The highest-paid All-Star player in MIN(teamid) team in the 1980s. Return the playerid. (using Salaries.csv and AllstarFull.csv)
9. The year in which Scott Aldred has the fewest walks. (using Master.csv and Pitching.csv)
10. The number of managers who served for only one team in their career in Managers.csv.
11. The standard deviation of errors Hank Aaron had in his career (round to 2 decimal places). (using Fielding.csv)
12. The average age of players at the beginning and end of their career (round to 2 decimal places). (using SchoolsPlayers.csv and Master.csv)
13. The average salary of all stars in 1998. (round to 2 decimal places) (already solved for you)
14. The average of non-all stars in 1998. (round to 2 decimal places)

**Note please skip rows that have missing data and invalid values in some csv files.** We present 2 examples of performing data-clean, you may apply similar operations in other CSV files involved in this homework.

- Case 1

  In the Master.csv file, values of some fileds are missed,
  |lahmanID|playerID |managerID|hofID|birthYear|birthMonth|birthDay|birthCountry|birthState|birthCity  |deathYear|deathMonth|deathDay|deathCountry|deathState|deathCity|nameFirst|nameLast|nameNote|nameGiven    |nameNick|weight|height|bats|throws|debut    |finalGame|college       |lahman40ID|lahman45ID|retroID |holtzID  |bbrefID  |
  |--------|---------|---------|-----|---------|----------|--------|------------|----------|-----------|---------|----------|--------|------------|----------|---------|---------|--------|--------|-------------|--------|------|------|----|------|---------|---------|--------------|----------|----------|--------|---------|---------|
  |11      |abbotgl01|         |     |1951     |2         |16      |USA         |AR        |Little Rock|         |          |        |            |          |         |Glenn    |Abbott  |        |William Glenn|        |200   |78    |R   |R     |7/29/1973|8/8/1984 |Arkansas State|abbotgl01 |abbotgl01 |abbog001|abbotgl01|abbotgl01|

  Thus, this record should be disregarded if your need to manipulate the fields containing missing value. To do so, please use the dropna function like below:

  ```python
  caller = caller.dropna()
  ```
  More details about dropna in pandas: [pandas.DataFrame.dropna](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.dropna.html), [pandas.Series.dropna](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.dropna.html).

- Case 2

  In the SchoolsPlayers.csv file, the below row is invalid since year shouldn't be zero.
  |playerID |schoolID|yearMin|yearMax|
  |---------|--------|-------|-------|
  |aingeda01|byu     |0      |0      |

  To filter our the above row, you can where or master functions as below: 
  ```python
  # option 1:
  caller = caller.where(lambda row: (row['yearMin'] != 0) & (row['yearMax'] != 0)).dropna()

  # option 2:
  caller = caller.mask(lambda row: (row['yearMin'] == 0) | (row['yearMax'] == 0)).dropna()
  ```

  More details about where and mask functions in pandas: [pandas.Series.where](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.where.html), [pandas.Series.mask](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.mask.html), [pandas.DataFrame.mask](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.mask.html), and [pandas.DataFrame.where](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.where.html).

### Step 2 Record the answers in the mlb.ipynb and save it back to your repository

# Grading Rubrics

* 90 points for queries
* 10 points for comments,explanation and clarity
* When finished submit your url to brightspace for the assignment
