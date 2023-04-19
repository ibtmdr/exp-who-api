pipeline {
  agent any
  environment {
		DEPLOY_CREDS = credentials('deploy-anypoint-user')
	    MULE_VERSION = "4.4.0"
	    BG = "Cap4"
	    BGID = "6a860bc2-7c16-4fc4-8c42-9a362133e5f9"
	    WORKERS = "1"
	    WORKERSIZE = "Micro"
		APP_NAME = "exp-who-api"
		REGION= "us-east-2"
  } 
  stages {
    stage('Build') {
      steps {
          sh "mvn clean -DskipTests package"
      }
    }

    stage('Test') {
      steps {
          sh "mvn test"
      }
    }
    stage('Deploiment CloudHub to Sandbox') {
      when {
          branch 'dev'
      } 
      environment {
          ENVIRONMENT= "Sandbox"
      }
      steps {
          sh "mvn -e -X clean package deploy -DmuleDeploy -Dmule.version=${MULE_VERSION} -Danypoint.username=${DEPLOY_CREDS_USR} -Danypoint.password=${DEPLOY_CREDS_PSW} -Dcloudhub.app=${APP_NAME}-${ENVIRONMENT.toLowerCase()} -Dcloudhub.environment=${ENVIRONMENT} -Dcloudhub.bg=${BG} -Dcloudhub.bgid=${BGID}  -Dcloudhub.worker=${WORKERS} -Dcloudhub.workersize=${WORKERSIZE} -Dcloudhub.region=${REGION}"
      }
    }
    stage('Deploiment CloudHub to Design') { 
      when {
          branch 'main'
      }
      environment {
          ENVIRONMENT= "Develop"
      }
      steps {
          sh "mvn -e -X clean package deploy -DmuleDeploy -Dmule.version=${MULE_VERSION} -Danypoint.username=${DEPLOY_CREDS_USR} -Danypoint.password=${DEPLOY_CREDS_PSW} -Dcloudhub.app=${APP_NAME}-${ENVIRONMENT.toLowerCase()} -Dcloudhub.environment=${ENVIRONMENT} -Dcloudhub.bg=${BG} -Dcloudhub.bgid=${BGID}  -Dcloudhub.worker=${WORKERS} -Dcloudhub.workersize=${WORKERSIZE} -Dcloudhub.region=${REGION}"
      }
    }
  }
}

							