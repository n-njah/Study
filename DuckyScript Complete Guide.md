```
# DuckyScript Complete Study Guide 


## Table of Contents 

1. Introduction to DuckyScript 

2. Basic Syntax & Commands 

3. Keyboard Keys & Modifiers 

4. Timing & Delays 

5. String Output & Special Characters 

6. Variables & Logic (DuckyScript 3.0) 

7. Functions & Loops 

8. Practical Examples 

9. Best Practices & Tips 

10. Troubleshooting 


--- 


## 1. Introduction to DuckyScript 


**What is DuckyScript?** 

DuckyScript is a simple scripting language developed by Hak5 for the USB Rubber Ducky and other keystroke injection tools. It allows you to automate keyboard inputs by writing scripts that execute as if a human were typing. 


**Use Cases:** 

- Penetration testing and security assessments 

- IT automation and system administration 

- Accessibility solutions 

- Educational demonstrations 


**Versions:** 

- **DuckyScript 1.0**: Original version with basic commands 

- **DuckyScript 3.0**: Enhanced version with variables, logic, functions, and more advanced features 


--- 


## 2. Basic Syntax & Commands 


### Comment Syntax 

``` 

REM This is a comment - ignored during execution 

// This is also a comment (DuckyScript 3.0) 

``` 


### Core Commands 


**DELAY** - Pause execution for specified milliseconds 

``` 

DELAY 1000    REM Wait 1 second (1000ms) 

DELAY 500     REM Wait 0.5 seconds 

``` 


**STRING** - Type out a string of text 

``` 

STRING Hello World! 

STRING username@example.com 

``` 


**ENTER** - Press the Enter/Return key 

``` 

ENTER 

``` 


**REPEAT** - Repeat the previous command 

``` 

STRING A 

REPEAT 10     REM Types "AAAAAAAAAAA" 

``` 


--- 


## 3. Keyboard Keys & Modifiers 


### Special Keys 

``` 

ENTER          REM Enter/Return key 

TAB            REM Tab key 

SPACE          REM Spacebar 

BACKSPACE      REM Backspace 

DELETE         REM Delete key 

ESC or ESCAPE  REM Escape key 

PAUSE          REM Pause/Break key 

PRINTSCREEN    REM Print Screen 

INSERT         REM Insert key 

HOME           REM Home key 

END            REM End key 

PAGEUP         REM Page Up 

PAGEDOWN       REM Page Down 

CAPSLOCK       REM Caps Lock toggle 

SCROLLLOCK     REM Scroll Lock 

NUMLOCK        REM Num Lock 

``` 


### Function Keys 

``` 

F1 through F24 

``` 


### Arrow Keys 

``` 

UP or UPARROW 

DOWN or DOWNARROW 

LEFT or LEFTARROW 

RIGHT or RIGHTARROW 

``` 


### Modifier Keys 

``` 

CTRL or CONTROL 

SHIFT 

ALT 

GUI or WINDOWS    REM Windows key (Command on Mac) 

``` 


### Key Combinations 

Combine modifiers with other keys using spaces or hyphens: 

``` 

CTRL c            REM Copy 

CTRL-c            REM Also works 

GUI r             REM Run dialog (Windows) 

CTRL-SHIFT-ESC    REM Task Manager 

ALT F4            REM Close window 

``` 


### Common Shortcuts 

``` 

CTRL-ALT-DELETE   REM Security screen (Windows) 

CTRL-SHIFT-T      REM Reopen closed tab 

GUI-l             REM Lock screen (Windows) 

COMMAND-SPACE     REM Spotlight (Mac) 

``` 


--- 


## 4. Timing & Delays 


### DEFAULT_DELAY and DEFAULTDELAY 

Set a default delay between commands: 

``` 

DEFAULTDELAY 100    REM 100ms delay after each command 

STRING test 

ENTER               REM 100ms delay happens here automatically 

``` 


### Strategic Delay Usage 

``` 

REM Wait for applications to load 

DELAY 2000 


REM Quick successive commands 

DEFAULTDELAY 50 

STRING fast typing sequence 

``` 


### Common Delay Scenarios 

``` 

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

``` 

STRING This is regular text 

``` 


### Special Characters in Strings 

DuckyScript handles most characters directly: 

``` 

STRING user@domain.com 

STRING C:\Windows\System32 

STRING $variable = 100; 

``` 


### Multi-line Text 

``` 

STRING Line 1 

ENTER 

STRING Line 2 

ENTER 

STRING Line 3 

``` 


### Escaping and Special Cases 

For special key combinations within strings, break them out: 

``` 

STRING Email: 

STRING user@example.com 

ENTER 

STRING Password: 

STRING P@ssw0rd! 

``` 


--- 


## 6. Variables & Logic (DuckyScript 3.0) 


### Variable Declaration 

``` 

VAR $name = "John" 

VAR $count = 0 

VAR $isActive = TRUE 

``` 


### Using Variables 

``` 

VAR $username = "admin" 

STRING $username 

``` 


### Conditional Statements 

``` 

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

``` 

==    Equal to 

!=    Not equal to 

>     Greater than 

<     Less than 

>=    Greater than or equal 

<=    Less than or equal 

``` 


### System Variables 

``` 

$_OS              REM Operating system 

$_CAPSLOCK_ON     REM Caps Lock state 

$_SCROLLLOCK_ON   REM Scroll Lock state 

$_NUMLOCK_ON      REM Num Lock state 

``` 


--- 


## 7. Functions & Loops 


### WHILE Loops 

``` 

VAR $counter = 0 


WHILE ($counter < 5) 

    STRING Iteration 

    STRING $counter 

    ENTER 

    $counter = ($counter + 1) 

END_WHILE 

``` 


### Functions (DuckyScript 3.0) 

``` 

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

``` 

STRING * 

REPEAT 50    REM Types 50 asterisks 

``` 


--- 


## 8. Practical Examples 


### Example 1: Open Notepad and Type Message (Windows) 

``` 

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

``` 

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

``` 

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

``` 

DELAY 1000 

CTRL-ALT-DOWN 

DELAY 3000 

CTRL-ALT-UP 

``` 


### Example 5: Disable Network Adapter (Windows - Admin) 

``` 

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

``` 

DELAY 1000 

GUI r 

DELAY 500 

STRING https://www.youtube.com/watch?v=dQw4w9WgXcQ 

ENTER 

DELAY 3000 

F11    REM Fullscreen 

``` 


### Example 7: OS Detection and Action 

``` 

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

``` 

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

``` 

REM For slower systems 

DELAY 2000 


REM For faster systems 

DELAY 500 

``` 


### Error Prevention 

``` 

REM Clear any existing windows first 

ESC 

ESC 

DELAY 100 


REM Now execute main payload 

GUI r 

``` 


### Modular Design 

``` 

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

- Increase DELAY values 

- Add DEFAULTDELAY at the beginning 


**Wrong characters appear** 

- Check keyboard layout (US vs UK, etc.) 

- Some special characters may vary by layout 


**Commands don't execute** 

- Increase delays between commands 

- Verify correct syntax 

- Check for typos in commands 


**Window doesn't open in time** 

- Increase DELAY after GUI r or application launch commands 

- Consider system performance 


**Special characters not working** 

- Use STRING for most characters 

- Break out special key combinations 


### Debugging Tips 

``` 

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

``` 

REM                   Comment 

DELAY 1000           Wait 1 second 

DEFAULTDELAY 100     Set default delay 

STRING text          Type text 

ENTER                Press Enter 

GUI r                Open Run (Windows) 

CTRL c               Copy 

CTRL v               Paste 

ALT F4               Close window 

REPEAT 10            Repeat last command 

``` 


### Typical Script Structure 

``` 

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


Remember: Always use DuckyScript responsibly and only on systems you own or have explicit permission to test!
```
