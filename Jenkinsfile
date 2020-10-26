pipeline {
    agent { 
           node {
               label 'docker'
            }
          } 

   parameters {
  string(name: 'package', defaultValue: '', description: 'Packages to be installed')
  extendedChoice bindings: '', description: '', groovyClasspath: '', groovyScript: '''def choices=[]
    textFile= new File("/tmp/sam1.txt")
    textFile.eachLine{
    choices.add(it)
}
return choices''', multiSelectDelimiter: ',', name: 'testing', quoteValue: false, saveJSONParameterToFile: false, type: 'PT_SINGLE_SELECT', visibleItemCount: 5
}

    
        
    stages {
        stage('workspace') {
            steps {
           
            sh 'ls -l && echo $testing'
            
            }
        }
        
        stage('Test') {
            steps {
        sh 'echo Running terraform'

     
    }
        }
        stage('Triggering') {
          steps {
              ansibleTower extraVars: 'packs: "httpd"', jobTemplate: 'running-locals', jobType: 'run', throwExceptionWhenFail: false, towerCredentialsId: '8fb7ead4-0d33-4b1a-b6af-e605e915aa32', towerLogLevel: 'false', towerServer: 'localtower'
          }
        }
    }
        post {
            always {
                cleanWs()
            }
        }
    
    }