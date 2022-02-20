# coal_terminal_utilization_story
**Context**:
  - assessing which Coal Reclaimer machines require maintenance in the upcoming month
  - the machines run 24/7/365 and a minute of downtime equates to millions of dollars in lost revenue

**Criterion**:
  - a reclaimer machine requires maintenances when withing the previous month there was at least one 8-hour period when the average idle capacity was ver 10%
  - **Idle Capacity** is a utlisation metric which is definied as:
  
    * ![equation](https://latex.codecogs.com/png.latex?%5Cdpi%7B100%7D%20%5Cbg_white%20Idle%20%5C%20Capcity%20%3D%20%5Cfrac%7B%28Actual%20%5C%20Tonnage%20-%20Nominal%20%5C%20Capacity%29%7D%7BNominal%20%5C%20Capacity%7D)
 
**Task**: 
- find out which of the 5 machines have exceeded this level and create a report for the executive stakeholders with your recommendations

**Techniques & Process**:
- a dummy column named 'Timeline' is created in Excel on a new sheet where we recreate the 'Datetime' column from the earliest date in the source data and increment by 1 hour
- the Timeline column is then used as a master to connect the rest of the data with left-joins
- after creating the joins in Tableau, the data was further cleaned by hiding the vestigial Datetime columns
- all rows with values were then pivoted

![alt text](https://github.com/latiful-hassan/coal_terminal_utilization_story/blob/main/coal_terminal_screenshots/coal_joins.png)

