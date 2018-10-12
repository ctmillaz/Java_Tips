# Java_Tips

# Unit Testing

### Use spring-boot-start-test (and these dependencies come with it):
#### JUnit - De-facto standard for unit testing java applications
#### Spring Test and Spring Boot Test - Utilities and integration test support for Spring Boot applications
#### AssertJ - A fluent assertion library
#### Hamcrest- A library of matcher objects
#### Mockito - A java mocking framework
#### JSONassert - An assertion library for JSON
#### JSONPath - XPath for JSON


---------------------------------------------------------------------------------------------------------------------------------

# Errors

## Error Type: Index file smaller than expected
### Problem
### fatal: index file smaller than expected
### index file has become corrupted, and you need to recreate.

### Solution
`rm .git/index`

`git add .`



## Error Type: No attribute found error

### Solution


#### First, it's kind of weird, to see you run java -jar "app" and not java -jar app.jar

#### Second, to make a jar executable... you need to jar a file called META-INF/MANIFEST.MF

#### the file itself should have (at least) this one liner:
`
Main-Class: com.mypackage.MyClass
`
#### Where com.mypackage.MyClass is the class holding the public static void main(String[] args) entry point.

#### Note that there are several ways to get this done either with the CLI, Maven, Ant or Gradle:

#### For CLI, the following command will do: (tks @dvvrt) 
`
jar cmvf META-INF/MANIFEST.MF <new-jar-filename>.jar  <files to include>
`

#### For Maven, something like the following snippet should do the trick. Note that this is only the plugin definition, not the full pom.xml:
`
<build>
  <plugins>
    <plugin>
      <!-- Build an executable JAR -->
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-jar-plugin</artifactId>
      <version>3.1.0</version>
      <configuration>
        <archive>
          <manifest>
            <addClasspath>true</addClasspath>
            <classpathPrefix>lib/</classpathPrefix>
            <mainClass>com.mypackage.MyClass</mainClass>
          </manifest>
        </archive>
      </configuration>
    </plugin>
  </plugins>
</build>
`
#### (Pick a <version> appropriate to your project.)

#### For Gradle:
`
plugins {
    id 'java'
}

jar {
    manifest {
        attributes(
                'Main-Class': 'com.mypackage.MyClass'
        )
    }
}
`
