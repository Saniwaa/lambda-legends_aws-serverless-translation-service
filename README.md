# AWS Translate Application - Complete Project

## 🏗️ Architecture Overview

This project creates a serverless translation application using AWS services, deployed with Terraform and GitHub Actions.

```
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│   React Frontend│    │   API Gateway    │    │  Lambda Function│
│   (CloudFront)  │───▶│                  │───▶│  (Translation)  │
│                 │    │                  │    │                 │
└─────────────────┘    └──────────────────┘    └─────────────────┘
         │                                                │
         │                                                ▼
         │              ┌──────────────────┐    ┌─────────────────┐
         │              │   AWS Cognito    │    │  AWS Translate  │
         └─────────────▶│  (Authentication)│    │     Service     │
                        │                  │    │                 │
                        └──────────────────┘    └─────────────────┘
                                                          │
                                                          ▼
                        ┌──────────────────┐    ┌─────────────────┐
                        │   S3 Buckets     │    │   S3 Buckets    │
                        │  (Requests)      │◀───│  (Responses)    │
                        │                  │    │                 │
                        └──────────────────┘    └─────────────────┘
```

## 📁 Project Structure

```
aws-translate-app/
├── .github/
│   └── workflows/
│       └── deploy.yml                 # GitHub Actions CI/CD pipeline
├── infrastructure/
│   ├── main.tf                        # Main Terraform configuration
│   ├── variables.tf                   # Terraform variables
│   ├── outputs.tf                     # Terraform outputs
│   └── backend.tf                     # Remote state configuration
├── lambda/
│   ├── translate_function.py          # Lambda function code
│   ├── requirements.txt               # Python dependencies
│   └── lambda_deployment.zip          # Deployment package (auto-generated)
├── frontend/
│   ├── public/
│   │   └── index.html                 # HTML template
│   ├── src/
│   │   ├── components/
│   │   │   ├── TranslationForm.js     # Upload and translation component
│   │   │   └── AuthComponent.js       # Authentication component
│   │   ├── App.js                     # Main React application
│   │   ├── index.js                   # React entry point
│   │   ├── aws-config.js              # AWS configuration
│   │   └── index.css                  # Styling
│   ├── package.json                   # Node.js dependencies
│   └── build/                         # Built React app (auto-generated)
├── sample-files/
│   └── sample-translation.json        # Example translation request
├── README.md                          # This documentation
└── .gitignore                         # Git ignore file
```

## 🚀 Getting Started

### Prerequisites

1. **AWS Account** with CLI configured
2. **GitHub Account** for repository and Actions
3. **Node.js** (v14 or later) installed locally
4. **Terraform** (v1.0 or later) installed locally
5. **Python 3.9** installed locally

### Step 1: Initial Setup

1. **Create your backend S3 bucket for Terraform state:**
   ```bash
   aws s3 mb s3://your-terraform-state-bucket-unique-name
   aws s3api put-bucket-versioning --bucket your-terraform-state-bucket-unique-name --versioning-configuration Status=Enabled
   ```

2. **Clone and setup the project:**
   ```bash
   git clone <your-repo-url>
   cd aws-translate-app
   ```

3. **Update the backend configuration:**
   - Edit `infrastructure/backend.tf`
   - Replace `your-terraform-state-bucket-unique-name` with your actual bucket name

### Step 2: Configure GitHub Secrets

Add these secrets to your GitHub repository (Settings → Secrets and variables → Actions):

- `AWS_ACCESS_KEY_ID`: Your AWS access key
- `AWS_SECRET_ACCESS_KEY`: Your AWS secret key
- `AWS_REGION`: Your preferred AWS region (e.g., `us-east-1`)
- `TF_STATE_BUCKET`: Your Terraform state bucket name

### Step 3: Deploy the Infrastructure

1. **Push to GitHub** - The GitHub Actions pipeline will automatically:
   - Deploy Terraform infrastructure
   - Build and deploy the Lambda function
   - Build and deploy the React frontend

2. **Monitor the deployment** in the GitHub Actions tab

### Step 4: Access Your Application

After successful deployment, you'll find the CloudFront URL in the GitHub Actions output or Terraform outputs.

## 🔧 Local Development

### Running Terraform Locally

```bash
cd infrastructure
terraform init
terraform plan
terraform apply
```

### Running the Frontend Locally

```bash
cd frontend
npm install
npm start
```

### Testing the Lambda Function Locally

```bash
cd lambda
pip install -r requirements.txt
python translate_function.py
```

## 📝 Usage

1. **Access the web application** using the CloudFront URL
2. **Sign up/Login** using the authentication system
3. **Upload a JSON file** with translation requests
4. **View translated results** in the interface

### JSON Input Format

```json
{
  "source_language": "en",
  "target_language": "es",
  "texts": [
    "Hello, world!",
    "How are you today?",
    "This is a test translation."
  ]
}
```

## 🛠️ Customization

### Adding New Languages

AWS Translate supports many language codes. Update the frontend dropdown in `TranslationForm.js` to add more options.

### Modifying Lambda Function

Edit `lambda/translate_function.py` to add new features like:
- Batch processing
- Different file formats
- Custom terminology

### Frontend Styling

Modify `frontend/src/index.css` and use Amplify UI components for consistent styling.

## 🔍 Troubleshooting

### Common Issues

1. **Terraform State Conflicts:**
   ```bash
   terraform force-unlock <lock-id>
   ```

2. **Lambda Permission Errors:**
   - Check IAM roles in AWS Console
   - Verify Lambda execution role has proper permissions

3. **Frontend Authentication Issues:**
   - Verify Cognito User Pool configuration
   - Check AWS Amplify configuration

4. **CORS Errors:**
   - Ensure API Gateway has CORS enabled
   - Check CloudFront distribution settings

### Logs and Monitoring

- **Lambda Logs:** CloudWatch Logs → `/aws/lambda/translate-function`
- **API Gateway Logs:** CloudWatch Logs → API Gateway execution logs
- **Frontend Errors:** Browser developer console

## 💰 Cost Considerations

This project is designed to work within AWS Free Tier:

- **AWS Translate:** 2M characters/month (first 12 months)
- **Lambda:** 1M requests + 400,000 GB-seconds/month
- **S3:** 5 GB storage + request quotas
- **CloudFront:** 1 TB data transfer out/month
- **Cognito:** 50,000 MAUs

## 🔐 Security Best Practices

- All resources use least-privilege IAM policies
- HTTPS enforced on all endpoints
- Cognito handles user authentication securely
- S3 buckets configured with proper access controls

## 📚 Additional Resources

- [AWS Translate Documentation](https://docs.aws.amazon.com/translate/)
- [Terraform AWS Provider](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)
- [AWS Amplify UI Components](https://ui.docs.amplify.aws/)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## 📄 License

This project is licensed under the MIT License.

---

**Happy Translating! 🌍**
