pipeline {
    agent any
    environment {
        DB_URL = "jdbc:mysql://192.168.0.216:3306/my_database?serverTimezone=Asia/Seoul"
        DB_USERNAME = "root"
        DB_PASSWORD = "Abcd1234!"
        CHANGELOG_FILE = "db/db.changelog-master.xml"
        ROLLBACK = false
    }
    parameters { // rollback 버튼 추가하기기
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
                    sh 'ssh -o StrictHostKeyChecking=no root@192.168.0.13'
                    if(params.ROLLBACK){
                        sh '''
                        echo "liquibase rollback start..."
                        liquibase \
                          --changeLogFile=db/db.changelog-master.xml \
                          --url="${DB_URL}" \
                          --username="${DB_USERNAME}" \
                          --password="${DB_PASSWORD}" \
                          --driver="com.mysql.cj.jdbc.Driver" \
                          rollbackCount 1
                        
                        '''
                    }else{
                        sh '''
                        echo "liquibase update start..."
                        liquibase \
                          --changeLogFile=db/db.changelog-master.xml \
                          --url="${DB_URL}" \
                          --username="${DB_USERNAME}" \
                          --password="${DB_PASSWORD}" \
                          --driver="com.mysql.cj.jdbc.Driver" \
                          update
                        '''
                    }
                }
            }
        }
    }
}