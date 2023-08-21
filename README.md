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
## MY Solution :
- First I edit my GitHub repo for Book Project to add Folder name Infrastrcuter that have a app.service, 
prepare-instance.sh so I change from this one https://github.com/suzanayesh/Book-mang-api.git to this https://github.com/suzanayesh/book-prjectgsg3.git (I upload all folders not as a terminal becouse I have problem in git ) .
- I add a Release that have a app.tar.gz package.json package-lock.json dist
![image](https://github.com/suzanayesh/aws/assets/100838193/f782db7d-c332-46bc-b279-35bbb2d42fba)
- I build to intances

- ![image](https://github.com/suzanayesh/aws/assets/100838193/450719cb-ad70-4814-8f71-9f055680cfa3)

- I have a security group and key from befor, so every time I use them .
 ![image](https://github.com/suzanayesh/aws/assets/100838193/a450dc07-1de9-41d7-ae14-7de1f6ba9337)
 
- I build a Target Group
![Screenshot 2023-08-19 142820](https://github.com/suzanayesh/aws/assets/100838193/a6b2b301-38c9-45df-ac86-19e7e84fbceb)

- I build a Load Balancer and connected with my Target Group 
  ![image](https://github.com/suzanayesh/aws/assets/100838193/07c53744-e5a2-492a-b0ba-bbf6affa5a52)
  ![image](https://github.com/suzanayesh/aws/assets/100838193/6262dfcd-950e-44f3-9160-ce3dbd658198)
- Then I build a Auto scalling Group and templete
![image](https://github.com/suzanayesh/aws/assets/100838193/aa810855-d2e1-4f5c-ad6d-51203185dcb2)
![image](https://github.com/suzanayesh/aws/assets/100838193/f5263ac4-0134-4e39-9a8c-d3e702081cea)
![image](https://github.com/suzanayesh/aws/assets/100838193/1dcd4ce6-801c-4297-8e4a-a16ed5962e74)
![Screenshot 2023-08-19 141827](https://github.com/suzanayesh/aws/assets/100838193/abce4621-b754-49a5-b772-b8442b1776e3)
`Then all instances faild and be unHealthy so I tried to fix it, and it doesnt work `

*and it seems correct but doesnt word I tried several times *
![Screenshot 2023-08-21 202735](https://github.com/suzanayesh/aws/assets/100838193/9ae05a08-7c79-472e-83be-392e9743df1f)
- I try to use this script then the second one
```javascript
#!/bin/sh
set -e

sudo apt update
sudo apt upgrade -y

# install nodejs repo
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -

sudo apt install nodejs jq curl -y

# create app & github users
sudo useradd --system --create-home --shell /usr/sbin/nologin app
sudo useradd -g app --no-create-home --no-user-group --home-dir /home/app --shell /bin/bash github
sudo usermod --append --groups app github

# deploy app
repo="suzanayesh/book-prjectgsg3"
download_url=$(curl "https://github.com/suzanayesh/book-prjectgsg3/releases/tag/Sprint" | jq --raw-output '.assets[0].browser_download_url')

curl -O "https://github.com/suzanayesh/book-prjectgsg3/blob/main/infrastructure/app.service"
sudo mv app.service /etc/systemd/system/
sudo systemctl daemon-reload
sudo systemctl enable app.service

sudo -u app sh -c "mkdir -p /home/app/app && cd /home/app/app && curl -LO $download_url  && tar xzvf app.tar.gz  && npm install --omit=dev"

sudo reboot
```
```javascript
#!/bin/bash
set -e

sudo apt update
sudo apt upgrade -y
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -

sudo apt install nodejs curl -y

cd /home/ubuntu
git clone https://github.com/suzanayesh/book-prjectgsg3.git 
cd app && npm install
npm run build

sudo mv ./infrastructure/app.service /etc/systemd/system/
sudo systemctl daemon-reload
sudo systemctl enable app.service
sudo reboot
```
- I try to use another repo from my colleague that is correct and every thing is work there , but in my PC doesnt work, lol .
 
