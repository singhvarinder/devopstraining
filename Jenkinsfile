pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: 'main']], extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'spring-boot-rest-2'], [$class: 'CheckoutOption', timeout: 5], [$class: 'CloneOption', noTags: false, reference: '', shallow: false, timeout: 5]], userRemoteConfigs: [[url: 'https://github.com/singhvarinder/devopstraining.git']]])
                echo "Checkout from github"
                
            }
        }
        stage('QualityCheck and Security Check - Parallel Steps') {
            steps {
                parallel(
                  QualityCheck: {
                    sh "/usr/local/bin/newman run QA8083.json -r html"
                    echo 'Quality Check P'
                  },
                  SecurityCheck: {
      	    node('Slaveone'){
                        dependencyCheck additionalArguments: '--scan=. --format=HTML', odcInstallation: 'OWASP-Dependency-Check'       
                        echo 'Security Check P'
                     }
                  }
                )
            }
        }
       stage('BuidPre Step') {
            steps {
                catchError(buildResult: 'SUCCESS', stageResult: 'SUCCESS') {
                    sh "fuser -k 8083/tcp"
                }
               echo 'Cleaning Existing Build'
            }
        }
        stage('BuildPush') {
            steps {
                dir('spring-boot-rest-2/spring-boot-rest-2') {
                   sh "mvn -e clean install"
                 }
                echo 'Build Push'
            }
        }
        stage('DeployApp') {
            steps {
                dir('spring-boot-rest-2/spring-boot-rest-2') {
                    withEnv(['JENKINS_NODE_COOKIE=dontkill']) {
	                    sh "java -jar target/spring-boot-rest-2-0.0.1-SNAPSHOT.jar &"
                    }
                }
                 echo 'Deploy App'
            }
        }
        stage('PostDeploy') {
            steps {
                mail bcc: '', body: 'This is test mail from post script in jenkins', cc: '', from: '', replyTo: '', subject: 'Security Check - Training', to: 'e3002311@localhost'
                echo 'Post Deploy'
            }
       }   
    }
}
