pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
                sh './gradlew build'
            }
        }

        stage('Test') {
            steps {
                echo 'Testing..'
                sh './gradlew test'
            }
        }

        // stage('Deliver') {
        //     steps {
        //         echo 'Delivering..'
        //         sh './gradlew bootJar'
        //         archiveArtifacts artifacts: 'build/libs/*.jar', fingerprint: true
        //     }
        // }

        // stage('Deploy') {
        //     steps {
        //         echo 'Deploying..'
        //         // This will deploy the jar to your target server. Replace <target_server> with your actual server details.
        //         sh "scp build/libs/*.jar user@<target_server>:/path/to/deploy/"
        //     }
        // }
    }
}
