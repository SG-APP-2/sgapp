
node("maven-label") {
    def mvnHome
    stage('Preparation') { 
        checkout scm
        //git branch: 'dev', url: 'https://github.com/SG-APP-2/sgapp.git'
        mvnHome = tool 'maven-3.6.3'
    }
  
      stage('Build') {
        withEnv(["MVN_HOME=$mvnHome"]) {
            if (isUnix()) {
                sh '"$MVN_HOME/bin/mvn" -Dmaven.test.failure.ignore clean install'
            } else {
                bat(/"%MVN_HOME%\bin\mvn" -Dmaven.test.failure.ignore clean package/)
            }
        }
    }
    stage('Results') {
        junit '**/target/surefire-reports/TEST-*.xml'
        archiveArtifacts 'target/*.jar'
    }
}
