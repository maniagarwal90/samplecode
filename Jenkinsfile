properties([
  parameters([
    string(name: 'ENTER_THE_PATH', defaultValue: '', description: 'enter the path in which you want to save the build'),
   ])
])

pipeline  {
    agent any
	tools {
		maven "Maven 3.5.4"
	}
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
				    sh "mvn clean deploy"
			    }
		    }
		}

	}
}

