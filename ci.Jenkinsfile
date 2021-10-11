pipeline {
    agent any

    tools {
        go 'Go 1.15'
    }

    environment {
        RELEASE_TYPE = "alpha"
        VERSION_POSTFIX = "-alpha-${GIT_COMMIT.substring(0, 8)}"

        PREFIX = "projects.registry.vmware.com/tanzu_observability_keights_saas"
        DOCKER_IMAGE = "kubernetes-collector-snapshot"

        HARBOR_CREDS = credentials("projects-registry-vmware-tanzu_observability_keights_saas-robot")
    }

    stages {
      stage("Publish") {
        steps {
            withEnv(["PATH+EXTRA=${HOME}/go/bin"]) {
              sh './hack/butler/install_docker_buildx.sh'

              sh 'make semver-cli'
              sh 'echo $HARBOR_CREDS_PSW | docker login $PREFIX -u $HARBOR_CREDS_USR --password-stdin'
              sh 'HARBOR_CREDS_USR=$(echo $HARBOR_CREDS_USR | sed \'s/\\$/\\$\\$/\') make publish'
            }
        }
      }
    }
}