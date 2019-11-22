# UATWithKatalon
UAT using Katalon

# Prerequiste
## Installing Katalon studio
For writing user acceptance tests with katalon, it is good to start with katalon studio. Download and install it from https://www.katalon.com/


![Download Katalon Studio](/images/1.png)

![Downloaded Katalon Studio](/images/2.png)

## Create Integration server
Follow instructions in https://github.com/acapozucca/devops/tree/master/pipeline/s2-automate-build and create an integration server.

## Hosting our product in a server

Here we are going to test the hello world web page developed in https://github.com/venkateshwarant/DemoDynamicServer

1. Clone that repository and open it in the eclipse. Right click on the repository and click export > Select war inside the web and click ok.

![Downloaded Katalon Studio](/images/15.png)

![Downloaded Katalon Studio](/images/16.png)

2. Select the destination for the war file which you have to generate.

![Downloaded Katalon Studio](/images/17.png)

3. The war file is generated in the specified location.


## Create a project repository in your integration server

1. Goto projects > your projects > new project

![Downloaded Katalon Studio](/images/3.png)

2. Give proper name to your project and create a repository.

![Downloaded Katalon Studio](/images/4.png)

## Create a katalon account

Create an account from https://www.katalon.com/sign-up/, you need an account to generate api key to initiate katalon test in future.

![Downloaded Katalon Studio](/images/7.png)

## Creating a team in katalon

You can create an organisation and a team to collaborate and work together to achieve CI/CD pipeline using katalon. Create it from https://analytics.katalon.com/home

![Downloaded Katalon Studio](/images/8.png)

# Creating new project in katalon

1. Open katalon studio, you an find "New project" option in the home page. goto it and put your project name and create one.

![Downloaded Katalon Studio](/images/5.png)

2. Select the location of your project, and select "web" in type radio button since we are going to do UAT in web application.

![Downloaded Katalon Studio](/images/6.png)

3. Select your organisaton and your team, create a new project/ select if you have created a new project in your organisation>team

![Downloaded Katalon Studio](/images/9.png)

![Downloaded Katalon Studio](/images/10.png)

![Downloaded Katalon Studio](/images/11.png)

4. Add and push all your project files to the project which you have created in your integration server

![Downloaded Katalon Studio](/images/12.png)

# Creating a new test object

It's a good practice to follow the page object model to create user acceptance test cases. Read more about the POM in https://www.geeksforgeeks.org/page-object-model-pom & https://medium.com/tech-tajawal/page-object-model-pom-design-pattern-f9588630800b


1. To create test object in katalon, open your project > right click "Object Repository" > New > Test Object

![Downloaded Katalon Studio](/images/13.png)


2. Give a title to the object an give a description

![Downloaded Katalon Studio](/images/14.png)
