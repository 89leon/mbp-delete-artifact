def repoName = "libs-snapshot-local"
node {
    def artifacts = ""
    def results = []
   
    def allArtifactsList = []
    
    stage("Get all artifacts from REPO:${repoName}") {
        echo 'curling to get artifacts....'
        sh "curl -u admin:password -X GET 'http://artifactory:8081/artifactory/api/search/dates?dateFields=created,lastModified,lastDownloaded&from=1&repos=${repoName}' > artifacts.json"
        artifacts = readJSON file: 'artifacts.json'
        //println artifacts.results.uri.toString()
        results = artifacts.results.uri
        //println results         
        results.each {
        //println "item: $it"
        def tmp = removeSubURIrepo("$it")
        //println tmp;
        allArtifactsList.push(tmp)
        }
        //println allArtifactsList
    }
    stage('Remove depricated artifacts') {
        println allArtifactsList
        allArtifactsList.each {
            sh "curl -u admin:password -X DELETE 'http://artifactory:8081/artifactory/$it'"
        }
    }
}
def filter

def removeSubURI(String s){
        //example string
        //String s = "http://artifactory:8081/artifactory/api/storage/libs-snapshot-local/org/jfrog/test/multi2/3.7-SNAPSHOT/multi2-3.7-20200105.081210-1.pom"
        String s1 = s.substring(s.lastIndexOf("/")+1);
        //println s1.trim();
        return s1.trim();
}

def removeSubURIrepo(String s){
        String s1 = s.substring(s.lastIndexOf("libs-snapshot-local"));
        println s1.trim();
        return s1.trim();

}
