## Java build tasks

![Gradle java builds](https://docs.gradle.org/current/userguide/img/javaPluginTasks.png)
Source: https://docs.gradle.org/current/userguide/java_plugin.html#sec:java_tasks

## Multi project build structure

Use `gradle init` and answer interactive questions to generate project skeleton.

Use specific command line options if we know what we are doing.

```bash
# The below will use current directory name as the project name.
gradle init --type java-application  --java-version 21 --dsl groovy \
    --split-project \
    --no-incubating \
    --project-name $(basename `pwd`)
```

### Nested projects structure

If sub-projects are located inside another folder, the projects are nested. To properly include the sub-projects into
the build, do the below:

Folder structure
```
─┬── Project root
 └┬─ FolderFoo
  └─ ProjectBar
```

/settings.gradle
```
include(':FolderFoo:SubProjectBar')
```

## Customize tasks

### Do something in the end of a task

```groovy
tasks.named('generateProto') {
    doLast {
        // ...
    }
}
```

## Plugins

- Shadow jar (fat jar)

  Add the below gradle plugin
    
  ```groovy
  plugins {
     id "com.github.johnrengelman.shadow" version "7.1.2"
  }
  ```

- [Publish](publish)
- [Release](release)
- Protobuf
  
  Below is an basic example that only generate the java classes from src/main/proto.

  #### /gradle/libs.versions.toml

  ```toml
  [versions]
  protobuf = "4.28.2"
  
  [libraries]
  protobuf-java = {module = "com.google.protobuf:protobuf-java", version.ref = "protobuf"}
  protobuf-java-util = {module = "com.google.protobuf:protobuf-java-util", version.ref = "protobuf"}
  
  [bundles]
  protobuf = ["protobuf-java", "protobuf-java-util"]
  
  [plugins]
  protobuf = { id = "com.google.protobuf", version="0.9.4" }
  ```

  #### /build.gradle

  ```groovy
  plugins {
    alias libs.plugins.protobuf
  }
  dependencies {
    api libs.bundles.protobuf
  }
  protobuf {
    // Configure the protoc executable
    protoc {
      // Download from repositories
      artifact = "com.google.protobuf:protoc:" + libs.versions.protobuf.get()
    }
  }
  ```
  
  See [protobuf gradle plugin](https://github.com/google/protobuf-gradle-plugin/tree/master) for references.