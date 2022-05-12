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

Continious Deployment goes one step further than continuous delivery. With continuous deployment, every single change that passes all stages of the production pipeline are released straight to the customer. Unlike continuous delivery, there is no human intervention at all, and no button has to be pressed to deploy the application. Only a failed test will prevent a new change being deployed to production.

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

![](https://i.imgur.com/dqSuxR3.png)

### Set Up Jenkins - Steps

> 1. Code available in our localhost and GitHub repository.

> 1. SSH set up from localhost to GitHub.

> 2. Test the SSH connection.

> 3. SSH set up from GitHub to Jenkins.

> 4. Copy the code of our app (SCP).

> 5. Run the provision.sh file.

















