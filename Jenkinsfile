pipeline {
    agent any

    environment {
        MAVEN_HOME = '/opt/apache-maven-3.9.7'
        PATH = "${MAVEN_HOME}/bin:${env.PATH}"

    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Cedric-Hj/CICD_SRC.git'
            }
        }
        stage("build"){
            steps {
                 echo "----------- build started ----------"
                sh 'mvn clean deploy -Dmaven.test.skip=true'
                 echo "----------- build complted ----------"
            }
        }
        stage("test"){
            steps{
                echo "----------- unit test started ----------"
                sh 'mvn surefire-report:report'
                 echo "----------- unit test Complted ----------"
            }
        }

        stage("Sonar Analysis") {
            environment {
                scannerHome = tool 'sonar-scanner' // Name configured in SonarQube Scanner tools section
            }
            steps {
                echo '<--------------- Sonar Analysiss started  --------------->'
                withSonarQubeEnv('sonar-server') { // Server name configured in Jenkins SonarQube server configuration
                    sh "${scannerHome}/bin/sonar-scanner"
                }
                echo '<--------------- Sonar Analysis stopped  --------------->'
            }
        }
    }

post {
        success {
            echo 'Build completed successfully'
        }
        failure {
            echo 'Build failed'
        }
    }
}
