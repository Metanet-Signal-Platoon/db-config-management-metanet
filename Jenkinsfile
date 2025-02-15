pipeline {
    agent any
    environment {
        DB_URL = "jdbc:mysql://192.168.0.213:3306/my_database?serverTimezone=Asia/Seoul"
        DB_USERNAME = "root"
        DB_PASSWORD = "Abcd1234!"
        CHANGELOG_FILE = "db/db.changelog-master.xml"
        ROLLBACK = false
    }
    parameters { // jenkins rollback 버튼 추가하기
        booleanParam(name: 'ROLLBACK', defaultValue: false, description: 'Rollback the last change?')
    }
    stages {
        stage('Jenkinsfile') {
            steps {
                echo "hello jenkins console log, i'm ready"
            }
        }
        //git clone
        stage('Checkout') {
            steps {
                git branch: 'main', 
                    credentialsId: 'github-app-jenkins',
                    url: 'https://github.com/Metanet-Signal-Platoon/db-config-management-metanet'
            }
        }
        //liquibase 적용
        stage('Apply Liquibase') {
        steps {
            script {
                if (params.ROLLBACK) {
                    sh '''
                    echo "Liquibase rollback start..."
                    liquibase --classpath=/var/jenkins_home/workspace/b-config-management-metanet_main/mysql-connector-java-8.0.18.jar rollbackCount 1
                    '''
                } else {
                    sh '''
                    echo "Liquibase update start..."
                    liquibase --classpath=/var/jenkins_home/workspace/b-config-management-metanet_main/mysql-connector-java-8.0.18.jar update
                    '''
                    }
                }
            }
        }
    }
}