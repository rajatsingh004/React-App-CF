# React App
AWS S3 with Static Hosting and Cloudfront
React App which points to a DNS which goes through the CDN to route us to the the website.
Setting up of a certificate and connecting it to the CDN.

Go to AWS Console:

Create a bucket with your website name but without www and one with www. And then upload the files and folder of the npm app.
Why both? Bcoz one of them is going to be the authority so it'll have all of our content and the other is jsut for the redirect.

On the main bucket, which is the one which includes www.
In S3 Bucket Permissions, don't change Block all public access.
Now add the bucket policy which allow to anyone to call the S3 get object API.
Noe enable the static website hosting and select the option to host a static website. Select https protocol.

And for the other bucket just go to the static website hosting option and enable it. ANd then select the redirect requests for an object option.
And in hostname, give the name of the other bucket which has www included.

Now we to the DNS resolver and domain registration, Route53.
Just register a domain with Route53.
Go to the hosted zone where we should have 2 records of type NS which are name servers and SOA.
Now create the records set and then select the choice of your routing. And then add the two buckets in two different records.

Now you can access your bucket with the website name but it won't be secure.

Now we set up our CDN and also the certificates.
Go to the certificates manager and then provision certificates. Request a public certificate and give our domain name with and without www.
For the validation to complete add the CNAMEs to the record set of your domain.
If you go ur website right now it will be serving the content from S3 but once we setup the CDN it'll be served through the CDN.
Now we go to CloudFront to set up our regional distribution to do two things. One is to allow caching and other to enable https for our buckets.
Select create distribution. In origin domain name, give the s3 bucket website endpoint which you can get by going to the static website hosting section of the bucket.
Now change the viewer protocol policy to redirect http to https. Add CNAMEs if required. Now select the custom SSL certificate which we created earlier.  
Also in the end have the distribution state to enabled.
Repeat the same process for the other bucket.

Now we upate our route53 settings to point to the CDN. As earlier they were pointing to S3. Update for both the records, with www and without www. Now route the traffic to the CDN.  
