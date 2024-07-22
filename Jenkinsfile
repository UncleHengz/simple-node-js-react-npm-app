pipeline 
{
    agent any

    tools 
    {
        nodejs "nodejs"
    }

    stages 
    {
        stage('Build') 
        {
            steps 
            {
                sh 'npm install'
            }
        }
        stage('Test') 
        { 
            steps 
            {
                sh 'chmod 777 ./jenkins/scripts/test.sh'
                sh './jenkins/scripts/test.sh' 
            }
        }
        stage('OWASP Dependency-Check Vulnerabilities') 
        {
            steps 
            {
                dependencyCheck additionalArguments: ''' 
                    -o './'
                    -s './'
                    -f 'ALL' 
                    --prettyPrint''', odcInstallation: 'OWASP Dependency-Check Vulnerabilities'
        
                dependencyCheckPublisher pattern: 'dependency-check-report.xml'
            }
        }
        stage('Deliver') 
        { 
            steps 
            {
                sh 'chmod 777 ./jenkins/scripts/deliver.sh'
                sh './jenkins/scripts/deliver.sh' 
                input message: 'Finished using the web site? (Click "Proceed" to continue)' 
                sh 'chmod 777 ./jenkins/scripts/kill.sh'
                sh './jenkins/scripts/kill.sh' 
            }
        }
    }
}