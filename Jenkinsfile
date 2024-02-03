pipeline{
    agent any
    stages{
        stage('list fils')
        {
           steps{
            sh 'ls'
           }
        }
         stage('deploy')
        {
           steps{
            sh 'cp -r * /var/www/html/'
           }
        }
    }
}