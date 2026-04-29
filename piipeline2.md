```
pipeline {
    agent any 

    stages {
        stage('Checkout Source') {
            steps {
                // Pulls the code from your specific GitHub repo
                git branch: 'main', url: 'your git repo.'
            }
        }

        stage('Deploy to SSH Server') {
            steps {
                script {
                    sshPublisher(
                        publishers: [
                            sshPublisherDesc(
                                configName: 'Your_SSH_Config_Name', // MUST match the name in Manage Jenkins > System
                                transfers: [
                                    sshTransfer(
                                        cleanRemote: false,
                                        excludes: '.git/**', // Best practice: don't send the git metadata
                                        execCommand: 'll', // Optional: verify files landed
                                        execTimeout: 120000,
                                        flatten: false,
                                        makeEmptyDirs: false,
                                        noDefaultExcludes: false,
                                        patternSeparator: '[, ]+',
                                        remoteDirectory: '/jenkins', // The destination you requested
                                        remoteDirectorySDF: false,
                                        removePrefix: '', // Leave empty to keep repo structure
                                        sourceFiles: '**/*' // The relative path fix we discussed
                                    )
                                ],
                                usePromotionTimestamp: false,
                                useWorkspaceInPromotion: false,
                                verbose: true // Helps you debug if 0 files are sent
                            )
                        ]
                    )
                }
            }
        }
    }
}
```
