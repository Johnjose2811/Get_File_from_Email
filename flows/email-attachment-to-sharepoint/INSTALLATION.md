# Installation Guide - Email Attachment to SharePoint

## Prerequisites

### Required Services

✅ Microsoft 365 subscription
✅ Power Automate license (included in M365)
✅ Office 365 Outlook (Exchange Online)
✅ SharePoint Online

### Required Permissions

✅ Power Automate maker permissions
✅ Read access to Outlook mailbox
✅ Write access to target SharePoint site/folder
✅ Ability to create connections in Power Automate

### Preparation Checklist

- [ ] Identify target SharePoint site
- [ ] Create destination folder in SharePoint
- [ ] Confirm folder permissions (write access)
- [ ] Note exact email subject line patterns
- [ ] Test email account access

## Step-by-Step Installation

### Step 1: Prepare SharePoint Destination

#### Navigate to SharePoint Site

1. Go to your target SharePoint site
2. Navigate to the document library

#### Create Destination Folder (if not exists)

1. Example: HR Global/Reqs/

#### Verify Permissions

1. Ensure you have **Edit** or **Contribute** permissions
2. Verify the service account (flow owner) has access

#### Note the Folder Path

1. Copy the full folder path for configuration
2. Example: `/HR Global/Reqs`

### Step 2: Import the Flow

#### Access Power Automate

1. Go to [flow.microsoft.com](https://flow.microsoft.com)
2. Sign in with your Microsoft 365 account

#### Import Package

1. Click **My flows** in left navigation
2. Click **Import** → **Import Package (Legacy)**
3. Click **Upload**
4. Select the flow package file (`.zip`)

#### Review Import Details

1. Flow name: "Email Attachment to SharePoint"
2. Review resource list

#### Configure Import Settings

1. Under "Related resources", click **Select during import** for each connection

### Step 3: Configure Connections

#### Office 365 Outlook Connection

1. **Select Connection**
   - Click **Select during import** for Office 365 Outlook
   - Choose existing connection OR click **Create new**

2. **Create New Connection** (if needed)
   - Click **Create new**
   - Sign in with your Microsoft 365 account
   - Grant requested permissions
   - Click **Create**

3. **Confirm Selection**
   - Ensure connection is selected
   - Verify account email is correct

#### SharePoint Online Connection

1. **Select Connection**
   - Click **Select during import** for SharePoint
   - Choose existing connection OR click **Create new**

2. **Create New Connection** (if needed)
   - Click **Create new**
   - Sign in with same Microsoft 365 account
   - Grant requested permissions
   - Click **Create**

3. **Confirm Selection**
   - Ensure connection is selected
   - Verify account is correct

### Step 4: Complete Import

1. **Review All Settings**
   - Verify both connections are configured
   - Check import configuration

2. **Click Import**
   - Wait for import to complete
   - Success message should appear

3. **Open Imported Flow**
   - Click **Open flow** from success message
   - OR go to My flows and open "Email Attachment to SharePoint"

### Step 5: Configure Flow Settings

#### Update Email Retrieval Settings

1. **Open Flow for Editing**
   - Click **Edit** in the top menu

2. **Locate "Get emails (V3)" Action**
   - First action after the trigger

3. **Configure Parameters** (if needed)
   - **Folder**: Keep as "Inbox" or select different folder
   - **Top**: 25 (adjust if expecting more emails)
   - **Include Attachments**: Yes (leave checked)
   - **Fetch Only With Attachment**: Yes (leave checked)
   - **Fetch Only Unread**: No (or change to Yes if preferred)

#### Update Subject Line Filters

1. **Locate First "Condition" Action**
   - Inside "Apply to each" loop

2. **Review Subject Filters**
   - Current filters:
     - "EE List w/ HR and RE is available"
     - "A new version of Pending Requisition Report is available"

3. **Modify If Needed**
   - Click on condition to edit
   - Update subject text to match your emails
   - Add additional OR conditions if needed
   - Keep the `contains` function for partial matching

**Example of adding a new subject filter:**
```
OR contains(items('Apply_to_each')?['subject'], 'Your New Subject Text')
```

#### Configure SharePoint Destination

1. **Locate "Create file" Action**
   - Inside nested loops
   - After second condition (Excel file check)

2. **Click on "Create file" Action**

3. **Configure SharePoint Settings**
   - **Site Address**: Select your SharePoint site from dropdown
   - **Folder Path**: Type or select your folder path
     - Example: `/HR Global/Reqs`
   - **File Name**: Leave as-is (includes date prefix expression)
   - **File Content**: Leave as-is (attachment content)

4. **Verify Filename Expression**
   - Should look like:
     ```
     concat(
       formatDateTime(items('Apply_to_each')?['receivedDateTime'], 'yyyyMMdd'),
       '_',
       items('Apply_to_each_2')?['name']
     )
     ```
   - This creates: `YYYYMMDD_filename.xlsx`

### Step 6: Configure Schedule

1. **Click on "Recurrence" Trigger**
   - At the top of the flow

2. **Review Schedule**
   - **Interval**: 1
   - **Frequency**: Week
   - **Time Zone**: India Standard Time (or change as needed)
   - **On these days**: Monday, Friday
   - **At these hours**: 10

3. **Modify Schedule** (if needed)
   - Change days of week
   - Change time (0-23 hour format)
   - Change time zone
   - Change frequency (Day, Week, Month)

### Step 7: Save the Flow

1. **Click "Save"** (top right)
   - Wait for save to complete
   - Fix any validation errors if they appear

2. **Review Flow Checker**
   - Look for warnings or errors
   - Address any issues before testing

### Step 8: Test the Flow

#### Prepare Test Email

1. **Send Test Email** to your Outlook inbox
   - Use a subject line that matches your filter
   - Attach an Excel file (.xlsx or .xls)
   - Example subject: "EE List w/ HR and RE is available"

2. **Wait a Moment**
   - Ensure email arrives in inbox

#### Run Manual Test

1. **Click "Test"** (top right)

2. **Select Test Type**
   - Choose **Manually**
   - Click **Test**

3. **Trigger the Flow**
   - Click **Run flow**
   - Click **Done**

4. **Monitor Execution**
   - Watch each action complete
   - Green checkmarks indicate success
   - Review any errors

5. **Verify Results**
   - Go to your SharePoint folder
   - Look for the file with date prefix
   - Example: `20260603_Employee_List.xlsx`
   - Verify content matches email attachment

#### Validate Output

- [ ] File created in correct SharePoint folder
- [ ] Filename has date prefix (YYYYMMDD_)
- [ ] File content matches attachment
- [ ] Date in filename matches email received date
- [ ] No errors in run history

### Step 9: Enable the Flow

1. **Turn On the Flow**
   - Click **Turn on** (top menu)
   - Status should change to "On"

2. **Verify Status**
   - Check that flow is enabled
   - Note next scheduled run time

### Step 10: Monitor Initial Runs

1. **Wait for Scheduled Run**
   - First run: Next Monday or Friday at 10:00 AM IST

2. **Check Run History**
   - Go to flow details page
   - Click **Run history** tab
   - Verify successful runs

3. **Verify Files in SharePoint**
   - Check destination folder for new files
   - Ensure naming convention is correct

## Post-Installation Checklist

- [ ] Flow imported successfully
- [ ] Both connections configured and authenticated
- [ ] SharePoint site and folder selected
- [ ] Subject line filters updated (if needed)
- [ ] Schedule configured appropriately
- [ ] Test email sent and processed successfully
- [ ] File appeared in SharePoint with correct naming
- [ ] Flow is turned on
- [ ] No errors in test run
- [ ] Run history reviewed

## Validation Tests

### Test 1: Single Attachment
- Send email with matching subject + 1 Excel file
- Expected: 1 file in SharePoint

### Test 2: Multiple Attachments
- Send email with matching subject + 3 Excel files
- Expected: 3 files in SharePoint

### Test 3: Non-Excel Attachment
- Send email with matching subject + 1 PDF
- Expected: No files created (PDF ignored)

### Test 4: Mixed Attachments
- Send email with matching subject + Excel + PDF + Word
- Expected: Only Excel file saved

### Test 5: Wrong Subject
- Send email with different subject + Excel file
- Expected: No files created (subject doesn't match)

## Troubleshooting Installation Issues

### Import Fails
- **Check**: Power Automate license status
- **Check**: Permissions in environment
- **Solution**: Contact admin for environment access

### Connection Errors
- **Check**: Sign-in credentials
- **Check**: Account has necessary permissions
- **Solution**: Re-authenticate connections

### SharePoint Folder Not Found
- **Check**: Folder path spelling and format
- **Check**: Folder exists in selected site
- **Solution**: Verify folder path or create folder

### Flow Won't Save
- **Check**: All required fields filled
- **Check**: Expressions are valid
- **Solution**: Review flow checker warnings

### Test Run Fails
- **Check**: Test email matches subject filter
- **Check**: Email has Excel attachment
- **Check**: SharePoint permissions
- **Solution**: Review run history error details

## Configuration Tips

### For High Email Volume
- Increase "Top" parameter to 50 or 100
- Run more frequently (daily instead of twice weekly)
- Consider filtering by unread emails only

### For Multiple Report Types
- Add more subject line conditions
- Use descriptive folder names
- Consider separate flows for different report types

### For Better Organization
- Include report type in filename
- Create subfolders by month or quarter
- Enable SharePoint versioning

## Next Steps

After successful installation:

1. **Monitor** first few scheduled runs
2. **Document** any customizations made
3. **Train** team members on how it works
4. **Set up** alerts for failed runs (optional)
5. **Plan** for file retention and archival

## Support

If you encounter issues during installation:
- Review troubleshooting section
- Check Power Automate documentation
- Contact your IT automation team
- Review flow run history for error details

---

**Estimated Installation Time**: 15-30 minutes  
**Difficulty Level**: Intermediate  
**Prerequisites**: Understanding of email subjects and SharePoint folder structures
