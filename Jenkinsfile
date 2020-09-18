// donner le choix à l'utilisateur, Builder ou Monter de version
def projet_settings = null
def TARGET_ENV = 'dev'
def MCS_CONTAINER_IMAGE_FULLNAME = null
def env='Qualif'


    properties(
        [
            buildDiscarder(
                    logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '5',numToKeepStr: '5')
            ),
            disableConcurrentBuilds(),
            disableResume(),
            pipelineTriggers([pollSCM('* * * * *')])
        ]
    )


node('master'){

    stage('set environment'){
  
               timeout(time: 30, unit: 'SECONDS') {
           
     try{          
    def userInput = input(id: 'choix', message: 'some message',ok: 'Valider', parameters: [
    [$class: 'ChoiceParameterDefinition', description: 'Environnement ?', name:'input', choices: '\nDev\nQualif\nPreprod\nProd'],
    
    
    
    
    
    ])
    env = userInput     
                
    }catch (err) {
                    def user = err.getCauses()[0].getUser()
                    if ('SYSTEM' == user.toString()) { // SYSTEM means timeout
                        env= 'Qualif'     // Set default Environment to 'dev'
                    } else {
                        didInput = false
                        echo "Aborted by: [${user}]"
                    }
                }            
                
                
                
                
                
                } 
  
        echo "Environnement : ${env}"          // cette variable permet de choisir sur quel serveur on va deployer 
        
        //GCP_PROJECT_ID="linkinnov-env-test"
        //GOOGLE_CLOUD_PROJECT_ID="linkinnov-env-test"
        SVN_CREDENTIALS_ID='jenkins'

        GCP_SERVICE_ACCOUNT_PUSH_ID='jenkins-linkinnov-imagepush'
        GCP_JSON_SVN_PATH=''
        GCP_JSON_LOCAL_PATH=''
        // BUILD_CONTAINER_IMAGE_NAME='mkenney/npm:node-8-debian'
        BUILD_CONTAINER_IMAGE_NAME='node:8-stretch'
        PUSH_CONTAINER_IMAGE_NAME='cloudsdk-maven-java8-docker'

        //MCS_DOCKER_REGISTRY_URL = 'eu.gcr.io/'+GCP_PROJECT_ID+'/'
        DOCKERFILE_PATH = './build_scripts/Dockerfile'

        MCS_CONTAINER_IMAGE_FULLNAME='' //do not set. jenkins will set it

        USERS_TO_NOTIFY = 'hichem.elamine@breakpoint-technology.fr'
       
       
       

       
       
       //variable server pour le mot cle qualif -------prod
       //varibale pour idprojet       linkinnov-env-test --------- linkinnov-221611
       //variable pour cluster        linkinnov-qualif-app--------linkinnov-prod-app 
       //variable pour jsonfile       .............98c45.json -------         ....573.json
       
       
       

       if(env=="Qualif"){
       server='qualif'
       idproject="linkinnov-env-test"
       cluster="linkinnov-qualif-app"
       jsonfile='file-linkinnov-env-test-a00032f98c45.json'
       } 
       else if(env=='Prod'){
       server='prod'
       idproject="linkinnov-221611"
       cluster="linkinnov-prod-app"
       jsonfile='file-linkinnov-221611-1b9d82fcb573.json'       
       } 
       else{
       server='qualif'
       idproject="linkinnov-env-test"
       cluster="linkinnov-qualif-app"
       jsonfile='file-linkinnov-env-test-a00032f98c45.json'       
       }  

       
   echo "Environnement : ${env}" 
   echo "server    =   ${server}"
   echo "idproject =   ${idproject}"
   echo "cluster   =   ${cluster}"
   echo "jsonfile  =   ${jsonfile}"  

      
   }

       
   }
   



node {

    if (server=='qualif'){
     stage('clone-qualif') {
    //git 'https://github.com/hichemlamine28/jenkins-helloworld.git'
    checkout scm

        echo "Environnement clone: ${server}"  
    }
    stage('build-qualif') {
    sh label: '', script: 'javac Main.java'
        echo "Environnement build : ${server}"  
    }
    stage('run-qualif') {
    sh label: '', script: 'java Main'
        echo "Environnement run: ${server}"  
    }
    }else if (server =='prod'){
    
      stage('clone-prod') {
    //git 'https://github.com/hichemlamine28/jenkins-helloworld.git'
    checkout scm

        echo "Environnement clone: ${server}"  
    }
    stage('build-prod') {
    sh label: '', script: 'javac Main.java'
        echo "Environnement build : ${server}"  
    }
    stage('runp-rod') {
    sh label: '', script: 'java Main'
        echo "Environnement run: ${server}"  
    }   
    
    
    }
        
}



def getEnvName() {

 if (env == 'Qualif') {
        return 'qualif'
    } else if (env =='Prod') {
        return 'prod'
    } else if (env =='Dev' ) {
        return 'dev'
    } else if (env =='Preprod' ) {
        return 'preprod'
    }else {
        return 'no env selected'
    }
          
}

