pipeline {
    agent any 
    stages {
        stage('Build') { 
            steps {
                sh 'pip3 install -r requirement.txt'
            }
        }
        stage('Test') { 
            steps {
                sh 'python3 manage.py test' 
            }
        }
        stage('Deploy to development') { 
            steps {
                sh 'ssh -o StrictHostKeyChecking=no deployment-user@192.168.56.110 "source venv/bin/activate; \
                cd polling; \
                git pull origin master; \
                pip install -r requirement.txt  --no-warn-script-location; \
                python manage.py migrate; \
                deactivate; \
                sudo systemctl restart nginx; \
                sudo systemctl restart gunicorn "' 
            }
        }
        stage('Deploy') { 
            input {
                message "Continuew with deployment to production?"
                ok "yes please"
            }
            steps {
                sh 'ssh -o StrictHostKeyChecking=no deployment-user@192.168.56.105 "source venv/bin/activate; \
                cd polling; \
                git pull origin master; \
                pip install -r requirement.txt  --no-warn-script-location; \
                python manage.py migrate; \
                deactivate; \
                sudo systemctl restart nginx; \
                sudo systemctl restart gunicorn "' 
            }
        }
    }
}