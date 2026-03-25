# Absence Management

## Overview

Absence management is one of the core features of Dynamics Dossier, specifically designed for healthcare inspections organizations and companies that need to track employee absences, sick leave, and return-to-work processes efficiently.

## Key Features

### 🏥 Medical Absence Tracking
- **Sick Leave Management**: Track short, medium, and long-term absences
- **Medical Certifications**: Store and manage medical documentation
- **Duration Monitoring**: Automatic categorization based on absence length
- **Return-to-Work Planning**: Structured re-integration processes

### 📊 Absence Analytics  
- **Absence Statistics**: Comprehensive reporting on absence patterns
- **Trend Analysis**: Identify patterns and risk factors
- **Compliance Reporting**: Meet regulatory requirements for absence tracking
- **Performance Metrics**: Monitor absence reduction initiatives

### 🔄 Automated Workflows
- **Notification Systems**: Automatic alerts to managers and HR
- **Process Automation**: Standardized steps for absence management
- **Deadline Tracking**: Monitor critical dates and milestones
- **Escalation Procedures**: Automatic escalation for extended absences

## Setting Up Absence Management

### Dossier Type Configuration
Create absence-specific dossier types:

1. **Navigate to**: Dossier Types → New
2. **Configure fields**:
   - **Code**: `ABSENCE`
   - **Description**: `Employee Absence Management`
   - **Category**: `Absence`
   - **Sickness**: ✅ Enabled
   - **Duration of Dossier**: `90D` (or as per policy)

### Absence Categories
Configure absence duration categories:

| Category | Duration | Auto-Actions |
|----------|----------|--------------|
| **Short-term** | 1-7 days | Manager notification |
| **Medium-term** | 8-30 days | HR review, medical certificate required |
| **Long-term** | 30+ days | Specialist referral, re-integration planning |

### Workflow Setup for Absences

#### Standard Absence Workflow
Create automated workflow for absence management:

```
Day 1: Employee reports absence
↓
Day 1: Manager notification sent
↓  
Day 3: Medical certificate reminder
↓
Day 7: HR notification for medium-term absence
↓
Day 14: Medical review appointment scheduled  
↓
Day 30: Long-term absence procedures initiated
↓
Day 45: Re-integration assessment
```

#### Workflow Configuration
1. **Workflow Header**:
   - **No.**: `WF-ABSENCE`
   - **Description**: `Standard Absence Management`
   - **Dossier Type**: `ABSENCE`

2. **Workflow Lines**:
   | Line | Type | Term | Description |
   |------|------|------|-------------|
   | 10000 | After | 0D | Send manager notification |
   | 20000 | After | 3D | Request medical certificate |
   | 30000 | After | 7D | HR review initiation |
   | 40000 | After | 14D | Schedule medical appointment |
   | 50000 | After | 30D | Long-term absence procedures |
   | 60000 | After | 45D | Re-integration assessment |

## Creating Absence Dossiers

### New Absence Registration
1. **From Contact Card**:
   - Open employee contact
   - **Actions** → **Dossier** → **Create Absence Dossier**

2. **From Dossier List**:
   - **New** → Select **Absence** dossier type
   - Enter employee as Primary Contact

### Essential Information
Complete these key fields for absence dossiers:

#### Basic Details
- **Primary Contact**: Employee who is absent
- **Document Date**: First day of absence
- **Starting Date**: When absence began
- **Dossier Type**: `ABSENCE` (or specific absence type)
- **Description**: Brief reason (if appropriate to record)

#### Medical Information
- **Sick Leave**: ✅ Mark as sick leave related
- **Medical Certificate**: Track if medical documentation is required/received
- **Treating Physician**: Record doctor or medical facility information
- **Expected Return Date**: Estimated return to work date

## Absence Workflow Process

### Day 1: Absence Reporting
**Automatic Actions**:
1. Dossier created with absence details
2. Manager notification email sent
3. Workflow log entry created
4. Absence statistics updated

**Manual Actions**:
1. Verify employee contact details
2. Add initial absence reason (if disclosed)
3. Set expected duration if known

### Day 3: Medical Documentation
**Automatic Actions**:
1. Email reminder sent to employee for medical certificate
2. To-do created for HR to follow up
3. Status updated in workflow log

**Manual Actions**:
1. Upload medical certificate when received
2. Update expected return date based on medical advice
3. Add notes about medical requirements

### Day 7: HR Review (Medium-term)
**Triggered for absences lasting 7+ days**:

**Automatic Actions**:
1. HR department notification
2. Medical review appointment scheduled
3. Case escalated for medium-term procedures

**Manual Actions**:
1. Review absence circumstances
2. Contact employee to discuss support needs
3. Coordinate with manager for work coverage

### Day 30: Long-term Procedures  
**For absences extending beyond 30 days**:

**Automatic Actions**:
1. Long-term absence flag activated
2. Re-integration workflow initiated  
3. Specialist referral procedures started
4. Compliance notifications sent

**Manual Actions**:
1. Comprehensive case review
2. Develop return-to-work plan
3. Coordinate with occupational health
4. Update management on long-term impact

### Return to Work Process

#### Pre-Return Assessment
1. **Medical Clearance**: Verify fitness for work
2. **Capacity Assessment**: Determine any work limitations  
3. **Workplace Assessment**: Review environment for accommodations
4. **Gradual Return Plan**: Phased return schedule if needed

#### Return Documentation
- **Fit for Work Certificate**: Medical clearance
- **Work Restrictions**: Any limitations or accommodations
- **Follow-up Schedule**: Ongoing monitoring requirements
- **Risk Assessment**: Updated workplace risk evaluation

## Absence Reporting and Analytics

### Standard Reports

#### Absence Overview Report
Comprehensive view of all current absences:
- **Current Absences**: All active absence dossiers
- **Duration Analysis**: Short, medium, long-term breakdown
- **Department Summary**: Absence rates by department  
- **Trend Indicators**: Month-over-month comparison

#### Medical Certificate Tracking
Monitor compliance with medical documentation:
- **Outstanding Certificates**: Overdue medical documents
- **Compliance Rate**: Percentage with valid certificates
- **Follow-up Actions**: Required administrative steps

#### Return-to-Work Statistics  
Track successful re-integration:
- **Return Rates**: Successful returns by timeframe
- **Re-occurrence Tracking**: Repeat absences within timeframes
- **Accommodation Success**: Effectiveness of workplace adjustments

### Custom Analytics 

#### Absence Pattern Analysis
Identify trends and risk factors:
- **Seasonal Patterns**: Absence peaks by time of year
- **Department Hotspots**: Areas with high absence rates
- **Individual Patterns**: Employees with recurring absences
- **Duration Trends**: Average absence lengths over time

#### Cost Analysis
Calculate absence impact:
- **Direct Costs**: Salary costs during absence
- **Replacement Costs**: Temporary staffing expenses
- **Administrative Costs**: HR and management time
- **Productivity Impact**: Work delays and disruptions

## GDPR Compliance for Medical Data

### Data Protection Requirements
Special considerations for medical absence data:

#### Sensitive Data Handling
- **Limited Access**: Restrict access to authorized personnel only
- **Encryption**: Secure storage of medical information
- **Anonymization**: Remove identifying information from reports
- **Consent Management**: Explicit consent for medical data processing

#### Data Retention
- **Legal Requirements**: Comply with employment law retention periods
- **Automatic Cleanup**: Schedule deletion of old medical records
- **Archive Procedures**: Long-term storage with restricted access
- **Audit Trails**: Complete history of data access and modifications

### Permission Configuration
Set up role-based access for medical data:

| Role | Access Level | Permissions |
|------|--------------|-------------|
| **HR Manager** | Full Access | Create, read, update absence records |
| **Line Manager** | Limited | Read absence status, no medical details |
| **Occupational Health** | Medical Only | Access medical certificates and assessments |
| **Administrator** | System Admin | Configuration and system maintenance |
| **Auditor** | Read-Only | Compliance reporting and audit access |

## Integration with Payroll Systems

### Automatic Data Exchange
Connect absence data with payroll processing:

1. **Absence Dates**: Automatically transfer absence periods
2. **Medical Certificates**: Flag certified vs. uncertified absences  
3. **Return Dates**: Update payroll when employees return
4. **Statutory Payments**: Calculate sick pay entitlements

### API Integration
For organizations with external payroll systems:
- **RESTful APIs**: Standardized data exchange formats
- **Real-time Updates**: Immediate notification of absence changes
- **Bulk Export**: Periodic transfer of absence data
- **Error Handling**: Robust error reporting and retry mechanisms

## Best Practices for Absence Management

### Initial Setup
✅ **Clear Policies**: Establish clear absence reporting procedures  
✅ **Training**: Train managers and HR staff on system usage  
✅ **Communication**: Inform employees about absence reporting requirements  
✅ **Legal Compliance**: Ensure processes meet employment law requirements  

### Daily Operations  
✅ **Prompt Recording**: Enter absences immediately when reported  
✅ **Regular Follow-up**: Monitor workflow progress and take required actions  
✅ **Document Management**: Maintain complete records with medical certificates  
✅ **Confidentiality**: Protect sensitive medical information appropriately  

### Performance Monitoring
✅ **Regular Reviews**: Analyze absence trends and patterns monthly  
✅ **Process Improvement**: Refine workflows based on experience  
✅ **Compliance Checks**: Regular audit of procedures and documentation  
✅ **Support Evaluation**: Assess effectiveness of return-to-work programs  

## Troubleshooting Common Issues

### Workflow Not Triggering
**Problem**: Absence workflow steps not executing automatically  
**Solutions**:
1. Verify dossier type has **Sickness** checkbox enabled
2. Check workflow assignment in Dossier Type configuration  
3. Confirm workflow lines are properly configured with correct terms

### Email Notifications Not Sending
**Problem**: Manager or HR notifications not being delivered  
**Solutions**:
1. Check email queue for failed messages
2. Verify SMTP configuration in Business Central
3. Confirm recipient email addresses are correct
4. Check email templates are properly configured

### Medical Certificate Tracking Issues
**Problem**: Certificates not being tracked or flagged properly  
**Solutions**:
1. Ensure document upload functionality is working
2. Check document categorization settings
3. Verify workflow steps for certificate requests
4. Confirm user permissions for document management

## Getting Support

### Specialized Absence Management Support
Q-Team Solutions provides specialized assistance for absence management:

- **Process Consulting**: Optimize absence management workflows
- **Compliance Advice**: Ensure legal and regulatory compliance  
- **Integration Services**: Connect with payroll and HR systems
- **Training Programs**: Comprehensive user and administrator training

### Contact Information
- **Email**: [support@q-team.nl](mailto:support@q-team.nl)  
- **Phone**: +31 030 820 13 33
- **Documentation**: [Absence Management Docs](https://www.q-teamsolutions.com/docs/dynamics-dossier/usage)

---

*Next: Learn about [Medical Records Management](medical-records.md) for comprehensive patient and medical documentation.*