This project is an example of using OSGi, OSGi Blueprint, Camel, and Apache Karaf together.
Primarily this example shows how you can leverage OSGi Config Admin with Camel, and how those
properties can be updated

Requirements:

* Apache Karaf 4.x (http://karaf.apache.org/)
* Maven 3.x (http://maven.apache.org/)
* Java SE 7

To run:

1) Build this project so bundles are deployed into your local maven repo

    <project home> $ mvn clean install

2) Start Apache Karaf

    <Apache Karaf home> $ bin/kafaf

3) Add this projects features.xml config to Apache Karaf from the Karaf
   Console (makes it easier to install OSGi bundles with all required dependencies)

    karaf@root()> feature:repo-add mvn:org.camelcookbook.blueprint/features/1.0-SNAPSHOT/xml/features

4) Install the test bundle

    karaf@root()> feature:install blueprint-properties-test1

5) To see what is happening look at the Apache Karaf log file, either from the console

    karaf@root()> log:display

   or from the command line

    <Apache Karaf home> $ tail -f data/log/karaf.log

The Camel Route is setup to generate a new message every 60 seconds (60000 milliseconds).

The following steps take you through how to use the Karaf console to change the property that
Camel is using as the prefix string for the message its printing out.

1) Start a properties editing session

    karaf@root()> config:edit org.camelcookbook.testing

2) Set the value of the property used as the message prefix. The following sets it to the value
   of 'Override', and you can use any value you like

    karaf@root()> config:property-set transform.message Override
 
3) Update the property set, which pushes the properties to our deployed Camel test application,
   and that will trigger the OSGi to reload based on the update-strategy we set in the
   Blueprint XML file.

    karaf@root()> config:update

If everything worked correctly, you should see in the log that Karaf has restarted the Camel
test application and it should be now using your new property value, e.g. 'Override', for the
messages being printed out.