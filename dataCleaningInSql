select * from layoff_copy;

SELECT *, ROW_NUMBER() over(Partition by company,location, industry, total_laid_off, percentage_laid_off, `date`, stage, country,funds_raised_millions) as row_num
from layoff_copy;
-- here this query group by the above columns then it finds all the value which are same and start numbering them 1, 2,3.. so above 1 means there are duplicate 
-- now we will use cte to find duplicate

with finding_duplicate as (
SELECT *, ROW_NUMBER() over(Partition by company,location, industry, total_laid_off, percentage_laid_off, `date`, stage, country,funds_raised_millions) as row_num
from layoff_copy)
select * from finding_duplicate 
where row_num>1; 

select *from layoff_copy -- just to see one example of row_num>1
 where company='Casper';
 
 -- deleting redundant data, particition by or any windows function does not alter table so we will make duplicate table and then there we will add row_num, then we will add row_num to that table by adding new column then we will populate it, then we can use delete  
 with finding_duplicate as (
SELECT *, ROW_NUMBER() over(Partition by company,location, industry, total_laid_off, percentage_laid_off, `date`, stage, country,funds_raised_millions) as row_num
from layoff_copy)
 DELETE from finding_duplicate
 where row_num>1;
 drop table layoff_copy2;
 -- creating duplicate table with extra column
 create table `layoff_copy2`(
 `company` text,
 `location` text,
 `industry` text,
 `total_laid_off` int default NULL,
 `percentage_laid_off` text,
 `date` text,
 `stage`text,
 `country` text,
 `funds_raised_millions` int default null,
 `id` int,
 `row_num` int
 
 
 );
select* from layoff_copy2;

insert into layoff_copy2
select* ,ROW_NUMBER() over(Partition by company,location, industry, total_laid_off, percentage_laid_off, `date`, stage, country,funds_raised_millions) as row_num

from layoff_copy;
-- now deletion work can start

select * from layoff_copy2
where row_num>1; -- checking data
delete from layoff_copy2 
where row_num>1; -- deletion done

-- sttandardize data
select distinct company 
from layoff_copy2;
select count(distinct company )
from layoff_copy2; -- just to check how many distinct companies are there

-- trimming company table
select trim(company), company
from layoff_copy2;

update layoff_copy2
set company=trim(company);

-- checking industry to see if there are cells with same meaning fillings but different way of writing
select distinct industry
from layoff_copy2
order by 1;

select * from layoff_copy2
where industry like '%crypto%'; 
-- we will change crypto like field to crypto
update layoff_copy2
set industry='crypto' 
where industry like '%crypto%';

-- similar cell checking for issues like above

select distinct country
from layoff_copy2
order by country;

update layoff_copy2
set country='USA' 
where country like 'United States%';

--  now date is txt so we will do str_to_date function on it
select date, str_to_date(`date`,'%m/%d/%Y' )
from layoff_copy2
order by str_to_date(`date`,'%m/%d/%Y') ;

update layoff_copy2
set `date`=str_to_date(`date`,'%m/%d/%Y');

select `dadatete` 
from layoff_copy2;

-- our date column is still text and now we will alter the table to change data type
alter table layoff_copy2
modify `date` date;

-- next step is seeing how many nulls we got per column, the less the better

select * 
from layoff_copy2 
where total_laid_off is null and percentage_laid_off is null;

-- we will delete rows with all numerical values as null as we cant do much analysis with them
delete 
from layoff_copy2 
where total_laid_off is null and percentage_laid_off is null;

select * 
from layoff_copy2
where industry='' or industry is null;

select * 
from layoff_copy2
where company='Airbnb';

update layoff_copy2
set industry=null 
where industry='';


SELECT 
    t1.company,
    t1.industry as industry_null,
    t2.industry as industry_valid
FROM layoff_copy2 t1 
JOIN layoff_copy2 t2
    ON t1.company = t2.company
WHERE t1.industry IS NULL 
  AND t2.industry IS NOT NULL;

update layoff_copy2 t1
join layoff_copy2 t2
	on t1.company=t2.company
set t1.industry=t2.industry
where t1.industry is null and t2.industry is not null;
select * from layoff_copy2
where company='Carvana';

