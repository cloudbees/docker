This repository will be no longer used as of March 30th. In its stated we will use the [Unified Distribution Repository](https://github.com/cloudbees/unified-distribution).

---

# CloudBees Jenkins Enterprise Docker image

This is a fully functional [CloudBees Jenkins Enterprise](https://www.cloudbees.com/products/cloudbees-jenkins-platform).


![CloudBees](https://www.cloudbees.com/sites/all/themes/custom/cb_2016rev2/images/cloudbees-logo.png)


# Usage

```
docker run -p 8080:8080 -p 50000:50000 cloudbees/jenkins-enterprise
```

NOTE: read below the _build executors_ part for the role of the `50000` port mapping.

This will store the workspace in /var/jenkins_home. All Jenkins data lives in there - including plugins and configuration.
You will probably want to make that an explicit volume so you can manage it and attach to another container for upgrades :

```
docker run -p 8080:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home cloudbees/jenkins-enterprise
```

this will automatically create a 'jenkins_home' volume on docker host, that will survive container stop/restart/deletion. 

Avoid using a bind mount from a folder on host into `/var/jenkins_home`, as this might result in file permission issue. If you _really_ need to bind mount jenkins_home, ensure that directory on host is accessible by the jenkins user in container (jenkins user - uid 1000) or use `-u some_other_user` parameter with `docker run`.

## Backing up data

See [Jenkins Docker Image Documentation / Backing up data](https://hub.docker.com/_/jenkins/).

# Setting the number of executors

See [Jenkins Docker Image Documentation / Setting the number of executors](https://hub.docker.com/_/jenkins/).


# Attaching build executors

See [Jenkins Docker Image Documentation / Attaching build executors](https://hub.docker.com/_/jenkins/).

# Passing JVM parameters

See [Jenkins Docker Image Documentation / Passing JVM parameters](https://hub.docker.com/_/jenkins/).

# Configuring logging

See [Jenkins Docker Image Documentation / Configuring logging](https://hub.docker.com/_/jenkins/).

# Passing Jenkins launcher parameters

See [Jenkins Docker Image Documentation / Passing Jenkins launcher parameters](https://hub.docker.com/_/jenkins/).

# Installing more tools

See [Jenkins Docker Image Documentation / Installing more tools](https://hub.docker.com/_/jenkins/).

## Preinstalling plugins

See [Jenkins Docker Image Documentation / Preinstalling plugins](https://hub.docker.com/_/jenkins/).

# Upgrading

See [Jenkins Docker Image Documentation / Upgrading](https://hub.docker.com/_/jenkins/).


# Support?

Contact CloudBees support at https://support.cloudbees.com
