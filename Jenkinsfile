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
        // stage("Quality Gate") {
        //     steps {
        //       timeout(time: 1, unit: 'HOURS') {
        //         waitForQualityGate abortPipeline: true
        //       }
        //     }
        // }
       stage("Test") {
           steps {
               sh 'mvn -Dcheckstyle.skip test'
               echo 'Test cases passed successfully...'
           }
       }
       stage("Ansible PlayBook") {
           steps {
//               ansiblePlaybook disableHostKeyChecking: true, installation: 'Ansible', inventory: '/etc/ansible/', playbook: '/et', vaultTmpPath: ''
                 sh 'ansible-playbook -i /etc/ansible/hosts /etc/ansible/deploy.yml'
           }
       }
       stage("Deploy") {
           steps {
                  sh 'chmod +x ./target/*.jar'
                  echo 'Deployed...'
           }
       }
    }
post {
always{
script{
sh 'java -jar -Dserver.port=3000 ./target/*.jar'
}
}    
}
}
