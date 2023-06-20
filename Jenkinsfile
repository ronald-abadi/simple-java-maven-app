node {
    docker.image('maven:3.9.0').inside('-v /root/.m2:/root/.m2') {
        stage('Build') {           
            checkout scm 
            sh 'mvn -B -DskipTests clean package'
        }
        stage('Test') {
            checkout scm 
            sh 'mvn test'
            junit 'target/surefire-reports/*.xml'
        }
    }
}