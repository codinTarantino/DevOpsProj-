pipeline {
    agent any

    tools {
        maven "MAVEN"
        jdk "JDK"
    }

    environment {
        // Dynamically resolve Maven and Java tool paths
        MAVEN_HOME = tool 'MAVEN'
        JAVA_HOME  = tool 'JDK'
    }

    stages {
        stage('Initialize') {
            steps {
                script {
                    echo "Operating System  : ${isUnix() ? 'Linux/macOS' : 'Windows'}"
                    echo "MAVEN_HOME        : ${env.MAVEN_HOME}"
                    echo "JAVA_HOME         : ${env.JAVA_HOME}"
                    echo "PATH              : ${env.PATH}"
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    def mvnCmd = "mvn -B -DskipTests clean package"

                    // Unified OS detection
                    if (isUnix()) {
                        echo "Running on a Unix-based system..."
                        sh mvnCmd
                    } else {
                        echo "Running on a Windows-based system..."
                        bat mvnCmd
                    }
                }
            }
        }
    }

    post {
        always {
            junit(
                allowEmptyResults: true,
                testResults: '**/test-reports/*.xml'
            )
            echo "Pipeline completed. Cleaning up workspace..."
        }
    }
}

