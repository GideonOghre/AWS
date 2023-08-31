# **Host A Static Website on AWS And CI CD Pipeline**

Project goal was to host a static website on AWS and implement continuous integration and deployment (CICD) using various AWS services. If user wants access to the website, the DNS service Namecheap will be accessed in the backend. The DNS service will have a CNAME record which will redirect the DNS to the CDN service (CloudFront distribution). Infront of CloudFront, an ACM will be used to secure the connection between the user and https. To automate the complete process, the website’s code will be hosted on a GitHub repository and whenever the developer’s code is uploaded to GitHub, it automatically triggers a code pipeline within AWS. The code pipeline will deploy the code to the S3 bucket.

## Create S3 Bucket
 I first created an S3 bucket with the same name as my website. I didn’t uncheck the ‘Block all public access’ box because I want to serve the website using CloudFront. I then chose the option to enable static web hosting from properties selection and added the index.html and the error.html to the bucket. Using the Object selection, I uploaded the static website and CSS page to the bucket. At this point the website is access has been denied because the website isn’t public yet.

![image](https://github.com/GideonOghre/Cloud-Projects/assets/74324801/e6f91252-cd18-42ce-a276-14b638a9f9c5)

![image](https://github.com/GideonOghre/Cloud-Projects/assets/74324801/159fbd55-5b3d-443d-9a33-db7121711bdf)

![image](https://github.com/GideonOghre/Cloud-Projects/assets/74324801/f9b9665c-d726-4061-b3d0-8c485886d8d0)



## Create CloudFront Distribution:
The next step is to create a CloudFront distribution, the S3 bucket created earlier will be used for the CloudFront’s original domain. For origin access, the legacy access identities are selected to enable access to original access identity, it will be used for a user that has access to the bucket and the bucket policy will be updated. For the additional settings the viewer protocol policy is set to ‘Redirect HTTP to HTTPS’ to remove HTTP access and for allow HTTP method is set to ‘GET, HEAD, OPTIONS, PUT, PUSH, PATCH, DELETE’ for more flexibility. To create a custom SSL certificate. I requested a certificate, and the domain name is confirmed by Namecheap. After creating the distribution, it took around 5 minutes to deploy.

![image](https://github.com/GideonOghre/Cloud-Projects/assets/74324801/366a3ae0-5e30-421e-a067-5ed368f71e6d)

## S3 Bucket Permissions:
A bucket policy was created when the CloudFront distribution was enabled which allows access to the custom CloudFront origin access identity. The object inside the S3 bucket can only be accessible through the CloudFront origin access identity. I then copied the identity into Namecheap to redirect it to the custom domain name.

![Screenshot 2023-08-31 at 16 46 17](https://github.com/GideonOghre/Cloud-Projects/assets/74324801/1cfc9932-94c3-45f5-bc09-666f6a64ad99)

## Automate CI/CD:
To automate the CI/CD I used Code Pipeline and connected AWS to my GitHub. I Chose Amazon S3 as my deploy provider with my domain name as the bucket. 

![image](https://github.com/GideonOghre/Cloud-Projects/assets/74324801/b92d2518-4dfa-4ad9-8153-8a13c2ca9351)

![image](https://github.com/GideonOghre/Cloud-Projects/assets/74324801/5ea13295-35ba-4c27-9395-3a34f7aaab49)

![image](https://github.com/GideonOghre/Cloud-Projects/assets/74324801/08018f47-b025-484a-98fc-6ce0413e6d9e)

How changing the code in GitHub automatically adds the changes to the website:
At this point, the code changes in GitHub aren’t updated to the static website because CloudFront caches the static content in its edge location. Once the cache is validated or pause, then we will see the updated content, this must be done manually in CloudFront by going to the invalidation selection and creating an invalidation by using ‘/*’ to have access to all the files. Now changes will automatically be updated to the website.

![image](https://github.com/GideonOghre/Cloud-Projects/assets/74324801/da230d2a-f7e5-4bcb-adb5-f40c3dc95963)

















