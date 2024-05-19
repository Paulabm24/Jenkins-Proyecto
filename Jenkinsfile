pipeline {
    agent any

    stages {
        stage('Obtener el repositorio') {
            steps {
                git branch: 'main', url: 'https://github.com/Paulabm24/Jenkins-Proyecto.git'
            }
        }
        stage('Construir la documetación') {
            steps {
                sh "doxygen"
            }
            
        }

        stage('Archivar la documentación') {
            steps {
                sh "zip documentation.zip -r html/*"
            }
        }
    }
	stage('Analisis estatico') {
            steps {
                sh 'make cppcheck-xml'
		recordIssues sourceCodeRetention: 'LAST_BUILD', tools: [cppCheck(pattern: 'reports/cppcheck/*.xml')]
            }
        }
    }

    post {
        success {
            publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'html/', reportFiles: 'html/', reportName: 'Documentación', reportTitles: ''])
            archive "documentation.zip"
        }
    }
}
