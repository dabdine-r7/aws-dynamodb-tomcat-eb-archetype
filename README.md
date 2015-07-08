**__At the moment, only tomcat8 is suported__**

# aws-dynamodb-tomcat-eb-archetype
A maven archetype that will generate a new elastic beanstalk compatible war enabled for aws dynamodb session persistence a la aws-dynamodb-session-tomcat. The project generated by this archetype will:

1.  Unpack the war given by your maven coordinates to target/war-contents
2.  Create an .ebextensions directory in the war file that contains the scripts to configure tomcat.
3.  Repackage new war+ebextensions layout as a new war.

On the elastic beanstalk host, the libraries in the dependencies section of the generated pom will be included in /usr/share/tomcat8/lib-managed. Whenever a new version of this war is is deployed, the libs there will be automatically deleted and recrated by way of the 00-tomcat-setup.config script.

# Usage
1.  You will need to first clean, compile and install this archetype. At the moment it is not hosted in maven central:

  ```
  mvn clean install
  ```
2.  Generate your wrapped war project:

  ```
  mvn archetype:generate \
  -DarchetypeGroupId=com.rapid7.aws.archetype \
  -DarchetypeArtifactId=aws-dynamodb-tomcat-eb-archetype \
  -DarchetypeVersion=1.0-SNAPSHOT \
  -DgroupId=<YOURGROUPID> \
  -DartifactId=<YOURARTIFACTID> \
  -Dversion=1.0-SNAPSHOT
  ```

3.  Edit ``pom.xml`` inside the generated project and update the maven coordinates of your war:

  ```
  ...
    <artifactItems>
        <artifactItem>
            <groupId>[wrapped war groupId]</groupId>
            <artifactId>[wrapped war artifactId]</artifactId>
            <version>[wrapped war version]</version>
            <type>war</type>
            <outputDirectory>${project.build.directory}/war-contents</outputDirectory>
        </artifactItem>
    </artifactItems>
  ...
  ```
