# paste-elements-webjar

WebJar packaging for the PasteStack Elements UI modules. The default release build
downloads the upstream `paste-elements` release tag named by the `upstreamVersion`
property in `pom.xml` and repackages its tree under
`META-INF/resources/webjars/paste-elements-webjar/{project.version}/`, so a JVM
application can serve the modules without a separate asset checkout.

Branch builds instead run with `-Pdevelopment-upstream`, which downloads the immutable
upstream development archive committed in the pom (`upstream.development.url`,
`upstream.development.version`, `upstream.development.sha256`), verifies its SHA-256
before extracting, and packages that tree under the same namespace. The jar records
the pin it was built from in `META-INF/paste-upstream.properties`.

## Consuming

```xml
<repository>
  <id>paste-registry</id>
  <url>https://gitlab.com/api/v4/projects/70289607/packages/maven</url>
</repository>

<dependency>
  <groupId>com.pastestack</groupId>
  <artifactId>paste-elements-webjar</artifactId>
  <version>0.3.0</version>
</dependency>
```

```scala
// build.sbt
resolvers += "paste-registry" at "https://gitlab.com/api/v4/projects/70289607/packages/maven"

libraryDependencies += "com.pastestack" % "paste-elements-webjar" % "0.3.0"
```

## Contents

The jar carries the upstream tree as published, not a repackaged `src/` layout:

```
/webjars/paste-elements-webjar/0.3.0/base/*.scss          Variables, reset, base styles
/webjars/paste-elements-webjar/0.3.0/modules/*/*.js        Element behaviour
/webjars/paste-elements-webjar/0.3.0/modules/*/*.scss      Element styles
/webjars/paste-elements-webjar/0.3.0/structure/*           Grid, layout, spacing, typography
```

## Versioning

`VERSION`, the pom's `<version>`, and the pom's `<upstreamVersion>` are one value: the
WebJar's version is the upstream version it wraps. CI refuses to build when they differ,
and refuses to publish a tag that does not match `VERSION`.

Tag pipelines run the release build and wrap the upstream release tag. Branch
pipelines activate `-Pdevelopment-upstream` instead: they wrap the pinned development
archive and publish under CI-assigned branch versions, so the resource namespace
always matches the published Maven version. The development pin is refreshed by
updating `upstream.development.version`, `upstream.development.url`, and
`upstream.development.sha256` together to a new immutable upstream archive; the base
version follows the repository's release flow; CI assigns development versions
with `versions:set` during publication.

## Building

```shell
mvn --settings ./settings.xml clean package
mvn --settings ./settings.xml -Pdevelopment-upstream clean package
```

`unzip` must be on the path — the release build extracts the downloaded upstream
archive with it.

Publishing is CI's job. To deploy by hand, supply the registry credentials the
`settings.xml` server entry reads and run the deploy:

```shell
export GL_USERNAME=<user> GL_PASSWORD=<token>
mvn --settings ./settings.xml deploy
```

## License

This repository's build files: Apache License, Version 2.0 (`LICENSE`, `NOTICE.md`).

The published jar contains only the packaged upstream tree, so its terms are that
tree's — MIT for the `paste-elements` 0.3.0 release tag, and Apache-2.0 for the
development archives branch builds wrap. The `pom.xml` license properties and the
flattened published POM reflect whichever upstream the jar was built from. The
upstream `LICENSE` travels inside the jar.
