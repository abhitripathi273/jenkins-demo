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
    withSonarQubeEnv(credentialsId: 'sqa_b9d1362bf4d1d18dc93e0e9e5983061757feca2c') { // You can override the credential to be used
      sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.8.0:sonar'
    }
   }
    stage("Deployment") {
      sh 'nohup ./mvnw spring-boot:run -Dserver.port=8001 &'
    }
}
