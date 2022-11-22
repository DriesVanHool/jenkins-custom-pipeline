# Jenkins custome pipline

This repo describes how to setup a Jenkins job with a custome pipeline to a Github repository.

## Project setup

Add a file named *Jenkinsfile* to your root directory with the following code (In this case the foldername is jenkins-custom-pipeline)
```
pipeline {
    agent any

    tools {
        maven 'Maven'
        jdk 'jdk-17'
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean install -DskipTests'
            }
        }
        stage('jenkins-custom-pipeline') {
            steps {
                sh 'cd jenkins-custom-pipeline && mvn -Dmaven.test.failure.ignore=true test'
            }
        }
    }

    post {
        always {
            junit '**/target/surefire-reports/*.xml'
        }
    }
}
```

Add the correct maven plugins to your POM file

```
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-surefire-plugin</artifactId>
				<version>2.22.2</version>
			</plugin>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-surefire-report-plugin</artifactId>
				<version>2.22.2</version>
			</plugin>
```

## Pipline setup in Jenkins

1. First create a new Item and select Pipeline.

![alt text](https://github.com/DriesVanHool/jenkins-custom-pipeline/blob/main/images/newItem.jpg)

2. Preferably discard your builds. You can choose the maximum amount of builds.

![alt text](https://github.com/DriesVanHool/jenkins-custom-pipeline/blob/main/images/discardBuilds.jpg)

3. Fill in the url to your repository

![alt text](https://github.com/DriesVanHool/jenkins-custom-pipeline/blob/main/images/githubURL.png)

4. To schedual your builds use Poll SCM. In the example bellow the trigger is set to every 5 minutes

![alt text](https://github.com/DriesVanHool/jenkins-custom-pipeline/blob/main/images/pollSCM.jpg)

5. After this set the Pipeline script from SCM (Git). To add credentials you have to create a token. This you can do via *Github usersettings > Developer settings > Personal token*.

![alt text](https://github.com/DriesVanHool/jenkins-custom-pipeline/blob/main/images/pipelineDefenition.png)

You also need to specify your branch. In the case of Github this will be main.

After this you are ready to go.