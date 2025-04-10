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
>To remove the duplicate first we need below steps:
 
- First we need to add unique identifier column. We can do that using ROW_NUMBER() function

````sql
WITH CTE_DUPLICATE AS 
( SELECT *, row_number() OVER(PARTITION BY COMPANY,LOCATION,INDUSTRY,TOTAL_LAID_OFF,`DATE`,PERCENTAGE_LAID_OFF) AS row_no
  FROM layoffs_working
)
 SELECT *FROM CTE_DUPLICATE WHERE ROW_NO >1;
````
Result: Below screenshot shows the duplicate rows that needs to be deleted
![Duplicate_SC](https://github.com/user-attachments/assets/c080e536-0b50-4ae8-8208-0612d1c09251)

- Now we need to create a table layoffs_working1 including column row_no:
````sql
CREATE TABLE layoffs_working1 LIKE layoffs_working;
alter table layoffs_working1 add column row_no int;
````
- Now copy the data from layoffs_working to layoffs_working1:
````sql
INSERT INTO LAYOFFS_WORKING1 
SELECT *,
row_number() OVER(PARTITION BY COMPANY,LOCATION,INDUSTRY,TOTAL_LAID_OFF,STAGE,COUNTRY,funds_raised_millions,`DATE`,PERCENTAGE_LAID_OFF)
AS row_no
FROM layoffs_working;
````
- Now delete the duplicates from layoffs_working1:
````sql
DELETE FROM LAYOFFS_WORKING1 WHERE ROW_NO > 1;
````
### 4. Standardizing Data.
Now we will do the data cleaning.
- First of all I run query to see names of all companies and to check if there are any inconsistencies.
````sql
SELECT DISTINCT(company), trim(company) FROM LAYOFFS_WORKING1;
````
Few company name have blank space in the begining

![Trim_SC](https://github.com/user-attachments/assets/7ee6a367-b816-406c-808b-1a3a9e0ee2b4)

To remove the space i used trim function and update the table:
````sql
UPDATE LAYOFFS_WORKING1 SET COMPANY = TRIM(COMPANY);
````
![UpdatedTBL_SC](https://github.com/user-attachments/assets/7263eca4-14ee-42b8-b3de-7792950017cc)

When checking the industry column i noticed Crypto is coming with three different name:
````sql
SELECT DISTINCT(INDUSTRY) FROM LAYOFFS_WORKING1;
````
![Industry_SC](https://github.com/user-attachments/assets/3b51a289-3d7c-4a11-9bd9-188af63250a8)

To update it i used below query:
````sql
UPDATE LAYOFFS_WORKING1 SET INDUSTRY = "Crypto Currency"
WHERE INDUSTRY LIKE "Crypto%";
````


