pipeline {
    agent any
    
    environment {
            
            GITHUB_TOKEN= credentials('github_token')

    }

    stages {
        stage('Pre-commit'){
            steps {
                
                echo '####### Pre-commit stage ######\n'
                sh 'trufflehog github --repo=https://github.com/El-houdhaiffouddine/Python_with_Flask.git --results=verified,unknown --token=${GITHUB_TOKEN}'



            }

        }
    }


}