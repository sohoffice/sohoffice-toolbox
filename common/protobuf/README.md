Protocol Buffers (protobuf)
===========================

protobuf is a cross platform data exchange format.
Get more information in the [main web site](https://protobuf.dev/)

Plugins extension point
-----------------------

In the document, it mentioned the
[plugin insertion points](https://protobuf.dev/reference/java/java-generated/#plugins). It's not super clear by just
reading the documentation, but it becomes clearer if read alongside the generated source codes.

See an example of generated code in the below:

```java
public final class FooPb extends
        com.google.protobuf.GeneratedMessage implements
        // @@protoc_insertion_point(message_implements:proto.FooPb)
        FooPbOrBuilder {

  // @@protoc_insertion_point(class_scope:proto.FooPb)
  private static final com.sohoffice.security.authorization.io.AuthStatementPb DEFAULT_INSTANCE;

}

```

The `@@protoc_insertion_point(message_implements:proto.FooPb)` and `@@protoc_insertion_point(class_scope:proto.fooPb)`
are the plugin insertion points.

Examples to make use of these insertion points can be:

- Implements arbitrary interfaces
- Add methods to the class, such as record style methods or
  [return Optional](https://github.com/Fadelis/protoc-gen-java-optional/tree/master) for optional fields.

Theoretically, it is done by adding a plugin.
[jprotoc](https://github.com/salesforce/grpc-java-contrib/tree/master/jprotoc/jprotoc) project can be leveraged.

However, in the most simplest form, we can also just manipulate the generated source codes via string replace.
Below is the example to implement an interface for `FooPb` class.

```groovy
tasks.named('generateProto') {
    doLast {
        // copy the generated proto files to proto.tmp
        copy {
            from "${layout.buildDirectory.get()}/generated/source/proto/main/java"
            into "${layout.buildDirectory.get()}/generated/source/proto.tmp/main/java"
        }

        // Process the implement statement in generated proto file in proto.tmp folder
        copy {
            from "${layout.buildDirectory.get()}/generated/source/proto.tmp/main/java"
            into "${layout.buildDirectory.get()}/generated/source/proto/main/java"
            filter { line ->
                // Replace the protoc insertion point to implement AuthStatement interface
                line.replaceAll("\\/\\/ @@protoc_insertion_point\\(message_implements:proto.FooPb\\)",
                        "com.example.Foo,")
            }
        }
    }
}
```