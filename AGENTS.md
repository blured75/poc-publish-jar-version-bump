# Repository Instructions

## Project Shape
- This is a single-module Maven Java CLI project, not a multi-module workspace.
- The application entrypoint is `src/main/java/com/example/App.java` (`com.example.App`).
- `pom.xml` sets `maven.compiler.release` to `25`; use JDK 25 for local builds to match CI.
- The code uses Java 25 preview-style `java.lang.IO.println` and an instance-free `static void main()` entrypoint, so avoid rewriting it to older Java conventions unless the target JDK changes.

## Commands
- Build/package exactly as CI does: `mvn -B -ntp clean package`.
- There is no Maven wrapper in this repo; commands assume `mvn` is installed.
- There are currently no `src/test` sources or test framework dependencies; `mvn -B -ntp test` only verifies compilation/test phase wiring.
- Run the packaged CLI after a build with `java -jar target/hello-cli-*.jar`.

## Release/CI Notes
- GitHub Actions uses Temurin JDK 25 and only builds on pushes/PRs to `main`.
- Manual `workflow_dispatch` releases derive the release version from `project.version` with `-SNAPSHOT` stripped unless an input `release_version` is supplied.
- The release workflow runs `versions:set`, builds, commits `pom.xml`, tags `v<release_version>`, publishes `target/*.jar`, then bumps to the next patch `-SNAPSHOT`.
- `RELEASE_NOTES.md` is used as the GitHub Release body.

## Generated Files
- `target/` is build output and ignored; do not edit or commit it.
