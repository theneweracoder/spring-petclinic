pipeline {
    agent any
    // agent {
    //     docker {
    //         image 'maven:3.9.0'
    //         args '-v /root/.m2:/root/.m2'
    //     }
    // }
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
        // stage('SonarQube Analysis') {
        //   tools {
        //        jdk "jdk20" // the name you have given the JDK installation using the JDK manager (Global Tool Configuration)
        //   }
        //    withSonarQubeEnv('jenkins_integration') {
        //    sh 'mvn -Dcheckstyle.skip clean verify sonar:sonar -Dsonar.projectKey=theneweracoder_spring-petclinic_AYvL80AHsTFBDlqEZXJo -Dsonar.projectName='spring-petclinic''
        //  }
        //  }
        stage("SonarQube analysis") {
            withSonarQubeEnv("sonarqube") {
              sh "mvn -Dcheckstyle.skip clean verify sonar:sonar -Dsonar.projectKey=sonarqube"
            }
        }
        // stage("Quality Gate") {
        //     steps {
        //       timeout(time: 1, unit: 'HOURS') {
        //         waitForQualityGate abortPipeline: true
        //       }
        //     }
        // }
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
       stage('Deliver') {
           steps {
               sh 'chmod +x ./deliver.sh'
               sh 'chmod +x ./target/*.jar'
               sh './deliver.sh'
           }
       }
    }
}
