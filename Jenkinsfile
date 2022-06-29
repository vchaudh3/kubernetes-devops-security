pipeline {
  agent any

  stages {

    stage('Build Artifact - Maven') {
      steps {
        sh "mvn clean package -DskipTests=true"
        archive 'target/*.jar'
      }
    }

    stage('Unit Tests - JUnit and JaCoCo') {
      steps {
        sh "mvn test"
      }
      post {
            always { 
              junit 'target/surefire-reports/*.xml'
              jacoco execPattern: 'target/jacoco.exec'
        }
    }
    }
    stage('Docker Build and Push') {
    steps {
    withDockerRegistry([credentialsId: "docker-hub", url: ""]) {
        sh 'printenv'
        sh 'sudo docker build -t vchaudh3/numeric-app:""$GIT_COMMIT"" .'
        sh 'docker push vchaudh3/numeric-app:""$GIT_COMMIT""'
    }
    }
}

 }
}
