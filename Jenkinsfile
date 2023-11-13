pipeline {
    agent {
        docker {
            image 'maven:3.9.0'
            args '-v /root/.m2:/root/.m2'
        }
    }
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
        stage("build & SonarQube analysis") {
            steps {
              withSonarQubeEnv('My SonarQube Server') {
                sh 'mvn clean package sonar:sonar'
              }
            }
          }
        stage("Quality Gate") {
            steps {
              timeout(time: 1, unit: 'HOURS') {
                waitForQualityGate abortPipeline: true
              }
            }
        }
       stage('Test') {
           steps {
               sh 'mvn -Dcheckstyle.skip test'
           }
           // post {
           //     always {
           //         junit 'target/surefire-reports/*.xml'
           //    }
           // }
       }
//        stage('Deliver') {
//            steps {
//                sh './jenkins/scripts/deliver.sh'
//            }
//        }
    }
}
