def git_address = "http://xxx.41.xx.15:8160/xxx/xxx.git"
def git_auth = "87791646-e23e-4b09-bf31-36f4c56e81ce"
def project_repo = "xxx_home"
def profix = "test-"
def maven_home = "/home/jenkins/maven-v3.6.3/bin"
def hosts = "xxx.xxx.xx.xx"
def server_dest = "/opt/server/javaApp/xxx/test"
def server_bin = "/opt/server/bin/xxx/test"
def file = "xxx-file"
def system = "xxx-system"
def job = "xxx-job"
def file_with_module = "xxx-modules-file"
def system_with_module = "xxx-modules-system"
def job_with_module = "xxx-modules-job"
def parent = "xxx-modules"

pipeline {

  agent any
	stages {
		stage('拉取代码') { 
			steps {
			  checkout([$class: 'GitSCM', branches: [[name: '${branch}']],
			  userRemoteConfigs: [[credentialsId: "${git_auth}", url: "${git_address}"]]])
			}
		  }
		stage('打包') {
			steps {
			   sh "${maven_home}/mvn clean install -Dmaven.test.skip=true"
			}
		  }
		stage('推送服务器') {
			steps {
				script {
					def target_host = "${hosts}".split(',')				
					def selected_projects = "${params.project_name}".split(',')
					for (int i=0;i<target_host.size();i++) {
						current_host = target_host[i]
						for(int j=0;j<selected_projects.size();j++){
							//取出每个项目的名称
							 currentProject = selected_projects[j];
							//项目名称
							if ("${currentProject}" == "${system_with_module}") {
								sh "mv ${parent}/${system}/target/*.jar ${parent}/${system}/target/${profix}${currentProject}.jar"							    
								sh "cp ${parent}/${system}/target/*.jar ${server_dest}"
								sh "sh ${server_bin}/${currentProject}.sh start"						
							} else if ("${currentProject}" == "${file_with_module}"){
								sh "mv ${parent}/${file}/target/*.jar ${parent}/${file}/target/${profix}${currentProject}.jar"								    
								sh "cp ${parent}/${file}/target/*.jar ${server_dest}"
								sh "sh ${server_bin}/${currentProject}.sh start"		
							} else if ("${currentProject}" == "${job_with_module}"){
								sh "mv ${parent}/${job}/target/*.jar ${parent}/${job}/target/${profix}${currentProject}.jar"							    
								sh "cp ${parent}/${job}/target/*.jar ${server_dest}"
								sh "sh ${server_bin}/${currentProject}.sh start"								
							} else {
								sh "mv $currentProject/target/*.jar $currentProject/target/${profix}${currentProject}.jar"							    
								sh "cp $currentProject/target/*.jar ${server_dest}"
								sh "sh ${server_bin}/${currentProject}.sh start"
							}		
						}
						
					}
					sh "${maven_home}/mvn clean"
				}			
			}
		  }
	}
}
