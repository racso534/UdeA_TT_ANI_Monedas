pipeline {
	agent any
	
	environment {
		DOCKER_IMAGE = 'apimonedastt'
		CONTAINER_NAME = 'dockerapimonedastt'
		DOCKER_NETWORK = 'dockermonedas_red'
		DOCKER_BUILD_DIR = 'presentacion'
		HOST_PORT = '9080'
		CONTAINER_PORT = '8080'
	}
	
	stages{		
		/*stage('Compilacion Maven'){
			steps{				
				bat 'mvn clean package -DskipTests'
			}
		}*/
		stage('Configure Git') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: '94b1d549-e867-4f68-a96d-5c5b7b1a01b6', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                        bat "git config --global user.name '${env.GIT_USERNAME}'"
                        bat "git config --global user.email '${env.GIT_USERNAME}@hotmail.com'"
                    }
                }
            }
        }
		stage('Construir Imagen'){
			steps{				
				bat "docker build . -t ${DOCKER_IMAGE}"					
			}
		}	
		stage('Limpiar contenedor existente') {
            steps { 
				script {               
            		catchError(buildResult: 'SUCCESS', stageResult: 'UNSTABLE') {					
                        bat "docker container rm -f ${CONTAINER_NAME}"
                    }
                }
            }
        }						
		stage('Desplegar Contenedor'){
			steps{				
				bat "docker run --network ${DOCKER_NETWORK} --name ${CONTAINER_NAME} -p ${HOST_PORT}:${CONTAINER_PORT} -d ${DOCKER_IMAGE}"				
			}
		}
	}
}