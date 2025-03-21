pipeline {
    agent any  // Run on any available agent

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Anirudhmdmd/wd.git'
            }
        }

        stage('Compile C Code') {
            steps {
                sh 'gcc -o output project.c'  // Change 'main.c' if you have multiple files
            }
        }

        stage('Run Executable') {
            steps {
                sh './output'  // Run the compiled program
            }
        }

        stage('Archive Build Artifact') {
            steps {
                archiveArtifacts artifacts: 'output', fingerprint: true
            }
        }
    }

    post {
        success {
            echo '✅ Build and Compilation Successful!'
        }
        failure {
            echo '❌ Build Failed!'
        }
    }
}
