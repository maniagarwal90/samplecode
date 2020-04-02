properties([
  parameters([
    string(name: 'ENTER_THE_PATH', defaultValue: '', description: 'enter the path in which you want to save the build'),
   ])
])

pipeline  {
    agent any
    options {
        buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '5', numToKeepStr: '5')
    }
    environment { 
        REPOSITORY = 'https://github.com/maniagarwal90/samplecode.git'
        BRANCH = 'master'
        BUILDVERSION = 'sh(script: "echo `date +%s`", returnStdout: true).trim()'
	}
	stages {
		stage("Checkout code from SCM") {
		    steps {
		    	git branch: "${BRANCH}",
                url: "${REPOSITORY}"
		    }
		}
		stage("Build and Publish") {
		    steps {
			    script {
				echo "Build BRE CMDB for cc${params['CLIENT_SITE']}-${params['CLIENT_ID']}-${params['CLIENT_CLUSTERID']}-*-${params['CLIENT_ENV']}"
				rtMaven.tool = 'Maven 3.5.4'
				    sh "'${mvnHome}/bin/mvn' clean deploy"
			    }
		    }
		}
		stage("Build and push module images") {
		    steps {
    			script {
                    for(int i=0; i < MODULE_LIST.size(); i++) {
                        env["CURRENT_MODULE"] = MODULE_LIST[i]
						stage("Build ${CURRENT_MODULE} docker image (${BUILD_TYPE})") {
				               sh "docker --config /appli/tomcat/.docker/ build --no-cache --network=host --build-arg ARTIFACT_VERSION=${ARTIFACT_VERSION} --build-arg IMAGE_VERSION_TAG=${IMAGE_VERSION_TAG} -t ${DOCKER_REGISTRY_URI}/${MODULE_DIR}/${CURRENT_MODULE}:${IMAGE_VERSION} ./${CURRENT_MODULE}"
				        }
				        stage("Push ${CURRENT_MODULE} docker image (${BUILD_TYPE})") {
				               sh "docker --config /appli/tomcat/.docker/ push ${DOCKER_REGISTRY_URI}/${MODULE_DIR}/${CURRENT_MODULE}:${IMAGE_VERSION}"
				        }
				        stage("Remove ${CURRENT_MODULE} local image (${BUILD_TYPE})") {
				                sh "docker image rm ${DOCKER_REGISTRY_URI}/${MODULE_DIR}/${CURRENT_MODULE}:${IMAGE_VERSION}"
				        }
					}
				}
			}
        }
        stage('Clean up') {
           steps {
                sh 'docker builder prune -f'
            }
        }
	}
}

