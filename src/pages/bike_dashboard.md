# Number of bikes available

When a user finds a station is empty, they cannot start their journey where they would like. This means they either:
1. Abandon their bike hire 
2. Walk to another station with bikes

both of which are a poor UX

A key KPI for how well the bike network is therefore what proportion of stations have bikes available. We also show how many have a low number (less than 4 bikes), and so are at risk of running out.


```bike_query

select
    date_trunc('minute', timestamp)::timestamp as timestamp_date,
    case 
        when num_bikes_available < 5 then cast(num_bikes_available as text)
        else '5+'
    end as bikes_available_group, 
    count(*) / (sum(count(*)) over (partition by timestamp)) as stationsavailable_pct
from station_status_log
group by timestamp, bikes_available_group 
order by timestamp, bikes_available_group
limit 300

```

<AreaChart 
    data={data.bike_query} 
    x=timestamp_date
    y=stationsavailable_pct
    series=bikes_available_group
    fmt=pct
    sort =false
    title='Percent of Stations with each number of Bikes available'
    yAxisTitle='% of Stations'
    xAxisTitle='Date & Time'
/>


The most recent day of data was logged on <Value data={data.bike_query} fmt=date/> and the number of bikes available was <Value data={data.bike_query} column="stationsavailable_PCT"/>.
