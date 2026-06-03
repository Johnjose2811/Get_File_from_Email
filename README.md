# Get File from Email

Automated workflow that monitors Outlook inbox for emails with specific subject lines, extracts Excel file attachments, and saves them to SharePoint with date-stamped filenames for centralized report management.

## Overview

This repository contains Power Automate flows designed to automate email attachment processing and storage in SharePoint. The primary use case is collecting recurring email reports and organizing them centrally with consistent naming conventions.

## Available Flows

### Email Attachment to SharePoint

**Path:** `flows/email-attachment-to-sharepoint/`

**Purpose:** Automatically extracts Excel attachments from specific emails and saves them to SharePoint with date-stamped filenames for centralized report storage.

**Key Features:**
- Twice-weekly scheduled execution (Monday and Friday)
- Subject line-based email filtering
- Excel file type filtering (.xlsx, .xls)
- Automatic date-prefixed filename (YYYYMMDD_filename)
- Chunked file transfer for large attachments
- SharePoint integration

**Use Case:** Automates the collection and organization of recurring email reports (HR reports, requisition reports, financial reports) by extracting Excel attachments and storing them in SharePoint with consistent naming conventions, eliminating manual download and file management.

**Connectors Used:**
- Office 365 Outlook
- SharePoint Online

**Trigger:** Recurrence (Monday and Friday at 10:00 AM IST)

## Flow Documentation

Each flow includes comprehensive documentation:

1. **definition.json** - Flow definition with all actions and configurations
2. **README.md** - Detailed flow documentation including:
   - Business purpose and overview
   - Trigger configuration
   - Flow logic and action descriptions
   - File naming conventions
   - Email filtering criteria
   - Attachment processing rules
   - SharePoint configuration
   - Configuration steps
   - Testing procedures
   - Troubleshooting guide
   - Performance optimization
   - Security and compliance information

3. **INSTALLATION.md** - Step-by-step installation guide
4. **config.template.json** - Configuration template for reference

## Quick Start

1. Review the [Installation Guide](flows/email-attachment-to-sharepoint/INSTALLATION.md)
2. Follow the step-by-step setup instructions
3. Configure your SharePoint destination and email filters
4. Test the flow with sample emails
5. Enable the flow for scheduled execution

## Repository Structure

```
.
├── README.md                                    (This file)
├── flows/
│   ├── .gitignore
│   └── email-attachment-to-sharepoint/
│       ├── definition.json                      (Flow definition)
│       ├── README.md                            (Flow documentation)
│       ├── INSTALLATION.md                      (Installation guide)
│       └── config.template.json                 (Configuration template)
```

## Requirements

### Services
- Microsoft 365 subscription
- Power Automate license (included in M365)
- Office 365 Outlook (Exchange Online)
- SharePoint Online

### Permissions
- Power Automate maker permissions
- Read access to Outlook mailbox
- Write access to target SharePoint site/folder
- Ability to create connections in Power Automate

## Configuration Overview

### Email Configuration
- **Folder:** Inbox
- **Top Emails:** 25
- **Include Attachments:** Yes
- **Fetch Only With Attachment:** Yes

### Subject Filters
The flow filters for emails containing:
1. "EE List w/ HR and RE is available"
2. "A new version of Pending Requisition Report is available"

### File Type Filters
Only processes Excel files:
- .xlsx (Excel 2007 and later)
- .xls (Excel 97-2003)

### File Naming Convention
Files are saved with date prefix based on email received date:
- Format: `YYYYMMDD_[original_filename]`
- Example: `20260603_Employee_List.xlsx`

### Schedule
- **Frequency:** Weekly
- **Days:** Monday and Friday
- **Time:** 10:00 AM
- **Time Zone:** India Standard Time (IST)

## Support & Troubleshooting

For detailed troubleshooting guidance, configuration help, and best practices, please refer to:
- [Flow README](flows/email-attachment-to-sharepoint/README.md) - Comprehensive flow documentation
- [Installation Guide](flows/email-attachment-to-sharepoint/INSTALLATION.md) - Step-by-step setup and validation

## Version History

- **v1.0** - Initial release with scheduled processing and date-stamped filenames

## License

This flow is provided as-is for use within Microsoft 365 environments.

---

**Last Updated:** June 3, 2026  
**Repository Owner:** Johnjose2811
