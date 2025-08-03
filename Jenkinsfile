pipeline {
    agent any   
    environment {
        NEXUSPASS = credentials('nexuspass')
    }

    stages {
        
        stage('Setup parameters') {
            steps {
                script {
                    properties([
                        parameters([
                            string(
                                defaultValue: '',
                                name: 'BUILD',
                            ),
                            string(
                                defaultValue: '',
                                name: 'TIMESTAMP',
                            )
                        ])
                    ])
                }
            }
        }

        stage('Ansible Deploy to PROD') {
            steps {
                ansiblePlaybook([
                    inventory : 'ansible/prod.inventory',
                    playbook : 'ansible/site.yml',
                    installation : 'ansible',
                    colorized : true,
                    credentialsId : 'applogin-prod',
                    disableHostKeyChecking : true,
                    extraVars : [
                        USER: 'admin',
                        PASS: NEXUSPASS,
                        nexusip: '172.31.8.41',
                        reponame: 'vprofile-release',
                        groupid: 'QA',
                        time: "${env.TIMESTAMP}",
                        build: "${env.BUILD}",
                        artifactid: "vproapp",
                        vprofile_version: "vproapp-${env.BUILD}-${env.TIMESTAMP}.war"
                    ]
                ])
            }
        }
    }
}