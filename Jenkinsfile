pipeline {
    agent any

    parameters {
        choice(name: 'BRANCH', choices: [''], description: '选择需要构建的分支')
        string(name: 'VERSION', defaultValue: '1', description: '选择需要构建的版本号（输入数字，如1、2等）')
    }

    stages {
        stage('Initialize') {
            steps {
                script {
                    // 定义远程仓库的 URL
                    def repoUrl = 'https://github.com/yyyybt/Jenkins_Test.git'
                    def repoDir = "Jenkins_Test"

                    // 检查目录是否存在，如果存在则删除
                    if (fileExists(repoDir)) {
                        bat "rmdir /s /q ${repoDir}"
                    }

                    // 克隆远程仓库到本地
                    bat """
                    git clone ${repoUrl} ${repoDir}
                    cd ${repoDir}
                    """

                    // 获取所有分支，并存储到文件
                    bat """
                    cd ${repoDir}
                    git branch -r | findstr /v '->' | powershell -Command "\$input | ForEach-Object { \$_ -replace 'origin\\\\/','' }" > branches.txt
                    """

                    // 读取文件中的分支，并存储为一个列表
                    def branches = []
                    def branchesFile = new File("${repoDir}/branches.txt")
                    branchesFile.eachLine { line ->
                        branches.add(line.trim())
                    }

                    // 更新参数，动态设置分支选项
                    currentBuild.rawBuild.rawActions.find { it.class.name.contains('ParametersDefinitionProperty') }.parameterDefinitions = [
                        new ChoiceParameterDefinition('BRANCH', branches, '选择需要构建的分支'),
                        new StringParameterDefinition('VERSION', '1', '选择需要构建的版本号（输入数字，如1、2等）')
                    ]
                    currentBuild.rawBuild.save()
                    currentBuild.result = 'ABORTED'
                    error('Pipeline 重新启动中...')
                }
            }
        }

        stage('Get Commits') {
            when {
                expression {
                    return params.BRANCH != ''
                }
            }
            steps {
                script {
                    // 定义远程仓库的 URL
                    def repoUrl = 'https://github.com/yyyybt/Jenkins_Test.git'
                    def repoDir = "Jenkins_Test"

                    // 检查目录是否存在，如果存在则删除
                    if (fileExists(repoDir)) {
                        bat "rmdir /s /q ${repoDir}"
                    }

                    // 克隆远程仓库到本地
                    bat """
                    git clone ${repoUrl} ${repoDir}
                    cd ${repoDir}
                    """

                    // 获取指定分支上的所有提交哈希值，并保存到文件
                    bat """
                    cd ${repoDir}
                    git checkout ${params.BRANCH}
                    git log --pretty=format:"%%H" > commits.txt
                    """

                    // 读取文件中的提交哈希值，并存储为一个列表
                    def commits = []
                    def commitsFile = new File("${repoDir}/commits.txt")
                    commitsFile.eachLine { line ->
                        commits.add(line.trim())
                    }

                    // 解析用户输入的版本号
                    def versionIndex = params.VERSION.toInteger() - 1
                    if (versionIndex < 0 || versionIndex >= commits.size()) {
                        error "无效的版本号：${params.VERSION}"
                    }
                    env.SELECTED_COMMIT = commits[versionIndex]

                    // 输出选择的提交哈希值
                    echo "选择的提交哈希值：${env.SELECTED_COMMIT}"
                }
            }
        }

        stage('Build with Version') {
            steps {
                script {
                    // 使用选择的提交哈希值进行构建
                    echo "使用提交哈希值进行构建：${env.SELECTED_COMMIT}"
                    // 在这里可以调用具体的构建任务，传递 env.SELECTED_COMMIT 作为参数
                    // 例如：
                    // bat "build_script.bat ${env.SELECTED_COMMIT}"
                }
            }
        }
    }
}
