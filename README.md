# mysql_sys.host_summary_by_stages

SELECT 
    IF((`performance_schema`.`events_stages_summary_by_host_by_event_name`.`HOST` IS NULL),
        'background',
        `performance_schema`.`events_stages_summary_by_host_by_event_name`.`HOST`) AS `host`,
    `performance_schema`.`events_stages_summary_by_host_by_event_name`.`EVENT_NAME` AS `event_name`,
    `performance_schema`.`events_stages_summary_by_host_by_event_name`.`COUNT_STAR` AS `total`,
    FORMAT_PICO_TIME(`performance_schema`.`events_stages_summary_by_host_by_event_name`.`SUM_TIMER_WAIT`) AS `total_latency`,
    FORMAT_PICO_TIME(`performance_schema`.`events_stages_summary_by_host_by_event_name`.`AVG_TIMER_WAIT`) AS `avg_latency`
FROM
    `performance_schema`.`events_stages_summary_by_host_by_event_name`
WHERE
    (`performance_schema`.`events_stages_summary_by_host_by_event_name`.`SUM_TIMER_WAIT` <> 0)
ORDER BY IF((`performance_schema`.`events_stages_summary_by_host_by_event_name`.`HOST` IS NULL),
    'background',
    `performance_schema`.`events_stages_summary_by_host_by_event_name`.`HOST`) , `performance_schema`.`events_stages_summary_by_host_by_event_name`.`SUM_TIMER_WAIT` DESC
