pipeline {
    agent any
    stages {
        stage("checkout from github"){
            steps{
                git branch: 'main',
                url:'https://github.com/theneweracoder/spring-petclinic.git'
                echo 'Git Checkout successfully...'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn -B -Dcheckstyle.skip -DskipTests clean package'
                echo 'Build Completed successfully...'
            }
        }
        stage("SonarQube analysis") {
            steps{
            withSonarQubeEnv("sonarqube") {
              sh "mvn -Dcheckstyle.skip clean verify sonar:sonar -Dsonar.projectKey=sonarqube"
              echo 'Static analysis completed successfully...'
            }
            }
        }
       stage("Test") {
           steps {
               sh 'mvn -Dcheckstyle.skip test'
               echo 'Test cases passed successfully...'
           }
       }
       stage("Ansible PlayBook") {
           steps {
                 sh 'ansible-playbook /etc/ansible/deploy.yml -i /etc/ansible/hosts -vvv'
           }
       }
       stage("Deploy") {
           steps {
                  sh 'chmod +x ./target/*.jar'
                  echo 'Deployed...'
           }
       }
    }
}
