# paste-elements-webjar

WebJar packaging for the PasteStack Elements UI modules. The build downloads the upstream
`paste-elements` archive named by the `upstreamVersion` property in `pom.xml` and repackages
its tree under `META-INF/resources/webjars/paste-elements-webjar/{upstreamVersion}/`, so a
JVM application can serve the modules without a separate asset checkout.

## Consuming

```xml
<repository>
  <id>paste-registry</id>
  <url>https://gitlab.com/api/v4/projects/70289607/packages/maven</url>
</repository>

<dependency>
  <groupId>com.pastestack</groupId>
  <artifactId>paste-elements-webjar</artifactId>
  <version>0.2.0</version>
</dependency>
```

```scala
// build.sbt
resolvers += "paste-registry" at "https://gitlab.com/api/v4/projects/70289607/packages/maven"

libraryDependencies += "com.pastestack" % "paste-elements-webjar" % "0.2.0"
```

## Contents

The jar carries the upstream tree as published, not a repackaged `src/` layout:

```
/webjars/paste-elements-webjar/0.2.0/base/*.scss          Variables, reset, base styles
/webjars/paste-elements-webjar/0.2.0/modules/*/*.js        Element behaviour
/webjars/paste-elements-webjar/0.2.0/modules/*/*.scss      Element styles
/webjars/paste-elements-webjar/0.2.0/structure/*           Grid, layout, spacing, typography
```

## Versioning

`VERSION`, the pom's `<version>`, and the pom's `<upstreamVersion>` are one value: the
WebJar's version is the upstream version it wraps. CI refuses to build when they differ,
and refuses to publish a tag that does not match `VERSION`.

## Building

```shell
mvn --settings ./settings.xml clean package
```

`unzip` must be on the path — the build extracts the downloaded upstream archive with it.

Publishing is CI's job. To deploy by hand, supply the registry credentials the
`settings.xml` server entry reads and run the deploy:

```shell
export GL_USERNAME=<user> GL_PASSWORD=<token>
mvn --settings ./settings.xml deploy
```

## License

This repository's build files: Apache License, Version 2.0 (`LICENSE`, `NOTICE.md`).

The published jar contains only the packaged `upstreamVersion`, so its terms are that
version's — MIT for `paste-elements` 0.2.0, as `pom.xml` declares, and Apache-2.0 once
`upstreamVersion` reaches a relicensed release. The upstream `LICENSE` travels inside
the jar.
