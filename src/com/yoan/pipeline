package com.yona.pipeline
import java.io.Serializable

class NpmMgr implements Serializable {

    private def script
    private def steps
    private def npmDownloadRegistry
    private def npmPublishRegistry
    private def buildVersion

    NpmMgr(def script, def steps , def buildVersion) {
        this.script = script
        this.steps = steps
        this.buildVersion = buildVersion
    }

    public npmInstall() {
        this.script.echo "Install npm modules from registry: ${this.npmDownloadRegistry}"
        this.script.sh returnStdout: true, script: """
        npm install
        """        	
    }
    
    public npmBuild() {
        this.script.echo "Execute npm run build"
        this.script.sh """
        npm run build --registry ${this.npmDownloadRegistry}
        """
    }

    public npmTest() {
        this.script.echo "Execute unittests"
        this.script.sh """
        npm run test-web-app
        """
    }
    
    public setBuildVersion(def packDir="."){
        this.script.echo "Set build version to ${this.buildVersion} in package.json"
        this.script.sh returnStdout: true, script: """
        cd ${packDir}
        sed -i \'s@ \"version\":.*@ \"version\": \"${this.buildVersion}\",@\' package.json
        if [ -f Dockerfile ] ; then
           sed -i \'s/BUILD_VERSION=.*/BUILD_VERSION=${this.buildVersion}/g\' Dockerfile
           sed -i \'s/GIT_COMMIT=.*/GIT_COMMIT=${this.script.env.GIT_COMMIT}/g\' Dockerfile
           sed -i \'s/BRANCH_NAME=.*/BRANCH_NAME=${this.script.env.BRANCH_NAME}/g\' Dockerfile
        fi
        """
    }
}
