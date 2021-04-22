# Test for Analytics

Test design for Analytics functionality on a Battery Monitoring System.

## Analysis-functionality to be tested

This section lists the Analysis for which _tests_ must be written.

Battery Telemetrics are collected and stored on a server.
Data for a month is stored in a [csv file](https://en.wikipedia.org/wiki/Comma-separated_values).

Analysis must be done on this csv file to find the following:
- minimum
- maximum
- count of breaches (how many times did it cross the threshold in a month?)
- record trends (date & time when the reading was continuously increasing for 30 minutes)

A PDF report of the analysis must be stored every week.
Notification must be sent when a new report is available.

## Tasks

### Analytics software dependencies

1. Access to the Server containing the telemetrics in a csv file
1. Generation of a PDF report from the analytics output
1. Notification delivery mechanism
1. Thresholds for different parameters of the Battery Management System


### System boundaries

| Item                      | Included?     | Reasoning / Assumption
|---------------------------|---------------|---
Battery Data-accuracy       | No            | Data accuracy is out of scope
Computation of maximum      | Yes           | Part of the software requirements
Off-the-shelf PDF converter | No            | PDF write trigger mocked. PDF output not tested.
Counting the breaches       | Yes           | Part of the software requirements
Detecting trends            | Yes           | Part of the software requirements
Notification utility        | No            | Notification trigger mocked. Mechanism not tested.

### Test cases

1. Generate "Invalid input" output when the csv doesn't contain expected data
1. Generate "Server Inaccessible" output when the csv is not available on the server
1. Calculate minimum and maximum readings from a csv containing positive and negative readings
1. Calculate count of breaches to count instances when the readings from a csv cross the max/min thresholds
1. Make a dictionary of trends when the readings from a csv monotonically increase for 30 minutes
1. Trigger notification once a new PDF report is generated
1. Trigger write to the PDF once a python data report is available

### Fakes and Reality

| Functionality            | Input                            | Output                   | Faked/mocked part
|--------------------------|----------------------------------|--------------------------|-----------------------------
Read input from server     | csv file                         | internal data-structure  | Fake the server store
Validate input             | csv data                         | valid / invalid          | None - it's a pure function
Notify report availability | report generated / not generated | notified / not notified  | Mock the notification trigger
Report inaccessible server | server path                      | accessible / inaccessible| Fake the server store
Find minimum and maximum   | readings data-structure          | minimum, maximum values  | None - it's a pure function
Detect trend               | readings data-structure          | data-structure of trends | None - it's a pure function
Write to PDF               | output data-structure            | PDF file                 | Mock the PDF write function
