@Library('jenkins-test-library') _

def configMap = [
    project: "roboshop",
    component: "catalogue"
]

echo "triggering the library pipeline"

if (env.BRANCH_NAME.equalsIgnoreCase('main')){
    echo "checking later"

}
else{
  testPipeline(configMap)
}