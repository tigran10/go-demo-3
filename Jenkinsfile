import java.text.SimpleDateFormat

currentBuild.displayName = new SimpleDateFormat("yy.MM.dd").format(new Date()) + "-" + env.BUILD_NUMBER // NEW!!!
env.BUILDER_POD = "builder-pod-${UUID.randomUUID().toString()}"

env.DH_USER="digitalinside"
env.USERNAME="tigran10"

env.REPO = "https://github.com/${env.USERNAME}/go-demo-3.git" // Replace me
env.IMAGE = "${DH_USER}/go-demo-3" // Replace me
env.ADDRESS = "go-demo-3-${env.BUILD_NUMBER}-${env.BRANCH_NAME}.127.0.0.1.nip.io" // Replace `acme.com` with the $ADDR retrieved earlier
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
    node("docker") {
        stage("build") {
            git "${env.REPO}"

            sh """sudo docker version"""
        }
    }

    node(env.BUILDER_POD) {
        stage("func-test") {
            try {
                container("helm") {
                    sh " echo helming here"
                }
            } catch(e) {
                error "Failed functional tests"
            } finally {
                container("helm") {
                    sh """ finally """
                }
            }
        }
    }
}