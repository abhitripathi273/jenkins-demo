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
	        scannerHome = tool 'SonarScanner'
	    }
    	withSonarQubeEnv('AccountMangementSonar') {
            sh """${scannerHome}/bin/sonar-scanner"
     -Dsonar.projectVersion=1.0-SNAPSHOT \
       -Dsonar.login=admin \
      -Dsonar.password=sonar \
      -Dsonar.projectBaseDir=/var/jenkins_home/workspace/spring-boot-demo-pipeline/ \
        -Dsonar.projectKey=jenkins-sonar \
        -Dsonar.sourceEncoding=UTF-8 \
        -Dsonar.language=java \
        -Dsonar.sources=project/src/main \
        -Dsonar.tests=project/src/test \
        -Dsonar.host.url=http://localhost:9000/"""
        }
        timeout(time: 10, unit: 'MINUTES') {
            waitForQualityGate abortPipeline: true
        }
  }
    stage("Deployment") {
      sh 'nohup ./mvnw spring-boot:run -Dserver.port=8001 &'
    }
}
