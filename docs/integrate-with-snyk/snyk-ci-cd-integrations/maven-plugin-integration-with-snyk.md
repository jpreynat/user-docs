# Maven plugin integration with Snyk

Snyk offers a [Maven plugin](https://github.com/snyk/snyk-maven-plugin) based on the [Snyk CLI](https://docs.snyk.io/snyk-cli/cli-reference). This plugin allows you to scan and monitor your Maven dependencies for vulnerabilities.

See all releases on the [Maven Central Repository](https://search.maven.org/artifact/io.snyk/snyk-maven-plugin),

## Installation of Maven plugin

1. Get your [Snyk API token](https://docs.snyk.io/snyk-api-info/authentication-for-api).
2. Add the Snyk Maven Plugin to your `pom.xml` and configure it as needed.

```xml
<!-- Example Plugin Configuration -->
<build>
  <plugins>
    <plugin>
      <groupId>io.snyk</groupId>
      <artifactId>snyk-maven-plugin</artifactId>
      <version>2.2.0</version>
      <inherited>false</inherited>
      <executions>
        <execution>
          <id>snyk-test</id>
          <goals>
            <goal>test</goal>
          </goals>
        </execution>
        <execution>
          <id>snyk-monitor</id>
          <goals>
            <goal>monitor</goal>
          </goals>
        </execution>
      </executions>
      <configuration>
        <apiToken>${env.SNYK_TOKEN}</apiToken>
        <args>
          <arg>--all-projects</arg>
        </args>
      </configuration>
    </plugin>
  </plugins>
</build>
```

## Supported versions

* Java 8 and above.
* Maven 3.2.5 and above.

## Goals

### `code-test` (experimental)

Default phase: `test`

Performs a static-analysis of your project's source code and provides a list of vulnerabilities if any are found.

### `container-test` (experimental)

Default phase: `install`

Performs analysis of the layers of a container image. The tag of the image to be scanned should be provided as an argument:

```xml
<!-- Example of specifying the tag of the image to scan -->
<configuration>
  <args>
    <arg>--print-deps</arg>
    <arg>nginx:1.9.5</arg>
  </args>
</configuration>
```

### `test`

Default Phase: `test`

Scans your Project's dependencies and provides a list of vulnerabilities if any are found.

### `monitor`

Default Phase: `install`

Takes a snapshot of your Project's dependency tree and monitors it on [snyk.io](https://snyk.io). You'll be alerted when new relevant vulnerabilities, updates, or patches are disclosed.

## Configuration for Maven plugin

You can configure the following parameters inside the `<configuration>` section. All parameters are optional.

### `apiToken` \[string]

**Do NOT** include your API token directly in your `pom.xml`. Use a variable instead.

You must provide a Snyk API token to access Snyk's services. You can do so by:

* Providing `apiToken` in your configuration using a variable.
* Providing a `SNYK_TOKEN` environment variable.
* Authenticating via `snyk auth` using the Snyk CLI before using this plugin.

### `skip` \[boolean]

Default: `false`

Skip this execution entirely.

When running `mvn`, you can also use `-Dsnyk.skip` to enable this behavior.

### `failOnIssues` \[boolean]

Default: `true`

When set to `true`, should the Snyk CLI tool indicate that action is required to remedy a security issue, the Maven build will be considered failed. When set to `false` the build will continue even if action is required.

### `args` \[array\<string>]

This plugin uses [Snyk CLI](https://github.com/snyk/snyk) so you can pass any supported arguments using `<args>`. See the example below.

For a list of supported CLI options, see [Snyk CLI commands and options summary](https://docs.snyk.io/snyk-cli/cli-reference).

```xml
<!-- Example Arguments Configuration -->
<configuration>
  <args>
    <arg>--severity-threshold=high</arg>
    <arg>--scan-all-unmanaged</arg>
    <arg>--json</arg>
  </args>
</configuration>
```

### `cli` \[object]

Lets you configure the Snyk CLI used by this plugin.

By default, the CLI is automatically downloaded and updated for you.

See the CLI configuration section that follows.

## CLI configuration

For most use cases you don't need to set any `<cli>` options.

You can configure the CLI in three different modes:

* Auto-Download and Update (default)
* Custom CLI Executable
* Specific CLI Version

Follow the link for each mode to see which parameters are available.

```xml
<!-- Example CLI Configuration -->
<configuration>
  <cli>
    <updatePolicy>daily</updatePolicy>
  </cli>
</configuration>
```

### Auto-Download and Update

#### **`updatePolicy` \[string]**

Default: `daily`

How often to download the latest CLI release. Snyk recommends always keeping your CLI installation updated to the latest version. Can be one of the following:

* `daily` - On the first execution of the day
* `always` - On every execution
* `never` - Never update after the initial download
* `interval:<minutes>` - On the execution after more than `<minutes>` has passed since the last update. For example, `interval:60` will update after an hour

#### **`downloadDestination` \[string]**

Default: OS-specific

Where to place the downloaded executable. By default, this is OS-specific as follows:

* Linux - `$XDG_DATA_HOME/snyk/snyk-linux` or `~/.local/share/snyk/snyk-linux`
* macOS - `~/Library/Application Support/Snyk/snyk-macos`
* Windows - `%APPDATA%\Snyk\snyk-win.exe`

### Custom CLI Executable

#### **`executable` \[string]**

Example: `~/.local/share/snyk/snyk-linux`

Path to a pre-installed Snyk CLI executable. You can find executables on the [Snyk CLI Releases page](https://github.com/snyk/snyk/releases).

### Specific CLI Version

#### **`version` \[string]**

Example: `1.542.0`

Specify if you want to use a specific version. You can find versions on the [Snyk CLI Releases page](https://github.com/snyk/snyk/releases).

Setting this option triggers a download of the CLI on every execution.

## Demonstration

To try out this plugin, see [the demo project](https://github.com/snyk/demo-snyk-maven-plugin).

## Migrating from Snyk Maven Plugin v1 to v2

All plugin parameters from v1 should be moved to the `<args>` object, to keep them in line with the CLI usage. For example:

* `org` => `<arg>--org=my-org-name</arg>`
* `failOnSeverity` => `<arg>--severity-threshold=low|medium|high</arg>`
* `failOnAuthError` => Use `<skip>true</skip>` to skip plugin execution.
* `includeProvidedDependencies` => `provided` dependencies are always included.

For a list of supported arguments, see Configuration.

***
