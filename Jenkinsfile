pipeline {
    agent any

    stages {
        stage('Build and Run') {
            steps {
                script {
                    // 1. 拉取代码（如果未自动拉取）
                    checkout scm

                    // 2. 使用 Maven 编译并运行 Spring Boot
                    cmd 'mvn clean package spring-boot:run -Dspring-boot.run.profiles=jenkins'
                    
                    // 3. 或者后台运行（可选）
                    // sh 'nohup mvn spring-boot:run > backend.log 2>&1 &'
                }
            }
        }
    }

    post {
        always {
            // 记录日志（可选）
            archiveArtifacts artifacts: '**/backend.log', allowEmptyArchive: true
        }
    }
}