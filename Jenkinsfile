//gitlab的凭证
def git_auth = "f9a705b6-155b-45b6-b1a4-6fcbda3b42b3"
node {
  stage('拉取代码') {
    checkout([$class: 'GitSCM', branches: [[name: '*/${branch}']],doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [],userRemoteConfigs: [[credentialsId: "${git_auth}", url:'git@github.com:WongProcon/admin-web.git']]])
  }
  stage('打包，部署网站') {
  //使用NodeJS的npm进行打包
    nodejs('nodeJS16'){
      sh '''
        npm install
        npm run build:prod
        '''
    }
    //=====以下为远程调用进行项目部署========
    sshPublisher(publishers: [sshPublisherDesc(configName: 'app_server',transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '',execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes:
false, patternSeparator: '[, ]+', remoteDirectory: '/home/web/html',
remoteDirectorySDF: false, removePrefix: 'dist', sourceFiles: 'dist/**')],
usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
}
}
