pipeline {
    agent any

    stages {
        stage('Start Build') {
            steps {
                echo '开始构建...'
            }
        }

        stage('Query Git Commits') {
            steps {
                script {
                    echo '查询所有 Git 提交版本...'
                    // 使用 Git 命令查询所有提交
                    def commits = bat(script: 'git log -n 10 --pretty=format:"%%h - %%an, %%ad : %%s"', returnStdout: true).trim()
                    echo "所有提交记录如下："
                    echo "${commits}"
                    // 将获取到的提交版本存储起来
                    def commitList = commits.split('\n')
                    echo "第一条提交:${commitList[0]}"
                }
            }
        }

        stage('End Build') {
            steps {
                echo '构建完成！'
            }
        }
    }
}
