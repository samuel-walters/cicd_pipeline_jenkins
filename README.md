# CI — Continuous Integration and CD — Continuous Delivery and Deployment

![](https://www.synopsys.com/content/dam/synopsys/sig-assets/images/cicd.svg.imgo.svg)

CI/CD is a method used to frequently deliver apps to customers by introducing automation into the stages of app development. It is considered the backbone of DevOps practices and automation.

## Continuous Integration

![](https://miro.medium.com/max/1400/0*oT8URYV7NzjEplUZ.png)

Continuous integration (CI) is practice that involves developers making small changes and checks to their code. Due to the scale of requirements and the number of steps involved, this process is automated to ensure that teams can build, test, and package their applications in a reliable and repeatable way. CI helps streamline code changes, thereby increasing time for developers to make changes and contribute to improved software.

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

> 1. Create a new job called Sam_CI_merge.
> 2. Create a dev branch on our localhost/GitHub.
> 3. Push the new change from dev branch.
> 4. If the tests passed trigger the job to merge the code from dev to main.
> 5. Create a 3rd job to clone the code from main branch - deliver it to AWS EC2 to configure node app.
> 6. To do this, SSH into the EC2 instance from Jenkins and install the required dependencies.
> 7. Go to the app folder, and run `npm install` - then `nohup node app.js > /dev/null 2>&1 &`.
> 8. The app should be available on public ip of ec2 instance - share the ip in the chat.

### Blockers
> 1. Look up how to not hang when jenkins script tries to connect to ec2 (as input is required here).
dev branch
test commit in dev
another test
another test