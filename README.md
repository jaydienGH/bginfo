# BgInfo Deployment Script for Windows Server

## Overview

This PowerShell script automates the download, installation, and configuration of [BgInfo](https://learn.microsoft.com/en-us/sysinternals/downloads/bginfo) on Windows Server editions (2016, 2019, 2022, 2025). It ensures that essential system information is displayed on the desktop background for quick reference by IT staff and administrators.

---

## Features

- ✅ Detects administrative privileges before execution  
- 📂 Creates and cleans the `C:\BgInfo` working directory  
- 🔽 Downloads the latest version of **BgInfo** from Microsoft Sysinternals  
- 🧾 Downloads and applies a predefined `.bgi` configuration file  
- 🛠 Adds a registry key to launch BgInfo at user logon  
- 🚀 Executes BgInfo silently with no license prompt  

---

## Requirements

- Windows OS
- PowerShell 5.1 or later  
- Internet access to download BgInfo and configuration  
- Must be run **as Administrator**

---

## Usage

1. Clone the repository:
   ```bash
   git clone https://github.com/YOUR-USERNAME/BgInfo-Deployment.git
   cd BgInfo-Deployment
   ```

2. Run the script in PowerShell (as Administrator):
   ```powershell
   .\Deploy-BgInfo-WS2016-WS2019-WS2022-WS2025.ps1
   ```

---

## Customizing the Displayed Information

The information shown on the desktop is controlled by the `logon.bgi` file. You can customize it to show or hide specific system information.

### Option 1: Edit Using BgInfo GUI

1. Run BgInfo interactively:
   ```powershell
   C:\BgInfo\Bginfo64.exe
   ```

2. In the GUI, you can:
   - Add or remove fields (e.g., IP Address, Hostname, OS version)
   - Add static text (e.g., department name or asset tag)
   - Customize fonts, colors, and screen position

3. Save the customized layout as:
   ```text
   C:\BgInfo\logon.bgi
   ```

4. Test your configuration:
   ```powershell
   C:\BgInfo\Bginfo64.exe C:\BgInfo\logon.bgi /timer:0 /nolicprompt
   ```

### Option 2: Host Your Own `.bgi` File

If deploying to many systems:

- Save your customized `.bgi` configuration  
- Zip it and host it on an internal server or public URL  
- Replace this line in the script:
   ```powershell
   $logonBgiUrl = "https://tinyurl.com/yxlxbgun"
   ```
   with:
   ```powershell
   $logonBgiUrl = "https://yourdomain.com/files/logonbgi.zip"
   ```

---

## Registry Key for Auto-Start

The script creates the following registry key to auto-launch BgInfo at logon:

```registry
HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Run\BgInfo
```

**Value:**
```text
C:\BgInfo\Bginfo64.exe C:\BgInfo\logon.bgi /timer:0 /nolicprompt
```

This ensures BgInfo runs silently for all users.

---

## Logging and Output

The script outputs progress and status to the PowerShell console:
- ✅ Success messages in Green/Yellow
- ❌ Error messages in Red

---

## Notes

- The script **deletes** all existing contents of `C:\BgInfo` before reinstalling.
- The BgInfo EULA is bypassed with `/nolicprompt` to prevent user interaction.
- Make sure BgInfo is allowed to run by Group Policy or endpoint protection.

---

## Author

**Wim Matthyssen**  
Modified & Packaged by: **Your Name**  
Website: [wmatthyssen.com](https://wmatthyssen.com)  

---

## License

This project is provided "as is" without any warranties or guarantees.  
You may use, modify, and distribute it under the [MIT License](LICENSE).
