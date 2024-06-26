# What is Lightstreamer Server?

Lightstreamer is a real-time messaging server optimized for the Internet. Blending WebSockets, HTTP, and push notifications, it streams data to/from mobile, tablet, browser-based, desktop, and IoT applications.

For more information and related downloads for Lightstreamer Server and other Lightstreamer products, please visit [www.lightstreamer.com](https://www.lightstreamer.com).

%%LOGO%%

# How to use this image

## Up and Running

Launch the container with the default configuration:

```console
$ docker run --name ls-server -d -p 80:8080 %%IMAGE%%
```

This will map port 8080 inside the container to port 80 on local host. Then point your browser to `http://localhost` and watch the Welcome page showing real-time data flowing in from the locally deployed demo application, which is a first overview of the unique features offered by the Lightstreamer technology. More examples are available online at the [demo site](https://demos.lightstreamer.com).

## Custom settings

It is possible to customize each aspect of the Lightstreamer instance running into the container. For example, a specific configuration file may be supplied as follows:

```console
$ docker run --name ls-server -v /path/to/my-lightstreamer_conf.xml:/lightstreamer/conf/lightstreamer_conf.xml -d -p 80:8080 %%IMAGE%%
```

In the same way, you could provide a custom logging configuration, maybe in this case also specifying a dedicated volume to ensure both the persistence of log files and better performance of the container:

```console
$ docker run --name ls-server -v /path/to/my-lightstreamer_log_conf.xml:/lightstreamer/conf/lightstreamer_log_conf.xml -v /path/to/logs:/lightstreamer/logs -d -p 80:8080 %%IMAGE%%
```

If you also change in your `my-lightstreamer_log_conf.xml` file the default logging path from `../logs` to `/path/to/dest/logs`:

```console
$ docker run --name ls-server -v /path/to/my-lightstreamer_log_conf.xml:/lightstreamer/conf/lightstreamer_log_conf.xml -v /path/to/hosted/logs:/path/to/dest/logs -d -p 80:8080 %%IMAGE%%
```

Alternatively, the above tasks can be executed by deriving a new image through a `Dockerfile` as the following:

```dockerfile
FROM %%IMAGE%%

# Please specify a COPY command only for the the required custom configuration file
COPY my-lightstreamer_conf.xml /lightstreamer/conf/lightstreamer_conf.xml
COPY my-lightstreamer_log_conf.xml /lightstreamer/conf/lightstreamer_log_conf.xml
```

where `my-lightstreamer_conf.xml` and `my-lightstreamer_log_conf.xml` are your custom configuration files, placed in the same directory as the Dockerfile. By simply running the command:

```console
$ docker build -t my-lightstreamer .
```

the new image will be built along with the provided files. After that, launch the container:

```console
$ docker run --name ls-server -d -p 80:8080 my-lightstreamer
```

To get more detailed information on how to configure the Lightstreamer server, please see the inline documentation in the `lightstreamer_conf.xml` and `lightstreamer_log_conf.xml` files you can find under the `conf` folder of the installation directory.

## Deployment of Adapter Sets

You might want to use this image even with any Adapter Set, either developed by yourself or provided by third parties.

To accomplish such goal, you may use similar strategies to those illustrated above:

### Deployment of a single Adapter Set

To deploy a single custom Adapter Set, the simplest way is to attach its files into the factory adapters folder, as follows:

```console
$ docker run --name ls-server -v /path/to/my-adapter-set:/lightstreamer/adapters/my-adapter-set -d -p 80:8080 %%IMAGE%%
```

### Full replacement of the "adapters" folder

In the case you have many custom Adapter Sets to deploy, a more appropriate strategy is to replace the factory adapters folder with the one located in your host machine:

```console
$ docker run --name ls-server -v /path/to/my-adapters:/lightstreamer/adapters -d -p 80:8080 %%IMAGE%%
```

In this case, the `/path/to/my-adapters` folder has to be structured with the required layout for an adapters folder:

```console
/path/to/my-adapters+
                    +my_adapter_set_1
                    +my_adapter_set_2
                    ...
                    +my_adapter_set_N
```

### Building a new image

Once again, a linear and clean approach is to make a new image including all needed files.

In this case, you could write a simple Docker file in which the list of all your Adapter Sets configuration files is provided:

```dockerfile
FROM %%IMAGE%%

# Will copy the contents of N Adapter Sets into the factory adapters folder
COPY my-adapter-set-1 /lightstreamer/adapters/my-adapter-set-1
COPY my-adapter-set-2 /lightstreamer/adapters/my-adapter-set-2
COPY my-adapter-set-3 /lightstreamer/adapters/my-adapter-set-3
```

Then, just build and start the container as already explained.

## Deployment of web server pages

There might be some circumstances where you would like to provide custom pages for the internal web server of the Lightstreamer Server. Even in this case, it is possible to customize the container by employing the same techniques as above.

For example, with the following command you will be able to fully replace the factory `pages` folder:

```console
$ docker run --name ls-server -v /path/to/custom/pages:/lightstreamer/pages -d -p 80:8080 %%IMAGE%%
```

where `/path/to/custom/pages` is the path in your host machine containing the replacing web content files.
