---
title: "Java dependency management"
type: document
tags: ["#convention", "#java", "#deps"]
status: published
version: 1
---

# Java dependency management

## Tooling

- **Maven** or **Gradle** — pick one per repo, never mix.
- For new projects: Gradle with Kotlin DSL (`build.gradle.kts`). Better IDE support, less XML pain.
- Lock file: Gradle's `gradle.lockfile` or Maven enforcer plugin. Committed.
- Pin the build tool version via `gradle/wrapper/gradle-wrapper.properties` or `.mvn/wrapper/maven-wrapper.properties`.

## Pinning policy

- Top-level versions explicit. No `LATEST` or `RELEASE` metaversions.
- Dependency BOMs (Spring, Quarkus, Micronaut) manage transitive versions — use them.
- Snapshot dependencies (`*-SNAPSHOT`) only in development. Never released as a snapshot dep.

## Adding a dependency

1. Check existing `dependencies {}` — same library under different artifact IDs is common (e.g., several JSON libraries).
2. License check: Apache 2.0 / MIT / BSD / EPL OK; LGPL / GPL flagged for legal review.
3. Bundle size matters less than in JS but matters: a JAR pulling in 50 transitives costs cold-start time.
4. `./gradlew dependencies` or `mvn dependency:tree` to see what came along.
5. Commit `build.gradle.kts` / `pom.xml` + the lockfile.

## BOMs are your friend

For Spring / Spring Boot / Quarkus / Micronaut projects, the BOM declares compatible versions of dozens of libraries. Stop pinning individual versions; declare the BOM and use coordinates without versions.

```kotlin
implementation(platform("org.springframework.boot:spring-boot-dependencies:3.3.0"))
implementation("org.springframework.boot:spring-boot-starter-web")
```

## Vulnerability response

- **OWASP Dependency-Check** or **Snyk** in CI. Critical/high CVEs block PRs.
- See [[../../security/dep-vuln-policy]] for SLAs by severity.
- For known-but-acceptable risks, document in a suppression file; review quarterly.

## Antipatterns

- Pulling in Apache Commons "for one util method." Standard library or write the helper.
- Mixing Maven and Gradle outputs in the same repo.
- Transitive version overrides without recording why (`<dependencyManagement>` entries with no comment).
- Disabling vulnerability scanning to ship — never. Suppress with a documented reason or fix.

## See also

[[style-guide]] · [[../../security/dep-vuln-policy]]
