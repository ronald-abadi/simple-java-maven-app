node {
    docker.image('maven:3.9.0').inside('-v /root/.m2:/root/.m2') {
        stage('Build') {           
            echo 'submission-cicd-pipeline-rabadi-java: Build stage in progress...'
            checkout scm 
            sh 'mvn -B -DskipTests clean package'
        }
        stage('Test') {
            echo 'submission-cicd-pipeline-rabadi-java: Test stage in progress...'
            checkout scm 
            sh 'mvn test'
            junit 'target/surefire-reports/*.xml'
        }
        stage('ManualApproval') {
            echo 'submission-cicd-pipeline-rabadi-java: Manual Approval stage in progress...'
            script {
                try {
                    env.theInput = input message: 'Lanjutkan ke tahap Deploy?'
                    env.theInput = 'Proceed'
                } catch(e) {
                    env.theInput = 'Abort'
                }
            }
        }
        stage('Deploy') {
            if (env.theInput == 'Proceed') {
                echo 'submission-cicd-pipeline-rabadi-java: Deliver stage in progress...'            
                checkout scm 
                sh './jenkins/scripts/deliver.sh'
                sh 'sleep 1m'
                echo 'done...'     
            } else {
                echo 'submission-cicd-pipeline-rabadi-java: Approval is rejected and Deployment is cancelled...'
                sh 'sleep 10'
                echo 'done...'                
            }                   
        }
    }
}
