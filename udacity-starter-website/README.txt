
# 📦 Static Website Hosting on AWS S3 + CloudFront

This project demonstrates how to deploy a static website using **Amazon S3** and **CloudFront**. We'll configure S3 to host your website files and use CloudFront as a Content Delivery Network (CDN) for global distribution.

---

## 🚀 Project Steps

### 1. Create an S3 Bucket

1. Go to the **AWS Management Console**.
2. In the search bar, type **S3**, then click the **S3** service.
3. Click **Create bucket**.
4. Choose a **globally unique bucket name** (e.g., `my-123456789-bucket`).
5. Select a region.
6. In **Block Public Access settings**, **uncheck** `Block all public access`.
7. Click **Create bucket**.

---

### 2. Upload Files to the S3 Bucket

1. Click the name of your bucket.
2. Click the **Upload** button.
3. Click **Add files** and **Add folder**.
4. Upload the contents of the `udacity-starter-website` folder **one by one** (not the folder itself). This includes:
   - `index.html`
   - `css/`
   - `img/`
   - `vendor/`
5. Click **Upload**.

> ⚠️ If you encounter upload issues (e.g., ad blockers), try disabling ad blockers or use the AWS CLI instead.

---

### 3. [Optional] Upload via AWS CLI

```bash
aws configure list
aws configure
aws configure set aws_session_token "<TOKEN>" --profile default

# Upload files
aws s3api list-buckets
cd udacity-starter-website

aws s3api put-object --bucket my-bucket-name --key index.html --body index.html
aws s3 cp css/ s3://my-bucket-name/css/ --recursive
aws s3 cp img/ s3://my-bucket-name/img/ --recursive
aws s3 cp vendor/ s3://my-bucket-name/vendor/ --recursive
```

---

### 4. Add a Bucket Policy

1. Go to your bucket and click the **Permissions** tab.
2. Click **Edit** under **Bucket Policy**.
3. Paste the following (replace `my-bucket-name` with your actual bucket name):

```json
{
  "Version":"2012-10-17",
  "Statement":[
    {
      "Sid":"AddPerm",
      "Effect":"Allow",
      "Principal": "*",
      "Action":["s3:GetObject"],
      "Resource":["arn:aws:s3:::my-bucket-name/*"]
    }
  ]
}
```

---

### 5. Configure Static Website Hosting

1. Go to the **Properties** tab of your bucket.
2. Scroll to **Static website hosting** and click **Edit**.
3. Select **Enable**.
4. Set both the **Index document** and **Error document** fields to:

   ```
   index.html
   ```

5. Click **Save changes**.
6. Copy the **Website endpoint URL**—this is your public S3-hosted website link.

> 💡 Note: To access your site via this URL, the content must be publicly readable (as configured in the previous step).

---

### 6. Create a CloudFront Distribution

1. Go to the **CloudFront** service in the AWS console.
2. Click **Create Distribution**.
3. Under **Origin domain**, paste your S3 **Static Website Hosting endpoint**, for example:

   ```
   my-bucket.s3-website-us-west-2.amazonaws.com
   ```

4. Leave the rest of the settings at their defaults.
5. Scroll down and click **Create Distribution**.
6. Wait for the status to change from **In Progress** to **Deployed**.

---

### 7. Access Your Website

You can access your deployed website using:

- ✅ **CloudFront URL**  
  `https://<cloudfront-id>.cloudfront.net`

- 🌐 **S3 Website Endpoint**  
  `http://<bucket-name>.s3-website-<region>.amazonaws.com/`

- 📄 **S3 Object URL**  
  `https://<bucket-name>.s3.amazonaws.com/index.html`

> All three should display the same `index.html` content if permissions are correctly set.

---

## 🛠️ Troubleshooting

### Getting a 403 Access Denied Error?

Make sure:

- You used the **Static Website Hosting endpoint** (not the default S3 endpoint) for your CloudFront distribution.
- The S3 bucket has **public access enabled**.
- The **bucket policy** allows `s3:GetObject` for everyone (`Principal: "*"`).
- The `index.html` file is present and correctly named.
- CloudFront has **finished deploying** and caching the content.

Helpful resources:
- 🔗 [AWS Knowledge Center: Using S3 website endpoint with CloudFront](https://repost.aws/knowledge-center/cloudfront-serve-static-website)

---

## ✅ Done!

Your static website is now hosted globally using AWS S3 + CloudFront. 🎉
