## API Functionality Tests

### Area Creation and Management

- [x] Verify successful creation, modification, and deletion of areas.
- [x] Test setting and retrieving occupancy limits for different areas.
- [x] Validate assignment and removal of devices to areas.
- [x] Confirm proper handling of exceptions (e.g., specific users or groups).

### Device Status and Control

- [x] Check real-time occupancy status updates via API calls.
- [x] Test triggering actions (e.g., notifications, access denial) based on occupancy thresholds.
- [x] Verify device status changes are reflected correctly in the API responses.
	- [ ] Create an issue on expanding device-related issues in areas.

## SignalR Integration Tests

### Real-time Occupancy Updates

- [x] Validate that the SignalR client receives real-time occupancy updates when events occur (e.g., user enters/exits an area).
- [x] Test accuracy and timeliness of occupancy data received through SignalR.

### System log events

- [x] Verify that system log events are created for occupancy events (e.g., *Occupancy limit reached - Area locked/unlocked*).
- [x] Check the content and format of the system log records.

### Event Notifications

- [x] Verify that SignalR notifications are triggered for occupancy events (e.g., reaching the limit, exceeding the limit).
- [x] Check the content and format of the notification messages.

## End-to-End Scenario Tests

### Simulate User Access

- [x] Use API calls to simulate user access attempts under different occupancy scenarios (e.g., below limit, at limit, above limit).
- [x] Verify that the system responds correctly based on the configured occupancy rules and actions.
- [x] Monitor real-time occupancy updates through SignalR during the simulation.

### Test User Exception Handling

- [x] Simulate access attempts by users with exceptions configured (e.g., allowed to exceed the limit).
- [x] Confirm that the system handles exceptions correctly and overrides default occupancy rules.

### Stress Testing

- [x] Omitted due to the reported limited usage of occupancy across all customers.

## Error Handling and Validation Tests

### API Error Handling

- [x] Test API calls with invalid parameters, incorrect authentication credentials, and other error conditions.
- [x] Verify that the API returns appropriate error codes and messages.

### Data Validation

- [x] Validate the data integrity of occupancy information retrieved through the API and SignalR.
- [x] Check for inconsistencies, data corruption, or unexpected values.
