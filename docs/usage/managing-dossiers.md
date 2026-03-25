# Managing Dossiers

## Overview

Dossier management is the core functionality of Dynamics Dossier. This guide explains how to create, manage, and track dossiers effectively within Microsoft Dynamics 365 Business Central.

## What is a Dossier?

A **dossier** is a central container that groups all related information for a specific person, case, or project. Each dossier contains:

- **Basic Information**: Contact details, dates, status
- **Documents**: Files, attachments, correspondence  
- **Activities**: Appointments, tasks, interactions
- **Progress Tracking**: Status updates, milestones
- **Workflow History**: Automated process steps
- **Comments and Notes**: Additional context and observations

## Accessing Dossier Management

### Finding Dossiers
1. Use the search function (🔍) in Business Central
2. Type **"Dossiers"** or **"Dossier List"**
3. Select **"QTEAM Dossier List"** to view all dossiers

### Navigation Options
- **Role Center**: Dossier tiles show quick statistics
- **Activities**: Direct access from contact cards
- **Search**: Find specific dossiers by number or contact
- **Filters**: Use built-in filtering for quick access

## Creating a New Dossier

### Method 1: From Dossier List
1. Open **"QTEAM Dossier List"**
2. Click **"New"** from the ribbon
3. The new dossier card opens automatically

### Method 2: From Contact Card  
1. Open a **Contact Card**
2. Navigate to **Actions** → **Dossier** → **Create Dossier**
3. A new dossier is created with the contact as primary contact

### Method 3: Using Quick Create
1. Use **Alt+N** keyboard shortcut from dossier list
2. Quick create dialog opens for basic information entry

## Dossier Card - Core Fields

### General Information Tab

#### Primary Fields
- **No.**: Auto-generated number (based on number series)
- **Primary Contact No.**: The main person this dossier relates to
- **Description**: Brief description of the dossier purpose
- **Dossier Type Code**: Select from configured dossier types
- **Status**: Current dossier state (Open, In Process, Closed)

#### Date Fields
- **Document Date**: Official start date of the dossier
- **Starting Date**: When dossier activities should begin
- **Ending Date**: Expected or actual completion date
- **Date Closed**: Automatically set when status changes to Closed

#### Reference Information
- **External Document No.**: Reference to external systems
- **Your Reference**: Client or external reference number  
- **Our Reference**: Internal reference or case number

### Details Tab

#### Contact Information
- **Primary Contact**: Main person (automatically filled)  
- **Primary Company**: Company associated with the contact
- **Department**: Relevant organizational department
- **Salesperson/Purchaser**: Assigned case manager or coordinator

#### Classification
- **Priority**: High, Normal, Low priority classification
- **Confidentiality Level**: Determines access restrictions
- **Category**: Based on dossier type (Absence, Medical, etc.)

## Dossier Status Management

### Status Flow
```
New → Open → In Process → Closed → Archived
```

### Status Descriptions

| Status | Description | Actions Available |
|--------|-------------|-------------------|
| **New** | Recently created, not yet started | Edit all fields, assign workflows |
| **Open** | Active and in progress | Add content, schedule activities |
| **In Process** | Currently being worked on | Update progress, add documents |
| **Closed** | Completed - no further action needed | View only, reopen if necessary |
| **Archived** | Long-term storage | Read-only access |

### Changing Status
1. Open the dossier card
2. Change the **Status** field
3. System will automatically:
   - Update date fields
   - Trigger workflow actions
   - Send notifications if configured

## Working with Dossier Content

### Dossier Content Overview
The **Dossier Content** page shows all related information:

1. Click **Related** → **Content** from dossier card
2. View tabs for different content types:
   - **Documents**: Attached files and documents
   - **Interactions**: Communication history  
   - **To-Do's**: Tasks and appointments
   - **Comments**: Notes and observations
   - **Progress**: Status updates and milestones

### Adding Documents
1. From dossier card: **Actions** → **Add Document**
2. Choose document source:
   - **Upload File**: Browse and select from computer
   - **Drag & Drop**: Drag files directly into the interface  
   - **From Email**: Import email attachments
   - **SharePoint**: Link to cloud-stored documents

3. Complete document information:
   - **Description**: Document purpose or title
   - **Document Type**: Category (Medical, Administrative, Legal)
   - **Date**: Document date or creation date
   - **Confidential**: Mark sensitive documents

### Managing Interactions  
Track all communications related to the dossier:

1. **Phone Calls**: Log incoming and outgoing calls
2. **Meetings**: Record meeting details and outcomes  
3. **Emails**: Link email correspondence
4. **Letters**: Track postal communications

#### Adding an Interaction
1. From Dossier Content → **Interactions** tab
2. Click **New**
3. Fill interaction details:
   - **Date/Time**: When interaction occurred
   - **Contact**: Person involved in interaction  
   - **Type**: Phone, Meeting, Email, etc.
   - **Subject**: Brief description  
   - **Details**: Comprehensive notes

### Creating To-Do Items
Manage tasks and appointments within the dossier:

1. From Dossier Content → **To-Do's** tab
2. Click **New To-Do**
3. Configure task details:
   - **Description**: What needs to be done
   - **Date**: Due date or appointment date
   - **Time**: Specific time if applicable
   - **Salesperson**: Who is responsible
   - **Contact**: Person involved  
   - **Type**: Task or Appointment

## Workflow Automation

### How Workflows Work
When you create or modify a dossier:

1. **Automatic Detection**: System identifies applicable workflows
2. **Workflow Initialization**: Creates workflow log entry
3. **Task Generation**: Generates to-do items based on workflow steps  
4. **Progress Monitoring**: Tracks completion of workflow steps
5. **Notifications**: Sends automated emails or alerts

### Monitoring Workflow Progress
1. From dossier card: **Related** → **Workflow Log**
2. View workflow execution history:
   - **Completed Steps**: Green checkmarks
   - **Current Step**: In progress indicator
   - **Upcoming Steps**: Scheduled future actions
   - **Overdue Items**: Red highlighting for missed deadlines

### Manual Workflow Actions
Some workflow steps require manual intervention:

1. **Review Required**: Complete review and approve
2. **Decision Points**: Make selections to proceed
3. **External Dependencies**: Wait for external input
4. **Document Review**: Verify documents are complete

## Advanced Dossier Features

### Bulk Operations
Perform actions on multiple dossiers simultaneously:

1. From dossier list, select multiple dossiers (Ctrl+Click)
2. Use **Actions** → **Batch Operations**:
   - **Bulk Status Update**: Change status of selected dossiers  
   - **Mass Email**: Send notifications to multiple contacts
   - **Export Data**: Generate reports for selected dossiers
   - **Archive**: Move completed dossiers to archive

### Dossier Templates
Create templates for common dossier types:

1. **Actions** → **Create Template** from existing dossier
2. Define template fields:
   - **Default Values**: Pre-filled information
   - **Required Fields**: Mandatory fields for this type
   - **Workflow Assignment**: Automatically assign workflows

### Linking Related Dossiers
Connect related dossiers for comprehensive case management:

1. From dossier card: **Related** → **Linked Dossiers**
2. Add related dossiers:
   - **Parent/Child**: Hierarchical relationships
   - **Related Cases**: Connected but independent dossiers
   - **Previous/Next**: Sequential case progression

## Search and Filtering

### Quick Search
Use the search box at the top of the dossier list:
- **By Number**: Enter dossier number (e.g., D0001)
- **By Contact**: Type contact name or company
- **By Description**: Search dossier descriptions
- **By Reference**: Find by external reference numbers

### Advanced Filtering
Create complex filters for specific views:

1. Click **Filter Pane** (funnel icon)
2. Add filter criteria:
   - **Date Ranges**: Filter by creation or due dates  
   - **Status**: Show only open or closed dossiers
   - **Type**: Filter by dossier category
   - **Assigned To**: Show dossiers for specific users
   - **Priority**: High priority items only

### Saved Views
Create and save commonly used filters:

1. Set up your desired filters
2. Click **Save View** → **Name your view**
3. Access saved views from the view selector dropdown

## Dossier Analytics and Reporting

### Built-in Analytics
Access real-time dossier statistics:

1. **Dossier Statistics**: Overview of counts by status
2. **Performance Metrics**: Average processing times
3. **Workload Distribution**: Cases per user/department
4. **Trend Analysis**: Historical data and patterns

### Standard Reports
Generate comprehensive dossier reports:

1. **Dossier Overview Report**: Summary of all active dossiers
2. **Workflow Progress Report**: Status of automated processes  
3. **Document Inventory Report**: Attached document summary
4. **Communication Log Report**: Interaction history

### Custom Reporting
Create organization-specific reports:

1. Use **Report Builder** to create custom layouts
2. Define data sources and filters
3. Schedule automatic report generation  
4. Configure report distribution lists

## Best Practices

### Dossier Creation
✅ **Use Descriptive Names**: Clear, concise dossier descriptions  
✅ **Select Correct Type**: Choose appropriate dossier type for workflow automation  
✅ **Set Realistic Dates**: Accurate start and end dates for planning  
✅ **Assign Responsibility**: Clear ownership for each dossier  

### Content Management  
✅ **Consistent Documentation**: Use standard formats for notes and comments  
✅ **Regular Updates**: Keep progress and interactions current  
✅ **Organized Filing**: Use consistent document naming conventions  
✅ **Confidentiality**: Properly mark sensitive information  

### Workflow Optimization
✅ **Monitor Progress**: Regularly check workflow status  
✅ **Address Delays**: Take action on overdue workflow items  
✅ **Update Templates**: Refine workflows based on experience  
✅ **User Training**: Ensure all users understand workflow processes  

## Troubleshooting Common Issues

### Dossier Creation Problems
- **Error: "No Series Not Found"**: Check number series configuration in Dossier Setup
- **Required Field Missing**: Verify all mandatory fields are completed
- **Permission Denied**: Ensure user has appropriate QTEAM DOSSIER permissions

### Workflow Issues
- **Workflow Not Starting**: Check dossier type has assigned workflow
- **Tasks Not Created**: Verify workflow line configuration
- **Email Not Sending**: Check email queue and SMTP settings

### Document Issues  
- **Upload Failed**: Check file size limits and formats
- **Cannot Access Document**: Verify file storage path configuration
- **Drag & Drop Not Working**: Ensure Drag & Drop extension is installed

## Getting Help

### Additional Resources
- **User Guide**: [Official Documentation](https://www.q-teamsolutions.com/docs/dynamics-dossier/usage)
- **Video Tutorials**: [Q-Team YouTube Channel](https://www.youtube.com/channel/UC29xRURe_iZowUQnZYYTSOQ/)
- **Support Portal**: Contact Q-Team Solutions for assistance

### Contact Support  
- **Email**: [support@q-team.nl](mailto:support@q-team.nl)
- **Phone**: +31 030 820 13 33
- **Online**: Submit support requests through partner portal

---

*Next: Learn about [Absence Management](absence-management.md) features for tracking employee absences and sick leave.*