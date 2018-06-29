def label = "pl_scripted_mp-${UUID.randomUUID().toString()}"
def label2 = "pl_scripted_mp1-${UUID.randomUUID().toString()}"
def image_name = "stuartcbrown/jentest:${label}"
podTemplate(label: label,
        containers: [
                containerTemplate(name: 'docker', image: 'docker:17.12.1-ce-dind', args: 'cat', command: '/bin/sh -c', ttyEnabled: true)
        ],
        volumes: [
                hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock')
        ]
)podTemplate(label: label2,
        containers: [
                containerTemplate(name: 'docker', image: 'docker:17.12.1-ce-dind', args: 'cat', command: '/bin/sh -c', ttyEnabled: true)
        ],
        volumes: [
                hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock')
        ]
){
    node(label) {
        container("docker") {
            stage("docker") {
                checkout scm
                echo image_name
                withCredentials([usernamePassword(credentialsId: 'dockerhubstu', passwordVariable: 'PASSWORD', usernameVariable: 'USER')]) {


                    sh 'docker login -p ${PASSWORD} -u ${USER}'
                    sh "docker build . -t $image_name"
                    sh "docker push $image_name"

                }
            }
        }
    }
}

