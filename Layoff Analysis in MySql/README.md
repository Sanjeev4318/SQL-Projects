# Layoffs Analysis in MySql ##
 
This project was made using Alex Freberg's YouTube [tutorial](https://www.youtube.com/watch?v=4UltKCnnnTA).

### Objective:
- Remove duplicates
- Standardize the data
- Populate the null or blank cells.
- Delete data with nulls that we couldn't populate.

## PROCESS

### 1. Import data into MySql.

>### Steps 

Login to the MySql --> Right click on the table in which we want to import the data.--> Click on Table Data Import Wizard --> Click on Browse and select the file layoff.csv --> Click on Next --> Choose Create new table --> Click on Next->Next->Next->Next --> Now Click on Finish 


### 2. Create a duplicate of the source file layoffs_working
> [!IMPORTANT]
> Never do modification in the source file make a copy of that file and work on that file!!

````sql
CREATE TABLE layoffs_working LIKE layoffs;
