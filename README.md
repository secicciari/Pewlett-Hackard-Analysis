# Pewlett-Hackard-Analysis

## Project Overview

### Purpose of Analysis
I was asked to work with Bobby at Pewlett Hackard to determine the number of retiring employees per title and identify employees who are eligible to participate in a mentorship program so that the company can adequately preprare for these retirements.
## Results
### Overview
My analysis revealed the below:
- Overall, there are 90,398 employees that will be retiring.
![summary of retiree titles](https://github.com/secicciari/Pewlett-Hackard-Analysis/blob/main/Images/retirement_by_title.PNG)
- Only two employees at Manager level are retiring.
- There are 1,549 employees eligible for mentorship.
- The title with the most retirement-ready employees is Senior Engineer.

### Summary
- 90,398 roles will need to be filled as the "silver tsunami" begins to make an impact.
- Since there are only 1,549 current employees eligible for mentorship, there are more than enough retirement-ready employees to serve as mentors for the next generation.

### Recommendations for Additional Insight
- I would recommend going one step further in the mentorship eligibility analysis to determine which departments may be lacking employees who are qualified to learn from the retirement-ready employees in their department.
You can accomplish this analysis by running the following queries:
```sql
SELECT DISTINCT me.emp_no,
    me.first_name,
    me.last_name,
    me.title,
    de.dept_no
INTO mentorship_eligibility_dept
FROM mentorship_eligibility AS me
    INNER JOIN dept_emp AS de
        ON (me.emp_no = de.emp_no)
ORDER BY de.dept_no;

SELECT COUNT (med.emp_no), med.dept_no
INTO mentorship_by_dept
FROM mentorship_eligibility_dept as med
GROUP BY med.dept_no
ORDER BY count DESC;
```
- I would also recommend doing a deeper analysis on the salaries of the retirement-ready employees to identify cost savings and determine whether there will be excess funds that can be used to train up younger employees to fill the gaps.
You can get started on this analysis by running the following query:
```sql
SELECT DISTINCT ON (e.emp_no) e.emp_no,
    e.first_name,
    e.last_name,
    e.birth_date,
    de.from_date,
    de.to_date,
    s.salary
INTO retiring_salaries
FROM employees AS e
    INNER JOIN dept_emp AS de
        ON (e.emp_no = de.emp_no)
    INNER JOIN salaries AS s
        ON (e.emp_no = s.emp_no)
WHERE (e.birth_date BETWEEN '1952-01-01' AND '1955-12-31')
    AND (de.to_date = '9999-01-01')
ORDER BY e.emp_no;
```
