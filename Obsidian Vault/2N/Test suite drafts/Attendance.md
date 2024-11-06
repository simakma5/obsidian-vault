## API Functionality Tests

### Attendance Configuration

- [ ] Verify enabling and disabling attendance monitoring for specific users and groups using API calls.
- [ ] Test configuring different attendance modes (In-Out, Free) via the API and validate their impact on data collection.
- [ ] Validate setting and retrieving working time calendars for different user groups using API requests.
- [ ] Confirm correct configuration of attendance rules, including overtime calculation and break deductions, through API calls.

### User and Group Management

- [ ] Verify API functionality for assigning and removing users from attendance monitoring.
- [ ] Confirm that user and group permissions are correctly enforced for accessing attendance data via the API.

### Data Retrieval and Export

- [ ] Test API calls for retrieving attendance data for specific users, groups, and time periods.
- [ ] Validate the format and content of the retrieved data, including timestamps, access points, and attendance status.
    - [ ] **Validate timestamps:** Ensure timestamps are accurate and in the expected format.
    - [ ] **Verify access point information:** Confirm that the correct access points are associated with each attendance event.
    - [ ] **Check attendance status:** Validate that the attendance status (In/Out) is correctly recorded.
- [ ] Verify API functionality for exporting attendance data in different formats (e.g., CSV, PDF).
    - [ ] **CSV Export**
        - [ ] Validate the structure and delimiters of the CSV file.
        - [ ] Confirm that all relevant data fields are included in the export.
        - [ ] Verify that data is correctly encoded and escaped in the CSV format.
    - [ ] **PDF Export**
        - [ ] Validate the layout and formatting of the PDF report.
        - [ ] Confirm that all relevant data is presented clearly and accurately.
        - [ ] Check for correct pagination and overall report structure.

## End-to-End Scenario Tests

### Simulate User Activity

- [ ] Use API calls to simulate user check-ins and check-outs at different times and access points.
    - [ ] **Vary check-in/out times:** Simulate arrivals and departures at different times of the day, including outside of working hours.
    - [ ] **Use different access points:** Simulate access events using various access points to test location tracking.
- [ ] Verify that the system correctly records attendance data based on the configured rules and working time calendars.
- [ ] Retrieve the recorded attendance data via API calls and validate its accuracy and completeness.

### Test Different Attendance Modes

- [ ] Simulate user activity under different attendance modes (In-Out, Free) to validate their specific behaviors and data collection.
- [ ] Confirm that the system tracks and reports attendance data according to the selected mode.
- [ ] Retrieve and validate the data for each mode using API calls.

### Test Attendance Rule Variations

- [ ] Simulate scenarios that trigger different attendance rules, such as overtime, early departures, and breaks, using API calls.
- [ ] Verify that the system correctly calculates and applies these rules to the attendance data.
- [ ] Retrieve the data via API calls and confirm the correct application of attendance rules.

## Error Handling and Validation Tests

### API Error Handling

- [ ] Test API calls related to attendance with invalid parameters and other error conditions.
    - [ ] **Invalid parameters:** Test with missing or incorrect parameters in API requests.
    - [ ] **Data format errors:** Test with incorrect data formats in API requests (POST).
- [ ] Verify that the API returns appropriate error codes and messages for attendance-related errors.
    - [ ] **Validate error codes:** Ensure that the API returns the correct HTTP status codes for different error conditions.
    - [ ] **Check error messages:** Verify that the error messages are informative and helpful for troubleshooting.

### Data Validation

- [ ] Validate the data integrity of attendance records retrieved through the API.
    - [ ] **Check for data consistency:** Ensure that the data is consistent across different API calls and reports.
    - [ ] **Validate data types:** Confirm that the data types of retrieved values are correct.
    - [ ] **Test for data completeness:** Ensure that all expected data fields are present and populated.
- [ ] Verify that exported attendance reports (CSV and PDF) are accurate and complete.
    - [ ] **Cross-check data:** Compare the data in the exported reports with the data retrieved directly through the API.
    - [ ] **Validate data formatting:** Ensure that the data is correctly formatted in the exported reports.
    - [ ] **Bulk attendance export:** Verify data integrity, formatting and sorting in the resulting CSV file.
- [ ] Notable data verification cases inspired by the regression test suite and bugs discovered during previous releases which have not been covered by release tests:
	- [ ] Only the current user's data are included in either exported file.
	- [ ] Both exported files are named correctly (given by user name and time period).
	- [ ] User name, time period, working time, hours worked and balance are included in the header of the exported PDF.
	- [ ] Both exports show correct first and last access (from and to), balance, hours worked and comments for **all individual days**.

## TBD
-  [Non-export attendance regression cases](https://testrail.2n.cz/index.php?/suites/view/5&group_by=cases:section_id&group_order=asc&display_deleted_cases=0&group_id=479)
- Bugs from previous releases
	- https://dev.2n.cz/browse/HIPSU-7382
	- https://dev.2n.cz/browse/HIPSU-7659
	- https://dev.2n.cz/browse/HIPSU-7645
	- https://dev.2n.cz/browse/HIPSU-7982
	- https://dev.2n.cz/browse/HIPSU-8040
	- https://dev.2n.cz/browse/HIPSU-7988
	- https://dev.2n.cz/browse/HIPSU-7877

## JIRA issue
https://dev.2n.cz/browse/AUT-583