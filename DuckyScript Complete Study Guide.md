# DuckyScript Complete Study Guide

> A study reference for DuckyScript — the scripting language used by the USB Rubber Ducky and other keystroke-injection devices.

---

## Table of Contents

1. [Introduction to DuckyScript](#1-introduction-to-duckyscript)
2. [Basic Syntax & Commands](#2-basic-syntax--commands)
3. [Keyboard Keys & Modifiers](#3-keyboard-keys--modifiers)
4. [Timing & Delays](#4-timing--delays)
5. [String Output & Special Characters](#5-string-output--special-characters)
6. [Variables & Logic (DuckyScript 3.0)](#6-variables--logic-duckyscript-30)
7. [Functions & Loops](#7-functions--loops)
8. [Practical Examples](#8-practical-examples)
9. [Best Practices & Tips](#9-best-practices--tips)
10. [Troubleshooting](#10-troubleshooting)
11. [Quick Reference Card](#quick-reference-card)
12. [Resources for Further Learning](#resources-for-further-learning)

---

## 1. Introduction to DuckyScript

### What is DuckyScript?

DuckyScript is a simple scripting language developed by Hak5 for the USB Rubber Ducky and other keystroke-injection tools. It allows you to automate keyboard inputs by writing scripts that execute as if a human were typing.

### Use Cases

- Penetration testing and security assessments
- IT automation and system administration
- Accessibility solutions
- Educational demonstrations

### Versions

| Version | Characteristics |
| --- | --- |
| **DuckyScript 1.0** | Original version with basic commands |
| **DuckyScript 3.0** | Enhanced version with variables, logic, functions, and more advanced features |

---

## 2. Basic Syntax & Commands

### Comment Syntax

```duckyscript
REM This is a comment - ignored during execution
// This is also a comment (DuckyScript 3.0)
```

### Core Commands

| Command | Description |
| --- | --- |
| `DELAY <ms>` | Pause execution for the specified number of milliseconds |
| `STRING <text>` | Type out a string of text |
| `ENTER` | Press the Enter/Return key |
| `REPEAT <n>` | Repeat the previous command |

**DELAY** — pause execution for the specified milliseconds:

```duckyscript
DELAY 1000    REM Wait 1 second (1000ms)
DELAY 500     REM Wait 0.5 seconds
```

**STRING** — type out a string of text:

```duckyscript
STRING Hello World!
STRING username@example.com
```

**ENTER** — press the Enter/Return key:

```duckyscript
ENTER
```

**REPEAT** — repeat the previous command:

```duckyscript
STRING A
REPEAT 10     REM Types "AAAAAAAAAAA"
```

---

## 3. Keyboard Keys & Modifiers

### Special Keys

| Key | Notes |
| --- | --- |
| `ENTER` | Enter/Return key |
| `TAB` | Tab key |
| `SPACE` | Spacebar |
| `BACKSPACE` | Backspace |
| `DELETE` | Delete key |
| `ESC` or `ESCAPE` | Escape key |
| `PAUSE` | Pause/Break key |
| `PRINTSCREEN` | Print Screen |
| `INSERT` | Insert key |
| `HOME` | Home key |
| `END` | End key |
| `PAGEUP` | Page Up |
| `PAGEDOWN` | Page Down |
| `CAPSLOCK` | Caps Lock toggle |
| `SCROLLLOCK` | Scroll Lock |
| `NUMLOCK` | Num Lock |

### Function Keys

```duckyscript
F1 through F24
```

### Arrow Keys

| Key | Alias |
| --- | --- |
| `UP` | `UPARROW` |
| `DOWN` | `DOWNARROW` |
| `LEFT` | `LEFTARROW` |
| `RIGHT` | `RIGHTARROW` |

### Modifier Keys

| Modifier | Notes |
| --- | --- |
| `CTRL` | Alias: `CONTROL` |
| `SHIFT` | — |
| `ALT` | — |
| `GUI` | Alias: `WINDOWS` — Windows key (Command on Mac) |

### Key Combinations

Combine modifiers with other keys using spaces or hyphens:

```duckyscript
CTRL c            REM Copy
CTRL-c            REM Also works
GUI r             REM Run dialog (Windows)
CTRL-SHIFT-ESC    REM Task Manager
ALT F4            REM Close window
```

### Common Shortcuts

| Combination | Action |
| --- | --- |
| `CTRL-ALT-DELETE` | Security screen (Windows) |
| `CTRL-SHIFT-T` | Reopen closed tab |
| `GUI-l` | Lock screen (Windows) |
| `COMMAND-SPACE` | Spotlight (Mac) |

---

## 4. Timing & Delays

### DEFAULT_DELAY and DEFAULTDELAY

Set a default delay between commands:

```duckyscript
DEFAULTDELAY 100    REM 100ms delay after each command
STRING test
ENTER               REM 100ms delay happens here automatically
```

### Strategic Delay Usage

```duckyscript
REM Wait for applications to load
DELAY 2000

REM Quick successive commands
DEFAULTDELAY 50
STRING fast typing sequence
```

### Common Delay Scenarios

```duckyscript
GUI r                   REM Open Run dialog
DELAY 500              REM Wait for dialog to open
STRING notepad
ENTER
DELAY 1000             REM Wait for Notepad to launch
STRING Hello!
```

---

## 5. String Output & Special Characters

### Basic STRING Usage

```duckyscript
STRING This is regular text
```

### Special Characters in Strings

DuckyScript handles most characters directly:

```duckyscript
STRING user@domain.com
STRING C:\Windows\System32
STRING $variable = 100;
```

### Multi-line Text

```duckyscript
STRING Line 1
ENTER
STRING Line 2
ENTER
STRING Line 3
```

### Escaping and Special Cases

For special key combinations within strings, break them out:

```duckyscript
STRING Email:
STRING user@example.com
ENTER
STRING Password:
STRING P@ssw0rd!
```

---

## 6. Variables & Logic (DuckyScript 3.0)

### Variable Declaration

```duckyscript
VAR $name = "John"
VAR $count = 0
VAR $isActive = TRUE
```

### Using Variables

```duckyscript
VAR $username = "admin"
STRING $username
```

### Conditional Statements

```duckyscript
VAR $os = "Windows"

IF ($os == "Windows") THEN
    GUI r
    DELAY 500
    STRING cmd
    ENTER
ELSE IF ($os == "Linux") THEN
    CTRL-ALT-t
ELSE
    REM Do something else
END_IF
```

### Comparison Operators

| Operator | Meaning |
| --- | --- |
| `==` | Equal to |
| `!=` | Not equal to |
| `>` | Greater than |
| `<` | Less than |
| `>=` | Greater than or equal |
| `<=` | Less than or equal |

### System Variables

| Variable | Meaning |
| --- | --- |
| `$_OS` | Operating system |
| `$_CAPSLOCK_ON` | Caps Lock state |
| `$_SCROLLLOCK_ON` | Scroll Lock state |
| `$_NUMLOCK_ON` | Num Lock state |

---

## 7. Functions & Loops

### WHILE Loops

```duckyscript
VAR $counter = 0

WHILE ($counter < 5)
    STRING Iteration
    STRING $counter
    ENTER
    $counter = ($counter + 1)
END_WHILE
```

### Functions (DuckyScript 3.0)

```duckyscript
FUNCTION openNotepad()
    GUI r
    DELAY 500
    STRING notepad
    ENTER
    DELAY 1000
END_FUNCTION

REM Call the function
openNotepad()
```

### REPEAT Command

```duckyscript
STRING *
REPEAT 50    REM Types 50 asterisks
```

---

## 8. Practical Examples

### Example 1: Open Notepad and Type Message (Windows)

```duckyscript
REM Open Run dialog
GUI r
DELAY 500

REM Launch Notepad
STRING notepad
ENTER
DELAY 1000

REM Type message
STRING This is an automated message!
ENTER
STRING Created with DuckyScript.
```

### Example 2: Open Terminal and Run Commands (Linux)

```duckyscript
REM Open terminal
CTRL-ALT-t
DELAY 1000

REM Run update commands
STRING sudo apt update
ENTER
DELAY 500

REM Wait for password prompt
DELAY 2000
STRING password123
ENTER
```

### Example 3: Create File with Content (Windows PowerShell)

```duckyscript
REM Open PowerShell
GUI r
DELAY 500
STRING powershell
ENTER
DELAY 2000

REM Create file with content
STRING echo "Hello from DuckyScript" > C:\temp\test.txt
ENTER
DELAY 500

REM Close PowerShell
STRING exit
ENTER
```

### Example 4: Prank - Flip Screen (Windows)

```duckyscript
DELAY 1000
CTRL-ALT-DOWN
DELAY 3000
CTRL-ALT-UP
```

### Example 5: Disable Network Adapter (Windows - Admin)

```duckyscript
GUI r
DELAY 500
STRING cmd
CTRL-SHIFT-ENTER    REM Run as admin
DELAY 2000
ALT y               REM Accept UAC
DELAY 1000
STRING netsh interface set interface "Wi-Fi" disabled
ENTER
```

### Example 6: Rickroll Prank

```duckyscript
DELAY 1000
GUI r
DELAY 500
STRING https://www.youtube.com/watch?v=dQw4w9WgXcQ
ENTER
DELAY 3000
F11    REM Fullscreen
```

### Example 7: OS Detection and Action

```duckyscript
REM DuckyScript 3.0 feature
IF ($_OS == "Windows") THEN
    GUI r
    DELAY 500
    STRING notepad
    ENTER
ELSE IF ($_OS == "MacOS") THEN
    COMMAND SPACE
    DELAY 500
    STRING textedit
    ENTER
ELSE
    CTRL-ALT-t
END_IF
```

---

## 9. Best Practices & Tips

### Always Test First

Create a safe testing environment before deploying scripts. Test on your own systems with proper authorization.

### Use Appropriate Delays

- **Too short**: Commands may execute before the system is ready
- **Too long**: Script runs slower than necessary
- Start with longer delays and optimize

### Add Comments

```duckyscript
REM === SECTION: Open Application ===
GUI r
DELAY 500
STRING notepad
ENTER

REM === SECTION: Write Content ===
DELAY 1000
STRING Hello World
```

### Handle Different System Speeds

```duckyscript
REM For slower systems
DELAY 2000

REM For faster systems
DELAY 500
```

### Error Prevention

```duckyscript
REM Clear any existing windows first
ESC
ESC
DELAY 100

REM Now execute main payload
GUI r
```

### Modular Design

```duckyscript
FUNCTION cleanup()
    ALT F4
    DELAY 300
    ENTER
END_FUNCTION

FUNCTION openCmd()
    GUI r
    DELAY 500
    STRING cmd
    ENTER
    DELAY 1000
END_FUNCTION
```

---

## 10. Troubleshooting

### Common Issues and Solutions

**Script runs too fast**

- Increase `DELAY` values
- Add `DEFAULTDELAY` at the beginning

**Wrong characters appear**

- Check keyboard layout (US vs UK, etc.)
- Some special characters may vary by layout

**Commands don't execute**

- Increase delays between commands
- Verify correct syntax
- Check for typos in commands

**Window doesn't open in time**

- Increase `DELAY` after `GUI r` or application launch commands
- Consider system performance

**Special characters not working**

- Use `STRING` for most characters
- Break out special key combinations

### Debugging Tips

```duckyscript
REM Test section by section
REM Comment out later parts first

REM Section 1
GUI r
DELAY 500
STRING notepad
ENTER

REM REM Section 2 - commented out for testing
REM DELAY 1000
REM STRING Test message
```

### Testing Strategy

1. Start with a simple "Hello World" script
2. Add one section at a time
3. Test after each addition
4. Adjust delays as needed
5. Document what works

---

## Quick Reference Card

### Most Common Commands

| Command | Effect |
| --- | --- |
| `REM` | Comment |
| `DELAY 1000` | Wait 1 second |
| `DEFAULTDELAY 100` | Set default delay |
| `STRING text` | Type text |
| `ENTER` | Press Enter |
| `GUI r` | Open Run (Windows) |
| `CTRL c` | Copy |
| `CTRL v` | Paste |
| `ALT F4` | Close window |
| `REPEAT 10` | Repeat last command |

### Typical Script Structure

```duckyscript
REM === Initialize ===
DEFAULTDELAY 100

REM === Open application ===
GUI r
DELAY 500
STRING notepad
ENTER
DELAY 1000

REM === Execute payload ===
STRING Your content here
ENTER

REM === Cleanup ===
ALT F4
```

---

## Resources for Further Learning

- Hak5 Official Documentation
- Hak5 Forums and Community
- USB Rubber Ducky GitHub Repository
- DuckyScript 3.0 Documentation

> **Remember:** Always use DuckyScript responsibly and only on systems you own or have explicit permission to test!
