# coal_terminal_utilization_story
**Context**
  - Assessing which Coal Reclaimer machines require maintenance in the upcoming month
  - The machines run 24/7/365 and a minute of downtime equates to millions of dollars in lost revenue
  - There are two types of machines, **Reclaimers** and **Stackers**, the latter will have inherent downtime when it is stacking coal

**Criterion**
  - A reclaimer machine requires maintenances when withing the previous month there was at least one 8-hour period when the average idle capacity was ver 10%
  - **Idle Capacity** is a utlisation metric which is definied as:
  
    * ![equation](https://latex.codecogs.com/png.latex?%5Cdpi%7B100%7D%20%5Cbg_white%20Idle%20%5C%20Capcity%20%3D%20%5Cfrac%7B%28Actual%20%5C%20Tonnage%20-%20Nominal%20%5C%20Capacity%29%7D%7BNominal%20%5C%20Capacity%7D)
 
**Task**
- Find out which of the 5 machines have exceeded this level and create a report for the executive stakeholders with your recommendations

**Techniques & Process**
- A dummy column named 'Timeline' is created in Excel on a new sheet where we recreate the 'Datetime' column from the earliest date in the source data and increment by 1 hour
- The Timeline column is then used as a master to connect the rest of the data with **Left-Joins**
- After creating the joins in Tableau, the data was further cleaned by hiding the vestigial Datetime columns
- All rows with values were then **Pivoted**
- Below you can see the joins as well as the pivoted data that has been renamed:

![](https://github.com/latiful-hassan/coal_terminal_utilization_story/blob/main/coal_terminal_screenshots/coal_joins.png)
- To find the Idle Capacity, I created a graph showing machines against hours and added a **Table Calculation** of **Difference** computed with **Table Down**. This calculates the (Actual Tonnage - Nominal Capacity) in Tonnes:

![](https://github.com/latiful-hassan/coal_terminal_utilization_story/blob/main/coal_terminal_screenshots/coal_idle_capacity_table_calc.png)

- Now I changed the axis to show positive values by created a **Calculated Field** as well as showing percentage and added a **Reference Line**:

![](https://github.com/latiful-hassan/coal_terminal_utilization_story/blob/main/coal_terminal_screenshots/coal_idle_capacity_table_calc_pos.png)

- Created a graph showing an 8-hour moving average to assess if the machine needs maintenance via a calculated field with formula:
  * WINDOW_AVG([Idle Capacity Percent Positive], -7, 0 )

![](https://github.com/latiful-hassan/coal_terminal_utilization_story/blob/main/coal_terminal_screenshots/idle_capacity_8_hour_moving_average.png)

- As we can see from the viz above, there are some outliers in our data (SR1/SR4A) as there are times where this is no data. Due to this, the moving average is calculating on blank cells which is not an adequate representation. Therefore, we can exclude these discrepancies by editing the formula in the calculated field and adding a conditional statement: 

  IF (WINDOW_COUNT([Idle Capacity Percent Positive], -7, 0) = 8) <br />
  THEN WINDOW_AVG([Idle Capacity Percent Positive], -7, 0) <br />
  ELSE NULL <br />
  END

![](https://github.com/latiful-hassan/coal_terminal_utilization_story/blob/main/coal_terminal_screenshots/idle_capacity_8_hour_moving_average_conditional.png)

**Story**

- Below is a screenshot of the first page in the analysis story, please see the file named 'coal_terminal_maintenance_analysis' for the full story:

![](https://github.com/latiful-hassan/coal_terminal_utilization_story/blob/main/coal_terminal_screenshots/coal_terminal_maintenance_story.png)

**Summary of Process**
  * Visualised aggregate sum of data
  * Created a table calculation on the percentage difference
  * Applied a calcualted field using **Window Average** to find the 8-hour moving average

**Analysis & Insights**
- Looking at the viz we can see two problem areas - RL1 and SR6
- If we look at SR6, the time when the idle capacity exceeds 10% plateus at the same time that RL2 has a dip in production.
- From this insight as well as looking at an image of the machines, we can see that RL2 and SR6 work in the same line, therefore, one machine can work at a higher capacity while the other does not
- This means than SR6 does not neeed additional maintenance
- From the trend lines we can see that the P-Values for RL1 and SR4A project further down-time for both machines
- In conclusion, RL1 is the only machine that does require immediate maintenance while SR4A will need maintenance soon
