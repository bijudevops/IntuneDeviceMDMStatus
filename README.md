# Intune Device Status Checker

A comprehensive PowerShell script to check if a Windows device is managed by Microsoft Intune and provide detailed status information.

## Features

- **Comprehensive Checks**: Examines multiple indicators to determine Intune management status
- **Clear Status Messages**: Provides easy-to-understand results with color-coded output
- **Confidence Scoring**: Calculates a percentage-based confidence score
- **Detailed Reporting**: Optional detailed mode for troubleshooting
- **Professional Output**: Clean, formatted console output with helpful recommendations

## Requirements

- **Windows PowerShell 5.1** or later
- **Windows 10/11** or **Windows Server 2016** and later

## Usage

### Basic Usage
```powershell
# Run with default settings
.\Check-IntuneDeviceStatus.ps1
```

### Detailed Mode
```powershell
# Run with detailed information for each check
.\Check-IntuneDeviceStatus.ps1 -Detailed
```

### Execution Policy
If you encounter execution policy restrictions, you may need to adjust the policy:
```powershell
# Check current policy
Get-ExecutionPolicy

# Set policy for current user (if needed)
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

## What the Script Checks

The script performs 7 comprehensive checks:

1. **Intune Management Extension Service** - Checks if the IME service exists and is running
2. **Intune Management Extension Registry Keys** - Verifies IME registry configuration
3. **Intune Management Extension Scheduled Tasks** - Looks for IME maintenance tasks
4. **Device Enrollment Status** - Checks if the device is enrolled in Intune
5. **Intune Policies Registry** - Examines policy management configuration
6. **Intune Client Certificates** - Looks for Intune-related certificates
7. **Intune Management Extension Files** - Verifies IME installation files

## Output Interpretation

### Status Levels
- **🟢 DEVICE IS INTUNE MANAGED** (80-100% confidence)
  - All major Intune components are present and functional
  - Device is properly managed by Intune

- **🟡 DEVICE LIKELY INTUNE MANAGED** (50-79% confidence)
  - Some Intune components are present
  - May have partial management or configuration issues

- **🔴 DEVICE IS NOT INTUNE MANAGED** (0-49% confidence)
  - Few or no Intune components found
  - Device is not managed by Intune

### Confidence Scoring
The script calculates a confidence percentage based on the number of positive indicators found:
- **High Confidence (80-100%)**: Device is definitely Intune managed
- **Medium Confidence (50-79%)**: Device likely has some Intune management
- **Low Confidence (0-49%)**: Device is not Intune managed

## Sample Output

```
===============================================
    Intune Device Management Status Checker
===============================================

1. Checking Intune Management Extension Service...
   ✓ Intune Management Extension service found
   ✓ Service is running

2. Checking Intune Management Extension Registry Keys...
   ✓ Intune Management Extension registry keys found

3. Checking Intune Management Extension Scheduled Tasks...
   ✓ Found 3 Intune scheduled tasks

4. Checking Device Enrollment Status...
   ✓ Device is enrolled in Intune
   ✓ Enrollment ID: 12345678-1234-1234-1234-123456789012

5. Checking Intune Policies Registry...
   ✓ Found 2 Intune policy providers

6. Checking Intune Client Certificate...
   ✓ Found Intune-related certificates

7. Checking Intune Management Extension Files...
   ✓ Intune Management Extension files found

===============================================
              RESULTS SUMMARY
===============================================

STATUS: DEVICE IS INTUNE MANAGED
Confidence: 100%
Indicators Found: 7 out of 7

Recommendations:
• Device appears to be properly managed by Intune
• All major Intune components are present and functional
```

## Troubleshooting

### Common Issues

1. **"Access Denied" Errors**
   - Check if User Account Control (UAC) is blocking access
   - Some checks may require elevated privileges for full functionality

2. **Execution Policy Restrictions**
   - Use `Set-ExecutionPolicy` to allow script execution
   - Consider using `-ExecutionPolicy Bypass` for one-time execution

3. **Script Not Found**
   - Ensure you're in the correct directory
   - Use full path if running from different location

### When Results Are Unclear

- **Medium confidence results**: Check the Intune portal for device status
- **Missing components**: Verify Intune Management Extension installation
- **Service issues**: Check Windows Event Viewer for IME-related errors

## Integration

The script returns a PowerShell object that can be used in other scripts:

```powershell
$result = .\Check-IntuneDeviceStatus.ps1
if ($result.IsIntuneManaged) {
    Write-Host "Device is Intune managed with $($result.ConfidencePercentage)% confidence"
}
```

## Security Considerations

- The script reads system information and does not modify any settings
- All registry access is read-only
- No network communication is performed
- Some checks may require elevated privileges for full functionality

## Support

For issues or questions:
1. Check the troubleshooting section above
2. Verify PowerShell version and execution policy
3. Check if elevated privileges are needed for specific checks
4. Review Windows Event Viewer for related errors

## Version History

- **v1.0**: Initial release with comprehensive Intune management detection

## License

This script is provided as-is for educational and administrative purposes.
