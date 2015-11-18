# Embedded MySQL Gradle Plugin

This plugin is built around the [embedded mysql project](https://github.com/wix/wix-embedded-mysql).
It allows you to easily start up a MySQL process withouth having to install it by hand. Currently
the plugin only exposes the ability to be able to set the port the process is running on and a 
username a password to use when connecting. There are various other configuration options offered
in the [embedded mysql project](https://github.com/wix/wix-embedded-mysql) that could be exposed with
further development of the plugin if / when required.

## Usage

To use the plugin's functionality, you will need to add the its binary artifact to your build script's
classpath and apply the plugin.

### Adding the plugin binary to the build

The plugin JAR needs to be defined in the classpath of your build script. It is directly available on
[Maven Central](http://search.maven.org/).
The following code snippet shows an example on how to retrieve it from Bintray:

```
buildscript {
    repositories {
        mavenCentral()
    }
    dependencies {
        classpath 'uk.co.mruoc.mysql:embedded-mysql-plugin:0.1.0'
    }
}
```

### Applying the plugin

To use the plugin, include the following code snippetin your build script:

```
apply plugin: 'embedded-mysql'
```

This will add two new tasks to your gradle build:

* startEmbeddedMysql
* stopEmbeddedMysql

It should be fairly obvious what these two tasks do, before you can use them you will need to
configure how you want your mysql to run, you do this by providing properties to the EmbeddedMysqlExtension
as shown below.

```
embeddedMysql {
    databaseName = 'my-database-name'
    port = 3306
    username = 'root'
    password = ''
}
```

Once you have set these properties they will be used to spin up your database so you can connect to
it as you would any other mysql database.

### Connecting to the database

Given the example shown above and using the standard MySQL JDBC connector you would connect to
this database using the code shown below.

```
    Class.forName("com.mysql.jdbc.Driver")
    Connection connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/my-database-name", "root", "")
```