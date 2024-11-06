---
lastSync: Fri Oct 25 2024 18:10:34 GMT+0800 (Taipei Standard Time)
---
This test suite covers both occupancy and anti-passback functionalities of 2N Access Commander, which together comprise the "area restrictions" feature set. It leverages the system's open API and SignalR integration for comprehensive testing.

## API Functionality Tests

### Area Creation and Management

- [x] Verify successful creation, modification, and deletion of areas.
- [x] Test setting and retrieving occupancy limits for different areas.
- [x] Validate assignment and removal of devices to areas.
- [x] Confirm proper handling of exceptions (e.g., specific users or groups).

### Anti-passback Configuration

- [x] Verify enabling and disabling anti-passback for specific areas.
- [x] Test different anti-passback modes (hard, soft, etc.) and their impact on access decisions.
- [x] Validate configuration of allowed passage attempts before triggering anti-passback.

### Access Point Configuration

- [ ] Confirm correct assignment of access points as entry or exit points for anti-passback zones.
- [ ] Test API calls for modifying access point types and their effect on anti-passback logic.

### Device Status and Control

- [x] Check real-time occupancy status updates via API calls.
- [x] Test triggering actions (e.g., notifications, access denial) based on occupancy thresholds.
- [x] Verify device status changes are reflected correctly in the API responses.
	- [ ] Create an issue on expanding device-related issues in areas.

## SignalR Integration Tests

### Real-time Occupancy Updates

- [x] Validate that the SignalR client receives real-time occupancy updates when events occur (e.g., user enters/exits an area).
- [x] Test accuracy and timeliness of occupancy data received through SignalR.

### Anti-passback Events

- [x] Validate that the SignalR client receives real-time notifications for anti-passback events (e.g., violations, bypasses).
- [x] Check the content and format of the notification messages, including user details and access point information.

### Access Point Status

- [ ] Monitor access point status updates via SignalR to track entry and exit events.
- [ ] Verify that the status reflects the current anti-passback state of each access point.

### System log events

- [x] Verify that system log events are created for occupancy and anti-passback events.
- [x] Check the content and format of the system log records.

### Event Notifications

- [x] Verify that SignalR notifications are triggered for occupancy events (e.g., reaching the limit, exceeding the limit) and anti-passback events.
- [x] Check the content and format of the notification messages.

### Connection Stability

- [ ] Test the stability of the SignalR connection under various conditions (e.g., network interruptions, server restarts).
- [ ] Ensure proper reconnection and data recovery mechanisms are in place.

## End-to-End Scenario Tests

### Simulate User Access

- [ ] Use API calls to simulate user access attempts under different occupancy scenarios (e.g., below limit, at limit, above limit) and anti-passback scenarios (valid entry/exit, violations, bypasses).
- [x] Verify that the system responds correctly based on the configured occupancy and anti-passback rules and actions.
- [x] Monitor real-time occupancy updates and anti-passback events through SignalR during the simulation.

### Test Different Anti-passback Modes

- [ ] Simulate access attempts under different anti-passback modes to validate their specific behaviors.
- [ ] Confirm that the system enforces the correct restrictions and actions for each mode.

### Test Exception Handling

- [ ] Simulate access attempts by users with exceptions configured (e.g., allowed to exceed the occupancy limit, bypass anti-passback).
- [ ] Confirm that the system handles exceptions correctly and overrides default rules.

### Stress Testing
- [x] Omitted due to the reported limited usage of occupancy across all customers.

## Error Handling and Validation Tests

### API Error Handling

- [x] Test API calls with invalid parameters, incorrect authentication credentials, and other error conditions.
- [x] Verify that the API returns appropriate error codes and messages for both occupancy and anti-passback related errors.

### Data Validation

- [x] Validate the data integrity of occupancy information, anti-passback events, and access point status information retrieved through the API and SignalR.
- [x] Check for inconsistencies, data corruption, or unexpected values.

## Combined Occupancy and Anti-passback Tests

### Integrated Scenarios

- [ ] Design test scenarios that combine occupancy and anti-passback rules to verify their interaction.
- [ ] For example, simulate access attempts that trigger both occupancy and anti-passback restrictions.
- [ ] Confirm that the system enforces both sets of rules correctly and handles any conflicts appropriately.

### Shared Area Configuration

- [ ] Test scenarios where occupancy and anti-passback are enabled for the same area.
- [ ] Verify that both functionalities operate correctly without interfering with each other.
- [ ] Ensure that the system handles events and notifications for both features effectively.