
## Example of tvf and cross apply

this is good way to use a cte and cross apply stack table value function on top of each other  

      WITH CTE AS (
          SELECT 201827 AS FiscalWeek
          UNION ALL SELECT 201828
          UNION ALL SELECT 201829
          UNION ALL SELECT 201830
          UNION ALL SELECT 201831
          UNION ALL SELECT 201832
          UNION ALL SELECT 201833
          UNION ALL SELECT 201834
          UNION ALL SELECT 201835
          UNION ALL SELECT 201836
          UNION ALL SELECT 201837
          UNION ALL SELECT 201838
          UNION ALL SELECT 201839
          UNION ALL SELECT 201840)
      , tvf as (
      SELECT tvf.* FROM CTE AS c
          CROSS APPLY database.dbo.tfnFTE(c.FiscalWeek) AS tvf)
      select sum(tvf.FTE) as SumFTE
      ,tvf.yr_week
      ,a.cost_ctr_no 
      from tvf
      left join [database_Staging].[dbo].[Csxctrr] a on tvf.ctr_no= a.ctr_no
      group by 
      yr_week
      ,a.cost_ctr_no
