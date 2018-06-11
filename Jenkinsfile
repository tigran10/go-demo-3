import java.text.SimpleDateFormat

env.GH_USER = "tigran10" // Replace me
env.DH_USER = "digitalinside" // Replace me
env.PROJECT = "go-demo-3"

def label = "mypod-${UUID.randomUUID().toString()}"
def podYaml = ""

// http://jenkins.192.168.99.100.nip.io/computer/(master)/configure requires number > 0
node {
  checkout scm
  podYaml  = readFile('pod.yaml')
  echo "${podYaml}"
}

podTemplate(label: label, yaml: podYaml) {
  currentBuild.displayName = new SimpleDateFormat("yy.MM.dd").format(new Date()) + "-" + env.BUILD_NUMBER
  node(label) {
    stage('build docker') {
      container('git') {
            sh """git clone https://github.com/${env.GH_USER}/${env.PROJECT}.git ."""
      }
            
      container('docker') {
          withCredentials([usernamePassword(credentialsId: "docker", usernameVariable: "USER", passwordVariable: "PASS")]) {
              sh """docker login -u $USER -p $PASS"""
          }
          sh """docker image build -t ${env.DH_USER}/${env.PROJECT}:${currentBuild.displayName}-beta ."""
          sh """docker image push ${env.DH_USER}/${env.PROJECT}:${currentBuild.displayName}-beta"""
          sh "docker logout"
      }
    }      
  }
}
