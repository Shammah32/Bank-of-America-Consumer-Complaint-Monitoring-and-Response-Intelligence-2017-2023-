# Bank-of-America-Consumer-Complaint-Monitoring-and-Response-Intelligence-2017-2023-
## 

## Project overview

An interactive Power BI dashboard analysing CFPB consumer complaint records recorded against Bank of America from 1 May 2017 to 28 August 2023.

## Files

* `Consumer\\\_Complaints\\\_Analytical\\\_Report.pdf` — analytical report.
* `Complaints\\\_Analytical\\\_Report.docx` — editable report source.
* `KPI\\Definitions\\and\\Results.csv` — KPI definitions and validated headline results.
* `Data\\Quality\\Assessment.csv` — data-quality checks and treatment.
* `PowerQuery\\\_DAX\\\_Notes.md` — reproducible transformation and measure notes.
* Power BI `.pbix` file and dashboard screenshots.

## Data source and scope

Source files supplied for the assignment: `Consumer\\\_Complaints.xlsx` and `ConsumerComplaints\\\_DataDictionary.csv`. The source contains 62,516 records and 12 columns. Coverage is 1 May 2017 through 28 August 2023.

## Key validated results

* Total complaints: 62,516
* Timely response: 96.06% of 61,022 valid Yes/No records
* In-progress records: 1,494
* Average CFPB forwarding interval: 1.22 days
* Median CFPB forwarding interval: 0 days
* Maximum CFPB forwarding interval: 275 days

## KPI interpretation

The timely-response denominator includes only valid `Yes` and `No` values. Blank timely-response values are not classified as `No`; all 1,494 blanks in the supplied data correspond to records marked `In progress`.

`Forwarding Interval (Days)` = `Date received` - `Date submitted`. The assignment defines Date submitted as the date CFPB received the complaint and Date received as the date CFPB sent it to the company. This metric is **not** company response or resolution time.

## Data preparation

In Power Query:

1. Confirm date fields are Date type and Complaint ID is an identifier.
2. Check duplicate Complaint IDs and full-row duplicates.
3. Profile missing values.
4. Retain missing categorical values; do not invent category assignments.
5. Create `Forwarding Interval (Days)` with:
`Duration.Days(\\\[Date received] - \\\[Date submitted])`
6. Create Complaint Year and Complaint Month from Date submitted.
7. Close \& Apply and validate the maximum interval against the source date difference (275 days).

## DAX measures

```dax
Total Complaints = COUNTROWS('Data')

Complaints In Progress =
CALCULATE(\\\[Total Complaints],
    'Data'\\\[Company response to consumer] = "In progress")

Valid Timely Responses =
CALCULATE(\\\[Total Complaints],
    'Data'\\\[Timely response?] IN {"Yes", "No"})

Timely Yes =
CALCULATE(\\\[Total Complaints],
    'Data'\\\[Timely response?] = "Yes")

Timely Response Percentage =
DIVIDE(\\\[Timely Yes], \\\[Valid Timely Responses])

Average Forwarding Days =
AVERAGE('Data'\\\[Forwarding Interval (Days)])

Median Forwarding Days =
MEDIAN('Data'\\\[Forwarding Interval (Days)])

Maximum Forwarding Days =
MAX('Data'\\\[Forwarding Interval (Days)])
```

## Dashboard pages

1. Executive Complaint Overview
2. Product \& Issue Analysis
3. Company Response Analysis
4. Complaint Timeline Analysis

## Limitations and responsible use

* Complaint records are not findings of wrongdoing and do not independently establish that allegations are substantiated.
* Forwarding interval is not company resolution time.
* 2017 and 2023 are partial years; avoid direct comparisons with complete calendar years without qualification.
* Missing sub-issue and public-response fields remain missing unless a defensible treatment is documented.
* Findings describe the supplied dataset only.

## 

