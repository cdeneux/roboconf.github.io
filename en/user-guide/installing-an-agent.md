---
title: "Installing an Agent"
layout: page
id: "ug.snapshot.installing-an-agent"
menus: [ "users", "user-guide" ]
---

A Roboconf agent is an OSGi application which in charge of executing instructions on the host it is deployed on.  
These instructions are either given by the DM, or deduced by the agent from a change in the global application.
As an example, if there is a web application on the host, and that it depends on a database, and that the database
is undeployed, then the agent will stop the web application.

> The instructions, as well as the notifications, all go through the messaging server.

A Roboconf agent is associated to a given application.  
And more specifically, it is tied to a single *root instance* in this application. A *root instance* designates a machine in Roboconf's DSL.
So, an agent will undertake and communicate about all the applications parts that are deployed on the same host than it.

To install a Roboconf agent, you must have:

* A Java Virtual Machine (JVM) for Java 1.6+.  
OpenJDK and Oracle JDK are supported.

* Bash if you want to use the **Bash** plug-in.

* Puppet if you want to use the **Puppet** plug-in.  
Make sure you have a recent version of Puppet. See [this page](plugin-puppet.html)
for more information about Puppet installation. Puppet **must** be installed on the virtual image.

Although we generally test Roboconf with Ubuntu systems, there is no reason it does not work on other systems.  
However, **we recommend using Linux systems**.

Several options are available to install an agent.

* Use our Debian package for the agent. 
* Deploy our predefined OSGi distribution for the agent.
* Deploy the agent as a Karaf feature on your own Karaf server.
* Deploy the agent's bundles in an OSGi container.


# Root Access

The agent will install and modify applications on the host.  
To run correctly, the **agent must run with root privileges**.

Besides, an agent should be configured to start with the machine / VM.  
This is necessary if you deploy in a cloud environment.

On Linux systems, making a program start with the operating system can be achieved a **init.d** script.  
You can also edit the **/etc/rc.local** file and launch the program at the end of the script.

Notice that all these notices are handled automatically if you install the Debian package.  
Otherwise, you will have to handle this by hand.


# The Debian package for the Agent

The Debian package installs our custom Karaf distribution for Roboconf's agent.  
Once installed, the Karaf server is started as a service. The agent runs within the Karaf server.

> We do not have a public server yet to simply type in **sudo apt-get install roboconf-agent**.  
> Hopefully, this will come soon.

Grab the agent's Debian package on the [download page](../download.html) and install it.


# The Custom Karaf Distribution for the Agent

[Apache Karaf](http://karaf.apache.org/) is an OSGi server.  
It brings additional features to OSGi containers like Felix (Apache) or Equinox (Eclipse). It also allows to build custom distributions
with pre-installed bundles and their configurations. The custom Karaf distribution for the agent is thus a stand-alone server with the
Roboconf's agent being already deployed and configured. It is configured to run with [Apache Felix](http://felix.apache.org/).

> Having several Karaf servers on a machine can result in port conflicts.
> If it is your case, you may have to configure the ports differently. 
> Or you may prefer deploying our Karaf feature.

This distribution can be deployed either with a Debian package or manually.  
To deploy it manually, you must have a JDK 6 (or higher) installed. Then, [download the agent's ZIP archive](../download.html)
and unzip it somewhere on your disk. 

To start it, execute...

```bash
cd bin
sudo ./karaf
```

You can find help and more instructions about Karaf on [its web site](http://karaf.apache.org/).


# The Agent Feature for Karaf

If you already have your own Karaf server, you may prefer to install the agent as a feature.  
The agent feature is a XML file that lists the bundles to install and some of their dependencies so that the agent works.

We do not have any official release yet.  
However, you can add our snapshot repository to the list of features repository.

Start your karaf and execute...

```properties
# Add the feature URL
features:addurl https://oss.sonatype.org/content/repositories/snapshots/net/roboconf/roboconf-karaf-feature-agent/0.2-SNAPSHOT/feature.feature.xml

# Install the feature 
feature:install roboconf-agent
```

For the record, the **roboconf-agent** feature depends on the following features:

* **ipojo-all** since Roboconf bundles use iPojo.
* **config** since the agent can be reconfigured through *Config Admin*.


# The Agent Bundles

It is possible to deploy the agent bundles directly in Felix, Equinox or any other OSGi container.  
To determine all the required bundles, take a look at the Karaf feature described above. The previous section
also lists their dependencies (as other Karaf features).