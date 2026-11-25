# Power Query and DAX Notes

## Power Query custom column
```powerquery
Duration.Days([Date received] - [Date submitted])
```
Set the resulting column to Whole Number.

## DAX measures
```dax
Total Complaints = COUNTROWS('Data')

Complaints In Progress =
CALCULATE([Total Complaints],
    'Data'[Company response to consumer] = "In progress")

Valid Timely Responses =
CALCULATE([Total Complaints],
    'Data'[Timely response?] IN {"Yes", "No"})

Timely Yes =
CALCULATE([Total Complaints],
    'Data'[Timely response?] = "Yes")

Timely No =
CALCULATE([Total Complaints],
    'Data'[Timely response?] = "No")

Timely Response Percentage =
DIVIDE([Timely Yes], [Valid Timely Responses])

Missing Timely Response =
CALCULATE([Total Complaints],
    ISBLANK('Data'[Timely response?]))

Average Forwarding Days =
AVERAGE('Data'[Forwarding Interval (Days)])

Median Forwarding Days =
MEDIAN('Data'[Forwarding Interval (Days)])

Maximum Forwarding Days =
MAX('Data'[Forwarding Interval (Days)])
```

## Validation
Independent check from original date columns:
```dax
Maximum Forwarding Days - Check =
MAXX(
    ALL('Data'),
    DATEDIFF('Data'[Date submitted], 'Data'[Date received], DAY)
)
```
Expected result with no data restrictions: 275 days.
