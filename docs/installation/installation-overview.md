# Installation Overview

## Prerequisites

Before installing Dynamics Dossier, ensure your environment meets the following requirements:

### Microsoft Dynamics 365 Business Central
- **Version**: 26.0 or higher
- **Edition**: Essentials or Premium Edition
- **Deployment**: SaaS (Business Central Online) or On-Premises

### Required Dependencies
The following apps must be installed before Dynamics Dossier:

1. **Q-Team App Authenticator** (v26.0.45894.0)
   - Provides enhanced authentication services
   - Handles license validation and compliance

2. **Drag & Drop** (v26.0.45894.0)
   - Enables drag-and-drop functionality for documents
   - Enhances user interface experience

### System Requirements
- **Microsoft Outlook** integration (Office 365 compatible)
- **Modern web browser** (Chrome, Edge, Firefox, Safari)
- **Adequate storage** for dossier documents and attachments
- **Network connectivity** for email integration and online services

### User Requirements
- **Business Central user license** (Essentials or Premium)
- **Dynamics Dossier license** (€29/user/month, $30/user/month)
- **Appropriate permissions** for dossier management

## Installation Methods

### SaaS (Business Central Online) Installation

#### Step 1: Access Microsoft AppSource
1. Navigate to the [Microsoft AppSource marketplace](https://marketplace.microsoft.com/en-us/product/dynamics-365-business-central/PUBID.dynamics_dossier%7CAID.dynamicsdossier%7CPAPPID.cb3cc455-fb72-4c7c-949e-261433611d44)
2. Search for "Dynamics Dossier" by Q-Team Solutions
3. Click on the app listing

#### Step 2: Start Free Trial
1. Click **"Free Trial"** to start a trial period
2. Sign in with your Microsoft 365 administrator credentials  
3. Select your Business Central environment
4. Review and accept the terms and conditions

#### Step 3: Install the App
1. The system will automatically check for dependencies
2. Install required dependencies if not already present:
   - Q-Team App Authenticator
   - Drag & Drop
3. Click **"Install"** to proceed with installation
4. Wait for the installation to complete (typically 5-10 minutes)

#### Step 4: Verify Installation
1. Open your Business Central environment
2. Navigate to **Extension Management**
3. Verify that all three extensions are installed and enabled:
   - Dynamics Dossier
   - Q-Team App Authenticator  
   - Drag & Drop

### On-Premises Installation

#### Step 1: Obtain Installation Files
1. Contact Q-Team Solutions at [support@q-team.nl](mailto:support@q-team.nl)
2. Purchase the on-premises license
3. Download the installation files:
   - Dynamics Dossier (.app file)
   - Q-Team App Authenticator (.app file)
   - Drag & Drop (.app file)

#### Step 2: Install Prerequisites
1. Install Q-Team App Authenticator first
2. Install Drag & Drop extension
3. Verify both extensions are operational

#### Step 3: Install Dynamics Dossier
1. Use Business Central Administration Shell or Development Environment
2. Run installation command:
   ```powershell
   Install-NAVApp -ServerInstance BC260 -Path "Q-Team Solutions_Dynamics Dossier_26.0.45957.1.app"
   ```
3. Publish and sync the extension:
   ```powershell
   Publish-NAVApp -ServerInstance BC260 -Path "Q-Team Solutions_Dynamics Dossier_26.0.45957.1.app"
   Sync-NAVApp -ServerInstance BC260 -Name "Dynamics Dossier"
   ```

#### Step 4: Start the Extension
```powershell
Start-NAVAppDataUpgrade -ServerInstance BC260 -Name "Dynamics Dossier"
```

## Post-Installation Steps

### 1. License Registration
1. Navigate to **Dossier Setup** in Business Central
2. Click **"Register License"** 
3. Enter your license key provided by Q-Team Solutions
4. Verify license activation

### 2. User Permissions
1. Go to **Users** in Business Central
2. Assign **Dynamics Dossier** permission sets to appropriate users:
   - **QTEAM DOSSIER - ADMIN**: Full administrative access
   - **QTEAM DOSSIER - USER**: Standard user access  
   - **QTEAM DOSSIER - READ**: Read-only access

### 3. Initial Configuration
1. Complete the [Configuration Setup](configuration.md)
2. Configure number series for dossiers
3. Set up dossier types
4. Configure email integration

## Supported Countries and Languages

### Geographic Availability
- **SaaS**: Available in all countries where Business Central Online is provided
- **On-Premises**: Available worldwide

### Language Support
- **English (United States)** - en-US
- **Dutch (The Netherlands)** - nl-NL

## Pricing Information

### Subscription Costs
- **SaaS**: $30.00 USD per user per month
- **On-Premises**: €29.00 EUR per user per month

### What's Included
✅ **24/7 Online Support** - Technical assistance and troubleshooting  
✅ **Automatic Updates** - Latest features and security patches (SaaS only)  
✅ **GDPR Compliance** - Built-in data protection features  
✅ **Multi-language Support** - English and Dutch interface  
✅ **Outlook Integration** - Seamless email management  

## Trial Period

### Free Trial Benefits
- **30-day trial** period for full feature evaluation
- **No credit card required** for trial activation
- **Full functionality** available during trial
- **Technical support** included during trial period

### Trial Limitations
- Limited to evaluation purposes only
- Cannot be used for production data management
- Automatic conversion to paid subscription required after trial

## Support and Resources

### Getting Help
- **24/7 Online Support**: Available for all licensed users
- **Documentation**: [Q-Team Solutions Docs](https://www.q-teamsolutions.com/docs/category/dynamics-dossier/)
- **Email Support**: [support@q-team.nl](mailto:support@q-team.nl)  
- **Phone Support**: +31 030 820 13 33

### Additional Resources
- [Official Website](https://www.q-teamsolutions.com/product/dynamics-dossier/)
- [Release Notes](https://www.q-teamsolutions.com/docs/dynamics-dossier/release-notes)
- [FAQ](https://www.q-teamsolutions.com/docs/dynamics-dossier/faq)
- [Video Tutorials](https://www.youtube.com/channel/UC29xRURe_iZowUQnZYYTSOQ/)

## Troubleshooting Installation Issues

### Common Installation Problems
1. **Dependency Missing**: Ensure Q-Team App Authenticator and Drag & Drop are installed first
2. **Insufficient Permissions**: Verify administrator rights for installation
3. **License Issues**: Contact Q-Team Solutions for license validation
4. **Version Compatibility**: Confirm Business Central version compatibility

### Getting Installation Support
If you encounter installation issues:
1. Check the [troubleshooting guide](../troubleshooting/common-errors.md)
2. Contact Q-Team Solutions support with:
   - Your Business Central version
   - Installation error messages  
   - Environment details (SaaS/On-Premises)
   - User permissions information

---

*Next: Continue with [Configuration Setup](configuration.md) to complete your Dynamics Dossier implementation.*