pipeline {
    agent any
    stages {
        stage("build"){
            steps {
                bat "./mvnw.cmd package"
            }
        }
        stage("Tests") {
            steps {
                parallel testA:{
                    bat "echo test A"
                    sleep 2
                },
                testB:{
                    bat "echo test B"
                    sleep 4
                }, failFast: true
            }
        }
        stage("capture"){
            steps {
                archiveArtifacts artifacts: '**/target/*.jar', followSymlinks: false
                jacoco()
                junit stdioRetention: '', testResults: '**/target/surefire-reports/TEST*.xml'
            }
        }
    }
    
    post {
        always {
            emailext body: "${env.BUILD_URL}\n${currentBuild. absoluteUrl}",
                to: 'always@foo.bar',
                recipientProviders: [previous()],
                subject: "${currentBuild.currentResult}: Job ${env.JOB_NAME} [${env.BUILD_NUMBER}]"
        }
    }
    
    
}
