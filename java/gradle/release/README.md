Manage release version
----------------------

Similar to maven, gradle has a [release plugin](https://github.com/researchgate/gradle-release) to manage release
version.

- Take the current version number from properties, ex: 1.0.0-SNAPSHOT
- Calculate the to-be-released version. Usually it removes the -SNAPSHOT from the current version
- Calculate the next development version. Usually by increasing the patch number of the current version.
- Add a tag of the release to the git repository
- Support after release task to allow customized actions to be hooked.

### Simple project

gradle.properties
```properties
version=1.0.0-SNAPSHOT
```

build.gradle
```groovy
plugins {
    id 'net.researchgate.release' version '3.0.2'
}

// only needed if customizations are required
release {
    // Name the tag in the format of 'v1.0.0'
    tagTemplate = 'v${version}'
}
```

Running
```bash
./gradlew release
```

### Multi-module project

Assuming the modules are released separately.

build.gradle
```groovy
// No special modification is needed since there is no releasing at this level.
```

${module}/gradle.properties
```properties
version=1.0.0-SNAPSHOT
```

${module}/build.gradle
```groovy
plugins {
    id 'net.researchgate.release' version '3.0.2'
}

// only needed if customizations are required
release {
    // Name the tag in the format of 'foo-v1.0.0'
    tagTemplate = '${name}-v${version}'
    // Store the version outside gradle.property, it will needs to be retrieved any way. 
    versionPropertyFile = 'version.properties'
}


publishing {
    // read version from version.properties (created by release plugin)
    def props = new Properties()
    file("version.properties").withInputStream { props.load(it) }
    println("version: " + props.getProperty("version"))
    version = props.getProperty("version")
    publications {
        ...
    }
}
```

Running
```bash
# Release module 'core'
./gradlew clean :core:release
```