# Email Attachment to SharePoint Flow

## Overview

This Power Automate flow automatically monitors an Outlook inbox for emails with specific subject lines, extracts Excel file attachments, and saves them to a SharePoint document library with date-stamped filenames.

## Business Purpose

Automates the collection of recurring reports (HR reports, requisition reports) that are sent via email, ensuring they are automatically stored in a centralized SharePoint location with consistent naming conventions.

## Trigger

**Type:** Scheduled Recurrence

**Frequency:** Twice weekly

**Schedule:** Mondays and Fridays at 10:00 AM India Standard Time (IST)

## Flow Logic

### 1. Retrieve Emails

**Action:** Get emails (V3)

**Connector:** Office 365 Outlook

**Configuration:**
- Folder: Inbox
- Include attachments: Yes
- Fetch only with attachments: Yes
- Fetch only unread: No
- Top: 25 emails
- Importance: Any

### 2. Filter Emails by Subject

**Action:** Condition check

**Logic:** Filters for emails containing specific subject line text

**Supported Subjects:**
- "EE List w/ HR and RE is available"
- "A new version of Pending Requisition Report is available"

### 3. Process Attachments

**Action:** Loop through attachments

**Logic:** Iterates through all attachments in matching emails

### 4. Filter Excel Files

**Action:** Condition check

**Logic:** Only processes files with extensions:
- .xlsx (Excel 2007+)
- .xls (Excel 97-2003)

### 5. Save to SharePoint

**Action:** Create file

**Connector:** SharePoint Online

**Configuration:**
- Saves Excel attachments to designated SharePoint folder
- Filename format: YYYYMMDD_[original_filename].xlsx
- Uses email received date for timestamp
- Content transfer mode: Chunked (for large files)

## File Naming Convention

Files are saved with a date prefix based on the email's received date:

**Format:** `YYYYMMDD_[original_filename]`

**Examples:**
- Original: `Employee_List.xlsx`
- Saved as: `20260603_Employee_List.xlsx`

- Original: `Pending_Requisitions.xls`
- Saved as: `20260606_Pending_Requisitions.xls`

**Benefits:**
- Chronological sorting
- Easy identification of report date
- Prevents filename conflicts
- Maintains original filename context

## Required Connections

### Office 365 Outlook

**Purpose:** Read emails and retrieve attachments from inbox

**Permissions:**
- Read user mail
- Access attachments

### SharePoint Online

**Purpose:** Create files in SharePoint document library

**Permissions:**
- Write access to target SharePoint site
- Edit permissions on target folder

## Email Filtering Criteria

The flow processes emails that match ANY of these subject line patterns:

| Subject Contains | Purpose | Report Type |
|---|---|---|
| "EE List w/ HR and RE is available" | Employee list report | HR Report |
| "A new version of Pending Requisition Report is available" | Requisition tracking | Requisition Report |

**Note:** The flow uses partial matching (contains), so subjects don't need to match exactly.

## Attachment Processing Rules

### Accepted File Types

✅ .xlsx - Excel 2007 and later
✅ .xls - Excel 97-2003
❌ Other formats are ignored (PDF, Word, ZIP, etc.)

### Processing Behavior

- All Excel attachments from matching emails are saved
- Multiple attachments per email are all processed
- Non-Excel attachments are skipped without error
- Emails without attachments are skipped

## SharePoint Configuration

### Required Setup

- **SharePoint Site:** Specify your SharePoint site URL
- **Document Library:** Choose target library
- **Folder Path:** Define destination folder path
- **Permissions:** Ensure flow has write access

### Recommended Folder Structure

```
SharePoint Site/
└── HR Global/
    └── Reqs/
        ├── 20260603_Employee_List.xlsx
        ├── 20260603_Pending_Requisitions.xlsx
        ├── 20260606_Employee_List.xlsx
        └── ...
```

## Configuration Steps

### 1. Import the Flow
1. Download the flow package
2. Navigate to Power Automate portal
3. Go to **My flows** → **Import** → **Import Package (Legacy)**
4. Upload the flow package file

### 2. Configure Connections
During import, map connections:
- **Office 365 Outlook**: Select or create connection
- **SharePoint Online**: Select or create connection

### 3. Update Flow Parameters

#### Email Configuration
1. Open the **Get emails (V3)** action
2. Configure:
   - **Folder**: Keep as "Inbox" or select different folder
   - **Top**: Adjust number of emails to retrieve (default: 25)
   - **Include Attachments**: Keep as Yes
   - **Fetch Only With Attachment**: Keep as Yes

#### Subject Line Filters
1. Open the **Condition** action (after "Apply to each")
2. Update subject line patterns to match your email subjects:
   ```
   contains(@items('Apply_to_each')?['subject'], 'YOUR_SUBJECT_TEXT')
   ```

#### SharePoint Destination
1. Open the **Create file** action
2. Configure:
   - **Site Address**: Select your SharePoint site
   - **Folder Path**: Choose destination folder
   - Keep filename expression unchanged (includes date prefix)

### 4. Adjust Schedule (Optional)
1. Edit the **Recurrence** trigger
2. Modify as needed:
   - **Frequency**: Week
   - **Interval**: 1
   - **Days**: Monday, Friday (or customize)
   - **Hours**: 10 (or choose different time)
   - **Time Zone**: Adjust if not using IST

### 5. Test the Flow
1. Click **Test** → **Manually**
2. Send a test email with Excel attachment matching subject criteria
3. Verify file is created in SharePoint with correct naming
4. Check that date prefix matches email received date

### 6. Enable the Flow
1. Click **Turn on** to activate
2. Flow will run on scheduled days at configured time

## Testing

### Test Scenarios

#### Test 1: Valid Email with Excel Attachment
1. Send email with subject: "EE List w/ HR and RE is available"
2. Attach an Excel file (.xlsx or .xls)
3. Run flow manually
4. **Expected**: File saved to SharePoint with date prefix

#### Test 2: Multiple Attachments
1. Send email with matching subject
2. Attach multiple Excel files
3. Run flow
4. **Expected**: All Excel files saved individually

#### Test 3: Mixed Attachments
1. Send email with matching subject
2. Attach Excel file and PDF
3. Run flow
4. **Expected**: Only Excel file saved, PDF ignored

#### Test 4: Non-matching Subject
1. Send email with different subject
2. Attach Excel file
3. Run flow
4. **Expected**: Email ignored, no files saved

#### Test 5: No Attachments
1. Send email with matching subject
2. No attachments
3. Run flow
4. **Expected**: Email processed but no action taken

## Monitoring & Maintenance

### Run History
- Check flow run history in Power Automate portal
- Review success/failure rates
- Investigate any failed runs

### Common Success Indicators
- ✅ Flow runs on schedule (Mondays and Fridays at 10 AM IST)
- ✅ Files appear in SharePoint with correct date prefix
- ✅ Only Excel files from matching emails are saved
- ✅ No duplicate files created

### Regular Maintenance Tasks

**Weekly:**
- Review run history for errors
- Verify files are being saved correctly

**Monthly:**
- Check SharePoint folder for file accumulation
- Archive or move old files if needed
- Review subject line filters for accuracy

**Quarterly:**
- Verify email patterns haven't changed
- Update subject line filters if needed
- Review and optimize file retention

## Troubleshooting

### Flow Not Running on Schedule
- Verify flow is turned on
- Check recurrence schedule settings
- Confirm flow owner's license is active
- Review time zone configuration

### No Files Being Saved
- Verify emails with matching subjects exist
- Check that emails have Excel attachments
- Confirm SharePoint connection is authenticated
- Verify folder path is correct

### Files Not Found in SharePoint
- Check destination folder path in "Create file" action
- Verify flow has write permissions to SharePoint
- Review run history for specific error messages
- Check if files are in a different folder

### Incorrect Date Prefix
- Verify email received date is available
- Check date format expression: `formatDateTime(..., 'yyyyMMdd')`
- Ensure time zone is set correctly

### Permission Errors
- Re-authenticate SharePoint connection
- Verify write permissions on target folder
- Check site-level permissions
- Confirm connection uses correct service account

### Large File Failures
- Files >30 MB may timeout
- Chunked transfer mode is already enabled
- Consider splitting large files
- Check SharePoint file size limits

### Duplicate Files
- SharePoint will overwrite files with same name
- Different received dates create different filenames
- Manual renames may cause confusion
- Consider implementing versioning in SharePoint

## File Retention & Management

### Recommended Practices
- **Monthly Archive**: Move files older than 30 days to archive folder
- **Quarterly Cleanup**: Review and delete files no longer needed
- **Version Control**: Enable SharePoint versioning on the library
- **Backup**: Ensure SharePoint backup policies include this folder

### Storage Considerations
- Monitor folder size and file count
- Set up SharePoint storage alerts
- Plan for long-term archival strategy
- Document file retention policies

## Security & Compliance

### Data Handling
- Attachments are transferred securely via HTTPS
- No intermediate storage of attachment data
- Files inherit SharePoint security settings
- Access controlled by SharePoint permissions

### Audit Trail
- Power Automate run history provides audit log
- SharePoint tracks file creation and modifications
- Email received date preserved in filename
- Created/modified metadata available in SharePoint

### Best Practices
- Use service account for connections (not personal account)
- Limit SharePoint folder access to authorized users
- Review flow permissions regularly
- Monitor for unauthorized access

## Limitations & Known Issues

### Current Limitations
- **Email Volume**: Processes maximum 25 emails per run
- **File Types**: Only Excel files (.xlsx, .xls) supported
- **Subject Matching**: Case-sensitive partial match
- **Duplicate Filenames**: Same email processed multiple times creates same filename (overwrites)

### Known Issues
- Emails received between runs may be missed if inbox >25 emails
- Time zone differences may affect date prefix
- Very large Excel files (>30 MB) may timeout

### Workarounds
- Increase "top" parameter if >25 emails expected
- Run flow more frequently if needed
- Compress large files before emailing
- Add "unread only" filter if preferred

## Performance Optimization

### Current Configuration
- Fetches up to 25 emails per run
- Runs twice weekly (Monday, Friday)
- Uses chunked transfer for large files

### Optimization Options
- **Increase frequency** for more timely processing
- **Add folder filter** to reduce email volume
- **Enable unread filter** to avoid reprocessing
- **Adjust email count** based on volume

## Dependencies

### Required Licenses
- Microsoft 365 with Power Automate
- Exchange Online (Office 365 Outlook)
- SharePoint Online

### Required Permissions
- **Outlook**: Read mail, read attachments
- **SharePoint**: Write files, create items

### Service Dependencies
- Office 365 Outlook service availability
- SharePoint Online service availability
- Power Automate platform availability

## Version History
- **v1.0**: Initial release with scheduled processing and date-stamped filenames

## Related Flows
This flow works well in combination with:
- Email notification flows
- SharePoint approval workflows
- Report processing automation
- Data extraction flows

## Support

### Getting Help
- Review flow run history for error details
- Check SharePoint permissions
- Verify email subject line matches
- Contact IT automation team for assistance

### Reporting Issues
When reporting issues, include:
- Flow run ID from run history
- Error message (if any)
- Email subject line
- Expected vs. actual behavior

---

**Flow Type:** Scheduled  
**Complexity:** Intermediate  
**Maintenance:** Low  
**Last Updated:** June 3, 2026
