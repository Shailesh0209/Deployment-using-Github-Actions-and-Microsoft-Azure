# Deployment-using-Github-Actions-and-Microsoft-Azure
To deploy your GitHub repository using GitHub Actions and Microsoft Azure, follow these steps:

## Set Up Azure Resources

1. Create an Azure account if you don't have one already.
2. Set up an Azure App Service plan and a Web App for your application[1].

## Configure GitHub Actions Workflow

1. In your GitHub repository, create a new file named `azure-deploy.yml` in the `.github/workflows/` directory[1].
2. Add the following content to the file:

```yaml
name: Deploy to Azure

on:
  push:
    branches:
      - main  # or your default branch

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v4
    
    - name: Azure Login
      uses: azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
    
    - name: Deploy to Azure Web App
      uses: azure/webapps-deploy@v2
      with:
        app-name: YOUR_APP_NAME
        subscription-id: ${{ secrets.AZURE_SUBSCRIPTION_ID }}
```

Replace `YOUR_APP_NAME` with your actual Azure Web App name[1][2].

## Set Up Authentication

Create a Service Principal for secure authentication:

1. Open Azure Cloud Shell or use Azure CLI locally.
2. Run the following command:

```bash
az ad sp create-for-rbac --name "myGitHubActions" --role contributor --scopes /subscriptions/{subscription-id}/resourceGroups/{resource-group} --sdk-auth
```

Replace `{subscription-id}` and `{resource-group}` with your actual values[2].

## Configure GitHub Secrets

1. Go to your GitHub repository settings.
2. Navigate to "Secrets and variables" > "Actions".
3. Add the following secrets:
   - `AZURE_CREDENTIALS`: Paste the entire JSON output from the Service Principal creation.
   - `AZURE_SUBSCRIPTION_ID`: Your Azure subscription ID[2].

## Customize Your Workflow

Modify the workflow file based on your application type. For example, for a Node.js app, add these steps before the deployment:

```yaml
    - name: Set up Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '14'
    
    - name: Install dependencies
      run: npm ci
    
    - name: Build
      run: npm run build
```

## Commit and Push

Commit the workflow file and push it to your GitHub repository. This will trigger the GitHub Actions workflow, which will build your application and deploy it to Azure[1][2].

By following these steps, you'll have set up a continuous deployment pipeline that automatically deploys your application to Azure whenever you push changes to your main branch[5].

Citations:
[1] https://learn.microsoft.com/en-us/azure/app-service/deploy-github-actions?WT.mc_id=javascript-11915-gllemos
[2] https://docs.github.com/en/actions/use-cases-and-examples/deploying/deploying-nodejs-to-azure-app-service
[3] https://github.com/marketplace/actions/azure-webapp
[4] https://learn.microsoft.com/en-us/visualstudio/azure/azure-deployment-using-github-actions?view=vs-2022
[5] https://learn.microsoft.com/en-us/azure/app-service/deploy-container-github-action
[6] https://github.com/Azure/webapps-deploy
[7] https://azure.github.io/AppService/2020/04/14/Get-Started-with-GitHub-Actions-and-Azure-Webapps.html
[8] https://learn.microsoft.com/en-us/azure/azure-functions/functions-how-to-github-actions?tabs=linux%2Cpython&pivots=method-manual
[9] https://docs.github.com/en/actions/use-cases-and-examples/deploying/deploying-to-azure-static-web-app
[10] https://docs.github.com/en/actions/use-cases-and-examples/deploying/deploying-with-github-actions
[11] https://www.youtube.com/watch?v=QP0pi7xe24s
[12] https://learn.microsoft.com/en-us/azure/developer/github/github-actions
