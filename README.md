# index.html
CI/CD Professional Resume Website
# ☁️ Cloud Security Engineer Portfolio Site

Hi! Here is my personal resume and portfolio website where I deployed an S3 static website using the CI/CD pipeline. 
This portfolio will be used to aid me in my pursuit of Cloud Security Engineering.  I have a background as an IT support specialist. I also have experience with Linux system administration and cybersecurity. I received CompTIA Security+ certification and am currently studying for the AWS Cloud Solutions Architect Associate certification.

My domain site: www.moirainthecloud.com

---

## 🚀 AWS Cloud Services: 

These are the AWS services and tools I used to create my website:
| IAM | Configured user account in IAM with specific permissions. Set up MFA for user accounts

| Hosting | AWS S3 (static website) | used this service to host my website materials. I created an S3 bucket that houses all my objects from GitHub; set region to us-east-1 (N. Virginia). 

| AWS CloudFront + AWS Certificate Manager | Because AWS CloudFront is a Content Delivery Network (CDN) service, I was able to use this for secure transmission, availability, and faster site loading for my domain. I requested an SSL certificate from AWS Certificate Manager (ACM) and then added the Secure Hypertext Transfer Protocol (HTTPS) label in CloudFront for encryption on my custom domain name. 

| IONOS | Domain Registrar

| AWS Route 53 | AWS Route 53 is the DNS service I used for my server.  I created a hosted zone in Route 53. From there I took the 4 NS records (nameservers) and put them into IONOS as my default nameservers. Now Route 53 is in control of where incoming traffic goes instead of IONOS.  

| CI/CD | AWS CodePipeline (GitHub → S3 auto-deploy) | I used AWS CodePipeline for automation for continuous integration and continuous delivery of my GitHub workflows file. This allows me to push code from GitHub to CodePipeline to host, test, and deploy my portfolio website.  

| Frontend | HTML5, CSS3, Bootstrap 5 | I used HTML5 for the structure of the text, CSS3 to design the appearance and layout, and Bootstrap 5 as my template for a faster build. Bootstrap 5 also allows me to make my website available on mobile devices


## ⚙️ Deployment Pipeline: 

1. **Push code to GitHub** → this triggers the AWS CodePipeline
2. **CodePipeline** → detects the latest change, then pulls the code from GitHub repository and syncs to S3 bucket. 
3. **S3** → stores the updated file into a private bucket overwriting any previous files. 
4. **CloudFront**→ CodePipeline automatically calls the CloudFront API to create a cache invalidation (`/*`) this allows for any changes made in GitHub to automatically update to the website; This means no manual uploads. Every `git push` to `main` goes live automatically.


## 🛠️ Deployment Pipeline Structure: 

[ Local Machine ] 
         │ 
         ▼ (git push)
   [ GitHub Repo ] ──── (Webhook Trigger) ───► [ AWS CodePipeline ]
                                                      │
                                                      ├──► 1. Fetches Git Artifacts
                                                      ├──► 2. Syncs Private S3 Bucket 
                                                      └──► 3. Triggers CloudFront Invalidation
                                                                   │
                                                                   ▼
                                                       [ Global Users See Updates ]

---
## AWS Cybersecurity Features of Custom Domain Site
| IAM - For safe practices I created a “user” in IAM with least privileges access to host, test, and deploy my application instead of using my root AWS account. I added MFA to protect the account and then added specific permissions to the user account in order to build the pipeline.

| CloudFront Origin Access Control (OAC) - created an S3 policy restricting access to CloudFront OAC only; this security feature prevents public access to files in AWS S3 Bucket which protects backend part of website from public access.
- Using AWS IAM Service Principal to authenticate and SigV4 cryptographic request signing, the bucket will ONLY respond to requests originating from my designated CloudFront distribution. This feature ignores the public internet altogether.

| Encryption – By utilizing SSL/TLS encryption (HTTPS), my pipeline is fully encrypted using custom digital certificates from AWS Certificate Manger.
-	Accessing the site using HTTP will automatically upgrade to an HTTPS label

| Reducing the Attack Surface – By configuring CloudFront with Open Access Control (OAC) I was able to reduce the angles an attacker might take to infiltrate or exfiltrate data from the site; locks any open back doors. 


## 🧱 Security Architecture Diagram

 [Internet User] 
        │
        ▼
   [IONOS Registrar] (Delegates DNS Authority)
        │
        ▼
  [AWS Route 53] (DNS Alias Resolution)
        │
        ▼
 [Amazon CloudFront] ─── (Manages SSL/TLS Certificate via ACM)
        │
        ▼ (Authenticated via SigV4 / Service Principal)
 [Origin Access Control]
        │
        ▼
  [Amazon S3 Bucket] (Locked: Block All Public Access)


## 📌 My Custom Domain Features: 

- Responsive design (mobile, tablet, desktop) using Bootstrap 5
- Bio and professional summary
- Projects section with AWS and Linux/bash work (GitHub link listed on site)
- CompTIA Security+ certification highlight
- Keynote/speaking appearances
- Skills grid (Cloud, Linux, Networking, Security
This project is personal and not licensed for redistribution. Feel free to use the architecture as a reference for your own static site + AWS pipeline setup.
