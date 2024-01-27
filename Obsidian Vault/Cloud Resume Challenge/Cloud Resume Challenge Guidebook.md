
# The Challenge
## 1. Certification
- Your resume needs to have the *[AWS Cloud Practitioner certification](https://aws.amazon.com/certification/certified-cloud-practitioner/)* 
	- Introductory certification that orients you on the industry-leading AWS cloud
		- if you have a more advanced AWS cert, or a cert on a different cloud, that's fine but not expected
		- no cheating: include the validation code on the resume
		- you can sit the CCP exam online for $100USD
			- if that cost is a dealbreaker for you, let me know and I'll see if I can help
	- Here are some free exam prep resources:
		- https://www.freecodecamp.org/news/aws-certified-cloud-practitioner-certification-study-course-pass-the-exam/
## 2. HTML
- Your resume needs to be written in [HTML](https://developer.mozilla.org/en-US/docs/Web/HTML).
	- not a Word doc or a PDF.
	- Here is an example of what I mean:
		- https://codepen.io/emzarts/pen/OXzmym
## 3. CSS
- Your resume needs to be styled with CC
- no worries if you're not a designer
	- it doesn't have to be fancy
	- we do need to see something other than raw HTML when we open the webpage

## 4. Static S3 Website
- Your HTML resume should be deployed online as an Amazon S3 static website
- services like Netlify and GitHub Pages are great and I would normally recommend them for personal static site deployments
	- they make little sense here and too abstract for our purposes
- Use S3.

## 5. HTTPS
- the S3 website URL should use HTTPS for security.
- You will need to use Amazon CloudFront to help with this.

## 6. DNS
- Point a custom DNS domain name to the CloudFront distribution...
- this is so your resume can be accessed at something like `my-c00l-resume-website.com`
	- you can use Amazon Route 53 or any other DNS provider for this
- A domain name usually costs about ten bucks to register

## 7. Javascript
- Your resume webpage should include a visitor counter that displays how many people have accessed the site
	- You will need to write a bit of Javascript to make this happen
	- Here is a helpful tutorial to get you started in the right direction:
		- https://www.codecademy.com/learn/introduction-to-javascript

## 8. Database
- The visitor counter will need to retrieve and update its count in a database somewhere
- I suggest you use Amazon's DynamoDB for this
	- Use on-demand pricing for the database and you'll pay essentially nothing
		- unless you store or retrieve much more data than this project requires.
		- Here is a great free course on DynamoDB:
			- https://aws.amazon.com/dynamodb/getting-started/

## 9. API
- Do not communicate directly with DynamoDB from your Javascript code
- Instead, you will need to create an API that accepts requests from your web app and communicates with the database
	- I suggest using AWS's API Gateway and Lambda services for this
		- they will be free or close to free for what we are doing

## 10. Python
- you will need to write a bit of code in the Lambda function
	- you could use more Javascript, but it would be better for our purposes to explore Python
		- common language used in back-end programs and scripts
			- and its boto3 library for AWS...
	- Here is a good free Python tutorial:
		- https://www.learnpython.org/

## 11. Tests
- You should also include some tests for your Python code.
	- Here are some resources on writing good Python tests:
		- https://realpython.com/python-testing/

## 12. Infrastructure as Code
- You should not be configuring your API resources manually (i.e.), by clicking around in the AWS console:
	- the DynamoDB table
	- the API Gateway
	- the Lambda function
- Instead, define them in an AWS Serverless Application Model (SAM) template and deploy them using the AWS SAM CLI
	- This is called "infrastructure as code" or IaC
	- it saves you time in the long run.

## 13. Source Control
- You do not want to be updating either your back-end API or your front-end website by making calls from your laptop, though
	- You want them to update automatically whenever you make a change to the code (This is called continuous integration and deployment, or CI/CD)
	- Create a GitHub repository for your backend code

## 14. CI/CD (Back end)
- Set up GitHub Actions such that when you push an update to your *Serverless Application Model* template or Python code, your Python tests get run
	- If the tests pass, the SAM application should get packaged and deployed to AWS

## 15. CI/CD (Front end)
- Create a 2nd GitHub repository for your website code
	- create GitHub Actions such that when you push new website code, the S3 bucket automatically gets updated
		- you may need to invalidate your CloudFront cache in the code as well
	- Important note: DO NOT commit AWS credentials to source control!
		- Bad hats will find them and use them against you!

## 16. Blog post
- Finally, in the text of your resume, you should link a short blog post describing some things you learned while working on this project
	- Dev.to is a great place to publish if you don't have your own blog


**Why?**
- CRC isn't a tutorial or a how-to guide, it's a **project spec**
	- tells you what the outcome of the project should be, and provides enough structure that you don't go too far off the rails
		- other than that, you're on your own!
- maybe it's a mean thing?
	- well, who knows.
- almost 40% of challengers are coming into this with >3 years of professional IT experience

Things learned about
- Full-stack software development (the static website and Python pieces)
- Version control (the GitHub piece)
- Infrastructure as code (the CloudFormation / SAM piece)
- Continuous integration and delivery (connecting GitHub Actions, SAM, and AWS)
- Cloud services and "serverless"
	- Lambda, API Gateway, DynamoDB, Route 53, CloudFront, S3
- Application security
	- IAM
	- S3 policies
	- CORS
	- API authentication/authorization
- Networking (the way computers talk to each other)
	- DNS
	- CDNs
	- the whole "cloud" thing
- Networking
	- as in the best way people talk to each other
		- the blog post
		- discord community
		- Woo!
- The number 1 skill:
	- the ability to **learn fast and google well**
		- all of them insist that "ability to pick up new things quickly, in a self-motivated way"
			- is their highly prized quality in a team member
				- more than competence on any specific technology

## How long might this take
- self-report how many hours they spent working on the project
	- didn't ask for an overall time window....
- seems like less than 40 hours, wtf?
	- for each year of experience you have up to 3 years
		- shave 10 hours off the expected time
- individual mileage will vary and the amount of time you need is ultimately not important to the outcome

How I would approach the Cloud Resume Challenge
- if you're not sure how you want to tackle the 16 steps of the challenge, try this method.
- Projected Time Commitment: 50-70 hours over 6 weeks
	- adjust expected hours downward if you have previous experience
- Week 0: Certification Prep
	- Challenge Step 1:
		- Complete a Certified Cloud Practitioner prep course and practice exam (10ish hours)
- Week 1: Front End (steps 2-6)
	- Challenge Milestone: Get your resume laid out in HTML
		- style it in CSS
		- Get the website deployed to S3 and fronted with DNS / CloudFront
			- 5-10ish hours
- Week 2: Back-end API (8-10, 13)
	- Get source control setup.
	- Get the visitor counter saving to the database (10ish hours)
- Week 3: Front-end / Back-end integration (7, 11)
	- get the visitor counter displaying on the homepage
	- add tests for your code
		- 10-15ish hours
- Week 4: Automation / CI (12, 14 , 15)
	- Get your infrastructure automated with SAM and deploying via GitHub Actions
		- 10ish hours
- Week 5: The Blog and Beyond (1, 16)
	- Sit and pass CCP exam
	- Write blog post
	- Celebrate
		- as much time as you want/need
- 6 self-contained chunks of work you can do in sequence
	- if a chunk takes you > 1 wk, or you want to double up, thats fine too!
	- you're in the driver's seat here; go at your own pace.


Community resources
- maybe a bit of a disadvantage? who knows.
- 
- 