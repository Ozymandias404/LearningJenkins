pipeline {
agent any

    parameters {
        string(name: 'NAME', description: 'Please tell me your name?')
        choice(name: 'PYSCRIPT', choices: ['hello', 'goodbye'], description: 'Choose script')
    }
    stages {
        stage('Welcome'){
            agent { label 'built-in'}
            steps{
                echo "Hello Mr. ${params.NAME}"
                echo "Currently running on Agent: ${env.NODE_NAME}"
            }
        }
        stage('Script') {
            agent any
            steps {
               script{
                   int x = 1
                   int y = 2
                   echo "Sum = ${x + y}"
               }
            }
        }
        stage('Python') {
            agent {
                docker { image 'python:3.11.0rc1-bullseye' }
            }
            steps {
                git branch: 'main', url: 'https://github.com/Ozymandias404/LearningJenkinsPython'
                sh 'python --version'
                sh 'ls -a'
                sh 'pwd'
                dir('Scripts') {
                    sh "pwd"
                    sh "ls -a"
                    sh "python ${params.PYSCRIPT}.py"
                }
            }
        }
        stage('Mail') {
            agent any
            steps {
                emailext body: "${params.NAME}", subject: 'Test', to : 'marcin.buszta@grapeup.com', from: 'jenkins'
            }
        }
    }
    post { 
        always { 
            echo 'End of pipeline!'
        }
    }
}