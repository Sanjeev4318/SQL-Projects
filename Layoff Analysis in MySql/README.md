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
````
> Insert data into new table layoffs_working from layoffs

````sql
INSERT INTO layoffs_working SELECT *FROM layoffs;
````
### 3. Check and removing duplicates.
> [!NOTE]
> To remove the duplicate first we need to add unique identifier column.
> We can do that using ROW_NUMBER() function
````sql
WITH CTE_DUPLICATE AS 
( SELECT *, row_number() OVER(PARTITION BY COMPANY,LOCATION,INDUSTRY,TOTAL_LAID_OFF,`DATE`,PERCENTAGE_LAID_OFF) AS row_no
  FROM layoffs_working
)
 SELECT *FROM CTE_DUPLICATE WHERE ROW_NO >1;
````
Result: Below screenshot shows the duplicate rows that needs to be deleted
![Duplicate_SC](https://github.com/user-attachments/assets/c080e536-0b50-4ae8-8208-0612d1c09251)




