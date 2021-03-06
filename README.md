# UAT With Katalon
UAT using Katalon

# Prerequiste
## Get the license
* See to the katalon pricing in https://www.katalon.com/pricing/
* We need the Katalon runtime engine license to integrate the katalon test cases to the pipeline

## Installing Katalon studio
For writing user acceptance tests with katalon, it is good to start with katalon studio. Download and install it from https://www.katalon.com/


![Download Katalon Studio](/images/1.png)

![Downloaded Katalon Studio](/images/2.png)

## Create Integration server
Follow instructions in https://github.com/acapozucca/devops/tree/master/pipeline/s2-automate-build and create an integration server.
add following script inside Vagrantfile shell
This includes
1. Downloading Katalon and giving rwx permission to gitlab-runner user & gitlab-runner group.
2. downloaing chrome stable version in the vm
```
sudo apt-get install --yes python
sudo apt-get install openjdk-8-jre
set -xe
echo "Install Katalon"
version="7.0.10"
directory=$version
package=Katalon_Studio_Engine_Linux_64-$version.tar.gz
unzipped_directory=Katalon_Studio_Engine_Linux_64-$version
wget -O $package https://github.com/katalon-studio/katalon-studio/releases/download/v$version/Katalon_Studio_Engine_Linux_64-$version.tar.gz
ls
tar -xvzf $package -C $unzipped_directory
ls
rm $package
sudo chown -R "gitlab-runner" $unzipped_directory
sudo chgrp -R "gitlab-runner" $unzipped_directory
echo $unzipped_directory
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
sudo apt-get install fonts-liberation
sudo apt-get -f install
sudo dpkg -i google-chrome-stable_current_amd64.deb
```

## Hosting our product in a server

Here we are going to test the hello world web page developed in https://github.com/venkateshwarant/DemoDynamicServer

1. Clone that repository. Note that we have added following lines in Vagrantfile

```
  config.vm.box = "ubuntu/xenial64"
  config.vm.hostname = "DemoDynamicServer"
  config.vm.synced_folder "./data1", "/vagrant_data"
  config.vm.network "public_network", ip: "192.168.33.10"
  
  config.vm.provision "shell", inline: <<-SHELL
     apt-get update
     apt-get install -y apache2
     sudo cp /vagrant_data/helloworld.html /var/www/html/helloworld.html
  SHELL
```

2. Follow tutorial in https://docs.google.com/document/d/1N3C2pTh07zqisE98Ftp8_5eoDXXM2o8KlswE9XHeO3I/edit#heading=h.jtjkrfh88jwn to install Vagrant in your machine.

3. After installing vagrant, navigate to the location of Vagrantfile and run the following command.

```
vagrant up
```

4. Check if you can access http://192.168.33.10/helloworld.html from your machine. Note that we have configured "192.168.33.10" as VM's public network IP  in Vagrantfile, if you have to change the server ip, it is just enough to change the Vagrantfile and provision the vagrant again.

![Downloaded Katalon Studio](/images/18.png)


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

# Creating test case

1. Right click "Test Cases" > New > Test Case

![Downloaded Katalon Studio](/images/19.png)

2. Give proper test case name and description

![Downloaded Katalon Studio](/images/20.png)

3. Click on Script as shown in the image

![Downloaded Katalon Studio](/images/21.png)

4. A script view will be opened in the katalon studio. All the test cases are written in groovy in katalon.

5. The code to open a url, and check for the text at a particular field and close the browser are below.

```
WebUI.openBrowser("http://192.168.33.10/helloworld.html");
WebUI.verifyElementText(findTestObject('Object Repository/title'), "Hello world!");
WebUI.closeBrowser();
```

6. Click on Run button to run the automation via Katalon Studio

![Downloaded Katalon Studio](/images/23.png)

7. Results of the test run will be displayed in the UI

![Downloaded Katalon Studio](/images/24.png)

# Creating test suite

1. Right click on "Test Suites" > new > Test Suite

![Downloaded Katalon Studio](/images/25.png)

2. Click add icon

![Downloaded Katalon Studio](/images/26.png)

3. Select test cases, and click add and continue

![Downloaded Katalon Studio](/images/27.png)

4. See that your added test cases are added in your test suite

![Downloaded Katalon Studio](/images/28.png)

5. To run a test suite. click run icon

![Downloaded Katalon Studio](/images/29.png)

6. You can see the results of test in the UI

![Downloaded Katalon Studio](/images/30.png)

7. Push all the code to the repository created in our gitlab server.

![Downloaded Katalon Studio](/images/12.png)


# Register a runner for the repository
 
1. First of all, you need to execute the following command:
```
sudo gitlab-runner register
```

2. Then enter the requested information as follows:

For GitLab instance URL enter:
```
http://192.168.33.9/gitlab/
```

For the gitlab-ci token enter the generated token.
```
Example: 85y84QhgbyaqWo38b7qg
```

For a description for the runner enter:
```
shell
```

For the gitlab-ci tags for this runner enter:
```
shell
```

For the executor enter:
```
shell
```

For the Docker image (eg. ruby:2.1) enter:
```
alpine:latest
```

Restart the runner:

```
sudo gitlab-runner restart
```

3. Finally, in GitLab change the configuration of the runner to accept jobs without TAGS.

4. Now you can see our runner inside our gitlab server

![Downloaded Katalon Studio](/images/31.png)

# Creating GitLab CI

1. Create file named .gitlab-ci.yml at the root of the repository.

2. Add the following lines into the file:
NOTE: Before running the pipeline ensure that you put your Katalon RE license under /home/gitlab-runner/.katalon/license

```
run_katalon_test_suite:
      tags:
        - shell
      script:
        -  export DISPLAY=:0
        - /home/vagrant/Katalon_Studio_Engine_Linux_64-7.0.10/katalonc -noSplash -runMode=console -projectPath="/home/gitlab-runner/builds/nbb5yDmB/0/gitlab/venkateshwaran/uat_katalon/UAT_Katalon.prj"  -retry=0 -testSuitePath="Test Suites/HelloWorldTestSuite" -executionProfile="default" -browserType="Chrome (headless)" -apiKey="1dd161fc-ec7c-4215-a9f4-2611752d646e" --config -webui.autoUpdateDrivers=true
```

Now once you put a new commit to the gitlab repository, you can see that the pipeline is sucessfully invoked and all tests are completed sucessfully

![Downloaded Katalon Studio](/images/32.png)

![Downloaded Katalon Studio](/images/33.png)

![Downloaded Katalon Studio](/images/34.png)

The execution results will be autopushed to https://analytics.katalon.com/ by katalon

![Downloaded Katalon Studio](/images/35.png)
