# Patient Management Service

This repo contains the REST contract(s) for interacting with the Patient Management Service.

# Tools

See [ronin-contract-rest-tooling](https://github.com/projectronin/ronin-gradle/blob/main/gradle-plugins/ronin-contract-openapi-plugin) for more information about the tooling and how it works.

# Usage

## Project setup

The project looks like this:

```
├── .github                                    GitHub workflows
├── .gitignore                                 Generally applicable ignores
├── build.gradle.kts                           Gradle build script
├── settings.gradle.kts                        Gradle build settings script
└── src/main/openapi
    └── patient-management-api.json
```

The plugin outputs five artifacts:
- a tar.gz file that contains the compiled schemas
- a .json copy of the schema
- a .yaml copy of the schema
- a .jar file with the compiled schema and compiled kotlin classes
- a -sources.jar file with the source files

### Dependencies

If you need to add a dependency within this contract to _other_ contracts, you can reference them as project dependencies.  Make entries in your build.gradle.kts file like this:

```kotlin
dependencies {
    openapi("<dependency group id>:<dependency artifact id>:<dependency version>")
}
```

When you run the commands described below, these dependencies will be downloaded and placed in the `src/main/openapi/.dependencies` directory .  If they
are archives, they'll be unzipped, and you can reference the dependent contracts, schemas, etc. directly by path from inside your OpenAPI schema.

## Running

In general, run:

`gradle <COMMAND> <ARGUMENTS>`

Available commands are mostly general gradle commands.  Important ones are:

`gradle check`: Verifies the contract using spectral tooling, making sure it's a valid contract.

`gradle downloadApiDependencies`: downloads any API dependencies as specified in the `Dependencies` section above.

`gradle build`: Runs `check`, generates schema documentation, and generates simple combined schema files.

`gradle clean`: removes all generated files

`gradle assemble`: Assembles the schema into deployable archives.

`gradle publishToMavenLocal`: publishes all outputs to the local maven repo (e.g. `$HOME/.m2/repository`).  If you are using the docker image, it will try and
copy files in and out of the host's repository directory so they can be used for builds later on the host.

`gradle publish`: publishes all outputs to the remote Ronin maven repository.
