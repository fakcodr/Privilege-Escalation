# Windows Process Token Research

A C++ security research project exploring Windows process enumeration, access tokens, token duplication, and process creation using the Windows API.

## Overview

This project demonstrates how Windows processes can be enumerated using the Tool Help Library and how access-token-related APIs interact during process creation.

The implementation searches for `winlogon.exe`, attempts to open its process token, duplicates the token, and attempts to launch `cmd.exe` using `CreateProcessWithTokenW`.

The project is intended for **Windows internals learning, defensive security research, and understanding Windows access-control mechanisms**.

## Features

- **Process Enumeration** — Find running processes by executable name.
- **Process Identification** — Retrieve a process ID using the Windows Tool Help API.
- **Token Access Research** — Explore process-token access and Windows security permissions.
- **Token Duplication** — Demonstrate the use of `DuplicateTokenEx`.
- **Process Creation Analysis** — Examine the behavior of `CreateProcessWithTokenW`.
- **Error Handling** — Report failures encountered during API calls.

## Technologies Used

| Technology | Purpose |
|---|---|
| C++ | Core implementation |
| Windows API | Process and security operations |
| Tool Help Library | Process enumeration |
| `OpenProcessToken` | Accessing process tokens |
| `DuplicateTokenEx` | Token duplication |
| `CreateProcessWithTokenW` | Process creation using a token |

## How It Works

The program follows this general sequence:

1. Creates a process snapshot using `CreateToolhelp32Snapshot`.
2. Searches for `winlogon.exe` and retrieves its process ID.
3. Attempts to open the target process.
4. Requests access to its security token.
5. Attempts to duplicate the token.
6. Attempts to create a command prompt process using the duplicated token.
7. Reports whether the operations succeeded or failed.

**Important:** Finding a process ID does not grant permission to access that process or its token. Windows access controls, token permissions, process protection mechanisms, and the caller's security context can prevent these operations.

## Requirements

- Windows operating system
- C++ compiler with Windows SDK support
- Visual Studio or another compatible Windows C++ development environment
- Appropriate permissions for the API operations being investigated

## Build

1. Open Visual Studio.
2. Create a C++ Console Application.
3. Add the source code to the project.
4. Select an appropriate Windows SDK and C++ language standard.
5. Build the project.
6. Review the compiler output and Windows API error codes during testing.

## Security Notes

This project examines techniques associated with Windows access-token manipulation and potential privilege-escalation behavior.

The supplied implementation does **not** guarantee successful elevation. In particular:

- `winlogon.exe` is a highly privileged Windows process, but its PID is not a source of automatic access.
- Opening a process with `PROCESS_QUERY_LIMITED_INFORMATION` does not necessarily provide the rights needed to open and duplicate its token.
- Token duplication does not independently grant additional privileges.
- `CreateProcessWithTokenW` has specific token, privilege, and access requirements.
- A process created with a duplicated token does not automatically have unrestricted administrative or SYSTEM-level access.

Use Windows security documentation and an isolated, authorized test environment to study these mechanisms. Do not attempt to bypass access controls on systems you do not own or administer.

## Learning Objectives

This project is useful for studying:

- Windows process and thread architecture
- Access tokens and security identifiers
- Process access rights
- Token privileges and impersonation levels
- Windows API error handling
- Defensive detection of suspicious process-token activity

## Future Improvements

- Add detailed error reporting using `GetLastError()`.
- Improve resource management with RAII and smart-handle wrappers.
- Add checks for token type, integrity level, and enabled privileges.
- Document expected access-denied scenarios.
- Add defensive logging for process and token operations.
- Include safe tests using processes owned by the current user.

## Disclaimer

This repository is provided for educational purposes and authorized security research. Windows security behavior varies by operating-system version, process protection, privileges, and execution context.

The author does not encourage unauthorized access, privilege escalation, or modification of systems without permission.

## License

Choose and add an appropriate open-source license before distributing this project. Until a license is specified, the repository's default copyright restrictions apply.
