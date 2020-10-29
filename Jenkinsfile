#!/usr/bin/env groovy
// Define variables
//List category_list = ["\"Select:selected\"","\"playbook\"","\"package\""]
//List playbook_list = ["\"Select:selected\"","\"apache\"","\"nginx\"","\"tomcat\""]
List category_list = ["\"Select:selected\"","\"playbook\"","\"package\""]
List playbook_list = ["\"Select:selected\"","\"apache\"","\"nginx\"","\"tomcat\""]
List package_list = ["\"Select:selected\"","\"httpd\"","\"vsftpd\"","\"nginx\""]
List default_item = ["\"Not Applicable\""]
String categories = buildScript(category_list)
String playbook = buildScript(playbook_list)
String package = buildScript(package_list)
String items = populateItems(default_item,playbook_list,package_list)
// Methods to build groovy scripts to populate data
String buildScript(List values){
  return "return $values"
}
String populateItems(List default_item, List packageList, List playbookList){
return """if(Categories.equals('package')){
     return $packageList
     }
     else if(Categories.equals('playbook')){
     return $playbookList
     }else{
     return $default_item
     }
     """
}
// Properties step to set the Active choice parameters via 
// Declarative Scripting
properties([
    parameters([
        [$class: 'ChoiceParameter', choiceType: 'PT_SINGLE_SELECT',   name: 'Categories', script: [$class: 'GroovyScript', fallbackScript: [classpath: [], sandbox: false, script: 'return ["ERROR"]'], script: [classpath: [], sandbox: false,
        script:  categories]]],
[$class: 'CascadeChoiceParameter', choiceType: 'PT_SINGLE_SELECT',name: 'Items', referencedParameters: 'Categories', script: [$class: 'GroovyScript', fallbackScript: [classpath: [], sandbox: false, script: 'return ["error"]'], script: [classpath: [], sandbox: false, script: items]]]
    ])
])
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
        sh 'echo ${Categories}'

     
    }
        }
        stage('Triggering') {
          steps {
              ansibleTower extraVars: 'packs: "${package}"', jobTemplate: 'running-locals', jobType: 'run', throwExceptionWhenFail: false, towerCredentialsId: '8fb7ead4-0d33-4b1a-b6af-e605e915aa32', towerLogLevel: 'false', towerServer: 'localtower'
          }
        }
    }
        post {
            always {
                cleanWs()
            }
        }
    
    }