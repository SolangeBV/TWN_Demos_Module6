# TWN_Demos_Module6
Demo Projects from Module 6 - Artifact Repository Manager with Nexus

## Install and configure Nexus from scratch on a cloud server
- Create a Droplet with a bigger capacity -> Ubuntu, Regular, 8 GB
- Attach Firewall rules
- Install Java 8
- Install Nexus:
  - On the cloud server run ``cd /opt``
  - Download Nexus tar file from sonatype.com -> ``wget https://download.sonatype.com/nexus/3/nexus-3.71.0-06-unix.tar.gz``
  - Untar file -> ``tar -zxvf nexus-3.71.0-96-unix.tar.gz``
- Start Nexus:
  - Create nexus user:
    - On the cloud server run ``adduser nexus``
    - If we want to run the nexus application with the newly created 'nexus' user, we need to change the ownership of the nexus repositories (nexus-3.71.0-96, sonatype-work):
      - change ownership user and ownership group
      - Nexus user must be able to execute the nexus executable repo: ``chown -R nexus:nexus nexus-3.71.0-96``
      - Nexus user must be able to read/write the sonatype-work folder: ``chown -R nexus:nexus sonatype-work``
    - Set Nexus configurations so that it will run as nexus user:
      - ``vim nexus-3.71.0-96/bin/nexus.rc`` -> edit and write: run_as_user="nexus"
  - ``su - nexus`` -> switch to nexus user
  - ``/opt/nexus-3.71.0-96/bin/nexus start``
- Open port 8081 to access Nexus from browser on firewall rules: inbound connections, custom, TCP, 8081, All IPv4 + All IPv6
- Credentials to log in the first time:
  - username: admin
  - psw: found in this file -> opt/sonatype-work/nexus3/admin.password
- Configure repositories in Nexus:
  - Settings > Repository > Repositories (remember that Nexus is a repository manager)
  - Create Repository > pick a repository format > finish up configuration for that repo

## Create new User on Nexus with relevant permissions
- Settings > Security > Users > Create User
  - Status: active
  - Roles: nx-anonymous
- Create role:
  - Settings > Security > Roles > Create Role
    - Type: Nexus role
    - Privileges: nx-reposiroty-view-maven2-snapshots.*
  - Assign this custom role to the newly created user

## Java Gradle Project: Build Jar & Upload to Nexus
- Refer to "java-app" repo to see the project
- Install plugin: add 'maven-publish' to build.gradle file (to be able to publish a jar to a maven repository)
- 'publications' within build.gradle -> define jar artifact
- 'repositories' within build.gradle -> define where to upload the artifact to (nexus link + credentials of the nexus user)
- settings.gradle -> contains data to create the jar file
- ``gradle build`` -> to create the build folder and the jar file
- Upload to Nexus:
  - ``cd java-app``
  - ``gradle publish`` -> uses the plugin in build.gradle file
  - Browse > Components > Maven snapshots -> to see the app on Nexus

## Java Maven Project: Build Jar & Upload to Nexus
- Refer to "java-maven-app" repo to see the project
- pom.xml configuration:
  - plugin to upload jar files: ``maven-deploy-plugin``
  - snapshot url: ``distributionManagement``
  - nexus credentials: create a 'settngs.xml' file under C:/.m2 hidden folder -> this file will contain global variable for all maven projects
- Build jar file:
  - ``cd java-maven-app``
  - ``mvn package`` -> it creates a 'target' folder and the jar file
- ``mvn deploy`` to update the app onto maven repository
- Browse > Components > Maven snapshots -> to see the app on Nexus
