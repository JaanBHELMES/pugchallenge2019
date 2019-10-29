pipeline {
  agent { label 'Node15' }
  options {
    buildDiscarder(logRotator(numToKeepStr:'10'))
    timeout(time: 10, unit: 'MINUTES')
    skipDefaultCheckout()
  }
  stages {
    stage ('Build') {
      steps {
        checkout([$class: 'GitSCM', branches: scm.branches, extensions: scm.extensions + [[$class: 'CleanCheckout']], userRemoteConfigs: scm.userRemoteConfigs])
        withEnv(["DLC=${tool name: 'OpenEdge-12.1', type: 'openedge'}", "JAVA_HOME=${tool name: 'JDK8', type: 'jdk'}"]) {
          bat "%DLC%\\ant\\bin\\ant -DDLC=%DLC% -lib %DLC%\\pct\\pct.jar init build dist"
        }
        archiveArtifacts artifacts: 'target/DataDigger.zip'
      }
    }
  }
}