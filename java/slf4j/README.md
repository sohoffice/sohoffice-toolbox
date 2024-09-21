## Dependencies (using logback)

- org.slf4j:slf4j-api:2.0.16
- ch.qos.logback:logback-classic:1.5.8

## Sample slf4j configuration file

```xml
<configuration>

    <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
        <layout class="ch.qos.logback.classic.PatternLayout">
            <Pattern>
                %d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n
            </Pattern>
        </layout>
    </appender>

    <logger name="com.example" level="debug" additivity="false">
        <appender-ref ref="CONSOLE"/>
    </logger>

    <logger name="ch.qos.logback" level="error">
        <appender-ref ref="CONSOLE"/>
    ß</logger>

    <root level="info">
        <appender-ref ref="CONSOLE"/>
    </root>

</configuration>
```