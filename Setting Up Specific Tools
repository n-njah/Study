## Setting Up Specific Tools - Complete Guides
A. Setting Up CAPE Sandbox (Comprehensive Automated Analysis)
CAPE is one of the most powerful open-source malware sandboxes with automatic unpacking and configuration extraction.
System Requirements:
- Ubuntu 20.04/22.04 LTS (fresh install recommended)
- Minimum: 8GB RAM, 4 CPU cores, 100GB disk
- Recommended: 16GB RAM, 8 cores, 250GB SSD
- Nested virtualization enabled (if running in VM)
Installation Steps:
## Prepare Host System:
bash# Update system
sudo apt update && sudo apt upgrade -y

# Install dependencies
sudo apt install -y python3 python3-pip python3-dev \
  libffi-dev libssl-dev libjpeg-dev zlib1g-dev \
  yara mongodb postgresql postgresql-contrib \
  libpq-dev virtualbox-6.1

# Install additional tools
sudo apt install -y tcpdump apparmor-utils

# Disable AppArmor for tcpdump
sudo aa-disable /usr/sbin/tcpdump
2. Create CAPE User:
bash# Create dedicated user
sudo adduser cape

# Add to required groups
sudo usermod -a -G vboxusers cape
sudo usermod -a -G kvm cape

# Switch to cape user
sudo su - cape
3. Install CAPE:
bash# Clone repository
git clone https://github.com/kevoreilly/CAPEv2.git
cd CAPEv2

# Run installer (automated)
sudo ./installer/cape2.sh

# Or manual installation:
# Create virtual environment
python3 -m venv venv
source venv/bin/activate

# Install Python dependencies
pip3 install -U pip setuptools
pip3 install -r requirements.txt
4. Configure PostgreSQL:
bash# Create database
sudo -u postgres psql

# In PostgreSQL prompt:
CREATE DATABASE cape;
CREATE USER cape WITH PASSWORD 'your_secure_password';
GRANT ALL PRIVILEGES ON DATABASE cape TO cape;
\q

# Update configuration
cd /opt/CAPEv2/
cp conf/cuckoo.conf.default conf/cuckoo.conf

# Edit database settings
nano conf/cuckoo.conf
# Update [database] section:
# connection = postgresql://cape:your_secure_password@localhost/cape
5. Set Up Guest VMs:
Create Windows 10 Analysis VM:
bash# Download Windows 10 ISO (evaluation)
# Create VM in VirtualBox

# VM Settings:
- Name: win10_analysis
- RAM: 4GB
- Disk: 60GB dynamic
- Network: Host-only adapter (vboxnet0)
- Disable audio, USB
- Remove floppy drive
Inside Windows VM:
powershell# Disable Windows Defender (in VM only!)
Set-MpPreference -DisableRealtimeMonitoring $true

# Disable UAC
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System" /v EnableLUA /t REG_DWORD /d 0 /f

# Disable Windows Update
sc config wuauserv start= disabled

# Install Python 3.8
# Download from python.org

# Install Pillow (for screenshots)
pip install Pillow

# Download CAPE agent
# From CAPE host: cd /opt/CAPEv2/agent
# Copy agent.py to VM

# Set agent to run at startup
# Task Scheduler: Run agent.py at login
Configure agent startup:
batchREM Create startup script: C:\agent\start_agent.bat
@echo off
python C:\agent\agent.py
Add to Task Scheduler:

Trigger: At logon
Action: Start program - C:\agent\start_agent.bat
Run with highest privileges

Take VM Snapshot:
bash# Shutdown Windows VM cleanly
# In VirtualBox, take snapshot named "clean"

vboxmanage snapshot win10_analysis take clean --description "Clean analysis state"
6. Register VM with CAPE:
bash# Edit machinery configuration
cd /opt/CAPEv2
nano conf/virtualbox.conf

# Add VM:
[win10_analysis]
label = win10_analysis
platform = windows
ip = 192.168.56.101  # Must match VM IP
snapshot = clean
7. Configure Network:
bash# Setup host-only network
VBoxManage hostonlyif create
VBoxManage hostonlyif ipconfig vboxnet0 --ip 192.168.56.1

# Enable IP forwarding (optional, for internet access)
sudo sysctl -w net.ipv4.ip_forward=1

# Configure iptables for routing
sudo iptables -t nat -A POSTROUTING -o eth0 -s 192.168.56.0/24 -j MASQUERADE
sudo iptables -A FORWARD -i vboxnet0 -o eth0 -j ACCEPT
sudo iptables -A FORWARD -i eth0 -o vboxnet0 -m state --state RELATED,ESTABLISHED -j ACCEPT
8. Start CAPE Services:
bash# Start as cape user
cd /opt/CAPEv2
source venv/bin/activate

# Start rooter (as root)
sudo python3 utils/rooter.py

# In separate terminals:
# Start main daemon
python3 cuckoo.py

# Start web interface
python3 utils/web.py

# Start process
python3 utils/process.py
9. Submit Sample:
bash# Command line submission
python3 utils/submit.py /path/to/malware.exe

# Or via web interface
# Navigate to http://localhost:8000
# Upload sample through web UI
```

**10. Access Results:**
```
Web Interface: http://localhost:8000
- View analysis reports
- Download artifacts
- Export IOCs
- Search database
```

### **CAPE Analysis Features:**

**Automatic Capabilities:**
- Unpacks 100+ packer families
- Extracts malware configurations
- Identifies malware families
- Captures network traffic
- Takes screenshots
- Records API calls
- Dumps process memory
- YARA rule matching

**Advanced Configuration Extraction:**

CAPE recognizes and extracts configs from:
- Emotet
- TrickBot
- Qakbot
- Dridex
- Cobalt Strike
- Raccoon Stealer
- RedLine
- And 100+ more families

**Viewing Results:**
```
Analysis Report Sections:
1. Summary - Quick overview
2. Static Analysis - File info, strings, PE data
3. Signatures - Behavioral signatures triggered
4. Behavioral Analysis - Process tree, API calls
5. Network Analysis - Traffic, DNS, HTTP
6. Dropped Files - Files created/modified
7. Memory Analysis - Process dumps
8. CAPE - Extracted payloads and configs
Customizing CAPE:
bash# Add custom YARA rules
cd /opt/CAPEv2/data/yara
nano custom_rules.yar

# Add custom signatures
cd /opt/CAPEv2/modules/signatures
cp template.py my_signature.py
# Edit my_signature.py

# Add processing modules
cd /opt/CAPEv2/modules/processing
# Create custom processor
B. Setting Up IDA Pro / Ghidra (Disassembly & Reverse Engineering)
Ghidra Setup (Free, NSA Tool):
1. Prerequisites:
bash# Install Java JDK 11+
sudo apt install openjdk-11-jdk

# Verify installation
java -version
2. Download and Install:
bash# Download from ghidra-sre.org
wget https://github.com/NationalSecurityAgency/ghidra/releases/download/Ghidra_10.4_build/ghidra_10.4_PUBLIC_20230928.zip

# Extract
unzip ghidra_10.4_PUBLIC_20230928.zip
cd ghidra_10.4_PUBLIC

# Launch
./ghidraRun
```

**3. Basic Analysis Workflow:**
```
1. Create New Project
   - File → New Project → Non-Shared Project
   - Choose location and name

2. Import Binary
   - File → Import File
   - Select malware.exe
   - Accept default format detection

3. Analyze
   - Analysis → Auto Analyze
   - Select all analyzers
   - Wait for completion (can take minutes to hours)

4. Navigate Code
   - Window → Program Trees (file structure)
   - Window → Functions (all functions)
   - Window → Defined Strings (string table)
   - Window → Symbol Tree (imports/exports)
```

**4. Key Features:**

**Decompiler:**
```
- Right-click function → Decompile Function
- Shows pseudo-C code
- Much easier to read than assembly
- Can rename variables/functions
```

**Cross-References:**
```
- Right-click instruction → References → Show References To
- Find all places that call a function
- Trace data flow
Scripting:
python# Ghidra Python scripting example
# Window → Script Manager

from ghidra.program.model.symbol import SourceType

# Get current program
program = getCurrentProgram()

# Iterate all functions
fm = program.getFunctionManager()
for func in fm.getFunctions(True):
    print("Function: {} at {}".format(
        func.getName(),
        func.getEntryPoint()
    ))
    
# Find all calls to CreateFileA
for ref in currentProgram.getReferenceManager().getReferencesTo(toAddr("0x401000")):
    print("Called from: {}".format(ref.getFromAddress()))
5. Essential Plugins:
bash# Download from github.com/ghidra
# Extensions to install:

# 1. Ghidrathon (Python 3 scripting)
cd ghidra_scripts
git clone https://github.com/mandiant/Ghidrathon

# 2. Ghidra Emotet Config Extractor
git clone https://github.com/OALabs/ghidra-emotet-config-extractor

# 3. FindCrypt (find crypto constants)
git clone https://github.com/d3v1l401/FindCrypt-Ghidra

# Install via File → Install Extensions
```

**6. Advanced Usage:**

**Version Tracking:**
```
# Compare two versions of malware
- Tools → Version Tracking
- Add Session
- Select two programs
- Run automatic matching
- View differences
```

**Debugging Integration:**
```
# Connect to debugger (GDB)
- Debugger → Configure and Launch
- Select GDB
- Set target
- Synchronize with Ghidra view
IDA Pro Setup (Commercial, Industry Standard):
For students/hobbyists: IDA Free is available
1. Installation:
bash# Download from hex-rays.com/ida-free
# Linux installation:
chmod +x idafree*.run
./idafree*.run

# Follow installer
# Default location: /opt/idafree-8.3
```

**2. First Analysis:**
```
1. Launch IDA
2. File → New → Choose binary
3. IDA detects format automatically
4. Let IDA perform auto-analysis
5. View → Open Subviews:
   - Functions window
   - Strings window
   - Imports/Exports
   - Hex View
```

**3. Essential Shortcuts:**
```
Space - Toggle graph/text view
X - Cross-references to
G - Jump to address
N - Rename
; - Add comment
: - Add repeatable comment
Alt+T - Text search
Alt+B - Binary search
Ctrl+S - Segments
4. IDAPython Scripting:
python# Open File → Script File

import idaapi
import idc
import idautils

# Iterate all functions
for func_ea in idautils.Functions():
    func_name = idc.get_func_name(func_ea)
    print(f"Function: {func_name} at {hex(func_ea)}")

# Find all string references
strings = idautils.Strings()
for s in strings:
    print(f"{hex(s.ea)}: {str(s)}")

# Find all calls to specific function
target = idc.get_name_ea_simple("CreateFileA")
for xref in idautils.XrefsTo(target):
    print(f"Called from: {hex(xref.frm)}")

# Decode obfuscated string
def decode_string(ea):
    result = ""
    for i in range(100):  # max length
        byte = idc.get_wide_byte(ea + i)
        if byte == 0:
            break
        # XOR with 0x42 (example)
        result += chr(byte ^ 0x42)
    return result

# Apply to address
decoded = decode_string(0x401000)
print(decoded)
5. Essential Plugins:
python# Hex-Rays Decompiler (commercial add-on)
# Provides C-like pseudocode

# Free plugins to install:

# 1. FLARE IDA plugins
# Download from github.com/mandiant/flare-ida
# Includes:
- shellcode_hashes (resolve API hashes)
- StackStrings (decode stack strings)
- MSDN Annotations

# 2. Diaphora (binary diffing)
# Compare malware versions

# 3. findcrypt (crypto constants)
# Identifies crypto algorithms

# Install plugins:
# Copy .py files to: IDA/plugins/
C. Setting Up Volatility 3 (Memory Forensics)
Installation:
1. Install Dependencies:
bash# Python 3.6+ required
sudo apt install python3 python3-pip python3-dev

# Install volatility3
pip3 install volatility3

# Or from source:
git clone https://github.com/volatilityfoundation/volatility3.git
cd volatility3
pip3 install -r requirements.txt
python3 setup.py install
2. Download Symbol Tables:
bash# Download Windows symbol tables
# From: https://downloads.volatilityfoundation.org/volatility3/symbols/

# Create symbols directory
mkdir -p ~/.volatility3/symbols

# Extract downloaded symbol packs
unzip windows.zip -d ~/.volatility3/symbols/
unzip linux.zip -d ~/.volatility3/symbols/
unzip mac.zip -d ~/.volatility3/symbols/
3. Basic Usage:
bash# List available plugins
vol -h

# Identify OS (not always needed in vol3)
vol -f memory.raw windows.info

# List processes
vol -f memory.raw windows.pslist

# Scan for processes (includes hidden)
vol -f memory.raw windows.psscan

# Network connections
vol -f memory.raw windows.netscan

# Process tree
vol -f memory.raw windows.pstree

# Command lines
vol -f memory.raw windows.cmdline

# DLL list for process
vol -f memory.raw windows.dlllist --pid 1234

# Dump process
vol -f memory.raw windows.pslist --pid 1234 --dump

# Find code injection
vol -f memory.raw windows.malfind

# Registry hives
vol -f memory.raw windows.registry.hivelist

# Print registry key
vol -f memory.raw windows.registry.printkey --key "Software\Microsoft\Windows\CurrentVersion\Run"

# File scan
vol -f memory.raw windows.filescan

# Dump file
vol -f memory.raw windows.dumpfiles --virtaddr 0x12345678
4. Advanced Analysis Script:
python#!/usr/bin/env python3
"""
Automated Volatility Analysis Script
"""

import subprocess
import json
import os

def run_vol_command(plugin, memory_dump, **kwargs):
    """Run volatility command and return output"""
    cmd = ['vol', '-f', memory_dump, plugin]
    
    # Add additional arguments
    for key, value in kwargs.items():
        cmd.extend([f'--{key}', str(value)])
    
    result = subprocess.run(cmd, capture_output=True, text=True)
    return result.stdout

def analyze_memory(memory_dump):
    """Comprehensive memory analysis"""
    
    print(f"[+] Analyzing {memory_dump}")
    results = {}
    
    # Get system info
    print("[*] Getting system info...")
    results['info'] = run_vol_command('windows.info', memory_dump)
    
    # List processes
    print("[*] Listing processes...")
    results['pslist'] = run_vol_command('windows.pslist', memory_dump)
    
    # Find hidden processes
    print("[*] Scanning for hidden processes...")
    results['psscan'] = run_vol_command('windows.psscan', memory_dump)
    
    # Network connections
    print("[*] Checking network connections...")
    results['netscan'] = run_vol_command('windows.netscan', memory_dump)
    
    # Look for injected code
    print("[*] Searching for code injection...")
    results['malfind'] = run_vol_command('windows.malfind', memory_dump)
    
    # Check persistence mechanisms
    print("[*] Checking Run keys...")
    run_keys = [
        "Software\\Microsoft\\Windows\\CurrentVersion\\Run",
        "Software\\Microsoft\\Windows\\CurrentVersion\\RunOnce",
    ]
    
    for key in run_keys:
        results[f'registry_{key}'] = run_vol_command(
            'windows.registry.printkey',
            memory_dump,
            key=key
        )
    
    # Save results
    output_dir = f"{memory_dump}_analysis"
    os.makedirs(output_dir, exist_ok=True)
    
    for analysis_type, output in results.items():
        filename = os.path.join(output_dir, f"{analysis_type}.txt")
        with open(filename, 'w') as f:
            f.write(output)
        print(f"[+] Saved {filename}")
    
    # Generate report
    generate_report(results, output_dir)

def generate_report(results, output_dir):
    """Generate summary report"""
    report_file = os.path.join(output_dir, "summary_report.txt")
    
    with open(report_file, 'w') as f:
        f.write("="*60 + "\n")
        f.write("MEMORY FORENSICS ANALYSIS REPORT\n")
        f.write("="*60 + "\n\n")
        
        # Parse process list
        f.write("RUNNING PROCESSES:\n")
        f.write("-"*60 + "\n")
        # Parse and write key processes
        
        f.write("\n\nNETWORK CONNECTIONS:\n")
        f.write("-"*60 + "\n")
        # Parse and write connections
        
        f.write("\n\nSUSPICIOUS FINDINGS:\n")
        f.write("-"*60 + "\n")
        # Highlight malfind results
        
    print(f"[+] Report saved to {report_file}")

if __name__ == "__main__":
    import sys
    
    if len(sys.argv) < 2:
        print("Usage: python3 auto_volatility.py <memory_dump>")
        sys.exit(1)
    
    analyze_memory(sys.argv[1])
```

## D. Setting Up x64dbg (Advanced Debugging)

**1. Installation (Windows):**
```
Download from: https://x64dbg.com
- Get snapshot build (most recent)
- Extract to C:\x64dbg
- No installation needed
```

**2. Essential Plugins:**
```
Install to: x64dbg\x32\plugins\ and x64dbg\x64\plugins\

Must-Have Plugins:

1. ScyllaHide (anti-anti-debug)
   - Download from github.com/x64dbg/ScyllaHide
   - Hides debugger from detection
   
2. xAnalyzer (automatic analysis)
   - Download from github.com/ThunderCls/xAnalyzer
   - Calls analysis, API detection
   
3. x64dbgpy (Python scripting)
   - Download from github.com/x64dbg/x64dbgpy
   - Automate debugging tasks
   
4. APIInfo (API documentation)
   - Shows MSDN info for APIs
   
5. ControlFlowGuard
   - Visualize program flow
```

**3. Basic Debugging Workflow:**
```
1. Load executable
   - File → Open → malware.exe
   
2. Configure plugins
   - Plugins → ScyllaHide → Options
   - Enable all anti-debug options
   
3. Set entry point breakpoint
   - Debug → Run (or F9)
   - Automatically breaks at entry
   
4. Common operations:
   F2 - Set/remove breakpoint
   F7 - Step into
   F8 - Step over
   F9 - Run
   Ctrl+F2 - Restart
   F12 - Pause
   
5. Analyze:
   - View → Registers
   - View → Stack
   - View → Memory Map
   - Symbols → Import symbols
```

**4. Advanced Breakpoint Techniques:**
```
Hardware Breakpoints:
- Right-click instruction
- Breakpoint → Hardware → Execute/Read/Write
- Useful when software BP detected

Conditional Breakpoints:
- Set breakpoint (F2)
- Right-click → Edit
- Add condition: "EAX==0"
- Or command: "log {eax}"

Memory Breakpoints:
- Memory Map tab
- Right-click memory region
- Set memory breakpoint on access

API Breakpoints:
- Symbols tab
- Find API (e.g., CreateFileA)
- Right-click → Toggle breakpoint
```

**5. Unpacking Example:**
```
Generic Unpacker Approach:

1. Load packed malware
2. Plugins → ScyllaHide → Enable all
3. Set breakpoints on:
   - VirtualAlloc
   - VirtualProtect
   - WriteProcessMemory (if injecting)
   
4. Run (F9) until first breakpoint
5. Follow in dump the allocated memory
6. Continue running
7. Look for "JMP" to new section (tail jump)
8. When you see normal code:
   - Plugins → Scylla → IAT Autosearch
   - Get Imports
   - Dump
   - Fix dump
   
9. Save unpacked file
6. x64dbgPy Scripting:
python# Create script: scripts/auto_trace.py

import x64dbgpy
from x64dbgpy.pluginsdk import *

def trace_apis():
    """Trace all API calls"""
    
    # Get all imports
    imports = x64dbg.GetCallStack()
    
    for imp in imports:
        # Set breakpoint
        x64dbg.SetBreakpoint(imp.addr)
        
        # Set logging
        x64dbg.SetBreakpointCommand(
            imp.addr,
            f"log API: {imp.name}"
        )
    
    print("[+] Breakpoints set on all imports")
    x64dbg.Run()

def dump_strings():
    """Dump all strings from memory"""
    
    sections = x64dbg.GetSections()
    
    for section in sections:
        data = x64dbg.ReadMemory(section.addr, section.size)
        
        # Extract strings
        strings = extract_ascii(data)
        
        for s in strings:
            print(f"String: {s}")

if __name__ == "__main__":
    trace_apis()
E. Setting Up Custom YARA Rules
1. Install YARA:
bash# Ubuntu
sudo apt install yara

# Or compile from source:
wget https://github.com/VirusTotal/yara/archive/v4.3.2.tar.gz
tar -zxf v4.3.2.tar.gz
cd yara-4.3.2
./bootstrap.sh
./configure
make
sudo make install

# Install Python module
pip3 install yara-python
2. Basic YARA Rule Structure:
yararule MalwareDetection
{
    meta:
        description = "Detects specific malware family"
        author = "Your Name"
        date = "2024-01-11"
        hash = "abc123..."
        
    strings:
        $string1 = "evil.com" ascii wide
        $string2 = { 6A 40 68 00 30 00 00 }  // hex pattern
        $regex1 = /http:\/\/[a-z]{10}\.com/ nocase
        
    condition:
        uint16(0) == 0x5A4D and  // MZ header
        filesize < 1MB and
        2 of ($string*) or $regex1
}
3. Advanced YARA Rules:
yaraimport "pe"
import "math"

rule PackedExecutable
{
    meta:
        description = "Detects packed executables"
        
    condition:
        // Check if PE file
        uint16(0) == 0x5A4D and
        
        // High entropy sections
        for any section in pe.sections : (
            math.entropy(section.raw_data_offset, section.raw_data_size) > 7.0
        ) and
        
        // Suspicious section names
        for any section in pe.sections : (
            section.name contains "UPX" or
            section.name contains ".aspack"
        )
}

rule CobaltStrike_Beacon
{
    meta:
        description = "Detects Cobalt Strike Beacon"
        reference = "https://www.cobaltstrike.com/"
        
    strings:
        // Beacon configuration markers
        $config1 = { 00 01 00 01 00 02 }
        $config2 = { 69 68 69 68 69 6b }
        
        // Common strings
        $str1 = "%s.4%08x%08x%08x%08x%08x.%08x%08x%08x%08x%08x%08x%08x.%08x%08x%08x%08x%08x%08x%08x.%08x%08x%08x%08x%08x%08x%08x.%x%x.%s" ascii
        $str2 = "%02d/%02d/%02d %02d:%02d:%02d" ascii
        
        // Beacon object file
        $bof1 = "ReflectiveLoader" ascii
        $bof2 = "beacon.dll" ascii wide
        
    condition:
        uint16(0) == 0x5A4D and
        filesize < 5MB and
        (
            all of ($config*) or
            2 of ($str*) or
            all of ($bof*)
        )
}

rule Emotet_Payload
{
    meta:
        description = "Detects Emotet malware"
        
    strings:
        // Emotet RSA public key
        $rsa_key = { 30 81 89 02 81 81 00 }
        
        // C2 structure markers
        $c2_marker = { 00 00 00 00 00 00 00 00 [4-8] 00 00 00 00 }
        
        // API hashing
        $api_hash = { 8B 45 ?? 8B 4D ?? 33 C8 8B 45 ?? 33 C8 89 4D }
        
    condition:
        uint16(0) == 0x5A4D and
        pe.imports("kernel32.dll", "VirtualAlloc") and
        2 of them
}

rule Ransomware_Indicators
{
    meta:
        description = "Generic ransomware detection"
        
    strings:
        // Ransom note indicators
        $ransom1 = "your files have been encrypted" nocase
        $ransom2 = "bitcoin" nocase
        $ransom3 = ".onion" nocase
        
        // Crypto API
        $crypto1 = "CryptEncrypt" ascii
        $crypto2 = "CryptGenKey" ascii
        $crypto3 = "CryptImportKey" ascii
        
        // File extension changes
        $ext1 = ".encrypted" ascii
        $ext2 = ".locked" ascii
        $ext3 = ".crypto" ascii
        
        // Deletion commands
        $del1 = "vssadmin delete shadows" nocase
        $del2 = "wbadmin delete catalog" nocase
        $del3 = "bcdedit /set {default} recoveryenabled no" nocase
        
    condition:
        uint16(0) == 0x5A4D and
        (
            2 of ($ransom*) or
            (2 of ($crypto*) and 1 of ($del*))
        )
}
4. Using YARA:
bash# Scan single file
yara rules.yara malware.exe

# Scan directory recursively
yara -r rules.yara /path/to/samples/

# Use multiple rule files
yara rule1.yara rule2.yara malware.exe

# Output in JSON
yara -D -g rules.yara malware.exe

# Fast scan (less thorough)
yara -f rules.yara malware.exe

# Print matching strings
yara -s rules.yara malware.exe

# Print metadata
yara -m rules.yara malware.exe
5. Python Integration:
pythonimport yara
import os

def scan_with_yara(rules_file, target_path):
    """Scan files with YARA rules"""
    
    # Compile rules
    rules = yara.compile(filepath=rules_file)
    
    # Scan single file
    if os.path.isfile(target_path):
        matches = rules.match(target_path)
        print_matches(target_path, matches)
    
    # Scan directory
    elif os.path.isdir(target_path):
        for root, dirs, files in os.walk(target_path):
            for file in files:
                filepath = os.path.join(root, file)
                try:
                    matches = rules.match(filepath)
                    if matches:
                        print_matches(filepath, matches)
                except Exception as e:
                    print(f"Error scanning {filepath}: {e}")

def print_matches(filepath, matches):
    """Print YARA match results"""
    if not matches:
        return
        
    print(f"\n[+] {filepath}")
    for match in matches:
        print(f"  Rule: {match.rule}")
        print(f"  Tags: {match.tags}")
        print(f"  Meta: {match.meta}")
        
        if match.strings:
            print("  Matched strings:")
            for string in match.strings:
                print(f"    Offset: {hex(string.instances[0].offset)}")
                print(f"    Data: {string.instances[0].matched_data}")

def create_rule_from_sample(sample_path):
    """Generate YARA rule from malware sample"""
    
    import pefile
    import hashlib
    
    # Calculate hash
    with open(sample_path, 'rb') as f:
        data = f.read()
        sha256 = hashlib.sha256(data).hexdigest()
    
    # Extract PE characteristics
    try:
        pe = pefile.PE(sample_path)
        imphash = pe.get_imphash()
        
        # Get unique strings
        strings = []
        for s in pe.sections:
            section_data = s.get_data()
            # Extract ASCII strings
            found = extract_strings(section_data, min_length=8)
            strings.extend(found[:10])  # Top 10
        
        # Generate rule
        rule = f"""
rule AutoGenerated_{sha256[:16]}
{{
    meta:
        description = "Auto-generated from sample"
        hash = "{sha256}"
        imphash = "{imphash}"
        
    strings:Continue1:22 AM"""
    for i, s in enumerate(strings):
        rule += f'\n        $str{i} = "{s}" ascii wide'
    
    rule += """
    
condition:
    uint16(0) == 0x5A4D and
    pe.imphash() == "{}" and
    3 of ($str*)
}}
""".format(imphash)
    return rule
    
except Exception as e:
    print(f"Error: {e}")
    return None
def extract_strings(data, min_length=4):
"""Extract ASCII strings from binary data"""
import string
result = []
current_string = ""

for byte in data:
    char = chr(byte)
    if char in string.printable and char not in '\r\n\t':
        current_string += char
    else:
        if len(current_string) >= min_length:
            result.append(current_string)
        current_string = ""

return result
if name == "main":
# Scan with rules
scan_with_yara("rules.yara", "/path/to/samples/")
# Generate rule from sample
rule = create_rule_from_sample("malware.exe")
if rule:
    with open("generated_rule.yara", "w") as f:
        f.write(rule)

---

# 2. Analyzing Particular Malware Families

## A. Emotet Analysis

### **Background:**
- Banking trojan turned botnet
- Modular architecture
- Spreads via email
- Downloads additional payloads

### **Static Analysis:**

**Identifying Emotet:**
```yara
rule Emotet_Loader
{
    strings:
        // RSA public key structure
        $rsa = { 30 81 89 02 81 81 00 }
        
        // C2 IP list structure
        $c2_struct = { 00 00 00 00 00 00 00 00 }
        
        // Emotet registry key
        $reg = "Software\\Microsoft\\Windows\\CurrentVersion\\Run" wide
        
    condition:
        uint16(0) == 0x5A4D and
        all of them
}
```

**Extracting Configuration:**
```python
import pefile
import struct

def extract_emotet_config(filepath):
    """Extract Emotet C2 configuration"""
    
    with open(filepath, 'rb') as f:
        data = f.read()
    
    # Find RSA key (30 81 89 02 81 81 00)
    rsa_marker = b'\x30\x81\x89\x02\x81\x81\x00'
    rsa_offset = data.find(rsa_marker)
    
    if rsa_offset != -1:
        rsa_key = data[rsa_offset:rsa_offset+140]
        print(f"[+] RSA Key found at offset: {hex(rsa_offset)}")
        print(f"    Key: {rsa_key.hex()}")
    
    # Find C2 list (series of IP:Port pairs)
    # Structure: [IP][Port][IP][Port]...
    # IPs stored as DWORD, Ports as WORD
    
    c2_servers = []
    
    # Pattern: 4 bytes (IP) + 2 bytes (Port)
    for i in range(0, len(data) - 6, 1):
        # Check if looks like IP (not all zeros/0xFF)
        potential_ip = data[i:i+4]
        potential_port = data[i+4:i+6]
        
        if potential_ip != b'\x00'*4 and potential_ip != b'\xff'*4:
            ip = struct.unpack('<I', potential_ip)[0]
            port = struct.unpack('<H', potential_port)[0]
            
            # Port range check (1-65535)
            if 1 <= port <= 65535:
                ip_str = f"{ip & 0xFF}.{(ip >> 8) & 0xFF}.{(ip >> 16) & 0xFF}.{(ip >> 24) & 0xFF}"
                
                # Basic IP validation
                octets = [int(x) for x in ip_str.split('.')]
                if all(0 <= x <= 255 for x in octets):
                    c2_servers.append((ip_str, port))
    
    # Remove duplicates
    c2_servers = list(set(c2_servers))
    
    print(f"\n[+] Found {len(c2_servers)} potential C2 servers:")
    for ip, port in c2_servers:
        print(f"    {ip}:{port}")
    
    return {
        'rsa_key': rsa_key.hex() if rsa_offset != -1 else None,
        'c2_servers': c2_servers
    }

# Use:
config = extract_emotet_config('emotet.exe')
```

### **Dynamic Analysis:**

**Behavioral Indicators:**

Process Injection:

Creates suspended process (svchost.exe, explorer.exe)
Writes payload to remote process
Resumes execution


Persistence:

Registry Run key
Scheduled task
Service installation


Network Activity:

HTTP POST to C2 servers
Non-standard User-Agent
Encrypted payload in POST data


File Operations:

Copies itself to %APPDATA%
Creates random filename
Deletes original




**ProcMon Filters:**
Process Name is emotet.exe
Operation is RegSetValue
Operation is CreateFile
Operation is Process Create
Operation is WriteFile

**Network Capture:**
Wireshark filter:
http.request.method == "POST" && ip.addr == [Emotet_IP]
Look for:

POST to /[random]/
Cookie: with long encoded value
No typical web headers


### **Memory Analysis:**
```bash
# Dump Emotet process
vol -f memory.raw windows.pslist | grep emotet

# Dump process memory
vol -f memory.raw windows.memmap --pid 1234 --dump

# Extract injected code
vol -f memory.raw windows.malfind --pid 1234

# Look for network artifacts
vol -f memory.raw windows.netscan | grep 1234
```

### **CAPE Analysis:**
CAPE Output:

Unpacked Emotet payload
Extracted configuration:

C2 server list
RSA public key
Campaign ID


Behavioral signatures:

Process injection detected
Emotet family identified


Dropped files
Network IOCs


## B. Cobalt Strike Beacon Analysis

### **Identification:**
```yara
rule CobaltStrike_Beacon_Config
{
    strings:
        // Beacon config markers
        $cfg1 = { 00 01 00 01 00 02 00 00 00 }
        $cfg2 = { 00 02 00 01 00 02 00 00 00 }
        
        // Sleep time marker
        $sleep = { 00 0D 00 04 }
        
        // License ID
        $license = { 00 07 00 04 }
        
    condition:
        uint16(0) == 0x5A4D and
        2 of them
}
```

### **Configuration Extraction:**
```python
def extract_beacon_config(data):
    """Extract Cobalt Strike Beacon configuration"""
    
    import struct
    
    # Config starts after specific marker
    config_marker = b'\x00\x01\x00\x01\x00\x02'
    offset = data.find(config_marker)
    
    if offset == -1:
        print("[-] Config marker not found")
        return None
    
    config = {}
    pos = offset
    
    # Parse TLV (Type-Length-Value) structure
    while pos < len(data) - 4:
        config_type = struct.unpack('>H', data[pos:pos+2])[0]
        config_len = struct.unpack('>H', data[pos+2:pos+4])[0]
        pos += 4
        
        if pos + config_len > len(data):
            break
        
        config_data = data[pos:pos+config_len]
        pos += config_len
        
        # Parse based on type
        if config_type == 1:  # Beacon type
            config['beacon_type'] = struct.unpack('>H', config_data)[0]
        elif config_type == 2:  # Port
            config['port'] = struct.unpack('>H', config_data)[0]
        elif config_type == 3:  # Sleep time
            config['sleep_time'] = struct.unpack('>I', config_data)[0]
        elif config_type == 7:  # License ID
            config['license_id'] = struct.unpack('>I', config_data)[0]
        elif config_type == 8:  # C2 Server
            config['c2_server'] = config_data.decode('utf-8', errors='ignore')
        elif config_type == 9:  # User-Agent
            config['user_agent'] = config_data.decode('utf-8', errors='ignore')
        elif config_type == 12:  # HTTP Method
            config['http_method'] = config_data.decode('utf-8', errors='ignore')
        elif config_type == 13:  # URI
            config['uri'] = config_data.decode('utf-8', errors='ignore')
        elif config_type == 26:  # Watermark
            config['watermark'] = struct.unpack('>I', config_data)[0]
        
        if config_type == 0:  # End of config
            break
    
    return config

# Decrypt XOR'd config (if needed)
def decrypt_beacon_config(data, key=0x69):
    """XOR decrypt Beacon config"""
    decrypted = bytearray()
    for byte in data:
        decrypted.append(byte ^ key)
    return bytes(decrypted)

# Use:
with open('beacon.dll', 'rb') as f:
    data = f.read()

config = extract_beacon_config(data)
if config:
    print("[+] Beacon Configuration:")
    for key, value in config.items():
        print(f"    {key}: {value}")
```

### **Dynamic Analysis:**

**Beacon Behavior:**

Initialization:

Reflective DLL loading (no disk write)
Allocate RWX memory
Parse embedded config


C2 Communication:

HTTP/HTTPS GET requests (metadata)
POST requests (task results)
Sleep between callbacks
Jitter to randomize timing


Common Commands:

shell: Execute commands
upload/download: File operations
screenshot: Capture screen
keylog: Keylogger
inject: Process injection
spawn: Create new session




**Network Indicators:**
HTTP GET:

URI patterns from config
Custom User-Agent
Cookie with session data (base64)

HTTP POST:

Same URI patterns
Encrypted task results
May use Content-Type: application/octet-stream

HTTPS:

May use custom cert
Check certificate details
JA3 fingerprint


**Memory Analysis:**
```bash
# Find beacon in memory
vol -f memory.raw windows.malfind

# Look for common APIs
# GetProcAddress, VirtualAlloc, LoadLibrary

# Extract beacon DLL
vol -f memory.raw windows.dlllist --pid [PID]

# Dump suspicious memory regions
vol -f memory.raw windows.memmap --pid [PID] --dump
```

### **YARA Hunting:**
```yara
rule CobaltStrike_Sleep_Mask
{
    meta:
        description = "Cobalt Strike sleep mask technique"
        
    strings:
        $sleep_mask = { 48 89 5C 24 08 48 89 74 24 10 57 48 83 EC 20 }
        
    condition:
        $sleep_mask
}

rule CobaltStrike_Reflective_Loader
{
    strings:
        $ref_loader = "ReflectiveLoader" ascii
        $export = "beacon.dll" wide
        
    condition:
        all of them
}
```

## C. Ransomware Analysis (Generic)

### **Identifying Ransomware:**
```yara
rule Generic_Ransomware
{
    meta:
        description = "Generic ransomware indicators"
        
    strings:
        // Encryption APIs
        $crypt1 = "CryptEncrypt" ascii
        $crypt2 = "CryptGenKey" ascii
        $crypt3 = "BCryptEncrypt" ascii
        $crypt4 = "BCryptGenerateSymmetricKey" ascii
        
        // File operations
        $file1 = "FindFirstFileW" ascii
        $file2 = "FindNextFileW" ascii
        $file3 = "CreateFileW" ascii
        
        // Shadow copy deletion
        $vss1 = "vssadmin delete shadows" wide nocase
        $vss2 = "wmic shadowcopy delete" wide nocase
        $vss3 = "bcdedit" wide
        
        // Ransom indicators
        $ransom1 = "bitcoin" nocase
        $ransom2 = "decrypt" nocase
        $ransom3 = ".onion" nocase
        
    condition:
        uint16(0) == 0x5A4D and
        (
            (2 of ($crypt*) and 2 of ($file*)) or
            (1 of ($crypt*) and 1 of ($vss*)) or
            (2 of ($ransom*) and 1 of ($crypt*))
        )
}
```

### **Static Analysis:**
```python
def analyze_ransomware(filepath):
    """Analyze ransomware sample"""
    
    import pefile
    import re
    
    pe = pefile.PE(filepath)
    
    print("[+] Ransomware Analysis")
    print("="*60)
    
    # Check imports
    crypto_apis = [
        'CryptEncrypt', 'CryptDecrypt', 'CryptGenKey',
        'BCryptEncrypt', 'BCryptGenerateSymmetricKey',
        'CryptImportKey', 'CryptExportKey'
    ]
    
    file_apis = [
        'CreateFileW', 'WriteFile', 'ReadFile',
        'FindFirstFileW', 'FindNextFileW', 'DeleteFileW',
        'MoveFileW', 'SetFileAttributesW'
    ]
    
    found_crypto = []
    found_file = []
    
    try:
        for entry in pe.DIRECTORY_ENTRY_IMPORT:
            for imp in entry.imports:
                if imp.name:
                    name = imp.name.decode()
                    if name in crypto_apis:
                        found_crypto.append(name)
                    if name in file_apis:
                        found_file.append(name)
    except:
        pass
    
    print(f"\n[*] Crypto APIs: {len(found_crypto)}")
    for api in found_crypto:
        print(f"    - {api}")
    
    print(f"\n[*] File APIs: {len(found_file)}")
    for api in found_file:
        print(f"    - {api}")
    
    # Extract strings
    with open(filepath, 'rb') as f:
        data = f.read()
    
    # Look for extensions
    ext_pattern = rb'\.[a-z]{3,10}'
    extensions = re.findall(ext_pattern, data)
    extensions = [e.decode('ascii', errors='ignore') for e in set(extensions)]
    
    print(f"\n[*] Potential target extensions:")
    for ext in extensions[:20]:  # Top 20
        print(f"    {ext}")
    
    # Look for ransom note indicators
    ransom_keywords = [
        b'bitcoin', b'decrypt', b'encrypt', b'ransom',
        b'payment', b'.onion', b'recover', b'restore'
    ]
    
    found_keywords = []
    for keyword in ransom_keywords:
        if keyword.lower() in data.lower():
            found_keywords.append(keyword.decode())
    
    print(f"\n[*] Ransom indicators found: {found_keywords}")
    
    # Check for embedded config
    # Look for URLs
    url_pattern = rb'https?://[^\s<>"{}|\\^`\[\]]{10,}'
    urls = re.findall(url_pattern, data)
    
    if urls:
        print(f"\n[*] Potential C2 URLs:")
        for url in set(urls):
            print(f"    {url.decode('ascii', errors='ignore')}")
    
    return {
        'crypto_apis': found_crypto,
        'file_apis': found_file,
        'extensions': extensions,
        'ransom_indicators': found_keywords,
        'urls': [u.decode('ascii', errors='ignore') for u in urls]
    }

# Use:
results = analyze_ransomware('ransomware.exe')
```

### **Dynamic Analysis:**

**Monitoring Encryption:**
```bash
# ProcMon filters
Operation is WriteFile
Path ends with .txt
Path ends with .docx
Path ends with .pdf

# Watch for:
- Original file opened for read
- New file created (with ransomware extension)
- Original file deleted

# FsLogix or Process Monitor can show:
- File rename operations
- Attribute changes
- Deletion patterns
```

**Process Tree:**
Typical Ransomware Flow:
ransomware.exe
├── cmd.exe (delete shadow copies)
│   └── vssadmin.exe delete shadows /all /quiet
├── cmd.exe (disable recovery)
│   └── bcdedit.exe /set {default} recoveryenabled no
└── [Multiple threads encrypting files]

**Network Activity:**

Initial beacon:

Contact C2 to register victim
Receive encryption key
Report system info


During encryption:

May phone home with progress


After encryption:

May exfiltrate data (double extortion)
Download ransom note




### **Configuration Extraction:**
```python
def extract_ransom_config(filepath):
    """Extract ransomware configuration"""
    
    import re
    
    with open(filepath, 'rb') as f:
        data = f.read()
    
    config = {}
    
    # Extract Bitcoin addresses
    btc_pattern = rb'[13][a-km-zA-HJ-NP-Z1-9]{25,34}'
    btc_addresses = re.findall(btc_pattern, data)
    config['bitcoin_addresses'] = [b.decode() for b in set(btc_addresses)]
    
    # Extract .onion addresses
    onion_pattern = rb'[a-z2-7]{16,56}\.onion'
    onion_addresses = re.findall(onion_pattern, data)
    config['onion_sites'] = [o.decode() for o in set(onion_addresses)]
    
    # Extract email addresses
    email_pattern = rb'[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}'
    emails = re.findall(email_pattern, data)
    config['contact_emails'] = [e.decode() for e in set(emails)]
    
    # Extract ransom note template
    note_markers = [
        b'YOUR FILES',
        b'ENCRYPTED',
        b'HOW TO DECRYPT',
        b'BITCOIN',
        b'PAYMENT'
    ]
    
    # Look for section with multiple markers
    for i in range(len(data) - 1000):
        section = data[i:i+1000]
        marker_count = sum(1 for m in note_markers if m in section.upper())
        
        if marker_count >= 3:
            # Likely ransom note
            note = section.decode('utf-8', errors='ignore')
            config['ransom_note'] = note.strip()
            break
    
    # Extract file extension
    new_ext_pattern = rb'\.[a-z]{3,10}(?=\x00|\s)'
    extensions = re.findall(new_ext_pattern, data)
    
    # Filter to likely ransomware extensions
    ransomware_exts = [e.decode() for e in extensions 
                       if e.decode() not in ['.exe', '.dll', '.txt', '.log']]
    if ransomware_exts:
        config['new_extension'] = ransomware_exts[0]
    
    return config

# Use:
config = extract_ransom_config('ransomware.exe')
print("[+] Ransomware Configuration:")
for key, value in config.items():
    print(f"    {key}: {value}")
```

### **Recovery & Mitigation:**

**Decryption Possibilities:**

Check No More Ransom project:

https://www.nomoreransom.org/
Free decryption tools
130+ tools available


Identify ransomware family:

ID Ransomware: https://id-ransomware.malwarehunterteam.com/
Upload ransom note or encrypted file
Get family identification


Check for weaknesses:

Poor RNG (predictable keys)
Symmetric key not deleted
Backup key in memory




**Post-Incident Analysis:**
```bash
# Find patient zero (first infected file)
# Sort files by modification time

# Windows Event Logs
Get-EventLog -LogName Security -After (Get-Date).AddDays(-7) |
    Where-Object {$_.EventID -eq 4663} |
    Select-Object TimeGenerated, Message

# Check for persistence
reg query HKLM\Software\Microsoft\Windows\CurrentVersion\Run
schtasks /query /fo LIST /v

# Network connections (if still running)
netstat -ano | findstr ESTABLISHED
```

---

# 3. Advanced Debugging Techniques

## A. Anti-Debugging Bypass

### **Common Anti-Debug Checks:**

**1. IsDebuggerPresent API:**
```assembly
; Malware code:
call IsDebuggerPresent
test eax, eax
jnz debugger_detected

; Bypass in x64dbg:
1. Find call to IsDebuggerPresent
2. Set breakpoint
3. When hit, modify EAX:
   - Right-click EAX register
   - Set value to 0
4. Or patch the call:
   - Assemble → NOP the call and test
```

**2. PEB (Process Environment Block) Checks:**
```assembly
; Check BeingDebugged flag
mov eax, fs:[30h]      ; Get PEB address
movzx eax, byte ptr [eax+2]  ; BeingDebugged offset
test eax, eax
jnz debugger_detected

; Bypass:
1. Find the check
2. Patch the JNZ to JMP (skip check)
3. Or modify PEB:
   - Memory map → Find PEB
   - Offset +2 → Set to 0
```

**3. NtGlobalFlag Check:**
```assembly
; PEB.NtGlobalFlag (offset 0x68)
mov eax, fs:[30h]
mov eax, [eax+68h]
and eax, 70h          ; FLG_HEAP_* flags
jnz debugger_detected

; Bypass:
1. Find check
2. Patch jump
3. Or use ScyllaHide plugin
```

**4. Timing Checks:**
```assembly
; RDTSC (Read Time-Stamp Counter)
rdtsc                 ; EDX:EAX = timestamp
mov edi, eax
; ... some code ...
rdtsc
sub eax, edi
cmp eax, 10000h       ; Check if too slow
ja debugger_detected

; Bypass:
1. Use hardware breakpoint (faster)
2. Or patch the comparison:
   - Change JA to JMP (unconditional)
3. Or use time manipulation:
   - Modify EAX after second RDTSC
```

**5. OutputDebugString:**
```cpp
// Checks for debugger by OutputDebugString
SetLastError(0);
OutputDebugStringA("test");
if (GetLastError() == 0) {
    // Debugger present
}

// Bypass:
1. Set breakpoint on GetLastError
2. Modify return value to non-zero
```

### **Automated Bypass - ScyllaHide:**
ScyllaHide Configuration:

Plugins → ScyllaHide → Options
Enable all protections:
☑ NtQueryInformationProcess
☑ NtQuerySystemInformation
☑ NtQueryObject
☑ NtSetInformationThread
☑ NtClose
☑ NtYieldExecution/ZwYieldExecution
☑ OutputDebugString
☑ GetTickCount
☑ GetTickCount64
☑ GetLocalTime
☑ GetSystemTime
☑ NtGetContextThread
☑ NtSetContextThread
☑ BlockInput
☑ NtUserBuildHwndList
☑ NtUserFindWindowEx
☑ NtUserQueryWindow
☑ NtSetDebugFilterState
☑ PEB (BeingDebugged)
☑ PEB (NtGlobalFlag)
☑ PEB (HeapFlags)
☑ Kill Anti-Attach
Profile: Aggressive


## B. Code Injection Detection & Analysis

### **Types of Injection:**

**1. Classic DLL Injection:**
Detection:

Suspicious OpenProcess calls
VirtualAllocEx in remote process
WriteProcessMemory
CreateRemoteThread
LoadLibraryA address pushed

Debugging:

Set breakpoint on CreateRemoteThread
Examine stack for DLL path
Follow thread creation
Attach to target process


**2. Process Hollowing:**
Detection:

CreateProcess with CREATE_SUSPENDED
NtUnmapViewOfSection
VirtualAllocEx
WriteProcessMemory
SetThreadContext
ResumeThread

Debugging:

BP on NtUnmapViewOfSection
BP on WriteProcessMemory
Before ResumeThread:

Dump new image
Analyze payload




**3. Reflective DLL Injection:**
Detection:

VirtualAllocEx with RWX
WriteProcessMemory (large buffer)
CreateRemoteThread to shellcode
No LoadLibrary call

Debugging:

BP on VirtualAllocEx
Dump allocated memory
Find ReflectiveLoader function
Follow execution into DLL


### **Injection Analysis Script:**
```python
# x64dbgpy script
import x64dbgpy

def monitor_injection():
    """Monitor for code injection attempts"""
    
    injection_apis = [
        'VirtualAllocEx',
        'WriteProcessMemory',
        'CreateRemoteThread',
        'NtMapViewOfSection',
        'NtUnmapViewOfSection',
        'SetThreadContext',
        'QueueUserAPC'
    ]
    
    print("[+] Setting breakpoints on injection APIs...")
    
    for api in injection_apis:
        addr = x64dbg.GetProcAddress(api)
        if addr:
            x64dbg.SetBreakpoint(addr)
            x64dbg.SetBreakpointCommand(
                addr,
                f"log Injection API: {api}"
            )
            print(f"    [*] {api} @ {hex(addr)}")
    
    print("[+] Monitoring started. Press F9 to run.")

def dump_injected_code():
    """Dump code written by WriteProcessMemory"""
    
    # When at WriteProcessMemory breakpoint:
    # Args: HANDLE hProcess, LPVOID lpBaseAddress, 
    #       LPCVOID lpBuffer, SIZE_T nSize
    
    base_addr = x64dbg.GetArg(2)  # lpBaseAddress
    size = x64dbg.GetArg(4)        # nSize
    
    print(f"[+] WriteProcessMemory detected:")
    print(f"    Target: {hex(base_addr)}")
    print(f"    Size: {size} bytes")
    
    # Dump the buffer being written
    data = x64dbg.ReadMemory(x64dbg.GetArg(3), size)
    
    with open(f"injected_{hex(base_addr)}.bin", "wb") as f:
        f.write(data)
    
    print(f"    [+] Dumped to injected_{hex(base_addr)}.bin")

# Use in x64dbg:
# Plugins → Python → Run Script
```

## C. Encrypted/Packed Code Analysis

### **Multi-Layer Unpacking:**

**Strategy:**

Identify packer layers:

DIE (Detect It Easy)
Entropy analysis
Check for known signatures


Unpack layer by layer:

Set BP on VirtualProtect/VirtualAlloc
Find OEP (Original Entry Point)
Dump at each layer


Fix imports:

Use Scylla
Rebuild IAT


Analyze final payload


**Generic Unpacking Script:**
```python
import x64dbgpy

class GenericUnpacker:
    def __init__(self):
        self.dumps = []
        self.layer = 0
    
    def find_oep(self):
        """Find Original Entry Point"""
        
        print("[+] Searching for OEP...")
        
        # Method 1: VirtualProtect
        vp_addr = x64dbg.GetProcAddress("VirtualProtect")
        x64dbg.SetBreakpoint(vp_addr)
        
        # Method 2: Common tail jump patterns
        # JMP [reg]
        # JMP [reg+offset]
        # PUSH addr; RET
        
        x64dbg.Run()
        
        # After BP hit, step through
        for _ in range(100):
            x64dbg.StepOver()
            
            # Check if we hit normal code
            if self.looks_like_normal_code():
                print(f"[+] Potential OEP found: {hex(x64dbg.GetEIP())}")
                return x64dbg.GetEIP()
        
        return None
    
    def looks_like_normal_code(self):
        """Heuristic: Check if code looks unpacked"""
        
        eip = x64dbg.GetEIP()
        
        # Read 100 bytes
        code = x64dbg.ReadMemory(eip, 100)
        
        # Check for:
        # 1. Push EBP; Mov EBP, ESP (function prologue)
        if code[:2] == b'\x55\x8B':  # push ebp; mov ebp, esp
            return True
        
        # 2. Multiple valid instructions
        valid_count = 0
        for i in range(0, 50, 1):
            if self.is_valid_instruction(code[i:i+15]):
                valid_count += 1
        
        return valid_count > 10
    
    def is_valid_instruction(self, bytes):
        """Check if bytes form valid instruction"""
        # Simplified check
        # Real implementation would use disassembler
        if not bytes:
            return False
        
        #Continue1:25 AMCommon x86 opcodes
valid_opcodes = [
0x50, 0x51, 0x52, 0x53, 0x54, 0x55, 0x56, 0x57,  # PUSH
0x58, 0x59, 0x5A, 0x5B, 0x5C, 0x5D, 0x5E, 0x5F,  # POP
0x8B, 0x89,  # MOV
0xFF, 0xE8, 0xE9,  # CALL/JMP
]
    return bytes[0] in valid_opcodes

def dump_layer(self):
    """Dump current unpacked layer"""
    
    self.layer += 1
    oep = x64dbg.GetEIP()
    
    print(f"[+] Dumping layer {self.layer} at {hex(oep)}")
    
    # Use Scylla to dump
    x64dbg.ScyllaDump(f"layer_{self.layer}.exe")
    
    self.dumps.append(f"layer_{self.layer}.exe")
    print(f"    [+] Saved as layer_{self.layer}.exe")

def unpack_all(self):
    """Automated multi-layer unpacking"""
    
    print("[*] Starting automated unpacking...")
    
    max_layers = 5
    for i in range(max_layers):
        print(f"\n[*] Processing layer {i+1}...")
        
        oep = self.find_oep()
        if oep:
            self.dump_layer()
            
            # Check if more layers
            if not self.has_more_layers():
                print("[+] Final layer reached!")
                break
        else:
            print("[-] Could not find OEP")
            break
    
    print(f"\n[+] Unpacking complete. {len(self.dumps)} layers dumped.")
    return self.dumps

def has_more_layers(self):
    """Check if more unpacking layers exist"""
    
    # Look for signs of another packer:
    # - High entropy sections
    # - VirtualAlloc calls ahead
    # - Suspicious API imports
    
    # Simplified: check next 1000 instructions
    eip = x64dbg.GetEIP()
    code = x64dbg.ReadMemory(eip, 5000)
    
    # Look for VirtualAlloc/VirtualProtect calls
    return b'VirtualAlloc' in code or b'VirtualProtect' in code
Usage:
unpacker = GenericUnpacker()
dumped_files = unpacker.unpack_all()

## D. Anti-VM/Sandbox Evasion

### **VM Detection Methods:**

**1. Registry Checks:**
```assembly
; Check for VMware registry keys
push offset "SOFTWARE\\VMware, Inc.\\VMware Tools"
call RegOpenKeyExA
test eax, eax
jz vm_detected

; Bypass:
1. Modify registry before running
2. Or patch the jump
3. Or use VM with reg keys removed
```

**2. CPUID Checks:**
```assembly
; Check hypervisor bit
mov eax, 1
cpuid
bt ecx, 31        ; Hypervisor present bit
jc vm_detected

; Bypass:
1. Patch the jump
2. Or modify ECX after CPUID:
   - Set BP after CPUID
   - Clear bit 31 of ECX
```

**3. Timing Attacks:**
```assembly
; Check if execution is too slow
rdtsc
mov edi, eax
; Do something
rdtsc
sub eax, edi
cmp eax, threshold
ja sandbox_detected

; Bypass:
1. Hardware BP (fast)
2. Modify EAX
3. Or use bare metal
```

**4. File/Process Checks:**
Common checks:

C:\windows\System32\Drivers\Vmmouse.sys
C:\windows\System32\Drivers\vmhgfs.sys
VBoxService.exe
vmtoolsd.exe

Bypass:

Rename/hide these files
Patch checks
Use pafish tool to identify checks


### **Pafish - VM Detection Test:**
```bash
# Run pafish in VM to see what malware detects
pafish.exe

# Output shows:
[*] CPU hypervisor bit: detected
[*] VMware registry key: detected
[*] VirtualBox reg key: detected
[*] Sandbox username: detected

# Fix each detection before analysis
```

### **Building Better Analysis VM:**
Hardening Checklist:

System:

Real-looking username (not "admin" or "sandbox")
Install common software (browsers, Office)
Create file history
Add desktop files
Set wallpaper


Hardware:

Increase RAM (8GB+)
More CPU cores (4+)
Add random files to disk
Realistic disk size (100GB+)


Artifacts:

Browse web to create cookies/history
Run applications to create recent files
Populate Downloads folder
Create restore points


Modifications:

Hide VM identifiers
Modify BIOS strings
Remove VM tools
Randomize MAC address


Tools:

al-khaser (test evasion)
pafish (check detections)
Use results to patch VM




---

# 4. Creating Custom Analysis Scripts

## A. Automated Sample Processing

### **Batch Analysis Framework:**
```python
#!/usr/bin/env python3
"""
Automated Malware Analysis Framework
Processes multiple samples through various analysis stages
"""

import os
import sys
import json
import hashlib
import subprocess
import time
from datetime import datetime
from pathlib import Path

class MalwareAnalyzer:
    def __init__(self, sample_dir, output_dir):
        self.sample_dir = Path(sample_dir)
        self.output_dir = Path(output_dir)
        self.output_dir.mkdir(exist_ok=True)
        
        # Tool paths
        self.tools = {
            'strings': '/usr/bin/strings',
            'yara': '/usr/bin/yara',
            'pefile': 'python3 -m pefile',
            'volatility': 'vol',
            'capa': '/opt/capa/capa',
            'floss': '/opt/flare-floss/floss.exe',
        }
        
        self.yara_rules = Path('./rules/')
    
    def calculate_hashes(self, filepath):
        """Calculate file hashes"""
        hashes = {}
        
        with open(filepath, 'rb') as f:
            data = f.read()
        
        hashes['md5'] = hashlib.md5(data).hexdigest()
        hashes['sha1'] = hashlib.sha1(data).hexdigest()
        hashes['sha256'] = hashlib.sha256(data).hexdigest()
        hashes['size'] = len(data)
        
        return hashes
    
    def extract_strings(self, filepath):
        """Extract strings from file"""
        try:
            result = subprocess.run(
                [self.tools['strings'], '-n', '6', filepath],
                capture_output=True,
                text=True,
                timeout=60
            )
            
            strings = result.stdout.splitlines()
            
            # Filter interesting strings
            interesting = {
                'urls': [],
                'ips': [],
                'emails': [],
                'registry': [],
                'files': []
            }
            
            import re
            
            for s in strings:
                # URLs
                if re.match(r'https?://', s):
                    interesting['urls'].append(s)
                
                # IPs
                if re.match(r'\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}', s):
                    interesting['ips'].append(s)
                
                # Emails
                if re.match(r'[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}', s):
                    interesting['emails'].append(s)
                
                # Registry
                if 'HKEY_' in s or 'Software\\' in s:
                    interesting['registry'].append(s)
                
                # File paths
                if (':\\' in s or '/' in s) and ('.' in s):
                    interesting['files'].append(s)
            
            return {
                'count': len(strings),
                'interesting': interesting,
                'all_strings': strings[:1000]  # Limit to first 1000
            }
            
        except Exception as e:
            return {'error': str(e)}
    
    def run_yara(self, filepath):
        """Run YARA rules against sample"""
        try:
            matches = []
            
            for rule_file in self.yara_rules.glob('*.yar'):
                result = subprocess.run(
                    [self.tools['yara'], '-s', str(rule_file), filepath],
                    capture_output=True,
                    text=True,
                    timeout=60
                )
                
                if result.stdout:
                    matches.append({
                        'rule_file': rule_file.name,
                        'matches': result.stdout
                    })
            
            return matches
            
        except Exception as e:
            return {'error': str(e)}
    
    def analyze_pe(self, filepath):
        """Analyze PE file structure"""
        try:
            import pefile
            
            pe = pefile.PE(filepath)
            
            analysis = {
                'machine': hex(pe.FILE_HEADER.Machine),
                'timestamp': pe.FILE_HEADER.TimeDateStamp,
                'sections': [],
                'imports': {},
                'exports': [],
                'entropy': {},
                'suspicious': []
            }
            
            # Sections
            for section in pe.sections:
                sec_info = {
                    'name': section.Name.decode().rstrip('\x00'),
                    'virtual_address': hex(section.VirtualAddress),
                    'virtual_size': section.Misc_VirtualSize,
                    'raw_size': section.SizeOfRawData,
                    'entropy': section.get_entropy()
                }
                
                # Check for high entropy
                if sec_info['entropy'] > 7.0:
                    analysis['suspicious'].append(
                        f"High entropy section: {sec_info['name']} ({sec_info['entropy']:.2f})"
                    )
                
                analysis['sections'].append(sec_info)
                analysis['entropy'][sec_info['name']] = sec_info['entropy']
            
            # Imports
            try:
                for entry in pe.DIRECTORY_ENTRY_IMPORT:
                    dll_name = entry.dll.decode()
                    imports = []
                    
                    for imp in entry.imports:
                        if imp.name:
                            imports.append(imp.name.decode())
                    
                    analysis['imports'][dll_name] = imports
            except:
                pass
            
            # Check for suspicious imports
            suspicious_apis = [
                'VirtualAlloc', 'VirtualAllocEx', 'WriteProcessMemory',
                'CreateRemoteThread', 'LoadLibraryA', 'GetProcAddress',
                'IsDebuggerPresent', 'CheckRemoteDebuggerPresent',
                'CryptEncrypt', 'InternetOpenA', 'WinExec'
            ]
            
            for dll, imports in analysis['imports'].items():
                for api in imports:
                    if api in suspicious_apis:
                        analysis['suspicious'].append(f"Suspicious API: {dll}!{api}")
            
            # Exports
            try:
                for exp in pe.DIRECTORY_ENTRY_EXPORT.symbols:
                    if exp.name:
                        analysis['exports'].append(exp.name.decode())
            except:
                pass
            
            # Imphash
            try:
                analysis['imphash'] = pe.get_imphash()
            except:
                pass
            
            return analysis
            
        except Exception as e:
            return {'error': str(e)}
    
    def run_capa(self, filepath):
        """Run CAPA for capability detection"""
        try:
            result = subprocess.run(
                [self.tools['capa'], '-j', filepath],
                capture_output=True,
                text=True,
                timeout=300
            )
            
            if result.stdout:
                return json.loads(result.stdout)
            
            return {}
            
        except Exception as e:
            return {'error': str(e)}
    
    def generate_report(self, sample_path, results):
        """Generate analysis report"""
        
        report = {
            'metadata': {
                'filename': sample_path.name,
                'analysis_date': datetime.now().isoformat(),
                'hashes': results.get('hashes', {}),
            },
            'static_analysis': {
                'strings': results.get('strings', {}),
                'yara': results.get('yara', []),
                'pe_analysis': results.get('pe', {}),
                'capabilities': results.get('capa', {}),
            },
            'indicators': {
                'suspicious_findings': [],
                'iocs': {
                    'urls': [],
                    'ips': [],
                    'domains': [],
                    'registry_keys': [],
                    'file_paths': []
                }
            }
        }
        
        # Collect IOCs
        if 'strings' in results:
            strings_data = results['strings'].get('interesting', {})
            report['indicators']['iocs']['urls'] = strings_data.get('urls', [])
            report['indicators']['iocs']['ips'] = strings_data.get('ips', [])
            report['indicators']['iocs']['registry_keys'] = strings_data.get('registry', [])
            report['indicators']['iocs']['file_paths'] = strings_data.get('files', [])
        
        # Collect suspicious findings
        if 'pe' in results:
            suspicious = results['pe'].get('suspicious', [])
            report['indicators']['suspicious_findings'].extend(suspicious)
        
        # YARA matches
        if 'yara' in results:
            for match in results['yara']:
                report['indicators']['suspicious_findings'].append(
                    f"YARA match: {match.get('rule_file', 'unknown')}"
                )
        
        return report
    
    def analyze_sample(self, sample_path):
        """Perform comprehensive analysis on single sample"""
        
        print(f"\n[*] Analyzing: {sample_path.name}")
        print("="*60)
        
        results = {}
        
        # 1. Calculate hashes
        print("[*] Calculating hashes...")
        results['hashes'] = self.calculate_hashes(sample_path)
        print(f"    SHA256: {results['hashes']['sha256']}")
        
        # 2. Extract strings
        print("[*] Extracting strings...")
        results['strings'] = self.extract_strings(sample_path)
        print(f"    Found {results['strings'].get('count', 0)} strings")
        
        # 3. YARA scan
        print("[*] Running YARA rules...")
        results['yara'] = self.run_yara(sample_path)
        print(f"    {len(results['yara'])} rules matched")
        
        # 4. PE analysis
        if sample_path.suffix.lower() in ['.exe', '.dll', '.sys']:
            print("[*] Analyzing PE structure...")
            results['pe'] = self.analyze_pe(sample_path)
            
            if 'error' not in results['pe']:
                print(f"    Sections: {len(results['pe']['sections'])}")
                print(f"    Imports: {len(results['pe']['imports'])} DLLs")
        
        # 5. CAPA analysis
        if os.path.exists(self.tools.get('capa', '')):
            print("[*] Running CAPA...")
            results['capa'] = self.run_capa(sample_path)
        
        # 6. Generate report
        print("[*] Generating report...")
        report = self.generate_report(sample_path, results)
        
        # Save report
        report_file = self.output_dir / f"{results['hashes']['sha256']}_report.json"
        with open(report_file, 'w') as f:
            json.dump(report, f, indent=2)
        
        print(f"[+] Report saved: {report_file}")
        
        # Print summary
        self.print_summary(report)
        
        return report
    
    def print_summary(self, report):
        """Print analysis summary"""
        print("\n" + "="*60)
        print("ANALYSIS SUMMARY")
        print("="*60)
        
        # Suspicious findings
        findings = report['indicators']['suspicious_findings']
        if findings:
            print(f"\n[!] Suspicious Findings ({len(findings)}):")
            for finding in findings[:10]:  # Top 10
                print(f"    - {finding}")
        
        # IOCs
        iocs = report['indicators']['iocs']
        if iocs['urls']:
            print(f"\n[+] URLs ({len(iocs['urls'])}):")
            for url in iocs['urls'][:5]:
                print(f"    - {url}")
        
        if iocs['ips']:
            print(f"\n[+] IP Addresses ({len(iocs['ips'])}):")
            for ip in iocs['ips'][:5]:
                print(f"    - {ip}")
        
        print("\n" + "="*60)
    
    def batch_analyze(self):
        """Analyze all samples in directory"""
        
        samples = list(self.sample_dir.glob('*'))
        total = len(samples)
        
        print(f"[+] Found {total} samples to analyze")
        print("="*60)
        
        results = []
        
        for i, sample in enumerate(samples, 1):
            if sample.is_file():
                print(f"\n[{i}/{total}] Processing: {sample.name}")
                
                try:
                    result = self.analyze_sample(sample)
                    results.append(result)
                except Exception as e:
                    print(f"[!] Error analyzing {sample.name}: {e}")
                    continue
        
        # Generate summary report
        self.generate_batch_summary(results)
        
        return results
    
    def generate_batch_summary(self, results):
        """Generate summary of batch analysis"""
        
        summary_file = self.output_dir / "batch_summary.json"
        
        summary = {
            'total_samples': len(results),
            'analysis_date': datetime.now().isoformat(),
            'samples': []
        }
        
        for result in results:
            sample_summary = {
                'filename': result['metadata']['filename'],
                'sha256': result['metadata']['hashes']['sha256'],
                'suspicious_count': len(result['indicators']['suspicious_findings']),
                'yara_matches': len(result['static_analysis']['yara']),
                'ioc_count': sum(len(v) for v in result['indicators']['iocs'].values())
            }
            summary['samples'].append(sample_summary)
        
        with open(summary_file, 'w') as f:
            json.dump(summary, f, indent=2)
        
        print(f"\n[+] Batch summary saved: {summary_file}")

def main():
    if len(sys.argv) < 3:
        print("Usage: python3 analyzer.py <sample_dir> <output_dir>")
        sys.exit(1)
    
    sample_dir = sys.argv[1]
    output_dir = sys.argv[2]
    
    analyzer = MalwareAnalyzer(sample_dir, output_dir)
    analyzer.batch_analyze()

if __name__ == "__main__":
    main()
```

## B. API Monitoring Script
```python
#!/usr/bin/env python3
"""
API Call Monitor and Logger
Hooks common malware APIs and logs their usage
"""

import frida
import sys
import json
from datetime import datetime

class APIMonitor:
    def __init__(self, target_process):
        self.target = target_process
        self.session = None
        self.script = None
        self.api_calls = []
        
        # APIs to monitor
        self.monitored_apis = {
            'kernel32.dll': [
                'CreateFileW',
                'WriteFile',
                'ReadFile',
                'DeleteFileW',
                'CreateProcessW',
                'VirtualAlloc',
                'VirtualAllocEx',
                'VirtualProtect',
                'WriteProcessMemory',
                'CreateRemoteThread',
                'LoadLibraryA',
                'LoadLibraryW',
                'GetProcAddress',
                'Sleep',
            ],
            'advapi32.dll': [
                'RegOpenKeyExW',
                'RegSetValueExW',
                'RegQueryValueExW',
                'RegDeleteKeyW',
                'CreateServiceW',
            ],
            'wininet.dll': [
                'InternetOpenW',
                'InternetConnectW',
                'HttpOpenRequestW',
                'HttpSendRequestW',
                'InternetReadFile',
            ],
            'ws2_32.dll': [
                'socket',
                'connect',
                'send',
                'recv',
                'WSAStartup',
            ],
            'user32.dll': [
                'SetWindowsHookExW',
                'GetAsyncKeyState',
                'RegisterHotKey',
            ],
            'crypt32.dll': [
                'CryptEncrypt',
                'CryptDecrypt',
                'CryptGenKey',
            ]
        }
    
    def generate_frida_script(self):
        """Generate Frida script to hook APIs"""
        
        script_template = """
        var api_calls = [];
        
        function hook_api(module, func_name) {
            var addr = Module.findExportByName(module, func_name);
            if (addr) {
                Interceptor.attach(addr, {
                    onEnter: function(args) {
                        var call_info = {
                            timestamp: new Date().toISOString(),
                            module: module,
                            function: func_name,
                            thread_id: this.threadId,
                            args: []
                        };
                        
                        // Log arguments (type-specific handling)
                        if (func_name === 'CreateFileW') {
                            call_info.args.push({name: 'lpFileName', value: args[0].readUtf16String()});
                            call_info.args.push({name: 'dwDesiredAccess', value: args[1].toInt32()});
                        }
                        else if (func_name === 'WriteFile') {
                            call_info.args.push({name: 'hFile', value: args[0].toString()});
                            call_info.args.push({name: 'nNumberOfBytesToWrite', value: args[2].toInt32()});
                        }
                        else if (func_name === 'CreateProcessW') {
                            if (!args[1].isNull()) {
                                call_info.args.push({name: 'lpCommandLine', value: args[1].readUtf16String()});
                            }
                        }
                        else if (func_name === 'RegSetValueExW') {
                            call_info.args.push({name: 'lpValueName', value: args[1].readUtf16String()});
                            call_info.args.push({name: 'dwType', value: args[2].toInt32()});
                        }
                        else if (func_name === 'InternetConnectW') {
                            call_info.args.push({name: 'lpszServerName', value: args[1].readUtf16String()});
                            call_info.args.push({name: 'nServerPort', value: args[2].toInt32()});
                        }
                        else if (func_name === 'connect') {
                            // Parse sockaddr structure
                            var sa_family = args[1].readU16();
                            if (sa_family === 2) {  // AF_INET
                                var port = (args[1].add(2).readU8() << 8) | args[1].add(3).readU8();
                                var ip = args[1].add(4).readU8() + '.' +
                                        args[1].add(5).readU8() + '.' +
                                        args[1].add(6).readU8() + '.' +
                                        args[1].add(7).readU8();
                                call_info.args.push({name: 'address', value: ip + ':' + port});
                            }
                        }
                        else {
                            // Generic arg logging
                            for (var i = 0; i < Math.min(args.length, 4); i++) {
                                call_info.args.push({name: 'arg' + i, value: args[i].toString()});
                            }
                        }
                        
                        send(call_info);
                    },
                    onLeave: function(retval) {
                        send({
                            type: 'return',
                            function: func_name,
                            return_value: retval.toString()
                        });
                    }
                });
                
                console.log('[+] Hooked: ' + module + '!' + func_name);
            }
        }
        
        // Hook all APIs
        """
        
        # Add hooks for each API
        for dll, functions in self.monitored_apis.items():
            for func in functions:
                script_template += f"hook_api('{dll}', '{func}');\n"
        
        return script_template
    
    def on_message(self, message, data):
        """Handle messages from Frida script"""
        
        if message['type'] == 'send':
            payload = message['payload']
            self.api_calls.append(payload)
            
            # Print real-time
            if 'function' in payload:
                print(f"[{payload['timestamp']}] {payload['module']}!{payload['function']}")
                for arg in payload.get('args', []):
                    print(f"    {arg['name']}: {arg['value']}")
    
    def attach(self):
        """Attach to target process"""
        
        try:
            # Attach to process
            self.session = frida.attach(self.target)
            print(f"[+] Attached to process: {self.target}")
            
            # Create and load script
            script_code = self.generate_frida_script()
            self.script = self.session.create_script(script_code)
            self.script.on('message', self.on_message)
            self.script.load()
            
            print("[+] API hooks installed")
            print("[*] Monitoring API calls... (Ctrl+C to stop)\n")
            
            # Keep script running
            sys.stdin.read()
            
        except KeyboardInterrupt:
            print("\n[*] Stopping monitor...")
            self.save_results()
        except Exception as e:
            print(f"[!] Error: {e}")
    
    def save_results(self):
        """Save logged API calls"""
        
        output_file = f"api_calls_{self.target}_{datetime.now().strftime('%Y%m%d_%H%M%S')}.json"
        
        with open(output_file, 'w') as f:
            json.dump(self.api_calls, f, indent=2)
        
        print(f"[+] Logged {len(self.api_calls)} API calls to {output_file}")
        
        # Generate summary
        self.print_summary()
    
    def print_summary(self):
        """Print summary of API activity"""
        
        print("\n" + "="*60)
        print("API CALL SUMMARY")
        print("="*60)
        
        # Count calls per API
        api_counts = {}
        for call in self.api_calls:
            if 'function' in call:
                func = call['function']
                api_counts[func] = api_counts.get(func, 0) + 1
        
        # Sort by count
        sorted_apis = sorted(api_counts.items(), key=lambda x: x[1], reverse=True)
        
        print("\nTop API Calls:")
        for api, count in sorted_apis[:20]:
            print(f"    {api}: {count}")
        
        # Highlight suspicious activity
        suspicious = []
        
        for call in self.api_calls:
            if 'function' not in call:
                continue
            
            func = call['function']
            
            # Check for suspicious patterns
            if func == 'CreateRemoteThread':
                suspicious.append("Process injection detected")
            elif func == 'RegSetValueExW':
                for arg in call['args']:
                    if 'Run' in arg.get('value', ''):
                        suspicious.append("Persistence via Run key")
            elif func == 'CryptEncrypt':
                suspicious.append("Encryption activity")
            elif func == 'DeleteFileW':
                suspicious.append("File deletion")
        
        if suspicious:
            print("\n[!] Suspicious Activities:")
            for activity in set(suspicious):
                print(f"    - {activity}")

def main():
    if len(sys.argv) < 2:
        print("Usage: python3 api_monitor.py <process_name_or_pid>")
        sys.exit(1)
    
    target = sys.argv[1]
    
    # Convert to int if PID
    try:
        target = int(target)
    except:
        pass
    
    monitor = APIMonitor(target)
    monitor.attach()

if __name__ == "__main__":
    main()
```

**Usage:**
```bash
# Monitor by process name
python3 api_monitor.py malware.exe

# Monitor by PID
python3 api_monitor.py 1234

# Output shows real-time API calls
# Saves complete log to JSON file
```

---

# 5. Threat Hunting Strategies

## A. Proactive Threat Hunting

### **Hunt Methodology:**

Hypothesis Generation:

"Attackers are using PowerShell for lateral movement"
"Malware persists via scheduled tasks"
"Data exfiltration over DNS"


Data Collection:

Event logs
Network traffic
File system artifacts
Memory dumps
Registry snapshots


Analysis:

Look for anomalies
Compare to baseline
Correlate events


Response:

Investigate findings
Contain threats
Update defenses


Documentation:

Record findings
Share IOCs
Update playbooks




### **PowerShell Hunting:**
```powershell
# Hunt for encoded PowerShell commands
Get-WinEvent -FilterHashtable @{LogName='Microsoft-Windows-PowerShell/Operational'; ID=4104} |
    Where-Object {$_.Message -like "*-enc*" -or $_.Message -like "*-EncodedCommand*"} |
    Select-Object TimeCreated, Message |
    Export-Csv "encoded_powershell.csv"

# Hunt for suspicious downloads
Get-WinEvent -FilterHashtable @{LogName='Microsoft-Windows-PowerShell/Operational'; ID=4104} |
    Where-Object {$_.Message -like "*DownloadString*" -or $_.Message -like "*DownloadFile*"} |
    Select-Object TimeCreated, Message

# Hunt for Invoke-Expression usage
Get-WinEvent -FilterHashtableContinue1:27 AM@{LogName='Microsoft-Windows-PowerShell/Operational'; ID=4104} |
Where-Object {$.Message -like "IEX" -or $.Message -like "Invoke-Expression"}

### **Registry Persistence Hunt:**
```powershell
# Common persistence locations
$persistence_keys = @(
    "HKLM:\Software\Microsoft\Windows\CurrentVersion\Run",
    "HKLM:\Software\Microsoft\Windows\CurrentVersion\RunOnce",
    "HKCU:\Software\Microsoft\Windows\CurrentVersion\Run",
    "HKCU:\Software\Microsoft\Windows\CurrentVersion\RunOnce",
    "HKLM:\Software\Microsoft\Windows\CurrentVersion\Explorer\Shell Folders",
    "HKLM:\Software\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders"
)

foreach ($key in $persistence_keys) {
    if (Test-Path $key) {
        Write-Host "`n[*] Checking: $key"
        Get-ItemProperty $key | Format-List
    }
}

# Check for new/modified entries
# Compare against known good baseline
```

### **Network Threat Hunting:**
```bash
# Hunt for beaconing (regular intervals)
# Zeek/Bro logs analysis

#!/bin/bash
# analyze_beaconing.sh

# Extract connection intervals
cat conn.log | zeek-cut id.orig_h id.resp_h id.resp_p ts |
    awk '{
        key = $1" "$2" "$3
        if (last[key]) {
            interval = $4 - last[key]
            print key, interval
        }
        last[key] = $4
    }' |
    awk '{
        # Look for regular intervals (±10% variance)
        count[$1" "$2" "$3" "int($4/60)*60]++
    }
    END {
        for (conn in count) {
            if (count[conn] > 10) {  # 10+ connections at same interval
                print conn, count[conn], "connections"
            }
        }
    }'

# Hunt for DGA (Domain Generation Algorithm) domains
cat dns.log | zeek-cut query |
    awk '
    {
        # Calculate entropy
        len = length($1)
        for (i=1; i<=len; i++) {
            char = substr($1, i, 1)
            freq[char]++
        }
        
        entropy = 0
        for (c in freq) {
            p = freq[c] / len
            entropy -= p * log(p) / log(2)
        }
        
        # High entropy = possible DGA
        if (entropy > 3.5) {
            print $1, entropy
        }
        
        delete freq
    }'

# Hunt for data exfiltration
# Large outbound transfers
cat conn.log | zeek-cut id.orig_h id.resp_h orig_bytes |
    awk '$3 > 10000000 {print $1, $2, $3/1024/1024 " MB"}' |
    sort -k3 -rn
```

## B. YARA Hunt Rules

### **Memory Hunting:**
```yara
rule InMemory_Cobalt_Strike
{
    meta:
        description = "Detects Cobalt Strike in memory"
        author = "Threat Hunter"
        
    strings:
        // Beacon config in memory
        $config = { 00 01 00 01 00 02 00 00 00 }
        
        // Common Beacon functions
        $func1 = "ReflectiveLoader" ascii
        $func2 = "beacon.dll" wide
        
        // Sleep mask
        $sleep = { 48 89 5C 24 08 48 89 74 24 10 }
        
    condition:
        any of them
}

rule InMemory_Mimikatz
{
    meta:
        description = "Detects Mimikatz in memory"
        
    strings:
        $str1 = "sekurlsa::logonpasswords" ascii wide
        $str2 = "gentilkiwi" ascii wide
        $str3 = "benjamin@gentilkiwi.com" ascii wide
        $str4 = "kuhl_m_sekurlsa_" ascii
        
    condition:
        2 of them
}

rule Suspicious_PowerShell_Memory
{
    meta:
        description = "Suspicious PowerShell patterns in memory"
        
    strings:
        $amsi_bypass1 = "AmsiScanBuffer" ascii wide
        $amsi_bypass2 = "amsiInitFailed" ascii wide
        
        $download1 = "System.Net.WebClient" ascii wide
        $download2 = "DownloadString" ascii wide
        $download3 = "DownloadData" ascii wide
        
        $exec1 = "Invoke-Expression" ascii wide
        $exec2 = "IEX" ascii wide
        
        $reflection = "System.Reflection.Assembly" ascii wide
        
    condition:
        (any of ($amsi*)) or
        (2 of ($download*) and any of ($exec*)) or
        ($reflection and any of ($download*))
}
```

### **Using YARA for Hunting:**
```bash
#!/bin/bash
# yara_hunt.sh - Hunt across system

echo "[*] YARA Threat Hunt"
echo "===================="

# Hunt in running processes
echo "[*] Scanning process memory..."
for pid in $(ps aux | awk '{print $2}' | tail -n +2); do
    result=$(yara -p $pid hunt_rules.yara /proc/$pid/mem 2>/dev/null)
    if [ -n "$result" ]; then
        echo "[!] Match in PID $pid:"
        echo "$result"
    fi
done

# Hunt in common malware locations
echo "[*] Scanning file system..."
locations=(
    "/tmp"
    "/var/tmp"
    "$HOME/.config"
    "$HOME/Downloads"
    "/dev/shm"
)

for loc in "${locations[@]}"; do
    echo "[*] Scanning: $loc"
    yara -r hunt_rules.yara "$loc" 2>/dev/null
done

# Hunt in recent files
echo "[*] Scanning recently modified files..."
find / -type f -mtime -1 2>/dev/null | while read file; do
    yara hunt_rules.yara "$file" 2>/dev/null
done
```

## C. Timeline Analysis

### **Super Timeline Creation:**
```bash
#!/bin/bash
# create_timeline.sh

echo "[*] Creating forensic timeline..."

# Use log2timeline (plaso)
log2timeline.py \
    --storage-file timeline.plaso \
    /path/to/evidence

# Create timeline
psort.py \
    -o l2tcsv \
    -w timeline.csv \
    timeline.plaso

# Filter timeline
psort.py \
    -o l2tcsv \
    -w filtered_timeline.csv \
    timeline.plaso \
    "date > '2024-01-01 00:00:00' AND date < '2024-01-02 00:00:00'"

echo "[+] Timeline created: timeline.csv"
```

### **Timeline Analysis Script:**
```python
#!/usr/bin/env python3
"""
Timeline Analysis for Incident Response
"""

import pandas as pd
import matplotlib.pyplot as plt
from datetime import datetime, timedelta

class TimelineAnalyzer:
    def __init__(self, timeline_file):
        self.df = pd.read_csv(timeline_file)
        self.df['timestamp'] = pd.to_datetime(self.df['timestamp'])
    
    def find_anomalies(self, time_window='1H'):
        """Find unusual activity spikes"""
        
        # Group by time window
        activity = self.df.groupby(pd.Grouper(key='timestamp', freq=time_window)).size()
        
        # Calculate statistics
        mean = activity.mean()
        std = activity.std()
        
        # Find anomalies (3 sigma)
        anomalies = activity[activity > mean + (3 * std)]
        
        print(f"[+] Found {len(anomalies)} anomalous time periods:")
        for ts, count in anomalies.items():
            print(f"    {ts}: {count} events (avg: {mean:.2f})")
            
            # Show events in this window
            window_events = self.df[
                (self.df['timestamp'] >= ts) &
                (self.df['timestamp'] < ts + pd.Timedelta(time_window))
            ]
            
            print(f"    Top activities:")
            for activity_type in window_events['type'].value_counts()[:5].items():
                print(f"      {activity_type[0]}: {activity_type[1]}")
        
        return anomalies
    
    def pivot_analysis(self, ioc):
        """Pivot from known IOC"""
        
        print(f"\n[*] Pivoting from IOC: {ioc}")
        
        # Find events related to IOC
        related = self.df[self.df['description'].str.contains(ioc, case=False, na=False)]
        
        print(f"[+] Found {len(related)} related events")
        
        # Get time range
        if not related.empty:
            first = related['timestamp'].min()
            last = related['timestamp'].max()
            
            print(f"    Time range: {first} to {last}")
            
            # Get context (events around this time)
            context_start = first - timedelta(hours=1)
            context_end = last + timedelta(hours=1)
            
            context = self.df[
                (self.df['timestamp'] >= context_start) &
                (self.df['timestamp'] <= context_end)
            ]
            
            print(f"\n[*] Context ({len(context)} events):")
            for _, event in context.iterrows():
                print(f"    [{event['timestamp']}] {event['type']}: {event['description']}")
        
        return related
    
    def lateral_movement_detection(self):
        """Detect lateral movement patterns"""
        
        print("\n[*] Hunting for lateral movement...")
        
        # Look for authentication events
        auth_events = self.df[self.df['type'].str.contains('auth|login|logon', case=False, na=False)]
        
        # Group by user and host
        pivots = auth_events.groupby(['user', 'host']).size().reset_index(name='count')
        
        # Users accessing multiple hosts
        lateral = pivots[pivots.groupby('user')['host'].transform('nunique') > 3]
        
        if not lateral.empty:
            print("[!] Potential lateral movement detected:")
            for user in lateral['user'].unique():
                hosts = lateral[lateral['user'] == user]['host'].tolist()
                print(f"    User '{user}' accessed {len(hosts)} hosts:")
                for host in hosts[:10]:  # Top 10
                    print(f"      - {host}")
        else:
            print("[+] No obvious lateral movement detected")
    
    def visualize_timeline(self):
        """Create timeline visualization"""
        
        # Activity by hour
        hourly = self.df.groupby(self.df['timestamp'].dt.hour).size()
        
        plt.figure(figsize=(12, 6))
        hourly.plot(kind='bar')
        plt.title('Activity by Hour of Day')
        plt.xlabel('Hour')
        plt.ylabel('Event Count')
        plt.tight_layout()
        plt.savefig('timeline_hourly.png')
        print("[+] Saved: timeline_hourly.png")
        
        # Activity by day
        daily = self.df.groupby(self.df['timestamp'].dt.date).size()
        
        plt.figure(figsize=(14, 6))
        daily.plot(kind='line', marker='o')
        plt.title('Activity by Day')
        plt.xlabel('Date')
        plt.ylabel('Event Count')
        plt.xticks(rotation=45)
        plt.tight_layout()
        plt.savefig('timeline_daily.png')
        print("[+] Saved: timeline_daily.png")

# Usage:
analyzer = TimelineAnalyzer('timeline.csv')
analyzer.find_anomalies('1H')
analyzer.pivot_analysis('malware.exe')
analyzer.lateral_movement_detection()
analyzer.visualize_timeline()
```

## D. Threat Intelligence Integration

### **IOC Enrichment:**
```python
#!/usr/bin/env python3
"""
IOC Enrichment and Threat Intel Integration
"""

import requests
import json
import time

class ThreatIntel:
    def __init__(self, vt_api_key=None, ot_api_key=None):
        self.vt_key = vt_api_key
        self.ot_key = ot_api_key
    
    def virustotal_lookup(self, ioc, ioc_type='hash'):
        """Query VirusTotal for IOC"""
        
        if not self.vt_key:
            return None
        
        if ioc_type == 'hash':
            url = f"https://www.virustotal.com/api/v3/files/{ioc}"
        elif ioc_type == 'ip':
            url = f"https://www.virustotal.com/api/v3/ip_addresses/{ioc}"
        elif ioc_type == 'domain':
            url = f"https://www.virustotal.com/api/v3/domains/{ioc}"
        else:
            return None
        
        headers = {"x-apikey": self.vt_key}
        
        try:
            response = requests.get(url, headers=headers)
            
            if response.status_code == 200:
                data = response.json()
                
                result = {
                    'ioc': ioc,
                    'type': ioc_type,
                    'malicious': 0,
                    'suspicious': 0,
                    'harmless': 0,
                    'undetected': 0
                }
                
                if 'data' in data and 'attributes' in data['data']:
                    stats = data['data']['attributes'].get('last_analysis_stats', {})
                    result.update(stats)
                
                return result
            else:
                return {'error': response.status_code}
                
        except Exception as e:
            return {'error': str(e)}
        
        time.sleep(15)  # Rate limit
    
    def otx_lookup(self, ioc):
        """Query AlienVault OTX"""
        
        if not self.ot_key:
            return None
        
        url = f"https://otx.alienvault.com/api/v1/indicators/file/{ioc}/general"
        headers = {"X-OTX-API-KEY": self.ot_key}
        
        try:
            response = requests.get(url, headers=headers)
            if response.status_code == 200:
                return response.json()
        except:
            pass
        
        return None
    
    def enrich_iocs(self, ioc_list):
        """Enrich list of IOCs"""
        
        enriched = []
        
        for ioc in ioc_list:
            print(f"[*] Enriching: {ioc['value']}")
            
            # Determine type
            ioc_type = ioc.get('type', 'hash')
            
            # VT lookup
            vt_result = self.virustotal_lookup(ioc['value'], ioc_type)
            
            if vt_result:
                ioc['virustotal'] = vt_result
                
                if vt_result.get('malicious', 0) > 0:
                    print(f"    [!] Malicious: {vt_result['malicious']} detections")
            
            enriched.append(ioc)
            time.sleep(1)
        
        return enriched
    
    def export_to_misp(self, iocs, misp_url, misp_key):
        """Export IOCs to MISP"""
        
        headers = {
            "Authorization": misp_key,
            "Accept": "application/json",
            "Content-Type": "application/json"
        }
        
        # Create event
        event = {
            "Event": {
                "info": f"Automated IOC Export - {time.strftime('%Y-%m-%d')}",
                "threat_level_id": "2",
                "analysis": "1",
                "distribution": "1"
            }
        }
        
        # Add IOCs as attributes
        for ioc in iocs:
            attribute = {
                "type": ioc['type'],
                "value": ioc['value'],
                "category": "Network activity",
                "to_ids": True
            }
            
            if 'virustotal' in ioc:
                attribute['comment'] = f"VT: {ioc['virustotal'].get('malicious', 0)} detections"
            
            event["Event"].setdefault("Attribute", []).append(attribute)
        
        # Post to MISP
        response = requests.post(
            f"{misp_url}/events/add",
            headers=headers,
            json=event,
            verify=False
        )
        
        if response.status_code == 200:
            print("[+] IOCs exported to MISP")
        else:
            print(f"[!] Error: {response.status_code}")

# Usage:
ti = ThreatIntel(vt_api_key="YOUR_VT_KEY")

iocs = [
    {'type': 'hash', 'value': 'abc123...'},
    {'type': 'ip', 'value': '192.168.1.1'},
    {'type': 'domain', 'value': 'evil.com'}
]

enriched = ti.enrich_iocs(iocs)
```

---

This completes the comprehensive deep dive into all five advanced topics. You now have:

1. **Complete tool setup guides** for CAPE, Ghidra, IDA, Volatility, x64dbg, and YARA
2. **Detailed malware family analysis** for Emotet, Cobalt Strike, and ransomware
3. **Advanced debugging techniques** including anti-debug bypass and injection detection
4. **Custom analysis scripts** for automation and API monitoring
5. **Threat hunting strategies** with timeline analysis and threat intelligence integration
