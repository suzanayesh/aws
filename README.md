# AWS EC2 Project‏
In a previous project, you built a Book Management app, which is a REST API that uses express.js without a database.
In this project, you need to deploy that app to AWS.

-Requirements:
 - (BONUS) The app must be built and packaged, then uploaded as an asset in a GitHub release in the repo you created (it must be a public repo) - See note-1 below.
 - The app must be deployed in an Auto-Scaling Group behind an Application Load Balancer.
 - The app must be managed by systemd.
 - (BONUS) set the scaling policy to use the Average CPU utilization metric of value 60.

For your own testing, you can add an endpoint that returns the ec2 instance IP address (something similar to this: https://github.com/khaledez/echoinfo/blob/main/index.ts#L14-L29)

Deliverables:
- A README file in markdown that shows the steps you followed with screenshots in detail.
- (BONUS) Make it a blog post or a medium article so others can benefit and improve your brand. You can use chatGPT for editing

Note-1:
When building the app, make sure to package only the dist directory, package.json and package-lock.json in the gzipped package, for example:
tar czvf app.tar.gz package.json package-lock.json dis
