# What is Wisdom Framework

[Wisdom Framework](http://wisdom-framework.org) is a Java-based web stack enabling modularity and dynamism. Unlike others web stack, Wisdom let you build you web applications by assembling different _modules_ together, and thus dynamically. Wisdom provides a very efficient _watch mode_ making the development of such application very effective.

Wisdom is based on Apache Maven and OSGi, but makes it so easy that you don't even realize you are using them.

# Docker image

The docker image provided by this repository let you:

* Run a sample Wisdom application - just to see
* Use it as a development environment
* Build your own image embedding you application

# Run it

The image runs a sample application, the Wisdom monitor and also embeds the documentation. Run it using:

```
docker run -d  -p 9000:9000 wisdom/wisdom-framework
```

Then, you can access:

* the sample application of http://_host_:9000/samples
* the monitor application of http://_host_:9000/monitor (admin/admin)
* the sample application of http://_host_:9000/assets/index.html

Replace _host_ by either `localhost` on Linux, or the Boot2Docker IP on Mac (generally `http://192.168.59.103`)

# Use it as a development environment for a Maven build

To use this image as development environment, you may have to setup some _volumes_:

* `/wisdom/application` stores your application
* `/wisdom/conf` stores the configuration

If you project is using the Wisdom-Maven-Plugin, run an initial

```
mvn package
```

and then, launch the docker images as follows:

```
docker run -d \
  -p 9000:9000 \
  -v `pwd`/target/wisdom/logs:/wisdom/logs \
  -v `pwd`/target/wisdom/conf:/wisdom/conf \
  -v `pwd`/target/wisdom/application:/wisdom/application \
  wisdom/wisdom-framework
```

Your application is now running.

So enable the watch mode, launch maven using:

```
mvn wisdom:run -DwisdomDirectory=target/wisdom
```

# Use it as a development environment

As in the previous example using Maven, to use this image as development environment, you may have to setup some _volumes_:

* `/wisdom/application` stores your application
* `/wisdom/conf` stores the configuration
* `/wisdom/logs` stores the logs

In that case, create, on your file system, the directory structure you need:

```
mkdir -p ~/tmp/wisdom/application
# Only if you have configuration to provide
mkdir -p ~/tmp/wisdom/conf
# Only if you want to store the logs on your file system
mkdir -p ~/tmp/wisdom/logs
```

If you have a custom configuration, copy it to the `conf` directory you created.

Then, launch the docker image using:

```
docker run -d \
-p 9000:9000 \
-v ~/tmp/logs:/wisdom/logs \
-v ~/tmp/target/wisdom/conf:/wisdom/conf \
-v ~/tmp/target/wisdom/application:/wisdom/application \
wisdom/wisdom-framework
```

To get the _watch mode_ benefits, launch your project using:

```
mvn wisdom:run -DwisdomDirectory=~/tmp/wisdom
```

# Use it to package your own application

The last use case of this image is to use it as a _base_ image for your own image:

```
FROM wisdom/wisdom-framework

COPY target/wisdom/application /wisdom/application
COPY target/wisdom/conf /wisdom/conf
```

# Directory structure

The image file system is organised as follows:
* `/wisdom/bin` - bin directory
* `/wisdom/core` - core OSGi bundles
* `/wisdom/runtime` - runtime OSGi bundles
* `/wisdom/application` - application bundles and webjars (watched)
* `/wisdom/conf` - configuration directory

# Comments and Feedback

If you have comments, feedbacks, improvements, don't hesitate: https://groups.google.com/forum/#!forum/wisdom-discuss
