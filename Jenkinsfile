pipeline {
  agent any
  stages {
    stage('pull code') {
      steps {
        checkout([$class: 'GitSCM', branches: [[name: '${branch}']],
                        			  userRemoteConfigs: [[credentialsId: "${git_auth}", url: "${git_address}"]]])
      }
    }

    stage('compile') {
      steps {
        sh "${maven_home}/mvn clean install -Dmaven.test.skip=true"
      }
    }

    stage('deploy') {
      steps {
        script {
          def target_host = "${hosts}".split(',')
          def selected_projects = "${params.project_name}".split(',')
          for (int i=0;i<target_host.size();i++) {
            current_host = target_host[i]
            for(int j=0;j<selected_projects.size();j++){
              //å–å‡ºæ¯ä¸ªé¡¹ç›®çš„åç§°
              currentProject = selected_projects[j];
              //é¡¹ç›®åç§°
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