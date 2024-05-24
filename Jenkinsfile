pipeline {
    agent any 
    stages {
        stage('Clean') { 
            steps {
                sh 'rm -rf protected/'
            }
        }

        stage('Archive') { 
            steps {
                sh 'zip -r upload.zip . -i "*.js" "*.html" "*.ts"'
            }
        }

        stage('Protecting files (JSCrambler)') { 
            steps {
                withCredentials([string(credentialsId: 'JSCRAMBLER_ACCESS_KEY', variable: 'JSCRAMBLER_ACCESS_KEY'), string(credentialsId: 'JSCRAMBLER_ACCESS_SECRET', variable: 'JSCRAMBLER_ACCESS_SECRET'), string(credentialsId: 'JSCRAMBLER_APP_ID', variable: 'JSCRAMBLER_APP_ID')]){
                    sh 'jscrambler -a "${JSCRAMBLER_ACCESS_KEY}" -s "${JSCRAMBLER_ACCESS_SECRET}" -i "${JSCRAMBLER_APP_ID}" -c jscrambler.json -o protected'
                }
            }
        }

        stage('Compare files') { 
            steps {
                sh 'diff index.js protected/verademo-javascript-api/index.js'
            }
        }
    }
}