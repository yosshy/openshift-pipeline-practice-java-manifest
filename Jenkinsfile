#!groovy
def prod_branch = "origin/master"
def prod_project = "mosuke5-prod"
def dev_project = "mosuke5-dev"
def app_name = 'pipeline-practice-java'
def local_registry_path = "image-registry.openshift-image-registry.svc:5000"
def app_image = "${local_registry_path}/${dev_project}/${app_name}"

pipeline {
  agent {
    kubernetes {
      cloud 'openshift'
      label: 'maven'
    }
  }

  stages {
    stage('Checkout Source') {
      steps {
        scmSkip(deleteBuild: false)
      }
    }

    stage('deploy to prod') {
      when {
        expression {
          return env.GIT_BRANCH == "${prod_branch}" || params.FORCE_FULL_BUILD
        }
      }

      steps {
        echo "deploy"
        script {
          openshift.withCluster() {
            openshift.withProject("${prod_project}") {
              // Apply application manifests
              openshift.apply(openshift.process('-f', 'openshift/application-deploy.yaml', '-p', "NAME=${app_name}", '-p', "APP_IMAGE=${app_image}"))

              // Wait for application to be deployed
              def dc = openshift.selector("dc", "${app_name}").object()
              def dc_version = dc.status.latestVersion
              def rc = openshift.selector("rc", "${app_name}-${dc_version}").object()

              echo "Waiting for ReplicationController ${app_name}-${dc_version} to be ready"
              while (rc.spec.replicas != rc.status.readyReplicas) {
                sleep 5
                rc = openshift.selector("rc", "${app_name}-${dc_version}").object()
              }
            }
          }
        }
      }
    }
}
