pipeline{
  agent{
    node{
      label '611177209'
    }
  }
  options {
      skipDefaultCheckout(true)
  }
  stages{
    stage('Clean old DOCs & chekcout SCM'){
      steps{
        cleanWs()
        checkout scm
      }
    }
    stage('verify tools'){
     steps{
       sh '''
        docker info
        docker version
        docker-compose version
       '''
     }  
    }
    stage('Build image for application'){
      steps{
        sh '''
          docker-compose build
        '''
      }
    }
    stage('Start Container'){
      steps{
        sh '''
           docker-compose up -d
        '''
      }
    }
    stage('Check Container'){
      steps{
        sh '''
           docker ps -a
           docker images
        '''
      }
    }
    // stage('Dependency installation'){
    //   steps{
    //     sh '''
    //        docker-compose exec -T ci4_service sh -c "cd /app && composer install"
    //        docker-compose restart
    //     '''
    //   }
    // }
    // stage('Environment Setting Up'){
    //   steps{
    //     script{
    //       sh '''
    //         cp app/env app/.env
    //       '''
    //     }
    //   }
    // }
    // stage('Database migrate'){
    //   steps{
    //     sh '''
    //        docker-compose exec -T ci4_service sh -c "php spark migrate"
    //     '''
    //   }
    // }
    // stage('Database seed'){
    //   steps{
    //     sh '''
    //        sleep 2 
    //        docker-compose exec -T ci4_service sh -c "php spark migrate"
    //        docker-compose exec -T ci4_service sh -c "php spark db:seed Members"
    //        docker-compose exec -T ci4_service sh -c "php spark db:seed TodoLists"
    //        docker-compose up -d
    //     '''
    //   }
    // }
    // stage('Unit testing'){
    //   steps{
    //     sh '''
    //        docker-compose exec -T ci4_service sh -c "vendor/bin/phpunit --log-junit build/logs/blogger_unitTest.xml"
    //        ls
    //     '''
    //     junit 'app/build/logs/blogger_unitTest.xml'
    //   }
    // }
   }
   post {
        always {
          sh '''
          docker-compose down
          docker system prune -a -f
          '''
        }
      }
}
