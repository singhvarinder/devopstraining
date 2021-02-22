pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: 'main']], extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'spring-boot-rest-2'], [$class: 'CheckoutOption', timeout: 5], [$class: 'CloneOption', noTags: false, reference: '', shallow: false, timeout: 5]], userRemoteConfigs: [[url: 'https://github.com/singhvarinder/devopstraining.git']]])
                echo "Checkout from github"
                
            }
        }
        stage('QualityCheck') {
            steps {
                dir('spring-boot-rest-2/spring-boot-rest-2') {
                 sh "mvn -e clean install"
                }
                
  	 dependencyCheck additionalArguments: '--scan=. --format=HTML', odcInstallation: 'OWASP-Dependency-Check'         
                echo 'Quality Check'
            }
        }
        stage('SecurityCheck') {
            steps {
                echo 'Security Checkt'
            }
        }
        stage('BuildPush') {
            steps {
                echo 'Build Push'
            }
        }
        stage('DeployApp') {
            steps {
                echo 'Deploy App'
            }
        }
        stage('PostDeploy') {
            steps {
                echo 'Post Deploy'
            }
        }        
    }
}
