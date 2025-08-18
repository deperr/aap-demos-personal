# ServiceNow Custom Credential Type Setup

This guide provides step-by-step instructions for manually creating the ServiceNow ITSM credential type in Ansible Automation Platform 2.5 through the web UI.

## Step-by-Step Instructions

### 1. Access Credential Types

- **Navigate to Credential Types**
    - From the "Overview" Page, click **Automation Execution** in the left sidebar
    - From the navigation menu, click **Infrastructure***
    - In the left sidebar, click **Credential Types**
    - You'll see the list of existing credential types

### 2. Create New Credential Type

1. **Start Creation**
   - Click the **Create credential type** button in the top center
   - This opens the "Create Credential Type" form

2. **Basic Information**
   - **Name**: `ServiceNow ITSM`
   - **Description**: `Credential type for ServiceNow ITSM operations`

### 3. Configure Input Configuration

In the **Input Configuration** field, paste the following YAML:

```yaml
fields:
  - id: servicenow_instance
    type: string
    label: ServiceNow Instance URL
    help_text: "ServiceNow instance URL (e.g., dev12345.service-now.com)"
  - id: servicenow_username
    type: string
    label: ServiceNow Username
  - id: servicenow_password
    type: string
    label: ServiceNow Password
    secret: true
  - id: servicenow_client_id
    type: string
    label: OAuth Client ID
    help_text: OAuth Client ID (optional, for OAuth authentication)
    secret: true
  - id: servicenow_client_secret
    type: string
    label: OAuth Client Secret (Optional)
    secret: true
    help_text: OAuth Client Secret (optional, for OAuth authentication)
required:
  - servicenow_instance
  - servicenow_username
  - servicenow_password
```

### 4. Configure Injector Configuration

In the **Injector Configuration** field, paste the following YAML:

```yaml
env:
  SERVICENOW_INSTANCE: '{{ servicenow_instance }}'
  SERVICENOW_PASSWORD: '{{ servicenow_password }}'
  SERVICENOW_USERNAME: '{{ servicenow_username }}'
  SERVICENOW_CLIENT_ID: '{{ servicenow_client_id }}'
  SERVICENOW_CLIENT_SECRET: '{{ servicenow_client_secret }}'
extra_vars:
  servicenow_instance: '{{ servicenow_instance }}'
  servicenow_password: '{{ servicenow_password }}'
  servicenow_username: '{{ servicenow_username }}'
  servicenow_client_id: '{{ servicenow_client_id }}'
  servicenow_client_secret: '{{ servicenow_client_secret }}'
```

### 5. Save the Credential Type

1. **Review Configuration**
   - Verify all fields are correctly entered
   - Check that YAML syntax is valid (no red error indicators)

2. **Save**
   - Click the **Save** button at the bottom of the form
   - You should be redirected back to the Credential Types list
   - Your new "ServiceNow ITSM" credential type should appear

## Create ServiceNow Credential Instance

After creating the credential type, you'll need to create a credential instance:

### 1. Navigate to Credentials

1. From the "Overview" Page, click **Automation Execution** in the left sidebar
2. From the navigation menu, click **Infrastructure***
3. In the left sidebar, click **Credentials**

### 2. Configure Credential

1. **Basic Information**
   - **Name**: `ServiceNow (Dev)` (or your preferred name)
   - **Description**: `ServiceNow ITSM credentials`
   - **Organization**: Select your organization from dropdown
   - **Credential Type**: Select `ServiceNow ITSM` (the type you just created)

2. **ServiceNow Fields** (these will appear based on your credential type)
   - **ServiceNow Instance URL**: `your-instance.service-now.com`
   - **ServiceNow Username**: `your_service_account`
   - **ServiceNow Password**: `your_password`
   - **OAuth Client ID**: (optional, leave blank if using username/password)
   - **OAuth Client Secret**: (optional, leave blank if using username/password)

3. **Save**
   - Click **Save** to create the credential

## Verification

### Test the Credential Type

1. **Check Credential List**
   - Your new ServiceNow credential should appear in the credentials list
   - The credential type should show as "ServiceNow ITSM"

2. **Use in Job Template**
   - When creating job templates, you should be able to select your ServiceNow credential
   - The credential will inject the variables into your playbook runs

### Verify Variable Injection

When a job runs with this credential, the following variables will be available:
- `servicenow_instance`
- `servicenow_username` 
- `servicenow_password`
- `servicenow_client_id` (empty string if not provided)
- `servicenow_client_secret` (empty string if not provided)