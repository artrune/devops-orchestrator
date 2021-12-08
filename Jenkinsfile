pipeline {
    agent any
    stages {
        stage('deploy') {
            steps {
                bat "docker network create elasticnetwork || echo 'network exists'"
                bat "docker rm -f elasticsearch || 'echo no running elasticsearch container to remove' "
                bat "docker rm -f kibana || 'echo no running kibana container to remove' "
                bat "docker run -d --name elasticsearch --net elasticnetwork -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:7.14.2"
                bat "docker run -d --name kibana --net elasticnetwork -p 5601:5601 kibana:7.14.2"

                build job:'uadybuild/'+env.GIT_BRANCH
                build job:'frontbuild/'+env.GIT_BRANCH
                build job:'nagios-build-and-deploy/'+env.GIT_BRANCH
                build job:'cron-script/'+env.GIT_BRANCH
            }
        }
    }
    post {
        success {
            echo "Deploy success"
        }
    }
}