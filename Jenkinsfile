pipeline {
  agent any
  environment {
    APPSYSID = 'fbcf9d4e1becb1106e7f631e6e4bcb6e'
    BRANCH = "${BRANCH_NAME}"
    CREDENTIALS = 'f8e0a369-7928-4979-a809-3e74453fe611'
    DEVENV = 'https://navigissb3.service-now.com/'
    TESTENV = 'https://navigissb8.service-now.com/'
    PRODENV = 'https://psndev.service-now.com/'
    TESTSUITEID = '67fcab130b01220050192f15d6673a0c'
  }
  stages {
    stage('Build') {
      when {
        not {
          branch 'main'
        }
      }
      steps {
        snApplyChanges(appSysId: "${APPSYSID}", branchName: "${BRANCH}", url: "${DEVENV}", credentialsId: "${CREDENTIALS}")
        snPublishApp(credentialsId: "${CREDENTIALS}", appSysId: "${APPSYSID}", obtainVersionAutomatically: true, url: "${DEVENV}")
      }
    }
    stage('Test') {
      when {
        not {
          branch 'main'
        }
      }
      steps {
        snInstallApp(credentialsId: "${CREDENTIALS}", url: "${TESTENV}", appSysId: "${APPSYSID}")
        snRunTestSuite(credentialsId: "${CREDENTIALS}", url: "${TESTENV}", testSuiteSysId: "${TESTSUITEID}", withResults: true)
      }
    }
    stage('Deploy to Prod') {
      when {
        branch 'main'
      }
      steps {
        snInstallApp(credentialsId: "${CREDENTIALS}", url: "${PRODENV}", appSysId: "${APPSYSID}")
      }
    }
  }
}
