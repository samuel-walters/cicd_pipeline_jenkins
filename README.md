# CI — Continuous Integration and CD — Continuous Delivery and Deployment

![](https://www.synopsys.com/content/dam/synopsys/sig-assets/images/cicd.svg.imgo.svg)

CI/CD is a method used to frequently deliver apps to customers by introducing automation into the stages of app development. It is considered the backbone of DevOps practices and automation.

## Continuous Integration

![](https://miro.medium.com/max/1400/0*oT8URYV7NzjEplUZ.png)

Continuous integration (CI) is a practice that involves developers making small changes and checks to their code. Due to the scale of requirements and the number of steps involved, this process is automated to ensure that teams can build, test, and package their applications in a reliable and repeatable way. CI helps streamline code changes, thereby increasing time for developers to make changes and contribute to improved software.

Furthermore, by merging and committing code to the main branch multiple times a day, you can avoid the horrendous merge conflicts that can happen if people instead wait for the release day to merge all their changes into the main branch.

## Continuous Delivery

![](https://miro.medium.com/max/1400/0*AUo3q0hKivOsw6jU.png)

Continuous delivery (CD) is an extension of continuous integration. It makes sure you can release new changes to your customers quickly as it automates the release process so you can deploy the application at any time by just clicking a button. **In continuous Delivery the deployment is completed manually.**

## Continuous Deployment

![](https://miro.medium.com/max/1400/0*y2Pwj5q1aeZ6zweo.png)

Continuous Deployment goes one step further than continuous delivery. With continuous deployment, every single change that passes all stages of the production pipeline are released straight to the customer. Unlike continuous delivery, there is no human intervention at all, and no button has to be pressed to deploy the application. Only a failed test will prevent a new change being deployed to production.

## Relationship between the CICD practices

* Continuous integration forms a part of both continuous delivery and continuous deployment. 

* In continuous delivery the deployment is done manually and in continuous deployment it happens automatically.

# Tools to build CICD Pipelines

## Jenkins

![](https://miro.medium.com/max/1400/0*_qH98tt7Szpn71w5.png)

Multi Billion Dollar companies like Facebook, Netflix and Ebay have adopted Jenkins because of the advantages it brings.

* Great range of plugins available.

* Easy installation.

* Simple and user-friendly interface

* Community-contributed plugin resources

## Diagram for setting up Jenkins with our app

![](https://i.imgur.com/EQ8eodO.png)

## SSH Set Up for GitHub

1. Open GitBash as an administrator.

2. Go to your .ssh folder and paste in this command:

        ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

3. Enter the file name (lowercase, underscrolls).

4. For passphrase, click enter twice.

5. Copy the .pub file's content.

6. Paste the content into GitHub SSH keys.


## Set Up SSH from Jenkins to Github Repository

NOTE:
* GitHub repository - PUBLIC KEY
* Jenkins - PRIVATE KEY

> 1. Create a new key in your ssh folder with `ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`.

> 2. Run this command on the public (.pub) key generated: `clip < ~/.ssh/eng110_cicd_sam.pub`.

> 3. Go to `Settings` in your GitHub repository.

> 4. Go to `Deploy Keys`.

> 5. Add a key, and copy the contents from the `clip` command into the box. 

> 6. On Jenkins, create a build.

> 7. Remember naming conventions.

> 8. Tick Discard old builds. Max # of builds to keep = 3.

> 9. GitHub project - use the http link **NOT** ssh.

> 10. Tick restrict where this project can be run.

> 11. For Label Expression, type in `sparta-ubuntu-node` (might need to press backspace and fiddle around with it until it recognises the label).

> 12. For Source Code Management, choose `Git`.

> 13. For `Repository URL`, choose the repository's **SSH** link. 

> 13. Run this command on the private key generated: `clip < ~/.ssh/eng110_cicd_sam`.

> 14. Credientials: add a new key.

> 15. Choose SSH Keys. Give it the same name as your private key (for example eng110_cicd_sam).

> 16. Paste the private key's contents into the box.

> 17. Make sure it is */main and not */master

> 18. Tick Provide Node & npm bin/ folder to PATH

> 19. Go to build - execute shell

> 20. Type in these commands - make sure the path to your app folder is right:

    cd app
    npm instal
    npm test

## Webhook between GitHub and Jenkins.

> 1. Edit your build and go to `configure`.

> 2. Under `Build Triggers`, click `GitHub hook trigger for GITScm polling.

> 3. Go to the GitHub repository linked to your Jenkins job, and click `settings.`

> 4. Go to your `Webhooks`.

> 5. Click `Add webhook`.

> 6. Use the Jenkins IP, and add `github-webhook` at the end. For example, it may look like this:

                http://ipaddress:port/github-webhook/

> 7. Click `Let me select individual events`, and select `push` and `pull`.

> 8. Click `Add webhook`.

> 9. Test your webhook by pushing a commit to your GitHub repository. This should automatically trigger the Jenkins job to run.

## CICD/CDE for Jenkins

![](https://i.imgur.com/n2bDQnE.png)

> 1. Create a dev branch on our localhost/GitHub.
> 2. Create a new job called Sam_CI_merge.
> 3. Make a new change in dev branch and push it.
> 4. If the tests passed trigger the job to merge the code from dev to main.
> 5. Create a 3rd job to clone the code from main branch - deliver it to AWS EC2 to configure node app.
> 6. To do this, SSH into the EC2 instance from Jenkins and install the required dependencies.
> 7. Go to the app folder, and run `npm install` - then `nohup node app.js > /dev/null 2>&1 &`.
> 8. The app should be available on public ip of ec2 instance - share the ip in the chat.

### Blockers
> 1. Look up how to not hang when jenkins script tries to connect to ec2 (as input is required here).

## Job 1

> 1. Name: `sam-continuous-integration`.

> 2. Tick `Discard old builds` and choose 3 in `Max # of builds to keep`.

> 3. Tick `GitHub project` and link your https repository location.

> 4. Tick `Restrict where this project can be run` (`sparta-ubuntu-node`).

> 5. Tick `Git`. Link your GitHub repository again (but in SSH format).

> 6. Choose the right private key to unlock the repository's public key.

> 7. Choose `*/dev` in branch.

> 8. Tick `GithHub hook trigger`. 

> 9. Click `Provide Node & npm bin/ folder to PATH`.\

> 10. Go to `Build`, and choose `Execute shell`, set up with these commands:

        cd app
        npm install
        npm test

> 11. For `Post-build Actions`, choose `Sam_CI_merge` in `Build other projects`.

> 12. Click save.

## Job 2

> 1. Name: `Sam-CI-merge

> 2. Choose the GitHub settings again (and Discard old builds).

> 3. For branch, choose `*/dev`.

> 4. Go to `Additional Behaviours`, and add `origin` for `Name of repository`, and add `main` as `Branch to merge to`. 

> 5. For `Post-Build Actions`, choose `projects to build` and select `eng110-sam-cd`.

> 6. Click `Git Publisher` as well in `Post-Build-Actions`. Choose `Git Publisher` and tick `Push only if build succeeds` and `Merge results`.

> 7. Click save.

## Job 3

> 1. Name: `eng110-sam-cd`.

> 2. Link GitHub as in previous two jobs (and tick `Discard old builds` and put in 3).

> 3. SSH Agent: choose the private key to unlock the EC2 instance's public key.

> 4. Run these commands under `Execute shell`:
    
    # Connect to DB
    ssh -o "StrictHostKeyChecking=no" ubuntu@54.75.88.179 << EOF
        sudo apt-get update -y
        sudo apt-get upgrade -y
        sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv D68FA50FEA312927
        echo "deb https://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list
        sudo apt-get update -y
        sudo apt-get upgrade -y
        sudo apt-get install -y mongodb-org=3.2.20 mongodb-org-server=3.2.20 mongodb-org-shell=3.2.20 mongodb-org-mongos=3.2.20 mongodb-org-tools=3.2.20
        sudo systemctl status mongod
        sudo systemctl start mongod
        sudo systemctl enable mongod
        sudo systemctl status mongod
        sudo chown ubuntu: /etc/.
        sed '24d' /etc/mongod.conf -i
        awk 'NR==24{print "  bindIp: 0.0.0.0"}7' /etc/mongod.conf > change && mv change /etc/mongod.conf
        sudo chown root: /etc/.
        sudo systemctl restart mongod
        sudo systemctl enable mongod
        sudo systemctl status mongod
        
    EOF

    # ssh into ec2
    # update upgrade, run the provisioning script or install nginx to test
    # scp to copy data from github to ec2
    rsync -avz -e "ssh -o StrictHostKeyChecking=no" app ec2-54-75-45-88.eu-west-1.compute.amazonaws.com:~/.
    ssh -A -o "StrictHostKeyChecking=no" ubuntu@54.75.45.88 << EOF	
        killall npm
        pm2 kill
        sudo apt update -y
        # Run upgrades
        sudo apt upgrade -y
        # Removes nginx
        sudo apt-get purge nginx nginx-common -y
        # Install nginx
        sudo apt install nginx -y
        # Start nginx
        sudo systemctl start nginx
        # Enable nginx
        sudo systemctl enable nginx
        
        # Allows us to edit the file at /etc/nginx/sites-available/default
        sudo chown ubuntu: /etc/nginx/sites-available/.
        
        # Deletes lines 49-51 in the default file we now have permission to
        sed '49,51d' /etc/nginx/sites-available/default -i
        # Inserts all the necessary lines in the default file to set up a reverse proxy for nginx
        awk 'NR==49{print "             proxy_pass http://localhost:3000;"}7' /etc/nginx/sites-available/default > change && mv change /etc/nginx/sites-available/default
        awk 'NR==50{print "             proxy_http_version 1.1;"}7' /etc/nginx/sites-available/default > change && mv change /etc/nginx/sites-available/default      
        awk 'NR==51{print "             proxy_set_header Upgrade \$http_upgrade;"}7' /etc/nginx/sites-available/default > change && mv change /etc/nginx/sites-available/default
        awk 'NR==52{print "             proxy_set_header Connection 'upgrade';"}7' /etc/nginx/sites-available/default > change && mv change /etc/nginx/sites-available/default
        awk 'NR==53{print "             proxy_set_header Host \$host;"}7' /etc/nginx/sites-available/default > change && mv change /etc/nginx/sites-available/default 
        awk 'NR==54{print "             proxy_cache_bypass \$http_upgrade;"}7' /etc/nginx/sites-available/default > change && mv change /etc/nginx/sites-available/default
        
        # Restores the permissions we changed to edit the default file
        sudo chown root: /etc/nginx/sites-available/.
        sudo chown root: /etc/nginx/sites-available/default
        
        # restarts nginx as we have changed the default file
        sudo systemctl restart nginx
        
        # Changes the directory to where we want to install the dependencies
        cd app
        # Installs dependencies
        sudo apt install software-properties-common -y
        # Gets version 12
        curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
        # Installs nodejs
        export DB_HOST=mongodb://54.75.88.179:27017/posts
        sudo apt-get install -y nodejs
        node seeds/seed.js
        npm install 
        nohup node app.js > /dev/null 2>&1 &
        
    EOF

> 5. Make sure you have spun up a new EC2 instance with these security group rules:

![](https://i.imgur.com/r55Fttv.png)

test commit
another test test
test
test
linux command