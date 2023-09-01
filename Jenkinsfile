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
    def scannerHome = tool 'SonarScanner';
    withSonarQubeEnv() {
      sh "${scannerHome}/bin/sonar-scanner"
    }
  }
    stage("Deployment") {
      sh 'nohup ./mvnw spring-boot:run -Dserver.port=8001 &'
    }
}
