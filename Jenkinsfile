import java.text.SimpleDateFormat

currentBuild.displayName = new SimpleDateFormat("yy.MM.dd").format(new Date()) + "-" + env.BUILD_NUMBER // NEW!!!
env.BUILDER_POD = "builder-pod-${UUID.randomUUID().toString()}"

env.DH_USER = "digitalinside"
env.USERNAME = "tigran10"

env.REPO = "https://github.com/${env.USERNAME}/go-demo-3.git" // Replace me
env.IMAGE = "${DH_USER}/go-demo-3" // Replace me
env.ADDRESS = "go-demo-3-${env.BUILD_NUMBER}-${env.BRANCH_NAME}.192.168.0.10.nip.io" // Replace `acme.com` with the $ADDR retrieved earlier
env.TAG = "${currentBuild.displayName}"
env.TAG_BETA = "${currentBuild.displayName}-${env.BRANCH_NAME}"
env.CHART_NAME = "go-demo-3-${env.BUILD_NUMBER}-${env.BRANCH_NAME}"


podTemplate(
        label: env.BUILDER_POD,
        namespace: "go-demo-3-build",
        yaml: """         
apiVersion: v1
kind: Pod
spec:
  containers:    
  - name: helm
    image: vfarcic/helm:2.8.2    
    command: ["sleep"]
    args: ["100000"]
  - name: kubectl
    image: vfarcic/kubectl
    command: ["sleep"]
    args: ["100000"]
  - name: golang
    image: golang:1.9
    command: ["sleep"]
    args: ["100000"]
"""
) {

    node {
        checkout scm

        sh "git fetch origin 'refs/tags/*:refs/tags/*'"
        def version  = sh ( script: 'git tag -l | tail -n1', returnStdout: true ).trim() ?: '1.0.0'

        echo version

        def parser = /(?<major>\d+).(?<minor>\d+).(?<revision>\d+)/
        def match = version =~ parser
        match.matches()
        def (int major, int minor, int revision) = ['major', 'minor', 'revision'].collect { match.group(it) }

        def newVersion = "${ major + "." + minor + "." + (revision+ 1) }"

        echo newVersion
    }


}