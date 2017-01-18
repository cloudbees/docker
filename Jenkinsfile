/* Expected Parameters
  JENKINS_VERSION: the Jenkins version to release
  JENKINS_SHA: the sha1
  dockerhubCredentials: credentials to use for dockerhub
  INTERNAL: if true, artifacts will be retrieved from a maven repository and pushed to an internal docker registry instead of public
  internalMavenRepository: URL to internal maven repository. Shouldn't end with /
  internalMavenCredentials: credentials to use for private Maven credentials
  internalDockerRegistry: URL to internal docker registry. Shouldn't end with /
  internalDockerCredentials: credentials to use for private docker registry
*/
node('dockerhub') {
  def internal = (INTERNAL == 'true')
  def dockerRegistry, dockerCredentials, warUrl
  if (internal) {
    dockerRegistry = internalDockerRegistry
    dockerCredentials = internalDockerCredentials
    withCredentials([[$class: 'UsernamePasswordMultiBinding',
                      credentialsId: internalMavenCredentials,
                      usernameVariable: 'NEXUS_USERNAME',
                      passwordVariable: 'NEXUS_PASSWORD']]) {
      sh "curl -fsSL -u ${env.NEXUS_USERNAME}:${env.NEXUS_PASSWORD} $internalMavenRepository/com/cloudbees/operations-center/server/operations-center-war/${JENKINS_VERSION}/operations-center-war-${JENKINS_VERSION}.war"
      warUrl = "/jenkins.war"
    }
  } else {
    dockerRegistry = ''
    dockerCredentials = dockerhubCredentials
    warUrl = "http://jenkins-updates.cloudbees.com/download/oc/$JENKINS_VERSION/jenkins.war"
  }
  def repo = "cloudbees/jenkins-operations-center" + ("${JENKINS_VERSION}".split("\\.").length > 4 ? "-fixed" : "")
  def dockerTag = "${repo}:${JENKINS_VERSION}"
  def branch = "cjoc"

  stage 'Build'
  git url: 'https://github.com/cloudbees/docker.git', branch: branch
  sh """
    set +x
    docker build --pull \
                 --no-cache \
                 --build-arg "JENKINS_VERSION=${JENKINS_VERSION}" \
                 --build-arg "JENKINS_SHA=${JENKINS_SHA}" \
                 --build-arg "JENKINS_URL=${warUrl}" \
                  -t $dockerTag .
  """
  def img = docker.image(dockerTag)
  if (!internal) {
    img.tag("latest")
  }

  stage 'Push'
  docker.withRegistry(dockerRegistry, dockerCredentials) {
    img.push();
    if (!internal) {
      img.push('latest');
    }
  }
}
