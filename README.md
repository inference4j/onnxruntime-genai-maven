# ONNX Runtime GenAI → Maven Central

Republishes [Microsoft's official GenAI Java release JAR](https://github.com/microsoft/onnxruntime-genai/releases)
under `io.github.inference4j:onnxruntime-genai`, byte-for-byte unchanged.
This is a standalone publisher, not an upstream fork. It compiles no Java or native binaries.

```groovy
implementation 'io.github.inference4j:onnxruntime-genai:0.17.0'
```

## Publish a version

1. Open **Actions → Publish to Maven Central → Run workflow**.
2. Enter the upstream GenAI version without `v`, plus its matching ONNX Runtime dependency version.
   For GenAI **0.17.0**, use ONNX Runtime **1.28.0**. For later releases, check
   `ort_runtime_version` in the matching upstream tag's `.pipelines/packaging.yml`.
3. Run the workflow and check the deployment ID in its summary on
   [Sonatype Central Portal](https://central.sonatype.com/publishing/deployments).

The workflow downloads and checksums the release JAR, fetches the matching Java
sources, generates sources/Javadoc JARs, signs all artifacts, and submits them for
automatic publication. A successful workflow means Sonatype accepted the upload;
publication follows after Sonatype validation. Check an existing deployment before
retrying: published Maven Central versions cannot be overwritten.

Nothing runs on pushes or pull requests. There are no native builds, submodules,
release tags to synchronize, or upstream sources stored in this repository.

## Configuration

The repository needs access to four existing organization Actions secrets:

- `GPG_SIGNING_KEY` — ASCII-armored private key registered for Maven Central signing
- `GPG_SIGNING_PASSPHRASE`
- `MAVEN_CENTRAL_USERNAME` — Central Portal token username
- `MAVEN_CENTRAL_PASSWORD` — Central Portal token password

`pom.xml` is a publication metadata template, not a Maven build. The workflow fills
in its version tokens. The published POM adds the ONNX Runtime dependency because
the upstream JAR bundles GenAI natives but does not bundle ONNX Runtime itself.
The sources and Javadoc are generated only to satisfy Maven Central requirements.

The Java package stays `ai.onnxruntime.genai`; upstream's embedded metadata and
native signatures are preserved. The redistributed code is MIT-licensed, and
its license is included in the sources/Javadoc artifacts.

These community coordinates can be retired when Microsoft publishes the Java
artifact directly to Maven Central.
