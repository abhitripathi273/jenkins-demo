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
    def mvn = tool 'mvn-3.9.4';
    withSonarQubeEnv() {
      	sh "${mvn}/bin/mvn clean verify sonar:sonar -Dsonar.projectKey=jenkins-sonar -Dsonar.projectName='jenkins-sonar' -Dsonar.projectBaseDir=. -Dsonar.login=admin -Dsonar.password=sonar -X"
    }
   }
    stage("Deployment") {
      sh 'nohup ./mvnw spring-boot:run -Dserver.port=8001 &'
    }
}
