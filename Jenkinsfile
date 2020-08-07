
node("maven-label") {
    def mvnHome
    stage('Preparation') { 
        checkout scm
        mvnHome = tool 'maven-3.6.3'
    }
  stage('sonar-scanner') {
        withEnv(["MVN_HOME=$mvnHome"]) {
            if (isUnix()) {
                sh '"$MVN_HOME/bin/mvn" -Dmaven.test.failure.ignore sonar:sonar -Dsonar.host.url=http://40.78.122.51:9000'
            } else {
                bat(/"%MVN_HOME%\bin\mvn" -Dmaven.test.failure.ignore clean package/)
            }
        }
    }
    stage("sonar-qualitygate"){
	    withCredentials([string(credentialsId: 'sonar_token', variable: 'sonar_token')]) {
	    sh 'sh breakbuild.sh http://40.78.122.51:9000 "$sonar_token"'
		    
	    }
    }
      stage('Build') {
        withEnv(["MVN_HOME=$mvnHome"]) {
            if (isUnix()) {
                sh '"$MVN_HOME/bin/mvn" -Dmaven.test.failure.ignore -P sgapp-deploy clean deploy'
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
