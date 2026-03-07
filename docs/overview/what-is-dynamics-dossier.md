# What is Dynamics Dossier

## Introduction

**Dynamics Dossier** is a comprehensive dossier management extension for Microsoft Dynamics 365 Business Central, developed by Q-Team Solutions. It provides organizations with a centralized system to manage documents, appointments, tasks, and workflows through structured dossiers.

## Core Concept

A **dossier** in Dynamics Dossier serves as a central container that groups related information, documents, activities, and processes for a specific purpose or individual. Think of it as a digital file folder that can contain:

- Personal or medical information
- Documents and attachments
- Appointments and tasks
- Progress tracking
- Workflow activities
- Comments and notes

## Key Features

### 🗂️ Dossier Management
- Create and manage different types of dossiers
- Configurable dossier types with specific purposes
- Status tracking (Open, Closed, In Progress)
- Duration-based dossiers with automatic closure
- Advanced search and filtering capabilities

### 👥 Contact Integration
- Enhanced contact management with person-specific information
- Primary and secondary contact assignments
- Company and employee relationship management
- Contact-specific dossier views

### 📋 Document Management
- Centralized document storage per dossier
- Document categorization and organization
- Version control and history tracking
- Integration with Microsoft Office documents

### ⚕️ Medical & Absence Management
- Medical dossier creation and management
- Sick leave and absence duration tracking
- Medical examination scheduling
- Absence category classification (Short, Medium, Long term)
- Re-integration process management

### 🔄 Workflow Automation
- Configurable workflow templates
- Automated task creation based on dossier events
- Deadline tracking and notifications
- Multi-step process management
- Workflow progress monitoring

### 📊 Reporting & Analytics
- Comprehensive dossier reporting
- Absence statistics and analysis
- Workflow performance metrics
- Custom report generation
- Data export capabilities

## Dossier Types

Dynamics Dossier supports various dossier categories:

| Category | Description | Use Cases |
|----------|-------------|-----------|
| **Absence** | Manage employee absences and sick leave | Sick leave tracking, absence duration monitoring |
| **Medical** | Medical records and examinations | Patient records, medical appointments, treatments |
| **Re-integration** | Return-to-work processes | Rehabilitation planning, work capacity assessments |
| **Examination** | Various types of examinations | Safety inspections, medical examinations, assessments |
| **Safety** | Safety-related documentation | Safety protocols, incident reports, compliance tracking |

## Integration with Business Central

Dynamics Dossier seamlessly integrates with Microsoft Dynamics 365 Business Central:

- **Contact Management**: Extends standard contact functionality
- **User Permissions**: Integrated with BC security framework
- **Number Series**: Uses BC number series for dossier numbering
- **Interaction Log**: Integrates with BC interaction logging
- **Workflow**: Leverages BC workflow capabilities
- **Multi-language**: Supports BC localization features

## Benefits

### For Organizations
- **Centralized Information**: All relevant information in one place
- **Compliance**: Meet regulatory requirements for record keeping
- **Efficiency**: Streamlined processes and automated workflows
- **Visibility**: Clear overview of ongoing processes and deadlines
- **Scalability**: Handles large volumes of dossiers and documents

### For Users
- **Intuitive Interface**: Familiar Business Central user experience
- **Quick Access**: Fast search and filtering capabilities
- **Mobile Access**: Works on tablets and mobile devices
- **Collaboration**: Multiple users can work on the same dossier
- **Notifications**: Automatic alerts for deadlines and updates

## Technical Architecture

Dynamics Dossier is built using:

- **AL Language**: Native Business Central development language
- **Modern Architecture**: Cloud-ready and SaaS compatible
- **API Integration**: RESTful APIs for external integrations
- **Extensible Design**: Can be customized and extended
- **Security**: Built-in role-based security and data protection

## Supported Versions

- **Microsoft Dynamics 365 Business Central**: Version 26.0 and higher
- **Language Support**: English (en-US) and Dutch (nl-NL)
- **Deployment**: On-premises and SaaS (Business Central Online)

## Dependencies

Dynamics Dossier requires the following additional apps:

1. **Q-Team App Authenticator** (v26.0.45894.0)
2. **Drag & Drop** (v26.0.45894.0)

These dependencies provide additional functionality for authentication and enhanced user interface features.

---

*Next: Learn about the [technical architecture](architecture.md) of Dynamics Dossier.*