# Mini Projects: Hosting a Static Website on AWS S3 & Using Amazon Macie to Identify Sensitive Data in S3

## Mini Project 1: Hosting a Static Website on AWS S3

### Step-by-Step Instructions

#### Create a Bucket in S3
1. Navigate to the S3 service in the AWS Management Console.
2. Click on "Create bucket."
3. Enter a unique bucket name (e.g., `my-unique-bucket-name`).
4. Choose a region close to your location for better performance.
5. Click "Create" to create the bucket.

#### Upload Website Files
1. Click on the bucket name you just created.
2. Click "Upload" and then drag and drop all your website files (HTML, CSS, JS) into the upload area.
3. Click "Upload" to start the process.

#### Set Permissions for the Bucket
1. Go to the "Permissions" tab of your bucket.
2. Click on "Bucket Policy."
3. Copy and paste the following JSON policy, replacing `YOUR-BUCKET-NAME` with your actual bucket name:
    ```json
    {
      "Version": "2012-10-17",
      "Statement": [
        {
          "Sid": "PublicReadGetObject",
          "Effect": "Allow",
          "Principal": "*",
          "Action": "s3:GetObject",
          "Resource": "arn:aws:s3:::YOUR-BUCKET-NAME/*"
        }
      ]
    }
    ```
4. Click "Save."

#### Enable Static Website Hosting
1. Go to the "Properties" tab of your bucket.
2. Scroll down to "Static website hosting."
3. Click "Edit" and select "Enable."
4. Enter `index.html` for the Index document and `error.html` for the Error document (if you have one).
5. Click "Save changes."

#### Access Your Website
1. You will see the "Bucket website endpoint" in the static website hosting settings. This is the URL to access your website.
2. Open the URL in your browser to view your website.

### Summary
You've now hosted a static website on AWS S3. Your website files are uploaded to the S3 bucket, permissions are set to allow public access, and static website hosting is enabled to serve your files as a website.

## Mini Project 2: Using Amazon Macie to Identify Sensitive Data in S3

### Step-by-Step Instructions

#### Create and Upload Sample Data to S3
1. Navigate to the S3 service in the AWS Management Console.
2. Click "Create bucket."
3. Enter a unique bucket name (e.g., `your-initials-mixed-data-1234`) and choose the same region throughout the project.
4. Click "Create."
5. Upload the provided sample data files (`cc.txt`, `employees.txt`, `keys.txt`, `plates.txt`, and an image file) to this bucket.

#### Enable Amazon Macie
1. Navigate to the Macie service in the AWS Management Console.
2. Click "Get started" and then "Enable Macie."

#### Create a Macie Job to Analyze the Bucket
1. Go to the "S3 buckets" section in Macie.
2. Select your bucket (e.g., `your-initials-mixed-data-1234`).
3. Click "Create Job."
4. Select "One-time job" and click "Next."
5. Choose "All" for managed data identifiers and click "Next."
6. Skip the custom data identifiers and allow lists sections by clicking "Next."
7. Name your job (e.g., `sensitive-data-job`) and click "Next."
8. Review the job configuration and click "Submit."

#### Wait for Job Completion and Review Results
1. Wait for the job to complete (can take up to 20 minutes).
2. Once complete, click "Show results" and then "Show findings."
3. Review the findings to see the types of sensitive data Macie has identified in your S3 bucket.

#### Set Up Notifications with SNS and EventBridge
1. Navigate to the SNS service.
2. Create a new topic named `Macie-Alerts`.
3. Create a subscription for the topic with your email address.
4. Confirm the subscription via the email you receive.
5. Navigate to the EventBridge service.
6. Create a rule named `macie-events`.
7. Set the event source to AWS services and select "Macie."
8. Set the event type to "Macie Finding."
9. Set the target to the SNS topic you created (`Macie-Alerts`).
10. Review and create the rule.

#### Create a Custom Data Identifier for License Plates
1. Go back to the Macie console.
2. Under "Settings," click "Custom data identifiers."
3. Click "Create" and name it `LicensePlates`.
4. Enter the provided regular expression for Australian license plates.
5. Submit the custom data identifier.

#### Create Another Macie Job for Custom Identifier
1. Go to the "S3 buckets" section in Macie.
2. Select your bucket and click "Create Job."
3. Select "One-time job" and click "Next."
4. Choose "All" for managed data identifiers and click "Next."
5. Select the custom data identifier `LicensePlates` and click "Next."
6. Skip the allow lists section and click "Next."
7. Name your job (e.g., `licenseplates-job`) and click "Next."
8. Review the job configuration and click "Submit."
9. Wait for the job to complete and review the findings for the new custom data identifier.

#### Clean Up
1. Delete the SNS topic and subscriptions.
2. Disable Macie if it was enabled specifically for this project.
3. Delete the EventBridge rule.
4. Empty and delete the S3 bucket you created.

### Summary
You've now used Amazon Macie to analyze S3 bucket data for sensitive information, created notifications for findings, and defined custom data identifiers to detect specific patterns like Australian license plates.
