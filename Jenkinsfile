pipeline {
    agent any
   
    stages {

        stage('Run Selenium Tests with pytest') {
            steps {
                    echo "Running Selenium Tests using pytest"

                    // Install Python dependencies
                    bat 'pip install -r requirements.txt'

                    // ✅ Start Flask app in background
                    bat 'start /B python app.py'

                    // ⏱️ Wait a few seconds for the server to start
                    bat 'ping 127.0.0.1 -n 5 > nul'

                    // ✅ Run tests using pytest
                    //bat 'pytest tests\\test_registrationapp.py --maxfail=1 --disable-warnings --tb=short'
                    bat 'pytest -v'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Build Docker Image"
                bat "docker build -t my_repo:latest ."
            }
        }
        stage('Docker Login') {
            steps {
                  bat 'docker login -u durga2458 -p Krn19@2458'
                }
            }
        stage('push Docker Image to Docker Hub') {
            steps {
                echo "push Docker Image to Docker Hub"
                bat "docker tag seleniumdemoapp:latest durga2458/my_repo:formimg3"               
                    
                bat "docker push durga2458/my_repo:formimg3"
                
            }
        }
        stage('Deploy to Kubernetes') { 
            steps { 
                    // apply deployment & service 
                    bat 'kubectl apply -f deployment.yaml --validate=false' 
                    bat 'kubectl apply -f service.yaml' 
            } 
        }
    }
    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Please check the logs.'
        }
    }
}
