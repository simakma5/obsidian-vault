## Installation

Clone the emulator repository from [2N GitLab](https://gitlab.2n.cz/ac/tools/schindler-emulator):

```shell
git clone git@gitlab.2n.cz:ac/tools/schindler-emulator.git
```

Install dependencies:

```bash
  npm run install
```

Run app without debugging:

```bash
  npm run start
```

Run app with debugging:

```bash
  npm run start-debug
```
## Functionality

The emulator provides two services:

- a TCP server hosted on [localhost:3333](localhost:3333) for client-server communication,
- a minimalist HTML server on [localhost:3388](localhost:3388) for viewing the user DB.

For the TCP communication, telnet or a TCP client, such as YAT, can be used.

> [!WARNING] Message format
> The emulator is implemented so that the message framing using hexadecimal characters STX (Hex 02) at the beginning and ETX (Hex 03) at the end is **compulsory**.
> Using YAT: `<STX>[message]<ETX>` or `/h(02)[message]/h(03)`.
## Testing

- Go through the [Schindler PORT API specification document](https://dev.2n.cz/secure/attachment/127112/ThirdPartyDatabase.pdf) and verify that the emulator responds to the implemented calls accordingly.
- Document all discrepancies and report them in a comment to the JIRA issue.
## Currently implemented functions

- `RX_I_AM_ALIVE`
- `CHANGE_INSERT_PERSON_WITH_NACK`
- `DELETE_PERSON_WITH_NACK`
## Functions

| ID  | Function                       | Tx     | Rx     | Emulator response              |
| --- | ------------------------------ | ------ | ------ | ------------------------------ |
| 00  | I am alive                     | Client | PORT   | ACK                            |
| 01  | I am alive ACK                 | Client | PORT   | ERR: Unknown telegram type     |
| 02  | Change/insert person           | Client | PORT   | ERR: Not implemented           |
| 03  | Change/insert person ACK       | PORT   | Client | ERR: Unknown telegram type     |
| 04  | Delete person                  | Client | PORT   | ERR: Not implemented           |
| 05  | Delete person ACK              | PORT   | Client | ERR: Unknown telegram type     |
| 06  | Change/insert person with NACK | Client | PORT   | ACK/NACK                       |
| 07  | Change/insert person NACK      | PORT   | Client | ERR: Unknown telegram type     |
| 08  | Delete person with NACK        | Client | PORT   | ACK/NACK                       |
| 09  | Delete person NACK             | PORT   | Client | ERR: Unknown telegram type     |
| 10  | Set zone access                | Client | PORT   | ==ERR: Unknown telegram type== |
| 11  | Set zone access ACK            | PORT   | Client | ERR: Unknown telegram type     |
| 12  | Set zone access NACK           | PORT   | Client | ERR: Unknown telegram type     |
| >12 | ~~None~~                       | Client | PORT   | ERR: Unknown telegram type     |
## Error codes

| Code | Description                                           | Tested |
| ---- | ----------------------------------------------------- | ------ |
| 01   | System occupied                                       | No     |
| 02   | Database timeout                                      | No     |
| 03   | ==Unknown command==                                   | Yes    |
| 04   | Main database not present                             | No     |
| 05   | Unexpected answer from database                       | No     |
| 06   | ==Card already assigned to another user==             | Yes    |
| 10   | Date in record invalid                                | No     |
| 11   | Wrong length for badge number                         | Yes    |
| 12   | Wrong format for badge number                         | Yes    |
| 13   | ==Exit date is before entry date==                    | Yes    |
| 14   | User not found                                        | Yes    |
| 15   | Invalid access specified (must be 'Always' or 'None') | No     |
| 99   | Custom emulator error for other failures              | No     |

# Found issues

## General behaviour-related issues

- [x] The emulator accepts telegrams with message ID not starting with zero.
- [x] Invoking function ID 10 (Set zone access) should log the *Not implemented* error in the terminal rather than *Unknown telegram type*.
- [x] Unimplemented (from chapter 9 of the specification): Maximum time without data activity by the third-party system results in idle timeout, i.e. PORT Technology closes the TCP connection and waits for a recovery from the third-party system.
- [x] Data import using text files has not been implemented.
## Function ID 06 (Change/insert person with NACK) related issues

- [x] The emulator doesn't send an ACK message after successful reception.
- [x] The user-updated info log message in the terminal contains an uninterpreted format specifier `%d`, i.e. `Updated user ... (users in DB: %d)`.
- [x] The following badge number property is not implemented in the emulator. The client can't even pass `ignore` as badge number 1 because it is implemented to accept hexadecimal numbers without exception.
> If badge number 1 contains the text "ignore" (without quotation marks), the badges are not imported at all. For example, the assigned badge remains assigned without any change.
- [ ] The functionality with putting "ignore" as badge number 1 works fine with updating existing users but when creating a new user, badge number 1 will just be set to "ignore" and badge numbers 2 and 3 are imported as well.
- [ ] The following entry/exit date property is not implemented:
>The person is only inserted if the current date/time is between the entry date and the exit date. If the exit date is before the current date, the person is deleted.


## NACK messages-related issues

- [x] The implementation of NACK messages probably follows the traditional purpose of registering only transmission-based errors.
	- Based on the fact that NACK messages in Schindler PORT systems are extended to include more granular information (code and message), I believe the emulator should also send NACK messages upon receiving erroneous messages of a semantic character, much like the errors logged by the application into the system terminal.
	- At the very least, I don't think the TCP connection should be terminated after receiving a message containing a semantic error.
	- The specification chapter 8 lists error codes to be used in NACK messages. This implies the emulator should probably send NACK messages for any failure outside of the specific old functions (ID 02 and 04) as well, e.g. error code 03 - Unknown command.
- [x] **Function ID 06 (Change/insert person with NACK)** doesn't handle the following cases:
	- *Error code 06 (Card already assigned to another user)*
	- *Error code 13 (Exit date is before entry date)*
- [ ] The newly implemented Error `13|Entry date must be before exit date` works well with correct date comparison but is also thrown every time an incorrect date is inserted, such as `2024-13-00 34:17:05`. I suspect a date validation occurs within the program but is incorrectly displayed as a date comparison failure. 
- [x] **Function ID 09 (Delete person NACK)** sends a NACK message with function ID 05 instead of 09.