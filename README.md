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
  - Create nexus user (see next section)
  - ``su - nexus`` -> switch to nexus user
  - ``/opt/nexus-3.71.0-96/bin/nexus start``

## Create new User on Nexus with relevant permissions
- On the cloud server run ``adduser nexus``
- If we want to run the nexus application with the newly created 'nexus' user, we need to change the ownership of the nexus repositories (nexus-3.71.0-96, sonatype-work):
  - change ownership user and ownership group
  - Nexus user must be able to execute the nexus executable repo:
     - ``chown -R nexus:nexus nexus-3.71.0-96``
  - Nexus user must be able to read/write the sonatype-work folder:
    - ``chown -R nexus:nexus sonatype-work``
- Set Nexus configurations so that it will run as nexus user:
  - ``vim nexus-3.71.0-96/bin/nexus.rc`` -> edit and write: run_as_user="nexus"

## Java Gradle Project: Build Jar & Upload to Nexus

## Java Maven Project: Build Jar & Upload to Nexus
