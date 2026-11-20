# DAX Measures – Garment Production Performance Dashboard

All measures are stored in the `_Measures` table.
Each row in the `Production` table is one team on one day (a "team-day").

## Summary

| Measure | What it shows | Format |
|---|---|---|
| Team-Days | Number of team-days | Whole number |
| Avg Actual Productivity | Average productivity achieved | % (1 decimal) |
| Avg Target Productivity | Average target set by management | % (1 decimal) |
| Productivity Gap | Actual minus target (percentage points) | % (1 decimal) |
| Team-Days Met | Team-days that reached the target | Whole number |
| % Target Met | Share of team-days that reached the target | % (1 decimal) |
| Total Idle Time (min) | Total minutes of production stoppage | Whole number |
| Idle Events | Team-days with any stoppage | Whole number |
| Total Idle Workers | Workers affected by stoppages | Whole number |
| Avg Overtime (min) | Average overtime per team-day | 1 decimal |
| Avg WIP | Average unfinished pieces (Sewing only; Finishing has no WIP) | 1 decimal |
| Total Style Changes | Total style changeovers | Whole number |
| Avg Team Size | Average workers per team | 1 decimal |
| Median Incentive (BDT) | Typical bonus, not affected by a few very large values | 1 decimal |
| Avg Overtime per Worker (min) | Average overtime per worker, so teams of different sizes can be compared fairly | 1 decimal |

## DAX code

```DAX
Team-Days = COUNTROWS ( Production )

Avg Actual Productivity = AVERAGE ( Production[actual_productivity] )

Avg Target Productivity = AVERAGE ( Production[targeted_productivity] )

Productivity Gap = [Avg Actual Productivity] - [Avg Target Productivity]

Team-Days Met = CALCULATE ( [Team-Days], Production[met_target] = "Met" )

% Target Met = DIVIDE ( [Team-Days Met], [Team-Days] )

Total Idle Time (min) = SUM ( Production[idle_time] )

Idle Events = CALCULATE ( [Team-Days], Production[idle_time] > 0 )

Total Idle Workers = SUM ( Production[idle_men] )

Avg Overtime (min) = AVERAGE ( Production[over_time] )

Avg WIP = AVERAGE ( Production[wip] )

Total Style Changes = SUM ( Production[no_of_style_change] )

Avg Team Size = AVERAGE ( Production[no_of_workers] )

Median Incentive (BDT) = MEDIAN ( Production[incentive] )

Avg Overtime per Worker (min) = AVERAGE ( Production[overtime_per_worker] )
```

## Calculated columns (Production table)

```DAX
productivity_gap = Production[actual_productivity] - Production[targeted_productivity]

met_target = IF ( Production[actual_productivity] >= Production[targeted_productivity], "Met", "Missed" )

team_name = "Team " & FORMAT ( Production[team], "00" )

idle_flag = IF ( Production[idle_time] > 0, "Had stoppage", "No stoppage" )

style_change_label =
SWITCH (
    Production[no_of_style_change],
    0, "0 changes",
    1, "1 change",
    2, "2 changes",
    Production[no_of_style_change] & " changes"
)

overtime_per_worker = DIVIDE ( Production[over_time], Production[no_of_workers] )
```

`style_change_label` is sorted by `no_of_style_change`. `overtime_per_worker` divides team overtime by team size, because a large team naturally has more total overtime.

## Date table (DimDate)

```DAX
DimDate =
VAR MinDate = MIN ( Production[date] )
VAR MaxDate = MAX ( Production[date] )
RETURN
    ADDCOLUMNS (
        CALENDAR ( MinDate, MaxDate ),
        "Year", YEAR ( [Date] ),
        "Month Number", MONTH ( [Date] ),
        "Month", FORMAT ( [Date], "MMM" ),
        "Week Start", [Date] - MOD ( WEEKDAY ( [Date], 1 ), 7 ),
        "Day of Week", FORMAT ( [Date], "dddd" ),
        "Day Sort", MOD ( WEEKDAY ( [Date], 1 ), 7 ) + 1
    )
```

Weeks start on Saturday because the factory works Saturday to Thursday (Friday is the weekend in Bangladesh).
