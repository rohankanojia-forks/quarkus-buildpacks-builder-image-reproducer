# quarkus-buildpacks-builder-image-reproducer

## Using Quarkus Extension

In order to build container image using BuildPacks for your Quarkus application. Run this command:

```
gradle build -Dquarkus.container-image.build=true
```

This uses `paketobuildpacks/builder:base` as BuildPack builder image.

When I run the built image using `docker run`, I run into this error:
```
docker run rokumar/quarkus-buildpacks-builder-image-reproducer:1.0.0-SNAPSHOT
ERROR: failed to launch: determine start command: when there is no default process a command is required
```

When I try to customize the builder image I see these behaviors:

- When customizing buildpack builder image to `gcr.io/paketo-buildpacks/quarkus:0.2.2`, I get this error
```
gradle build -Dquarkus.container-image.build=true  -Dquarkus.buildpack.jvm-builder-image=gcr.io/paketo-buildpacks/quarkus:0.2.2

> Task :quarkusAppPartsBuild FAILED

FAILURE: Build failed with an exception.

* What went wrong:
Execution failed for task ':quarkusAppPartsBuild'.
> There was a failure while executing work items
   > A failure occurred while executing io.quarkus.gradle.tasks.worker.BuildWorker
      > io.quarkus.builder.BuildException: Build failure: Build failed due to errors
                [error]: Build step io.quarkus.container.image.buildpack.deployment.BuildpackProcessor#buildFromJar threw an exception: java.lang.NullPointerException: Cannot read the array length because "<local2>" is null
                at dev.snowdrop.buildpack.Buildpack.prep(Buildpack.java:224)
                at dev.snowdrop.buildpack.Buildpack.build(Buildpack.java:104)
                at dev.snowdrop.buildpack.Buildpack.<init>(Buildpack.java:98)
                at dev.snowdrop.buildpack.EditableBuildpack.<init>(EditableBuildpack.java:16)
                at dev.snowdrop.buildpack.BuildpackBuilder.build(BuildpackBuilder.java:72)
                at io.quarkus.container.image.buildpack.deployment.BuildpackProcessor.runBuildpackBuild(BuildpackProcessor.java:208)
                at io.quarkus.container.image.buildpack.deployment.BuildpackProcessor.buildFromJar(BuildpackProcessor.java:89)
                at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
                at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:77)
                at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
                at java.base/java.lang.reflect.Method.invoke(Method.java:568)
                at io.quarkus.deployment.ExtensionLoader$3.execute(ExtensionLoader.java:849)
                at io.quarkus.builder.BuildContext.run(BuildContext.java:256)
                at org.jboss.threads.ContextHandler$1.runWith(ContextHandler.java:18)
                at org.jboss.threads.EnhancedQueueExecutor$Task.doRunWith(EnhancedQueueExecutor.java:2516)
                at org.jboss.threads.EnhancedQueueExecutor$Task.run(EnhancedQueueExecutor.java:2495)
                at org.jboss.threads.EnhancedQueueExecutor$ThreadBody.run(EnhancedQueueExecutor.java:1521)
                at java.base/java.lang.Thread.run(Thread.java:833)
                at org.jboss.threads.JBossThread.run(JBossThread.java:483)

```
- When customizing buildpack builder image to `codejive/buildpacks-quarkus-builder:jvm`, I get this error
```
gradle build -Dquarkus.container-image.build=true  -Dquarkus.buildpack.jvm-builder-image=codejive/buildpcodejive/buildpacks-quarkus-builder:jvm

===> DETECTING
ERROR: No buildpack groups passed detection.
ERROR: Please check that you are running against the correct path.
ERROR: failed to detect: no buildpacks participating
> Task :quarkusAppPartsBuild FAILED

FAILURE: Build failed with an exception.

* What went wrong:
Execution failed for task ':quarkusAppPartsBuild'.
> There was a failure while executing work items
   > A failure occurred while executing io.quarkus.gradle.tasks.worker.BuildWorker
      > io.quarkus.builder.BuildException: Build failure: Build failed due to errors
                [error]: Build step io.quarkus.container.image.buildpack.deployment.BuildpackProcessor#buildFromJar threw an exception: java.lang.IllegalStateException: Buildpack build failed
                at io.quarkus.container.image.buildpack.deployment.BuildpackProcessor.runBuildpackBuild(BuildpackProcessor.java:211)
                at io.quarkus.container.image.buildpack.deployment.BuildpackProcessor.buildFromJar(BuildpackProcessor.java:89)
                at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke0(Native Method)

```


## Using Eclipse JKube

In order to build container image using Kubernetes Gradle Plugin for your Quarkus application. Run this command:
```
gradle k8sBuild
```

This uses `paketobuildpacks/builder:base` as BulildPack builder image.

When I run the built image using `docker run`, I run into this error:
```
ERROR: failed to launch: determine start command: when there is no default process a command is required
```
