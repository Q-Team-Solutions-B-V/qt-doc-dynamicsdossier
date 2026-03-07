# Common Errors

## Overview

This guide covers the most frequently encountered errors when using Dynamics Dossier and provides step-by-step solutions to resolve them quickly.

## Installation and Setup Errors

### Error: "Extension could not be installed"

**Problem**: Installation fails during the extension installation process

**Possible Causes**:
- Missing dependencies (Q-Team App Authenticator or Drag & Drop)
- Insufficient permissions
- Version compatibility issues
- Server/environment issues

**Solutions**:
1. **Check Dependencies**:
   - Ensure Q-Team App Authenticator is installed first
   - Verify Drag & Drop extension is installed
   - Check extension versions are compatible

2. **Verify Permissions**:
   ```
   Required Permissions:
   - Business Central Admin rights
   - Extension Management permissions
   - Company configuration access
   ```

3. **Check Version Compatibility**:
   - Verify Business Central version is 26.0 or higher
   - Confirm app version matches your BC version
   - Check for any conflicting extensions

### Error: "License not found or invalid"

**Problem**: License validation fails after installation

**Symptoms**:
- "License key is invalid" message
- Limited functionality available
- Trial period expired notifications

**Solutions**:
1. **Verify License Key**:
   - Check license key format (no spaces or special characters)
   - Ensure license key matches your organization
   - Confirm license is not expired

2. **Register License Properly**:
   ```
   Steps:
   1. Navigate to Dossier Setup
   2. Click "Register License"  
   3. Enter exact license key provided
   4. Restart Business Central session
   ```

3. **Contact Q-Team Solutions**:
   - Email: info@q-team.nl
   - Provide: Company name, license key, error message
   - Include: Business Central version and environment details

### Error: "Number series not found"

**Problem**: Dossier creation fails due to missing number series configuration

**Error Messages**:
- "Number series DOSSIER does not exist"
- "No. Series Code must have a value"
- "Cannot generate next number"

**Solutions**:
1. **Configure Number Series**:
   ```
   Path: Dossier Setup → Dossier Nos.
   
   Create Number Series:
   - Code: DOSSIER
   - Description: Dossier Numbers  
   - Starting No.: D0001
   - Ending No.: D99999
   - Increment-by No.: 1
   ```

2. **Verify Number Series Assignment**:
   ```
   Check Fields:
   - Dossier Setup → "Dossier Nos." field must be populated
   - Document Setup → "Document Nos." field must be populated
   ```

## Permission and Access Errors

### Error: "You do not have permission to access this page"

**Problem**: User lacks necessary permissions to access Dynamics Dossier features

**Symptoms**:
- Cannot access Dossier List
- Cannot create new dossiers
- Cannot view dossier content

**Solutions**:
1. **Assign Correct Permission Sets**:
   ```
   User Setup → Permission Sets:
   
   Standard Roles:
   - QTEAM DOSSIER - ADMIN (Full access)
   - QTEAM DOSSIER - USER (Standard user)
   - QTEAM DOSSIER - READ (Read-only)
   ```

2. **Check Company Access**:
   - Verify user has access to the correct company
   - Ensure user is not restricted by security filters
   - Confirm company-specific permissions are correct

3. **Verify Extension Permissions**:
   ```
   Extension Management:
   - Check Dynamics Dossier is enabled for user
   - Verify extension is installed in current environment
   - Confirm no permission conflicts exist
   ```

### Error: "Record is locked by another user"

**Problem**: Multiple users trying to access the same dossier simultaneously

**Solutions**:
1. **Wait and Retry**: Often resolves automatically within minutes
2. **Check Active Sessions**:
   ```
   Administration → Active Sessions
   - Identify user holding the lock
   - Contact user to close the record
   ```

3. **Force Unlock** (Admin only):
   ```
   Warning: Use with caution
   - End problematic user session
   - Clear temporary locks in database
   ```

## Workflow Errors

### Error: "Workflow could not be started"

**Problem**: Automated workflows fail to initiate or execute

**Symptoms**:
- Tasks not created automatically
- Email notifications not sent
- Workflow log shows no entries

**Solutions**:
1. **Verify Workflow Configuration**:
   ```
   Check:
   - Dossier Type has assigned workflow
   - Workflow Header exists and is active
   - Workflow Lines are properly configured
   ```

2. **Validate Workflow Conditions**:
   ```
   Required Elements:
   - Correct Dossier Type Code in workflow header
   - Valid date formulas in workflow lines
   - Proper workflow line sequencing
   ```

3. **Check Workflow Permissions**:
   ```
   User Permissions:
   - WORKFLOW-ADMIN (for setup)
   - WORKFLOW-USER (for execution)
   - Email notification permissions
   ```

### Error: "Email notifications not being sent"

**Problem**: Automatic email notifications fail to send

**Common Causes**:
- SMTP configuration issues
- Email queue problems
- Invalid recipient addresses

**Solutions**:
1. **Check Email Queue**:
   ```
   Path: Email Queue
   - Review failed email entries
   - Check error messages in queue
   - Retry failed emails manually
   ```

2. **Verify SMTP Settings**:
   ```
   Email Accounts Configuration:
   - SMTP server address correct
   - Authentication credentials valid
   - Port and security settings proper
   - Test connection successful
   ```

3. **Validate Email Addresses**:
   ```
   Check:
   - Contact email addresses are valid
   - No typos in email configuration
   - Recipients not blocking business emails
   ```

## Document Management Errors

### Error: "Cannot upload document"

**Problem**: Document upload functionality fails

**Symptoms**:
- Upload button unresponsive
- "File too large" errors
- "Invalid file type" messages

**Solutions**:
1. **Check File Constraints**:
   ```
   Limitations:
   - Maximum file size: 50MB (configurable)
   - Allowed formats: PDF, DOC, DOCX, XLS, XLSX, JPG, PNG
   - File name must not contain special characters
   ```

2. **Verify Drag & Drop Extension**:
   ```
   Requirements:
   - Drag & Drop extension must be installed
   - Extension must be enabled for current user
   - Browser compatibility for drag-and-drop
   ```

3. **Check Storage Configuration**:
   ```
   Dossier Setup:
   - Document Path must be accessible
   - Sufficient storage space available
   - Proper folder permissions configured
   ```

### Error: "Document could not be found"

**Problem**: Previously uploaded documents cannot be accessed

**Solutions**:
1. **Verify File Storage Path**:
   ```
   Check:
   - Document path in Dossier Setup is correct
   - Files exist in specified location
   - Folder permissions allow read access
   ```

2. **Check File System**:
   ```
   Validate:
   - Network connectivity to storage location
   - File not moved or deleted outside system
   - Storage server is accessible
   ```

## Data Integration Errors

### Error: "Contact not found"

**Problem**: Dossier creation fails due to invalid contact reference

**Solutions**:
1. **Verify Contact Exists**:
   ```
   Steps:
   1. Go to Contact List
   2. Search for contact by number/name
   3. Verify contact is not blocked
   ```

2. **Create Missing Contact**:
   ```
   If contact doesn't exist:
   1. Create new contact record
   2. Complete required fields
   3. Save and retry dossier creation
   ```

### Error: "Data synchronization failed"

**Problem**: Integration with external systems fails

**Solutions**:
1. **Check API Connections**:
   ```
   Verify:
   - Authentication tokens are valid
   - API endpoints are accessible
   - Network connectivity is stable
   ```

2. **Review Integration Logs**:
   ```
   Check:
   - Error messages in integration logs
   - Failed data transfers
   - Authentication failures
   ```

## Performance Issues

### Error: "System running slowly"

**Problem**: Dynamics Dossier pages load slowly or time out

**Common Causes**:
- Large datasets
- Inefficient filtering
- Network issues
- Server performance

**Solutions**:
1. **Optimize Filters**:
   ```
   Performance Tips:
   - Use date range filters
   - Filter by status or type
   - Limit result sets with $top parameter
   ```

2. **Database Maintenance**:
   ```
   Regular Tasks:
   - Rebuild indexes on dossier tables
   - Clean up old workflow logs
   - Archive closed dossiers
   ```

3. **Network Optimization**:
   ```
   Check:
   - Internet connectivity speed
   - Business Central server performance
   - Browser cache and cookies
   ```

## Browser and Client Errors

### Error: "Feature not working in web client"

**Problem**: Some functionality doesn't work in the browser

**Solutions**:
1. **Browser Compatibility**:
   ```
   Supported Browsers:
   - Microsoft Edge (recommended)
   - Google Chrome
   - Mozilla Firefox
   - Safari (limited support)
   ```

2. **Clear Browser Data**:
   ```
   Clear:
   - Browser cache
   - Cookies
   - Local storage
   - Session storage
   ```

3. **Check Browser Settings**:
   ```
   Enable:
   - JavaScript
   - Cookies
   - Pop-ups (if needed)
   - File downloads
   ```

## Error Reporting and Logging

### Collecting Error Information

When contacting support, provide:
1. **Error Details**:
   - Exact error message
   - Steps to reproduce
   - Time/date when error occurred

2. **Environment Information**:
   - Business Central version
   - Dynamics Dossier version
   - Browser version (if web client)

3. **System Information**:
   - Company name and environment
   - User permissions and roles
   - Recent changes or updates

### Debug Information Collection

1. **Browser Developer Tools**:
   ```
   Steps:
   1. Press F12 to open developer tools
   2. Go to Console tab  
   3. Reproduce the error
   4. Screenshot or copy error messages
   ```

2. **Business Central Event Log**:
   ```
   Administration → Event Log
   - Filter by date and source
   - Export relevant entries
   - Include in support request
   ```

## Getting Help

### Self-Service Resources
1. **Documentation**: [Q-Team Solutions Docs](https://www.q-teamsolutions.com/docs/category/dynamics-dossier/)
2. **FAQ**: [Frequently Asked Questions](../faq/faq.md)
3. **Release Notes**: Check latest updates and known issues

### Professional Support
1. **Email Support**: [info@q-team.nl](mailto:info@q-team.nl)
2. **Phone Support**: +31 030 820 13 33
3. **Emergency Support**: Available for production-critical issues

### Support Request Template
```
Subject: Dynamics Dossier Error - [Brief Description]

Environment Information:
- Business Central Version: 
- Dynamics Dossier Version:
- Company Name:
- Environment (Production/Sandbox):

Error Details:
- Error Message:
- Steps to Reproduce:
- Expected Behavior:
- Actual Behavior:

Additional Information:
- Browser/Client Used:
- Time of Error:
- Affected Users:
- Recent Changes:
```

### Escalation Process
1. **Level 1**: Standard email support (response within 24 hours)
2. **Level 2**: Technical specialist (response within 8 hours)  
3. **Level 3**: Development team (for complex technical issues)
4. **Emergency**: Phone support for production-critical issues

---

*Next: Review [Frequently Asked Questions](../faq/faq.md) for additional common issues and solutions.*