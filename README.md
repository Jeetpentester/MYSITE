Step-by-step implementation steps to set up and run the GitHub Actions workflow you provided:
“Sync Dependabot to Azure Boards (No Duplicates)”

What This Workflow Does
It Fetches open Dependabot alerts (with pagination)
Checks if corresponding work items already exist in Azure Boards by using a custom field GHSAID
If not found, creates new tasks in Azure Boards for each alert
Skips already-created ones, and logs the outcome

Implementation Steps
🔹 Step 1: Prepare Your GitHub Repository
Navigate to your repo (or create a new one).
Ensure Dependabot is enabled and configured to generate alerts.
Go to GitHub: https://github.com/settings/tokens
Click “Fine-grained tokens” or “Classic tokens” (either will work)
Click Generate new token
Set: Name: something like dependabot-sync-token
Expiration: choose appropriate expiration (e.g., 90 days or no expiration)
Repository access: select "Only selected repositories" or "All repositories"
Permissions:repo → Full control
security_events →  Read access
Click Generate token
Copy the token (you won’t see it again!)
Add it as a secret to your repo:
Go to your GitHub repo → Click Settings
Left sidebar → Secrets and variables → Actions
Click New repository secret
Name: GH_DEPENDABOT_PAT
Value: Paste the token
Click Add secret

Create AZURE_DEVOPS_PAT (Azure DevOps Token)
1. Go to Azure DevOps: https://dev.azure.com/
Click your profile icon (top-right) → Security
Under Personal Access Tokens, click + New Token
Set:Name: e.g., dependabot-to-azure
rganization: your Azure DevOps org
Scopes:Choose Custom defined
Enable:Work Items (Read & Write)
Set Expiration as needed
Click Create
Copy the token (you’ll only see it once)
Add it to GitHub as a secret:
Go back to your GitHub repo → Settings → Secrets and variables → Actions
Click New repository secret
Name: AZURE_DEVOPS_PAT
Value: Paste the token
Click Add secret

Step 2: Create Workflow File
In your repo, go to .github/workflows/
Create a new file: dependabot-to-azure.yml
Use the Scrit attached into the repo "dependabot-to-azure.yml" 
Update the Create Azure Boards Tasks step with your actual Azure DevOps organization and project info:
env:
  AZURE_ORG: https://dev.azure.com/YOUR_ORG_NAME
  AZURE_PROJ: YOUR_PROJECT_NAME
Step 4: Install Required Tools
This script uses:
curl and jq (already available in ubuntu-latest GitHub runners)
No additional setup needed here.

🔹 Step 5: Run the Workflow
You can trigger the workflow in two ways:
A. Manual Run
Go to Actions tab → Choose “Sync Dependabot to Azure Boards” → Click Run workflow
B. Scheduled Run
The cron expression 0 3 * * * runs every day at 03:00 UTC

🔹 Step 6: Monitor Output
In the GitHub Actions run logs, you’ll see:
Total number of alerts
Which were created
Which were skipped (duplicates)
Which failed (with status codes)

 Custom Field in Azure Boards
This script uses a custom field Custom.GHSAID in Azure DevOps:


🔹 Step 7:You must create this custom field in your Azure DevOps project:
Go to Azure DevOps → Project Settings → Process
Choose the process your project is using (e.g., Agile, Scrum, etc.)
Go to Work Item Types → Task
Click New Field
Name: Severity
Click Add Field


Output :






![azure Board](https://github.com/user-attachments/assets/18c8ac50-7301-4b84-92a9-83248168836f)



![Dashboard](https://github.com/user-attachments/assets/8f7970f0-ea09-43ee-89b5-2b2573ae3f51)



  
