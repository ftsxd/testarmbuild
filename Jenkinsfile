pipeline {
  agent {
    node {
      label 'maven'
    }
  }

    parameters {
        choice(name:'APP_DIR', choices: ['blade-gateway','ygj-biz-service/ygj-cms','ygj-biz-service/ygj-common','ygj-biz-service/ygj-config','ygj-biz-service/ygj-finance','ygj-biz-service/ygj-marketing','ygj-biz-service/ygj-member','ygj-biz-service/ygj-order','ygj-biz-service/ygj-product','ygj-biz-service/ygj-user'], description:'服务名称') //需要修改，多项目需指应用的目录 如oauth2/oauth2-server
        string(name:'APP_PORT', defaultValue: '8080', description:'服务端口号') //需要修改，服务端口号
        string(name:'HEALTH_PATH', defaultValue: '/health', description:'健康检查接口') //需要修改，健康检查接口
        string(name:'INITIALDELAYSECONDS',defaultValue: '45',description:'服务正常启动时间') //需要修改，服务正常启动时间
        choice(name:'NAMESPACE', choices: ['ygj-liangzhu-dev','ygj-liangzhu-prod'], description:'项目空间名') //需要修改, 项目空间名
    }

    environment {
        // 默认配置，无需修改
        DOCKER_CREDENTIAL_ID = 'harbor-id'
        SONAR_CREDENTIAL_ID = 'sonar-token'
    }

    stages {
        stage ('拉取代码') {
            steps {
                checkout(scm)
            }
        }

        stage ('初始化环境变量') {
            steps {
                script {
                    def app_name_arr = params.APP_DIR.split("/")
                    env.APP_NAME = app_name_arr[-1]
                    env.IMAGE_URL = "$APP_NAME:$BRANCH_NAME-$BUILD_NUMBER"
                    println(params)
                  }

                container ('maven') {
                    sh 'printenv'
                }
            }
        }





        stage ('构建镜像 & 推送镜像') {
            steps {
                container ('maven') {
                        sh 'docker  buildx build -t $IMAGE_URL  --platform linux/arm64 -f dockerfile .'
          
                }
            }
        }

    


    }
}
