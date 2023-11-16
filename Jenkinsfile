pipeline {
    agent any
    stages {
        stage("checkout from github"){
            steps{
                git branch: 'main',
                url:'https://github.com/theneweracoder/spring-petclinic.git'
                echo 'pulled from github successfully'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn -B -Dcheckstyle.skip -DskipTests clean package'
            }
        }
        stage("SonarQube analysis") {
            steps{
            withSonarQubeEnv("sonarqube") {
              sh "mvn -Dcheckstyle.skip clean verify sonar:sonar -Dsonar.projectKey=sonarqube"
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
           }
       }
       stage("Deploy") {
           steps {
                  sh 'chmod +x ./target/*.jar'
                  sh 'java -jar -Dserver.port=3000 ./target/*.jar &'
           }
       }
    }
}
