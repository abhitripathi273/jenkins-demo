node {
  stage("Clone the project") {
    git branch: 'main', url: 'https://github.com/abhitripathi273/jenkins-demo.git'
  }

  stage("Compilation") {
    sh "./mvnw clean install -DskipTests"
  }

   stage("Runing unit tests") {
      sh "./mvnw test -Punit"
    }
  stage('SonarQube Analysis') {
	  	environment {
	        scannerHome = tool 'SonarQubeScanner'
	    }
    	withSonarQubeEnv('sonarqube') {
            sh "${scannerHome}/bin/sonar-scanner"
            sh """/var/lib/jenkins/tools/hudson.plugins.sonar.SonarRunnerInstallation/sonarqube/bin/sonar-scanner \
     -D sonar.projectVersion=1.0-SNAPSHOT \
       -D sonar.login=admin \
      -D sonar.password=sonar \
      -D sonar.projectBaseDir=/var/lib/jenkins/workspace/jenkins-sonar/ \
        -D sonar.projectKey=project \
        -D sonar.sourceEncoding=UTF-8 \
        -D sonar.language=java \
        -D sonar.sources=project/src/main \
        -D sonar.tests=project/src/test \
        -D sonar.host.url=http://localhost:9000/"""
            echo 'Static Analysis Completed'
        }
        timeout(time: 10, unit: 'MINUTES') {
            waitForQualityGate abortPipeline: true
        }
  }
    stage("Deployment") {
      sh 'nohup ./mvnw spring-boot:run -Dserver.port=8001 &'
    }
}
