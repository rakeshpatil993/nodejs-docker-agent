Map config = [ 
    projectKey: "jenkins-agent"

    back_end_script: ["env-pkj-run1","env-pkj-run2"],
    back_end_env: ["test-pipeline"]
    ]

node("developer") { 
    properties(
         [
             parameter (
                 [
                     string(name: 'userid', defaultValue: 'master'),
                     password(name: 'passwd')
             )
    )// properties
    PASSWORD = "${params.passwd}"
    USER_ID = "${params.userid}"
    
    stage (" docker image Build") {
       sh"""
         docker build -t app-image:latest .
       """
    }
    
    def backEndscript = cofig.back_end_script
    backEndscript.eachWithIndex{backEndscript,index ->
    backendenv = config.back_end_env.get(index)
    stage ("Execute ${backEndscript}" node test){
        sh """
           set +x
           docker run --rm \
           -e USER_ID=${USER_ID} \
           -e PASSWORD=${PASSWORD} \
           -e appscript=${backEndscript} \
           -e appenv=${backendenv} \
           -v \$PWD/tes/results:/home/app/dir/results \
           app-image:latest
        """

    } finally {
        satge ("clean workspace") {
            sh"""
            rm -rf $WORKSPACE/*
            ecko"worspace cleaned"
            """
            sh"""
            docker image rm -f app-image:latest
            """
        }
    }
}