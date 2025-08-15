pipeline {
    agent any

    stages {
        stage('Start Build') {
            steps {
                echo '开始构建...'
            }
        }

        stage('Query Git Commits'){
        	steps{
        		echo '查询所有git提交的版本'
        		def commits = bat(
        			script: 'git log --pretty=format:"%H - %an, %ad : %s"', returnStdout: true
        		).trim()
        		echo "所有提交记录如下"
        		echo "${commits}"
        	}
        }

        stage('End Build') {
            steps {
                echo '构建完成！'
            }
        }
    }
}
