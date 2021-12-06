pipeline {
    agent any

     stages {
        stage('Get Code') {
            steps {
                // Obtener código del repo
                //git 'https://github.com/anieto-unir/helloworld.git'
		script {
			scmVars = checkout scm
			echo 'scm : the commit id is ' + scmVars.GIT_COMMIT
		}
            }
        }
        
        stage('Build') {
            steps {
                echo 'Eyyy, esto es Python. No hay que compilar nada!!!'
		echo 'El workspace contiene el commit \'' + scmVars.GIT_COMMIT + '\' de la rama \'' + scmVars.GIT_BRANCH + '\''
            }
        }
        
        stage('Tests') {
            parallel {
                stage('Unit') {
                    steps {
                        sh '''
                            export PYTHONPATH=${WORKSPACE}
                            pytest --junitxml=result-unit.xml test/unit
                        '''
                    }
                }
                stage('Service') {
                    steps {
                        sh '''
                            export FLASK_APP=app/api.py
                            export FLASK_ENV=development
                            flask run
                            java -jar /Users/pipi/Desktop/CURSO_DEVOPS/wiremock-jre8-standalone-2.32.0.jar --port 9090 -v --root-dir /Users/pipi/Desktop/CURSO_DEVOPS/wiremock
                            export PYTHONPATH=${WORKSPACE}
                            pytest --junitxml=result-unit.xml test/rest
                        '''
                    }    
                }
            }
        }
        stage ('Results') {
            steps {
                junit 'result*.xml'
            }
        }
    }
}
