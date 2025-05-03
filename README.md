# Static Website Hosting on AWS S3 + CloudFront

[![AWS][aws-shield]][aws-url]
[![S3][s3-shield]][s3-url]
[![CloudFront][cloudfront-shield]][cloudfront-url]

<!-- PROJECT LOGO -->
<br />
<div align="center">
  

  <h3 align="center">AWS Static Website Deployment</h3>

  <p align="center">
    A comprehensive guide to deploying static websites using AWS S3 and CloudFront
    <br />
    <a href="https://github.com/almasoriaw/deploy-static-website-s3-aws_cloud/blob/main/docs/architecture.md"><strong>Explore the architecture »</strong></a>
    <br />
    <br />
    
  </p>
</div>

<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about-the-project">About The Project</a>
      <ul>
        <li><a href="#built-with">Built With</a></li>
      </ul>
    </li>
    <li>
      <a href="#getting-started">Getting Started</a>
      <ul>
        <li><a href="#prerequisites">Prerequisites</a></li>
        <li><a href="#installation">Installation</a></li>
      </ul>
    </li>
    <li><a href="#usage">Usage</a></li>
    <li><a href="#project-structure">Project Structure</a></li>
    <li><a href="#troubleshooting">Troubleshooting</a></li>
    <li><a href="#contributing">Contributing</a></li>
    <li><a href="#license">License</a></li>
    <li><a href="#contact">Contact</a></li>
    <li><a href="#acknowledgments">Acknowledgments</a></li>
  </ol>
</details>

<!-- ABOUT THE PROJECT -->
## About The Project

This project demonstrates how to deploy a static website using **Amazon S3** and **CloudFront**. We configure S3 to host your website files and use CloudFront as a Content Delivery Network (CDN) for global distribution. Static websites are ideal for hosting HTML, CSS, and JavaScript files that do not require server-side processing.

<p align="right">(<a href="#top">back to top</a>)</p>

### Built With

* [![AWS][aws-shield]][aws-url]
* [![S3][s3-shield]][s3-url]
* [![CloudFront][cloudfront-shield]][cloudfront-url]
* [![HTML5][html5-shield]][html5-url]
* [![CSS3][css3-shield]][css3-url]
* [![JavaScript][javascript-shield]][javascript-url]

<p align="right">(<a href="#top">back to top</a>)</p>

<!-- GETTING STARTED -->
## Getting Started

To deploy your own static website on AWS, follow these steps:

### Prerequisites

* AWS Account
* AWS CLI (optional)
* Basic understanding of AWS services (S3, CloudFront)
* Static website files

### Installation

#### 1. Create an S3 Bucket

1. Go to the **AWS Management Console**.
2. In the search bar, type **S3**, then click the **S3** service.
3. Click **Create bucket**.
4. Choose a **globally unique bucket name** (e.g., `my-123456789-bucket`).
5. Select a region.
6. In **Block Public Access settings**, **uncheck** `Block all public access`.
7. Click **Create bucket**.

![image](https://github.com/user-attachments/assets/497287f5-6b59-4f8f-b021-efb80e7820c8)

#### 2. Upload Files to the S3 Bucket

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

![image](https://github.com/user-attachments/assets/244783f6-bc19-457e-9176-ade9669f5067)


#### 3. [Optional] Upload via AWS CLI

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

#### 4. Add a Bucket Policy

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

![image](https://github.com/user-attachments/assets/ae013642-210e-4558-8c3d-5e18770ea7ff)


#### 5. Configure Static Website Hosting

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
![image](https://github.com/user-attachments/assets/3be2a75f-091e-4556-aaeb-2dbc83293b82)

#### 6. Create a CloudFront Distribution

1. Go to the **CloudFront** service in the AWS console.
2. Click **Create Distribution**.
3. Under **Origin domain**, paste your S3 **Static Website Hosting endpoint**, for example:

   ```
   my-bucket.s3-website-us-west-2.amazonaws.com
   ```

4. Leave the rest of the settings at their defaults.
5. Scroll down and click **Create Distribution**.
6. Wait for the status to change from **In Progress** to **Deployed**.

<p align="right">(<a href="#top">back to top</a>)</p>

![image](https://github.com/user-attachments/assets/0ab09b0a-9c62-449c-bf2c-d9cfa874a7a1)

![image](https://github.com/user-attachments/assets/0e678859-965b-49f3-a6b3-b87b2adfe3c2)


<!-- USAGE EXAMPLES -->
## Usage

You can access your deployed website using:

- ✅ **CloudFront URL**  
  `https://<cloudfront-id>.cloudfront.net`

- 🌐 **S3 Website Endpoint**  
  `http://<bucket-name>.s3-website-<region>.amazonaws.com/`

- 📄 **S3 Object URL**  
  `https://<bucket-name>.s3.amazonaws.com/index.html`

> All three should display the same `index.html` content if permissions are correctly set.

> ![image](https://github.com/user-attachments/assets/cc753f94-9773-45c5-b43e-c568e6c9d5e6)

<p align="right">(<a href="#top">back to top</a>)</p>

<!-- PROJECT STRUCTURE -->
## Project Structure

- `udacity-starter-website/` - Contains the static website files
- `screenshots_submission/` - Contains screenshots of the deployment process
- `docs/` - Additional documentation:
  - `architecture.md` - Detailed AWS architecture explanation
  - `setup.md` - Comprehensive setup instructions
  - `screenshots.md` - Description of provided screenshots

<p align="right">(<a href="#top">back to top</a>)</p>

<!-- TROUBLESHOOTING -->
## Troubleshooting

### Getting a 403 Access Denied Error?

Make sure:

- You used the **Static Website Hosting endpoint** (not the default S3 endpoint) for your CloudFront distribution.
- The S3 bucket has **public access enabled**.
- The **bucket policy** allows `s3:GetObject` for everyone (`Principal: "*"`).
- The `index.html` file is present and correctly named.
- CloudFront has **finished deploying** and caching the content.

Helpful resources:
- 🔗 [AWS Knowledge Center: Using S3 website endpoint with CloudFront](https://repost.aws/knowledge-center/cloudfront-serve-static-website)

<p align="right">(<a href="#top">back to top</a>)</p>

<!-- CONTRIBUTING -->
## Contributing

Contributions are what make the open source community such an amazing place to learn, inspire, and create. Any contributions you make are **greatly appreciated**.

If you have a suggestion that would make this better, please fork the repo and create a pull request. You can also simply open an issue with the tag "enhancement".

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

<p align="right">(<a href="#top">back to top</a>)</p>

<!-- LICENSE -->
## License

Distributed under the MIT License. See `LICENSE.txt` for more information.

<p align="right">(<a href="#top">back to top</a>)</p>



Project Link: [https://github.com/almasoriaw/deploy-static-website-s3-aws_cloud](https://github.com/almasoriaw/deploy-static-website-s3-aws_cloud)

<p align="right">(<a href="#top">back to top</a>)</p>

<!-- ACKNOWLEDGMENTS -->
## Acknowledgments

* [Udacity Cloud DevOps Engineer Nanodegree](https://www.udacity.com/)
* [AWS Documentation](https://docs.aws.amazon.com/)
* [Best-README-Template](https://github.com/othneildrew/Best-README-Template)

<p align="right">(<a href="#top">back to top</a>)</p>

<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->
[aws-shield]: https://img.shields.io/badge/AWS-FF9900?style=for-the-badge&logo=amazonaws&logoColor=white
[aws-url]: https://aws.amazon.com/
[s3-shield]: https://img.shields.io/badge/S3-569A31?style=for-the-badge&logo=amazons3&logoColor=white
[s3-url]: https://aws.amazon.com/s3/
[cloudfront-shield]: https://img.shields.io/badge/CloudFront-232F3E?style=for-the-badge&logo=amazonaws&logoColor=white
[cloudfront-url]: https://aws.amazon.com/cloudfront/
[html5-shield]: https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white
[html5-url]: https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/HTML5
[css3-shield]: https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white
[css3-url]: https://developer.mozilla.org/en-US/docs/Web/CSS
[javascript-shield]: https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black
[javascript-url]: https://developer.mozilla.org/en-US/docs/Web/JavaScript
