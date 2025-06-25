pipeline {
    agent any

    environment {
        // 这里填写你的MySQL连接参数（密码建议用凭据管理）
        MYSQL_USER = 'root'
        MYSQL_PASSWORD = '你的密码'  // 建议用 Jenkins 凭据管理，这里简化写死
        MYSQL_HOST = 'localhost'
        MYSQL_PORT = '3306'
        MYSQL_DB = ''   // 空，执行脚本时包含create database
    }

    stages {
        stage('Init DB') {
            steps {
                // 执行初始化SQL
                sh """
                mysql -h${MYSQL_HOST} -P${MYSQL_PORT} -u${MYSQL_USER} -p${MYSQL_PASSWORD} < ${WORKSPACE}/data.sql
                """
            }
        }

        stage('Build and Run') {
            steps {
                script {
                    // 拉取代码
                    checkout scm

                    // 编译并启动Spring Boot（用jenkins配置）
                    sh 'mvn clean package spring-boot:run -Dspring-boot.run.profiles=jenkins'
                    
                    // 如果想后台启动改成下面注释的行
                    // sh 'nohup mvn spring-boot:run > backend.log 2>&1 &'
                }
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/backend.log', allowEmptyArchive: true
        }
    }
}
