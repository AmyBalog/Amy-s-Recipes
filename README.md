# Steps to Create a Static Website Hosted on AWS S3

Link to recipe website hosted on AWS S3: <br>
http://my-recipes-website.s3-website-us-west-2.amazonaws.com <br><br>

**Step 1: Create an S3 bucket on AWS:**
1. If not already created, create an AWS account
2. In the Search bar, type S3 and enter
3. Click on Create bucket
4. Select the Region where you would like to host your bucket
5. For Bucket type, select General Purpose
6. Choose your Bucket name
   - the name must be globally unique
7. For Object Ownership, keep the ACLs disabled
8. For Block Public Access
   - Normally, you would want to keep this blocked for security purposes if it contains confidential information
   - But, to create a static website, we must disable this and allow public access
9. Can leave the remaining options on default and click Create bucket

**Step 2: Adding items to S3 bucket:**
1. Click on the bucket you just created
2. Click on Upload to upload files or folders into your S3 bucket
3. If you click on the uploaded object, you can click on it to view its properties
4. If click on Open, it will open the item in a web page.
5. But if you copy the Object URL and paste it into the browser, the access will be denied.
     - This means you cannot access the item through the public URL, but can access via the Open button from the S3 bucket. This is an S3 pre-signed URL that contains a signature that verifies your credentials encoded.
     - The public URL needs to have a Bucket Policy to enable public access. 

**Step 3: Create a Bucket Policy to allow public access:**
1.  In your S3 bucket, click on the Permissions tab
2.  Click Edit to make sure the Block all public access is unchecked.
     - This needs to be unchecked to allow public access
3. Under Bucket Policy, need to click Edit to create a new Bucket policy
4. Can look at Policy examples or click Policy generator
5. Click Policy generator
6. Effect: Allow; Principle: *; AWS Service: Amazon S3; Actions: GetObject; Amazon Resource Name (ARN): can be found at the top of the Bucket policy page. Copy that ARN and at the end of it add: /\*
   - this action of GetObject applies to objects in your objects which are after the "/"
7. Click Add Statement
8. Click Generate Policy and copy the policy that displays
9. Paste the copied policy into the Bucket Policy. Save changes. 
    - This policy means that get objects are allowed from anyone on any object in that S3 bucket.
10. Now the object URL is publicly available

**Step 4: How to create a static website**
1. If the bucket policy does not allow public read, you will get a 403 Forbidden error
2. Click into the S3 bucket and click on Properties
3. Scroll to the bottom and you will see Static website hosting is disabled
4. Click Edit, then click Enable
5. Select Host a static website radio button
6. For the Index document, specify the default page of the website, which is usually index.html
7. If not already uploaded, upload your index.html file and any other files needed for your website
8. Click on Properties again and scroll down to the Static Website Hosting, there is a Bucket website endpoint URL
9. Copy the URL and paste it into a browser. You now have access to your website.

**Step 5: Add Route 53**
   - This will provide DDoS protection, a custom URL, and redirection
   - AWS does charge for this feature
1. Go to the Route 53 console
2. Click Registered domains in the left column to first register a domain name
3. Add the domain to the cart and register your information
4. On the left-hand side, click on Hosted Zones
5. Enter your domain name
6. Choose Public Hosted Zone, then click Create

**Step 6: Add CloudFront**
- This optional feature will reduce latency for your users by caching the content at an Edge location and reduce latency. DDoS protection is also provided.
1. Go to the CloudFront console
2. Click Create a CloudFront distribution
3. Choose an Origin domain which will be your S3 bucket
4. You have the option to use website endpoint which is recommended
5. Can enable Web Application Firewall for security
6. Can leave all other items default for now and click Create distribution
7. The distribution can take some time for creation. Once deployed, you copy the domain name and have access to it.
8. Now if you refresh the page, the page is served from the CloudFront cache rather than the S3 bucket itself which will provide quicker loading 
