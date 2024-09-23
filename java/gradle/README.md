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

# Plugins

## Shadow jar (fat jar)

Add the below gradle plugin

```groovy
plugins {
   id "com.github.johnrengelman.shadow" version "7.1.2"
}
```