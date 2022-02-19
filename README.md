# coal_terminal_utilization_story
**Context**:
  - Assessing which Coal Reclaimer machines require maintenance in the upcoming month. The machines run 24/7/365 and a minute of downtime equates to millions of dollars in lost revenue.

**Criterion**:
  - a reclaimer machine requires maintenances when withing the previous month there was at least one 8-hour period when the average idle capacity was ver 10%
  - **Idle Capacity** is a utlisation metric which is definied as:
    * Idle Capacity = (Actual Tonnage - Nominal Capacity)/Nominal Capacity
 
**Task**: 
- find out which of the 5 machines have exceeded this level and create a report for the executive stakeholders with your recommendations


