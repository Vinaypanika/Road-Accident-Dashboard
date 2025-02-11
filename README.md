# Accident Data Analysis - SQL Queries

## Basic SQL Queries

### 1. How many accidents occurred in total?
```sql
select count(*) as Total_accidents from accident_data;
```

### 2. What are the unique accident severity levels?
```sql
select distinct accident_severity from accident_data;
```

### 3. Find the total number of accidents by year.
```sql
select year(accident_date) as year, count(*) as Total_accidents
from accident_data
group by year(accident_date)
order by year(accident_date) desc;
```

### 4. Get the top 5 road types with the most accidents.
```sql
select top 5 road_type, count(*) as Total_accidents
from accident_data
group by road_type
order by count(*) desc;
```

### 5. How many accidents occurred in urban vs. rural areas?
```sql
select top 2 urban_or_rural_area, count(*) as total_accidents
from accident_data
group by urban_or_rural_area
order by count(*) desc;
```

### 6. Find accident trends for a year (2022).
```sql
select datename(month, accident_date) as month, count(*) as Total_accidents
from accident_data
where year(accident_date) = 2022
group by datename(month, accident_date), month(accident_date)
order by month(accident_date);
```

### 7. Get road types with more than 10,000 accidents.
```sql
select road_type, count(*) as Total_accident
from accident_data
group by road_type
having count(*) > 10000;
```

### 8. Find the top 3 weather conditions causing accidents.
```sql
select top 3 weather_conditions, count(*) as Total_accidents
from accident_data
group by weather_conditions
order by count(*) desc;
```

### 9. Compare accidents on weekdays vs weekends.
```sql
select
(case when datepart(weekday, accident_date) in (1,7) then 'Weekend'
else 'Weekday' end) as Day,
count(*) as Total_accidents from accident_data
group by (case when datepart(weekday, accident_date) in (1,7) then 'Weekend'
else 'Weekday' end)
order by count(*) desc;
```

### 10. Find the total number of accidents per district.
```sql
select district_area, count(*) as Total_accidents
from accident_data
group by district_area
order by count(*) desc;
```

## Intermediate SQL Queries

### 11. Rank accident severities within each road type.
```sql
select road_type, accident_severity, count(*) as total_accidents,
rank() over(partition by road_type order by count(*) desc) as severity_rank
from accident_data
group by road_type, accident_severity
order by road_type, severity_rank;
```

### 12. Find the Top 5 months with the highest accident increase compared to the previous month.
```sql
with cte_accidents as
(select month(accident_date) as m,
datename(month, accident_date) as month, count(*) as Current_month_accidents,
lag(count(*)) over(order by month(accident_date)) as previous_month_accidents
from accident_data
group by month(accident_date), datename(month, accident_date))
select top 5 month, Current_month_accidents,
(Current_month_accidents - previous_month_accidents) as Increase
from cte_accidents
order by Increase desc;
```

### 13. Get the accident count for each district and its percentage of total accidents.
```sql
select district_area, count(*) as number_of_accidents,
count(*)*100.0/sum(count(*)) over() as percentage
from accident_data
group by district_area
order by percentage desc;
```

### 14. Find the road types with the highest number of Serious accidents.
```sql
select road_type, count(*) as Total_accidents
from accident_data
where accident_severity = 'Serious'
group by road_type
order by count(*) desc;
```

### 15. Find the average number of accidents per day.
```sql
select count(*)*1.0 / count(distinct accident_date) as avg_accidents_per_day
from accident_data;
```

## Advanced SQL Queries

### 16. Find the longest accident-free period.
```sql
select max(datediff(day, previous_accident, accident_date)) as longest_accident_free_period
from (
select accident_date, lag(accident_date) over(order by accident_date) as previous_accident
from accident_data) t;
```

### 17. Find the percentage change in accident count per year.
```sql
with cte_year as
(select year(accident_date) as year, count(*) as current_year_accidents,
lag(count(*)) over(order by year(accident_date)) as previous_year_accidents
from accident_data
group by year(accident_date))
select year, (current_year_accidents - previous_year_accidents)*100.0/previous_year_accidents
as percentage_change from cte_year
order by year;
```

### 18. Find the month with the lowest accident count.
```sql
select top 1 datename(month, accident_date) as month, count(*) as Total_accidents
from accident_data
group by datename(month, accident_date)
order by count(*) desc;
```

### 19. Identify accident trends in rainy conditions.
```sql
select datename(month, accident_date) as month, count(*) as total_accident
from accident_data
where weather_conditions like '%rain%'
group by datename(month, accident_date), month(accident_date)
order by month(accident_date);
```

## 📞 Contact

If you have any questions or want to connect, feel free to reach out:

- 📧 Email: [vinaypanika@gmail.com](mailto:vinaypanika@gmail.com)
- 💼 LinkedIn: [Vinay Kumar Panika](https://www.linkedin.com/in/vinaykumarpanika)
- 📂 GitHub: [VinayPanika](https://github.com/Vinaypanika)
- 🌐 Portfolio: [Visit My Portfolio](https://sites.google.com/view/vinaykumarpanika/home)
- 📞 Mobile: [+91 7415552944](tel:+917415552944)
