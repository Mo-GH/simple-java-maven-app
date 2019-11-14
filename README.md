# simple-java-maven-app

This repository is for the
[Build a Java app with Maven](https://jenkins.io/doc/tutorials/build-a-java-app-with-maven/)
tutorial in the [Jenkins User Documentation](https://jenkins.io/doc/).

The repository contains a simple Java application which outputs the string
"Hello world!" and is accompanied by a couple of unit tests to check that the
main application works as expected. The results of these tests are saved to a
JUnit XML report.

The `jenkins` directory contains an example of the `Jenkinsfile` (i.e. Pipeline)
you'll be creating yourself during the tutorial and the `scripts` subdirectory
contains a shell script with commands that are executed when Jenkins processes
the "Deliver" stage of your Pipeline.


Maven Commands:

mvn --version

Creating a Project

You will need somewhere for your project to reside, create a directory somewhere and start a shell in that directory. On your command line, execute the following Maven goal:

mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=my-app -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.4 -DinteractiveMode=false

Build the Project:

mvn package

Frequently used Commands

Here are some frequently used commands. If you are new to maven, you should try each command to get familiar with them:

Command: mvn clean

Description: Invoking clean phase of Clean LifeCycle	

Task details:
Removes the files from a project's working directory, that were generated at build-time. Alternatively we can execute the plugin:  mvn clean:clean That will do the same thing. 

Command: mvn compile
Description: Invoking compile phase of Default Lifecycle

Task details:
Compiles the source code of the projects. This command will do all pre compile phases which are validate, initialize, generate-sources, process-sources, generate-resources, process-resources, and finally compile. Remember execution falls from start to the phase we are invoking. That makes sense because for a phase to work it's prerequisite state must be achieved.
One important point here: by default executing this phase will download external dependencies from remote repository. In fact any first plugin in execution sequence annotated with @requiresDependencyResolution will cause that (we will see that annotation in action when we learn how to write our own plugins)
Again we can do the same thing as: mvn compiler:compile


Command: mvn clean package

Description: Invoking clean phase of Clean Lifecycle followed by package phase of Default Lifecycle.

Task details:
Cleans the target directory first then compiles, runs unit tests and packages the compiled code and other files into a single file. The type of final file being created depends on what we have defined in <packaging> in our pom.xml. the valid packaging values are jar, war, ear and pom
	

Command: mvn clean install

Description: Invoking clean phase of Clean Lifecycle followed by install phase of Default Lifecycle.

Task details:
Cleans the previous build, then packages and then install the package into the local repository. Which by default is at User Home Directory/.m2/repository/. The local repository is a cache of the remote downloads(dependencies), and locally installed artifacts) which can be used as a dependency in other local projects. Our locally installed artifacts are actually temporary ones until we release that and put into some remote repository. 

Command: mvn test

Description: Invoking clean phase of Clean Lifecycle followed by install phase of Default Lifecycle.

Task details:
Cleans the previous build, then packages and then install the package into the local repository. Which by default is at User Home Directory/.m2/repository/. The local repository is a cache of the remote downloads(dependencies), and locally installed artifacts) which can be used as a dependency in other local projects. Our locally installed artifacts are actually temporary ones until we release that and put into some remote repository.


Command: mvn test

Description: Invoking test phase of Default Lifecycle.

Task details:
Runs the unit test from compiled source and test source files. This is one step above package phase. If we are running some post test phase and want to skip tests then use skip optionObject.
For example mvn install -Dmaven.test.skip=true


Command:mvn dependency:list
Description: Invoking list goal of dependency plugin (a tool, not bound to any phase by default)

Task details:
Lists all Maven dependencies from our project. Please use
mvn help:describe -Dplugin=dependency
to see other goals available with this plugin.
From help:describe you can discover more goals of a plugin, then to know more about a particular goal (let's say 'get' goal of 'dependency' plugin), you can use:
mvn help:describe -Dcmd= dependency:get


Command: mvn help:effective-pom
Description: Invoking effective-pom goal of help plugin(tool)
		

Task details:
Displays the effective POM as an XML for this build. This is also helpful to know what currently plugins (either bound to a phase or tool plugins) are available/installed. Please invoke  mvn help:describe -Dplugin= help to see all goals available with help plugin. 

*************************

more info about maven:

https://myopswork.com/maven-as-a-devops-tool-f5d04b0fc55c

--------------------------------
Maven:
Installing Apache Maven

The installation of Apache Maven is a simple process of extracting the archive and adding the bin folder with the mvn command to the PATH.

It should print out your installed version of Maven

js$ mvn --version

For more detailed installation please refer this link from Maven Home Site.

*******
Important Concepts:
1. Maven Build Lifecycle

Maven defines and follows conventions. Right from the project structure to building steps, Maven provides conventions to follow. If we follow those conventions, with minimal configuration we can easily get the build job done.

There are three built-in build life cycle ‘clean’, ‘default’ and ‘site’. A life cycle has multiple phases. For example, ‘default’ lifecycle has following phases (listed only the important phases),

    compile — compiles the source code
    test — executes unit test cases
    package — bundles the compiled code (Ex: war / jar)
    install — stores the built package in local Maven repository
    deploy — store in remote repository for sharing

So to go through the above phases, we just have to call one command:

mvn <phase> { Ex: mvn install }
js$ mvn install
  
For the above command, starting from the first phase, all the phases are executed sequentially till the ‘install’ phase.
A goal represents a specific task which contributes to the building and managing of a project. It may be bound to zero or more build phases. A goal not bound to any build phase could be executed outside of the build lifecycle by direct invocation.

The order of execution depends on the order in which the goal(s) and the build phase(s) are invoked. For example, consider the command below. The clean and package arguments are build phases while the dependency:copy-dependencies is a goal.

mvn clean dependency:copy-dependencies package

Here the clean phase will be executed first, followed by the dependency:copy-dependencies goal, and finally package phase will be executed.

2. Maven Repository

Repository is where the build artifacts are stored. Build artifacts means, the dependent files (Ex: dependent jar files) and the build outcome (the package we build out of a project).

There are two types of repositories, local and remote. Local maven repository(.m2) is in the user’s system. It stores the copy of the dependent files that we use in our project as dependencies. Remote maven repository is setup by a third party(nexus) to provide access and distribute dependent files. Ex: repo.maven.apache.org from internet.
3. POM Example

POM stands for Project Object Model. It is fundamental unit of work in Maven. It is an XML file that resides in the base directory of the project as pom.xml.And has all the configuration settings for the project build.

Generally we define the project dependencies (Ex: dependent jar files for a project), maven plugins to execute and project description /version etc.

Simplest pom.xml should have 4 important information.

    modelVersion-4.0.0 (POM version for Maven 2 and is always required)
    groupId —will identify your project uniquely across all projects, ex:ebs.obill.webs, com.companyname.project
    artifactId — is the name of the jar without version(keeping in mind that it should be jar-name friendly)
    version — if you distribute it then you can choose any typical version with numbers and dots (1.0, 1.1, 1.0.1, …)

<project>
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.intuit.jsapp</groupId>
  <artifactId>js-app</artifactId>
  <version>1</version>
</project>

4. Maven Dependencies

There is an element available for declaring dependencies in project pom.xml This is used to define the dependencies that will be used by the project. Maven will look for these dependencies when executing in the local maven repository. If not found, then Maven will download those dependencies from the remote repository and store it in the local maven repository.

Example declaring junit and log4j as project dependencies,

<dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>log4j</groupId>
      <artifactId>log4j</artifactId>
      <version>1.2.12</version>
      <scope>compile</scope>
    </dependency>
  </dependencies>

scope — describes under which context this dependency will be used.

5. Maven Plugins

All the execution in Maven is done by plugins. A plugin is mapped to a phase and executed as part of it. A phase is mapped to multiple goals. Those goals are executed by a plugin. We can directly invoke a specific goal while Maven execution. A plugin configuration can be modified using the plugin declaration.

An example for Maven plugin is ‘compiler’, it compiles the java source code. This compiler plugin has two goals compiler:compile and compiler:testCompile.

Using the configuration element, we can supply arguments to the plugin.

<build>
 <finalName>springexcelexport</finalName>
 	<plugins>
	 <plugin>
		<groupId>org.apache.maven.plugins</groupId>
		<artifactId>maven-war-plugin</artifactId>
		<version>2.2</version>
	</plugin>
	<plugin>
		<groupId>org.apache.maven.plugins</groupId>
		<artifactId>maven-surefire-plugin</artifactId>
		<version>2.16</version>
		<configuration>
			<skipTests>true</skipTests>
		</configuration>
	</plugin>
</build>
  
Maven Project Structure

Maven uses a convention for project folder structure. If we follow that, we need not describe in our configuration setting, what is located where. Maven knows from where to pick the source files, test cases etc. Following is a snap shot from a Maven project and it shows the project structure.

BANL13dd26e76:jaisriram agv$ tree
.
├── pom.xml
└── src
    ├── main
    │   └── java
    │       └── com
    │           └── intuit
    │               └── App.java
    └── test
        └── java
            └── com
                └── intuit
                    └── AppTest.java9 directories, 3 files

First Maven Project

Let us create our first maven project. Hope you have setup Maven already. In Maven we have ‘Archetype’. It is nothing by a template for projects. Maven provides templates to start a project and using this we can quickly start a Maven project. Execute the following command in cmd prompt,

agv$ mvn archetype:generate -DgroupId=com.js.demo.java  -DartifactId=com.js.demo.java -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

This will create a sample Maven project skeleton using we can start building the application.We will get a a pom.xml and let us use that to build the newly created Maven project. Go inside the newly created Maven project root and execute the command (this is where the pom.xml is available),

mvn package

Now this will execute all the Maven phases till the ‘package’ phase. That is, Maven will compile, verify and build the jar file and put it in target folder under the project.
