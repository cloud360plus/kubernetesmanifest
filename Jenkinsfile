node {
    def app

    stage('Clone repository') {
        checkout scm
    }

    stage('Update GIT') {
        script {
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                    
                    sh '''
                        git config user.email "santoshgupta022@gmail.com"
                        git config user.name "santoshgupta022"

                        echo "Before change:"
                        cat deployment.yaml

                        sed -i "s|dockerid/test.*|dockerid/test:${DOCKERTAG}|g" deployment.yaml

                        echo "After change:"
                        cat deployment.yaml

                        git add deployment.yaml
                        git commit -m "Done by Jenkins Job changemanifest: ${BUILD_NUMBER}" || echo "No changes to commit"
                        git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/kubernetesmanifest.git HEAD:main
                    '''
                }
            }
        }
    }
}
