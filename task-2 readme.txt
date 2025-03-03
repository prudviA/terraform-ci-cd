Let's set up a CI/CD pipeline using GitHub Actions for Terraform step by step.



 Step 1: Set Up Your GitHub Repository
Since you already have a GitHub repository, ensure it is linked to your local Git repository:


git remote -v
If not, add the remote:


git remote add origin https://github.com/yourusername/devops-task.git
Push the existing code (if not already pushed):


git push -u origin master
🔹 Step 2: Create a GitHub Actions Workflow File
In your repository, navigate to:
.github/workflows/terraform-ci-cd.yml
If .github/workflows/ doesn't exist, create the folders.

Create the workflow file:

mkdir -p .github/workflows
touch .github/workflows/terraform-ci-cd.yml
Open terraform-ci-cd.yml and add the following configuration:

yaml

name: Terraform CI/CD

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  terraform:
    name: Terraform Apply
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v3

      - name: Setup Terraform
        uses: hashicorp/setup-terraform@v2
        with:
          terraform_version: 1.5.5  # Change this to your Terraform version

      - name: Terraform Init
        run: terraform init

      - name: Terraform Format
        run: terraform fmt -check

      - name: Terraform Validate
        run: terraform validate

      - name: Terraform Plan
        run: terraform plan

      - name: Terraform Apply
        if: github.ref == 'refs/heads/main'
        run: terraform apply -auto-approve
🔹 Step 3: Configure GitHub Secrets (If Required)
If your Terraform setup requires AWS credentials, store them in GitHub Secrets:

Go to GitHub Repo → Settings → Secrets and variables → Actions → New repository secret
Add these secrets:
AWS_ACCESS_KEY_ID
AWS_SECRET_ACCESS_KEY
Modify the workflow to use these secrets:

      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v2
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: us-east-1  # Change to your AWS region
🔹 Step 4: Commit and Push Your Workflow

git add .github/workflows/terraform-ci-cd.yml
git commit -m "Added GitHub Actions CI/CD for Terraform"
git push origin master
🔹 Step 5: Verify GitHub Actions Execution
Go to your GitHub repository.
Click on the "Actions" tab.
Check if your workflow runs successfully when you push changes.
🎯 Deliverables for Task 2:
✅ GitHub Actions workflow file: .github/workflows/terraform-ci-cd.yml
✅ A successful Terraform Plan and Apply execution in GitHub Actions
✅ AWS credentials (if deploying to AWS) stored in GitHub Secrets.
 