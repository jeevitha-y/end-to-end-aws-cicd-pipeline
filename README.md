## AWS Services Used
- GitHub
- AWS CodeConnections
- AWS CodePipeline
- AWS CodeBuild
- AWS Systems Manager Parameter Store
- Docker
- Docker Hub
# Step 1: Clone the Repository
```bash
git clone https://github.com/jeevitha-y/aws-dockerized-django-web-application.git
cd aws-dockerized-django-web-application
```
# Step 2: Create Docker Hub Credentials
Store your Docker Hub credentials in AWS Systems Manager Parameter Store.
| Parameter Name | Type |
|---------------|------|
| /docker/registry/username | String |
| /docker/registry/password | SecureString |
# Step 3: Create CodeBuild Project  
Create a new CodeBuild project.
### Source
GitHub (via CodeConnections)
Repository
```
jeevitha-y/aws-dockerized-django-web-application
```
Branch

```
main
```
Buildspec

```
Use buildspec.yml from source
```
Environment

- Managed Image
- Ubuntu
- Privileged Mode = Enabled (Required for Docker builds)

---
## CodeBuild Service Role Permissions

Attach the following permissions to the CodeBuild Service Role.

### AWS Managed Policy

- AmazonSSMFullAccess

### Additional IAM Permissions

```json
{
  "Effect": "Allow",
  "Action": [
    "codeconnections:GetConnection",
    "codeconnections:GetConnectionToken",
    "codeconnections:UseConnection"
  ],
  "Resource": "*"
}
```These permissions allow CodeBuild to:

- Read Docker Hub credentials from Parameter Store
- Access the GitHub repository through CodeConnections```


# Step 4: Create CodePipeline

Pipeline Stages

```
Source
   ↓
Build
```

### Source Stage

Provider

```
GitHub (via CodeConnections)
```

Repository

```
jeevitha-y/aws-dockerized-django-web-application
```

Branch

```
main
```

### Build Stage

Provider

```
AWS CodeBuild
```

Choose the CodeBuild project created in Step 3.

---

## CodePipeline Service Role

AWS automatically creates the CodePipeline Service Role.

No additional permissions were required.

---

# Step 5: Trigger the Pipeline

Commit changes.

```bash
git add .
git commit -m "Updated application"
git push origin main
```


