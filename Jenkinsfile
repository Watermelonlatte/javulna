pipeline {
    agent any

    environment {
        JAVA_HOME = "/usr/lib/jvm/java-17-amazon-corretto.x86_64"
        PATH = "${env.JAVA_HOME}/bin:${env.PATH}"
        SONARQUBE_ENV = "WH_sonarqube"
    }

    stages {
        stage('📦 Checkout') {
            steps {
                checkout scm
            }
        }

        stage('🔨 Maven Build (optional)') {
            steps {
                sh 'mvn clean compile -DskipTests'
            }
        }

        // sonarqube 실행 후 다시 
        // 결과이상
        // 인스턴스 올려서 다시
        stage('🧪 SonarQube Analysis') {
            steps {
                withSonarQubeEnv("${SONARQUBE_ENV}") {
                    sh '''
                    /opt/sonar-scanner/bin/sonar-scanner \
                        -Dsonar.projectKey=javulna \
                        -Dsonar.sources=. \
                        -Dsonar.java.binaries=target/classes
                    '''
                }
            }
        }
    }

    post {
        success {
            echo "✅ SonarQube 분석 완료!"
        }
        failure {
            echo "❌ 분석 실패! 로그 확인 바람!"
        }
    }
}
