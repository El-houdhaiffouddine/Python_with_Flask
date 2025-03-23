pipeline {
    agent any
    
    environment {
            
            GITHUB_TOKEN= credentials('github_token')

    }

    stages {
        stage('Pre-commit'){
            steps {
                
                echo '####### Pre-commit stage ######\n'
                echo 'Cheking for secrets ...\n'
                sh 'trufflehog github --repo=https://github.com/El-houdhaiffouddine/Python_with_Flask.git --results=verified,unknown --token=${GITHUB_TOKEN}'
                


            }

        }

        stage('SAST with bandit'){
            steps {
                echo '###### Static Analysis with Bandit #######\n'

                script {
                sh '/home/user1/flask/bin/bandit -r /var/lib/jenkins/workspace/DevSecOps/'
                    //if(result !=0){
                     //   error('Security vulnerabilities has been reported ... !')
                    //}
                }
            
            }
        }

        stage('SCA with OWASP Dependency Check'){

             steps {

                 echo 'Software Composition Analysis ....'

                 dependencyCheck additionalArguments:  '--scan /var/lib/jenkins/workspace/DevSecOps/ --format xml',
                                 odcInstallation: 'owasp-dependency-check'
                
             }     
                 

        }

        stage('Building a Docker container') {

            steps {

                 sh 'docker build -t flask-app:1.0.0 -f /var/lib/jenkins/workspace/DevSecOps/Flask-App ./var/lib/jenkins/workspace/DevSecOps/'

            }
        }

    }

    post {

        always {
                 
                 echo 'Dependency Check Publisher'
               //  dependencyCheckPublisher pattern: 'dependency-check-report.xml',
                 //                    failedTotalCritical: 1,
                   //                  failedTotalHigh: 1,
                     //                failedTotalLow: 1, 
                       //              stopBuild: true

        }

        failure {

            mail to: 'bensidi.elhoudhaiffouddine@esprit.tn',
                 from: 'Google Security',
                 subject: '[Urgent] Security Alert !',
                 mimeType: 'text/html', 
                 body: 'Security vulnerabilities has been reported. Please look in the Jenkins Console for more informations. Thanks !'            


        }
    }


}