实现：查询h_production_management和h_pro

查询结果：项目名称，模具编号，管片编号，验收项，计划完成时间，实际完成时间，是否有违规记录，预制厂，分包，总包
//缺少的是模具编号、管片编号和验收项
SELECT
	h_production_management.id,
	h_production_management.NAME,
  h_plan_management.plan_end_time,
	h_plan_management.comloetion_end_time,
  h_plan_management.preset,
  h_plan_management.points,
  h_plan_management.total
FROM
	h_production_management
	LEFT JOIN h_plan_management ON h_plan_management.production_id = h_production_management.id 
ORDER BY
  
  
