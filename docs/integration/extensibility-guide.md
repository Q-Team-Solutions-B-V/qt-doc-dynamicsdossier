# Extensibility Guide

## Overview

Dynamics Dossier is built on Microsoft Business Central's extensible architecture, allowing organizations to customize and extend functionality to meet specific business requirements. This guide covers customization options, extension development, and integration possibilities.

## Extension Architecture

### Dynamics Dossier's Extensible Design

Dynamics Dossier follows Business Central's modern extension model:

- **Object-based Extensions**: Add fields, modify pages, extend functionality
- **Event-driven Architecture**: Hook into business processes with events  
- **API Extensions**: Extend web services and API endpoints
- **Workflow Extensions**: Custom workflow steps and automation
- **Report Extensions**: Modify or create custom reports

### Extension Boundaries

#### What Can Be Extended
✅ **Dossier Fields**: Add custom fields to dossier tables  
✅ **Page Modifications**: Extend dossier pages with new fields/controls  
✅ **Custom Workflows**: Create organization-specific workflow templates  
✅ **Reports and Analytics**: Build custom reports and dashboards  
✅ **API Endpoints**: Add custom web service endpoints  
✅ **Business Logic**: Extend with custom codeunits and procedures  

#### What Cannot Be Modified
❌ **Core AL Code**: Base Dynamics Dossier application logic  
❌ **Primary Keys**: Core table structure and relationships  
❌ **Security Model**: Base permission and authentication systems  
❌ **Upgrade Path**: Extensions must maintain upgrade compatibility  

## Common Extension Scenarios

### Adding Custom Fields

#### Extend Dossier Table
Add organization-specific fields to the dossier record:

```al
tableextension 50101 "Custom Dossier Extension" extends "QTEAM Dossier"
{
    fields
    {
        field(50100; "Custom Priority Level"; Integer)
        {
            Caption = 'Custom Priority Level';
            DataClassification = CustomerContent;
            MinValue = 1;
            MaxValue = 10;
        }
        
        field(50101; "Department Code"; Code[20])
        {
            Caption = 'Department Code';
            DataClassification = CustomerContent;
            TableRelation = Department;
        }
        
        field(50102; "External System ID"; Text[50])
        {
            Caption = 'External System ID';
            DataClassification = CustomerContent;
        }
        
        field(50103; "Custom Category"; Enum "Custom Dossier Category")
        {
            Caption = 'Custom Category';
            DataClassification = CustomerContent;
        }
    }
}
```

#### Extend Dossier Card Page
Add custom fields to the user interface:

```al
pageextension 50101 "Custom Dossier Card Ext" extends "QTEAM Dossier Card"
{
    layout
    {
        addafter("Ending Date")
        {
            field("Custom Priority Level"; Rec."Custom Priority Level")
            {
                ApplicationArea = All;
                ToolTip = 'Specify the custom priority level for this dossier.';
            }
            
            field("Department Code"; Rec."Department Code")
            {
                ApplicationArea = All;
                ToolTip = 'Select the department responsible for this dossier.';
            }
        }
        
        addlast(General)
        {
            group("Custom Information")
            {
                Caption = 'Custom Information';
                
                field("External System ID"; Rec."External System ID")
                {
                    ApplicationArea = All;
                }
                
                field("Custom Category"; Rec."Custom Category")
                {
                    ApplicationArea = All;
                }
            }
        }
    }
    
    actions
    {
        addlast(processing)
        {
            action("Custom Action")
            {
                Caption = 'Custom Process';
                ApplicationArea = All;
                Image = Process;
                
                trigger OnAction()
                var
                    CustomProcessing: Codeunit "Custom Dossier Processing";
                begin
                    CustomProcessing.ProcessCustomAction(Rec);
                end;
            }
        }
    }
}
```

### Custom Enumerations

#### Define Custom Categories
Create custom enums for extended classifications:

```al
enum 50100 "Custom Dossier Category"
{
    Extensible = true;
    
    value(0; " ")
    {
        Caption = ' ';
    }
    value(1; "Internal Review")
    {
        Caption = 'Internal Review';
    }
    value(2; "External Audit")
    {
        Caption = 'External Audit';
    }
    value(3; "Compliance Check")
    {
        Caption = 'Compliance Check';
    }
    value(4; "Quality Assurance")
    {
        Caption = 'Quality Assurance';
    }
}
```

### Custom Business Logic

#### Event Subscribers
Hook into Dynamics Dossier events for custom processing:

```al
codeunit 50101 "Custom Dossier Events"
{
    [EventSubscriber(ObjectType::Table, Database::"QTEAM Dossier", 'OnAfterInsertEvent', '', false, false)]
    local procedure OnAfterDossierInsert(var Rec: Record "QTEAM Dossier")
    var
        CustomNotification: Codeunit "Custom Notification Manager";
    begin
        // Custom processing when new dossier is created
        CustomNotification.NotifyDepartment(Rec."Department Code", Rec."No.");
        UpdateExternalSystem(Rec);
    end;
    
    [EventSubscriber(ObjectType::Table, Database::"QTEAM Dossier", 'OnAfterModifyEvent', '', false, false)]
    local procedure OnAfterDossierModify(var Rec: Record "QTEAM Dossier")
    begin
        // Custom processing when dossier is updated
        if Rec.Status <> xRec.Status then
            ProcessStatusChange(Rec);
    end;
    
    local procedure UpdateExternalSystem(DossierRec: Record "QTEAM Dossier")
    var
        ExternalAPI: Codeunit "External System API";
    begin
        ExternalAPI.CreateDossierRecord(
            DossierRec."No.", 
            DossierRec.Description, 
            DossierRec."External System ID"
        );
    end;
    
    local procedure ProcessStatusChange(DossierRec: Record "QTEAM Dossier")
    var
        WorkflowManager: Codeunit "Custom Workflow Manager";
    begin
        WorkflowManager.TriggerStatusChangeWorkflow(DossierRec);
    end;
}
```

### Custom Workflows

#### Custom Workflow Codeunit
Implement organization-specific workflow logic:

```al
codeunit 50102 "Custom Workflow Manager"
{
    procedure TriggerStatusChangeWorkflow(DossierRec: Record "QTEAM Dossier")
    var
        WorkflowLogEntry: Record "QTEAM Work Flow Log Entry";
        EmailManager: Codeunit "Custom Email Manager";
    begin
        case DossierRec.Status of
            DossierRec.Status::"In Process":
                HandleInProcessStatus(DossierRec);
            DossierRec.Status::Closed:
                HandleClosedStatus(DossierRec);
        end;
    end;
    
    local procedure HandleInProcessStatus(DossierRec: Record "QTEAM Dossier")
    var
        TaskManager: Codeunit "Custom Task Manager";
    begin
        // Create custom tasks based on dossier type
        TaskManager.CreateProcessingTasks(DossierRec);
        
        // Send notifications to stakeholders
        NotifyStakeholders(DossierRec, 'Dossier processing started');
    end;
    
    local procedure HandleClosedStatus(DossierRec: Record "QTEAM Dossier")
    var
        ArchiveManager: Codeunit "Custom Archive Manager";
    begin
        // Perform closure activities
        ArchiveManager.ArchiveDossierDocuments(DossierRec."No.");
        
        // Generate completion report
        GenerateCompletionReport(DossierRec);
        
        // Update external systems
        UpdateExternalSystemStatus(DossierRec);
    end;
}
```

## API Extensions

### Custom API Pages

#### Expose Custom Fields via API
Create API endpoints for custom dossier fields:

```al
page 50101 "Custom Dossier API"
{
    PageType = API;
    Caption = 'customDossiers';
    APIPublisher = 'yourcompany';
    APIGroup = 'dossier';
    APIVersion = 'v1.0';
    EntityName = 'customDossier';
    EntitySetName = 'customDossiers';
    EntityCaption = 'Custom Dossier';
    EntitySetCaption = 'Custom Dossiers';
    SourceTable = "QTEAM Dossier";
    DelayedInsert = true;
    
    layout
    {
        area(Content)
        {
            repeater(GroupName)
            {
                field(id; Rec.SystemId)
                {
                    Caption = 'Id';
                    Editable = false;
                }
                
                field(number; Rec."No.")
                {
                    Caption = 'Number';
                }
                
                field(description; Rec.Description)
                {
                    Caption = 'Description';
                }
                
                field(customPriorityLevel; Rec."Custom Priority Level")
                {
                    Caption = 'Custom Priority Level';
                }
                
                field(departmentCode; Rec."Department Code")
                {
                    Caption = 'Department Code';
                }
                
                field(externalSystemId; Rec."External System ID")
                {
                    Caption = 'External System ID';
                }
                
                field(customCategory; Rec."Custom Category")
                {
                    Caption = 'Custom Category';
                }
            }
        }
    }
}
```

#### API Query for Custom Reports
Create efficient data access for custom reporting:

```al
query 50101 "Custom Dossier Analytics"
{
    QueryType = API;
    Caption = 'Custom Dossier Analytics';
    APIPublisher = 'yourcompany';
    APIGroup = 'analytics';
    APIVersion = 'v1.0';
    EntityName = 'dossierAnalytics';
    EntitySetName = 'dossierAnalytics';
    
    elements
    {
        dataitem(Dossier; "QTEAM Dossier")
        {
            column(DossierNo; "No.")
            {
                Caption = 'Dossier Number';
            }
            
            column(DossierType; "Dossier Type Code")
            {
                Caption = 'Dossier Type';
            }
            
            column(Status; Status)
            {
                Caption = 'Status';
            }
            
            column(CustomPriority; "Custom Priority Level")
            {
                Caption = 'Priority Level';
            }
            
            column(Department; "Department Code")
            {
                Caption = 'Department';
            }
            
            column(DocumentDate; "Document Date")
            {
                Caption = 'Document Date';
            }
            
            column(DaysSinceCreated; "Document Date")
            {
                Caption = 'Days Since Created';
                Method = Count;
            }
        }
    }
}
```

## Custom Reports

### Extended Dossier Report
Create custom reports with additional fields and formatting:

```al
report 50101 "Custom Dossier Report"
{
    UsageCategory = ReportsAndAnalysis;
    ApplicationArea = All;
    Caption = 'Custom Dossier Report';
    ProcessingOnly = false;
    UseRequestPage = true;
    
    dataset
    {
        dataitem(Dossier; "QTEAM Dossier")
        {
            DataItemTableView = SORTING("No.");
            RequestFilterFields = "Dossier Type Code", Status, "Department Code";
            
            column(DossierNo; "No.")
            {
            }
            
            column(Description; Description)
            {
            }
            
            column(PrimaryContactNo; "Primary Contact No.")
            {
            }
            
            column(Status; Status)
            {
            }
            
            column(CustomPriorityLevel; "Custom Priority Level")
            {
            }
            
            column(DepartmentCode; "Department Code")
            {
            }
            
            column(DocumentDate; "Document Date")
            {
            }
            
            dataitem(WorkflowLog; "QTEAM Work Flow Log Entry")
            {
                DataItemLink = "Dossier No." = FIELD("No.");
                DataItemTableView = SORTING("Entry No.");
                
                column(WorkflowDescription; Description)
                {
                }
                
                column(DateAdded; "Date Added")
                {
                }
                
                column(TimeAdded; "Time Added")
                {
                }
            }
        }
    }
    
    requestpage
    {
        layout
        {
            area(Content)
            {
                group(Options)
                {
                    Caption = 'Options';
                    
                    field(IncludeWorkflowDetails; IncludeWorkflowDetails)
                    {
                        ApplicationArea = All;
                        Caption = 'Include Workflow Details';
                        ToolTip = 'Include workflow execution details in the report.';
                    }
                    
                    field(ShowOnlyOverdue; ShowOnlyOverdue)
                    {
                        ApplicationArea = All;
                        Caption = 'Show Only Overdue';
                        ToolTip = 'Show only dossiers that are past their expected completion date.';
                    }
                }
            }
        }
    }
    
    var
        IncludeWorkflowDetails: Boolean;
        ShowOnlyOverdue: Boolean;
}
```

## Integration Extensions

### External System Integration

#### Custom Integration Codeunit
Handle integration with external systems:

```al
codeunit 50103 "External System Integration"
{
    procedure SyncDossierData()
    var
        DossierRec: Record "QTEAM Dossier";
        ExternalAPI: Codeunit "External API Client";
        IntegrationLog: Record "Custom Integration Log";
    begin
        DossierRec.SetFilter(Status, '<>%1', DossierRec.Status::Closed);
        if DossierRec.FindSet() then
            repeat
                SyncSingleDossier(DossierRec, ExternalAPI, IntegrationLog);
            until DossierRec.Next() = 0;
    end;
    
    local procedure SyncSingleDossier(var DossierRec: Record "QTEAM Dossier"; var ExternalAPI: Codeunit "External API Client"; var IntegrationLog: Record "Custom Integration Log")
    var
        SyncResult: Text;
    begin
        if ShouldSyncDossier(DossierRec) then begin
            SyncResult := ExternalAPI.UpdateDossierStatus(
                DossierRec."External System ID",
                Format(DossierRec.Status),
                DossierRec."Custom Priority Level"
            );
            
            LogIntegrationResult(DossierRec."No.", SyncResult, IntegrationLog);
        end;
    end;
    
    local procedure ShouldSyncDossier(DossierRec: Record "QTEAM Dossier"): Boolean
    begin
        exit((DossierRec."External System ID" <> '') and (DossierRec."Custom Priority Level" > 5));
    end;
}
```

#### REST API Client
Implement REST API communication:

```al
codeunit 50104 "External API Client"
{
    var
        HttpClient: HttpClient;
        BaseURL: Text;
        ApiKey: Text;
    
    procedure Initialize(NewBaseURL: Text; NewApiKey: Text)
    begin
        BaseURL := NewBaseURL;
        ApiKey := NewApiKey;
        SetupHttpClient();
    end;
    
    procedure UpdateDossierStatus(ExternalID: Text; Status: Text; Priority: Integer): Text
    var
        HttpContent: HttpContent;
        HttpResponseMessage: HttpResponseMessage;
        RequestBody: JsonObject;
        ResponseText: Text;
    begin
        RequestBody.Add('externalId', ExternalID);
        RequestBody.Add('status', Status);
        RequestBody.Add('priority', Priority);
        
        HttpContent.WriteFrom(Format(RequestBody));
        HttpContent.GetHeaders().Clear();
        HttpContent.GetHeaders().Add('Content-Type', 'application/json');
        
        if HttpClient.Put(BaseURL + '/api/dossiers/' + ExternalID, HttpContent, HttpResponseMessage) then begin
            HttpResponseMessage.Content.ReadAs(ResponseText);
            exit(ResponseText);
        end else
            exit('ERROR: Failed to update external system');
    end;
    
    local procedure SetupHttpClient()
    var
        HttpHeaders: HttpHeaders;
    begin
        HttpClient.DefaultRequestHeaders(HttpHeaders);
        HttpHeaders.Add('Authorization', 'Bearer ' + ApiKey);
        HttpHeaders.Add('User-Agent', 'Dynamics-Dossier-Integration/1.0');
    end;
}
```

## Custom User Interface Extensions

### FactBox Extensions
Add custom information panels:

```al
page 50102 "Custom Dossier Statistics"
{
    Caption = 'Custom Statistics';
    PageType = CardPart;
    SourceTable = "QTEAM Dossier";
    
    layout
    {
        area(Content)
        {
            cuegroup("Custom Metrics")
            {
                Caption = 'Custom Metrics';
                
                field("Days Active"; CalculateDaysActive())
                {
                    Caption = 'Days Active';
                    DrillDownPageId = "Custom Activity Log";
                    ToolTip = 'Number of days since the dossier was created.';
                }
                
                field("Priority Score"; Rec."Custom Priority Level")
                {
                    Caption = 'Priority Score';
                    Style = Strong;
                    StyleExpr = Rec."Custom Priority Level" > 7;
                }
                
                field("External Sync Status"; GetExternalSyncStatus())
                {
                    Caption = 'External Sync';
                    ToolTip = 'Status of synchronization with external systems.';
                }
            }
        }
    }
    
    local procedure CalculateDaysActive(): Integer
    begin
        if Rec."Document Date" <> 0D then
            exit(Today - Rec."Document Date")
        else
            exit(0);
    end;
    
    local procedure GetExternalSyncStatus(): Text
    var
        IntegrationLog: Record "Custom Integration Log";
    begin
        IntegrationLog.SetRange("Dossier No.", Rec."No.");
        IntegrationLog.SetRange("Sync Date", CalcDate('-7D', Today), Today);
        if IntegrationLog.FindLast() then begin
            if IntegrationLog."Sync Successful" then
                exit('✅ Synced')
            else
                exit('❌ Failed');
        end else
            exit('⏳ Pending');
    end;
}
```

## Deployment and Packaging

### Extension Manifest (app.json)

```json
{
  "id": "12345678-1234-1234-1234-123456789012",
  "name": "Custom Dossier Extensions",
  "publisher": "Your Company Name",
  "version": "1.0.0.0",
  "brief": "Custom extensions for Dynamics Dossier",
  "description": "Adds custom fields, workflows, and integrations to Dynamics Dossier",
  "privacyStatement": "https://yourcompany.com/privacy",
  "EULA": "https://yourcompany.com/eula",
  "help": "https://yourcompany.com/help/dossier-extensions",
  "contextSensitiveHelpUrl": "https://yourcompany.com/help/dossier-extensions",
  "dependencies": [
    {
      "id": "cb3cc455-fb72-4c7c-949e-261433611d44",
      "name": "Dynamics Dossier",
      "publisher": "Q-Team Solutions",
      "version": "26.0.45957.1"
    }
  ],
  "idRanges": [
    {
      "from": 50100,
      "to": 50199
    }
  ],
  "platform": "1.0.0.0",
  "application": "26.0.0.0",
  "runtime": "15.0"
}
```

### Permission Sets

```al
permissionset 50100 "Custom Dossier - User"
{
    Assignable = true;
    Caption = 'Custom Dossier User';
    
    Permissions = 
        tabledata "QTEAM Dossier" = RMD,
        table "QTEAM Dossier" = X,
        page "Custom Dossier API" = X,
        codeunit "Custom Workflow Manager" = X,
        codeunit "External System Integration" = X,
        report "Custom Dossier Report" = X;
}

permissionset 50101 "Custom Dossier - Admin"
{
    Assignable = true;
    Caption = 'Custom Dossier Administrator';
    IncludedPermissionSets = "Custom Dossier - User";
    
    Permissions = 
        tabledata "Custom Integration Log" = RIMD,
        page "Custom Dossier Statistics" = X,
        codeunit "External API Client" = X;
}
```

## Best Practices for Extensions

### Development Guidelines

#### Code Quality
✅ **Descriptive Naming**: Use clear, descriptive names for all objects  
✅ **Error Handling**: Implement comprehensive error handling  
✅ **Documentation**: Include inline comments and XML documentation  
✅ **Testing**: Create comprehensive test coverage  

#### Performance Optimization
✅ **Efficient Queries**: Use appropriate filters and sorting  
✅ **Minimal Data Transfer**: Select only required fields in APIs  
✅ **Batch Operations**: Group multiple operations when possible  
✅ **Caching**: Cache frequently accessed data appropriately  

#### Security Considerations
✅ **Permission Validation**: Check user permissions before operations  
✅ **Data Classification**: Properly classify all custom fields  
✅ **Input Validation**: Validate all user input and API data  
✅ **Audit Logging**: Log security-relevant operations  

### Upgrade and Maintenance

#### Version Compatibility
- **Maintain Compatibility**: Ensure extensions work with Dynamics Dossier updates
- **Test Thoroughly**: Test extensions with each new version
- **Document Changes**: Maintain detailed change logs
- **Backup Procedures**: Implement proper backup and rollback procedures

#### Deployment Strategy
1. **Development Environment**: Build and test extensions
2. **Staging Environment**: Test with production-like data
3. **Production Deployment**: Deploy during maintenance windows
4. **Monitoring**: Monitor system performance post-deployment

## Getting Support for Extensions

### Development Support
- **Business Central Documentation**: [docs.microsoft.com/dynamics365/business-central/dev-itpro](https://docs.microsoft.com/dynamics365/business-central/dev-itpro)
- **AL Language Reference**: Complete AL language documentation
- **Q-Team Solutions**: [info@q-team.nl](mailto:info@q-team.nl) for Dynamics Dossier specific questions

### Professional Services
Q-Team Solutions offers:
- **Custom Extension Development**: Complete extension development services
- **Integration Services**: Connect with external systems and APIs
- **Training Programs**: Extension development training for your team
- **Support and Maintenance**: Ongoing support for custom extensions

### Community Resources
- **Business Central Community**: [community.dynamics.com](https://community.dynamics.com)
- **GitHub Samples**: Example extensions and code samples
- **Developer Forums**: Community support and discussions

---

*Next: Review the complete documentation structure in [README.md](../../README.md) for navigation and additional resources.*