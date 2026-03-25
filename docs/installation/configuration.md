# Configuration Setup

## Initial Configuration Wizard

After installing Dynamics Dossier, you'll need to complete the initial configuration to start using the system effectively.

## Accessing Dossier Setup

1. Open Microsoft Dynamics 365 Business Central
2. Use the search function (🔍) and type **"Dossier Setup"**
3. Select **"QTEAM Dossier Setup"** from the results
4. The Dossier Setup page will open

## Core Configuration Steps

### 1. Number Series Configuration

#### Dossier Numbers
Configure the number series for automatic dossier numbering:

1. In **Dossier Setup**, locate **"Dossier Nos."** field
2. Click the dropdown or use lookup (F6)
3. Select an existing number series or create a new one:
   - **Code**: `DOSSIER`
   - **Description**: `Dossier Numbers`
   - **Starting No.**: `D0001`
   - **Ending No.**: `D99999`
   - **Increment-by No.**: `1`

#### Document Numbers  
Configure numbering for dossier documents:

1. Set up **"Document Nos."** field
2. Recommended settings:
   - **Code**: `DOSSIER-DOC`
   - **Description**: `Dossier Document Numbers`
   - **Starting No.**: `DD0001`
   - **Ending No.**: `DD99999`

### 2. Dossier Types Setup

Create and configure dossier types that match your organization's needs:

#### Access Dossier Types
1. Search for **"Dossier Types"** 
2. Open **"QTEAM Dossier Types"** page

#### Standard Dossier Types

Create the following standard types:

| Code | Description | Category | Duration | Sick Leave |
|------|-------------|----------|----------|------------|
| ABSENCE | Absence Management | Absence | 90D | ✅ |
| MEDICAL | Medical Records | Medical | 1Y | ❌ |
| REINTEGR | Re-integration | Re-integration | 6M | ❌ |
| EXAM | Examinations | Examination | 30D | ❌ |
| SAFETY | Safety Management | Safety | 1Y | ❌ |

#### Creating a Dossier Type
1. Click **"New"** in Dossier Types list
2. Fill in the following fields:
   - **Code**: Unique identifier (e.g., `ABSENCE`)
   - **Description**: Clear description (e.g., `Absence Management`)
   - **Category**: Select from dropdown
   - **Duration of Dossier**: Set automatic closure period
   - **Sick Leave**: Enable for absence-related dossiers

### 3. Workflow Configuration

#### Accessing Workflow Setup
1. Search for **"Work Flow Headers"**
2. Open **"QTEAM Work Flow Headers"** page

#### Creating Standard Workflows

##### Absence Management Workflow
1. Create new workflow header:
   - **No.**: `WF-ABSENCE`
   - **Description**: `Standard Absence Workflow`
   - **Dossier Type Code**: `ABSENCE`

2. Add workflow lines:
   | Line No. | Type | Term | Description |
   |----------|------|------|-------------|
   | 10000 | After | 1D | Send notification to manager |
   | 20000 | After | 3D | Create return-to-work assessment |
   | 30000 | After | 7D | Schedule medical evaluation |

##### Medical Examination Workflow
1. Create workflow header:
   - **No.**: `WF-MEDICAL`
   - **Description**: `Medical Examination Process`
   - **Dossier Type Code**: `MEDICAL`

2. Configure workflow steps based on your organization's medical process

### 4. Email Integration Configuration

#### SMTP Settings
Configure email settings for automated notifications:

1. Go to **Email Accounts** in Business Central
2. Set up SMTP account for Dynamics Dossier:
   - **Account Name**: `Dossier Notifications`
   - **Email Address**: Your system email address
   - **SMTP Server**: Your mail server settings
   - **Authentication**: Configure based on your email provider

#### Email Queue Setup
1. In **Dossier Setup**, configure:
   - **Email Queue Processing**: Enable automatic processing
   - **Batch Size**: Set to 50-100 for optimal performance
   - **Retry Attempts**: Set to 3 attempts

### 5. Document Storage Configuration

#### File Locations
Configure where dossier documents are stored:

1. In **Dossier Setup**, set up document storage:
   - **Document Path**: Default location for file storage
   - **Archive Path**: Long-term storage location
   - **Temporary Path**: Temporary file processing location

#### SharePoint Integration (Optional)
For cloud-based document storage:

1. Configure SharePoint connection in Business Central
2. Set **Document Storage Type** to `SharePoint`
3. Configure SharePoint site and library settings

### 6. Medical Integration (Optional)

If your organization uses medical information systems:

#### Medical Connection Setup
1. In **Dossier Setup**, locate medical integration section:
   - **Medical System Connection**: Enable if applicable
   - **Patient Number Matching**: Configure ID matching rules
   - **Data Synchronization**: Set sync frequency

### 7. Security and Permissions Setup

#### Permission Sets
Assign appropriate permissions to user groups:

1. Go to **Permission Sets** in Business Central
2. Assign Dynamics Dossier permission sets:

| Permission Set | Description | Typical Users |
|----------------|-------------|---------------|
| QTEAM DOSSIER - ADMIN | Full administrative access | System administrators, HR managers |
| QTEAM DOSSIER - USER | Standard user access | Case managers, coordinators |
| QTEAM DOSSIER - READ | Read-only access | Supervisors, auditors |
| QTEAM DOSSIER - LIMITED | Restricted access | External consultants |

#### Data Security Configuration  
1. Configure field-level security for sensitive information
2. Set up confidentiality levels based on dossier types
3. Enable audit trail for compliance requirements

### 8. Department Setup (Optional)

If your organization uses departments:

#### Creating Departments
1. Search for **"Departments"** 
2. Open **"QTEAM Departments"** page
3. Create departments as needed:
   - **Code**: `HR`
   - **Name**: `Human Resources`
   - **Manager**: Assign department manager

### 9. Calendar Integration

#### Appointment Scheduling
Configure integration with Business Central calendar:

1. Enable calendar integration in **Dossier Setup**
2. Set default appointment duration and types
3. Configure reminder settings

### 10. Reporting Configuration

#### Standard Reports
Set up standard reports for dossier management:

1. **Absence Statistics Report**
2. **Medical Examination Overview**  
3. **Workflow Progress Report**
4. **GDPR Compliance Report**

#### Custom Report Setup
Configure organization-specific reporting requirements:

1. Define key performance indicators (KPIs)
2. Set up automated report generation
3. Configure report distribution lists

## GDPR and Compliance Configuration

### Privacy Settings
1. Enable GDPR compliance features in **Dossier Setup**:
   - **Data Retention Periods**: Set automatic cleanup periods
   - **Privacy Consent Tracking**: Enable consent management
   - **Anonymization Rules**: Configure data anonymization

### Audit Trail Configuration  
1. Enable comprehensive audit logging:
   - **Field Change Tracking**: Monitor all data modifications
   - **Access Logging**: Track user access to sensitive data
   - **Export Logging**: Monitor data export activities

## Testing Your Configuration

### Configuration Validation
1. Create a test dossier for each dossier type
2. Verify number series generation works correctly
3. Test workflow execution with sample data
4. Validate email notifications are sent
5. Confirm document storage and retrieval

### User Acceptance Testing
1. Have end users test core functionality:
   - Create dossiers
   - Add documents and comments
   - Execute workflows
   - Generate reports

## Backup and Maintenance

### Regular Maintenance Tasks
1. **Weekly**: Review email queue for failed messages
2. **Monthly**: Clean up completed workflows  
3. **Quarterly**: Review and update dossier types as needed
4. **Annually**: Audit user permissions and access rights

### Backup Considerations
1. **Business Central Database**: Include in standard BC backup
2. **Document Storage**: Separate backup for file storage locations
3. **Configuration Export**: Export setup data for disaster recovery

## Getting Support with Configuration

### Professional Services
Q-Team Solutions offers professional configuration services:
- **Initial Setup Consultation**: 2-4 hours
- **Custom Workflow Design**: Based on requirements
- **Integration Services**: For complex medical system integrations
- **Training Services**: User and administrator training

### Contact Information
- **Email**: [support@q-team.nl](mailto:support@q-team.nl)
- **Phone**: +31 030 820 13 33
- **Documentation**: [Online Help](https://www.q-teamsolutions.com/docs/category/dynamics-dossier/)

## Next Steps

After completing the configuration:

1. **Train Users**: Provide training on dossier management processes
2. **Create Templates**: Set up standard dossier templates for common use cases  
3. **Monitor Usage**: Track system utilization and performance  
4. **Optimize Workflows**: Fine-tune workflows based on actual usage patterns

---

*Next: Learn how to [manage dossiers](../usage/managing-dossiers.md) effectively in your daily operations.*