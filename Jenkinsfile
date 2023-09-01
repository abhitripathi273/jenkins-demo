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
  stage("build & SonarQube analysis") {
          node {
              withSonarQubeEnv('My SonarQube Server') {
                 sh 'mvn clean package sonar:sonar'
              }
          }
      }

      stage("Quality Gate"){
          timeout(time: 1, unit: 'HOURS') {
              def qg = waitForQualityGate()
              if (qg.status != 'OK') {
                  error "Pipeline aborted due to quality gate failure: ${qg.status}"
              }
          }
      }
    stage("Deployment") {
      sh 'nohup ./mvnw spring-boot:run -Dserver.port=8001 &'
    }
}
