# Setup for maven projects

Maven projects are initially deployed to the staging area of the [OSS Repository](https://s01.oss.sonatype.org/) with the release process.
See also: [Publish Guide](https://central.sonatype.org/publish/publish-guide/)

From there, they can be transferred to the central maven repository and finally released. Here some rules are checked. To comply with all rules the `pom.xml` must contain the following:

`<url>`- A URL for the project must be specified. This can be the URL to the github project.

`<developers><developer>` - At least one development must be specified.

The artifacts must be signed.
Everything is already prepared for this in the release action and also the pgp keys are stored. Nevertheless the following must still be entered in the `pom.xml`:

```xml
<project ...>

    <properties>
        <gpg.skip>true</gpg.skip>
    <properties>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-gpg-plugin</artifactId>
                <version>3.0.1</version>
                <executions>
                    <execution>
                        <id>sign-artifacts</id>
                        <phase>verify</phase>
                        <goals>
                            <goal>sign</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        <plugins>
    </build>
</project>
```

Furthermore the following plugins should be configured as follows:

```xml
<project ...>
    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-source-plugin</artifactId>
                <version>3.2.0</version>
                <executions>
                    <execution>
                        <id>attach-sources</id>
                        <goals>
                            <goal>jar-no-fork</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-javadoc-plugin</artifactId>
                <version>3.4.0</version>
                <executions>
                    <execution>
                        <id>attach-javadocs</id>
                        <goals>
                            <goal>jar</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
            <plugin>
                <artifactId>maven-release-plugin</artifactId>
                <version>3.0.0</version>
                <configuration>
                    <scmCommentPrefix>ci(release): </scmCommentPrefix>
                    <tagNameFormat>@{project.version}</tagNameFormat>
                </configuration>
            </plugin>
        <plugins>
    </build>
</project>
```
