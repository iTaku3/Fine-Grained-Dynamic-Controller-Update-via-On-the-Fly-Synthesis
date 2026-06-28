# Fine-Grained Dynamic Controller Update via On-the-Fly Synthesis

This repository contains a research artifact for evaluating fine-grained dynamic
controller update via on-the-fly synthesis. The artifact is based on MTSA/LTSA
and includes benchmark models, experiment configurations, experiment outputs,
and the source code needed to rebuild the executable tool.

## What The Tool Does

The tool supports synthesis and analysis of controller-update scenarios for
finite-state models written in LTSA/FSP-style `.lts` files. It is used to compare
traditional monolithic controller synthesis with on-the-fly update synthesis,
including variants with and without the abstraction option used in the
experiment files.

The main executable is an MTSA-based Java GUI application. It can load `.lts`
models, compile them, and run controller-synthesis/update workflows through the
MTSA interface.

## Repository Layout

```text
.
├── Experiment/
│   ├── Models/
│   ├── Experiment/
│   └── mtsa.jar
└── Source Code/
    └── maven-root/
        ├── mtsa/
        ├── lib/
        └── locallib/
```

## Experiment Directory

`Experiment/` contains the benchmark inputs and recorded experiment data.

- `Experiment/Models/`  
  Top-level benchmark `.lts` models used in the evaluation, such as GSM,
  Industry, MetaSocket, PowerPlant, ProductionCell, Railcab, Surveillance, and
  Workflow.

- `Experiment/Experiment/<date>/configs/`  
  YAML configuration files for dated experiment runs. These files define the
  benchmark model, synthesis/update variant, JVM options, and output directory
  for each run.

- `Experiment/Experiment/<date>/models/`  
  Copies of the benchmark models used for that dated run.

- `Experiment/Experiment/<date>/result/` and related result directories  
  Output files from the experiment runs, including metadata, stdout/stderr logs,
  synthesized output, and transition summaries.

- `Experiment/Experiment/result/`  
  Aggregated or post-processed result tables. Some large CSV files are excluded
  from normal Git tracking and should be distributed through GitHub Releases.

- `Experiment/mtsa.jar`  
  A prebuilt executable jar used for running the artifact. Because this file is
  larger than GitHub's normal file-size limit, it should be distributed as a
  GitHub Release asset.

## Source Code Directory

`Source Code/` contains the Java/Maven project used to build the tool.

- `Source Code/maven-root/mtsa/`  
  Main Maven project. The Maven descriptor is:

  ```text
  Source Code/maven-root/mtsa/pom.xml
  ```

- `Source Code/maven-root/mtsa/src/main/java/`  
  Java source code for MTSA, LTSA UI components, controller synthesis, dynamic
  controller update, enactment, and related utilities.

- `Source Code/maven-root/mtsa/src/main/resources/`  
  Runtime resources used by the GUI, including icons, documentation assets,
  logging configuration, and `ltsa-context.xml`.

- `Source Code/maven-root/mtsa/src/test/` and `src/test/resources/`  
  Test code and additional example models/resources.

- `Source Code/maven-root/mtsa/lib/`  
  Local Maven-style dependencies required by this historical MTSA codebase.
  Some large local dependencies are tracked with Git LFS.

- `Source Code/maven-root/locallib/`  
  Additional local dependency jars used by the project.

- `Source Code/maven-root/mtsa/target/`  
  Maven build output. This directory is ignored by Git and should be regenerated
  locally.

## Requirements

- Java JDK 10 or newer
- Apache Maven
- Git LFS, if cloning the repository and building from source

After cloning, fetch the LFS-managed local libraries:

```bash
git lfs install
git lfs pull
```

## Running The Prebuilt Tool

If you downloaded `mtsa.jar` from the repository's GitHub Releases page, run:

```bash
java -jar mtsa.jar
```

If you are using the local artifact layout:

```bash
java -jar "Experiment/mtsa.jar"
```

The command opens the MTSA GUI. A typical workflow is:

1. Open a benchmark model from `Experiment/Models/` or from a dated
   `Experiment/Experiment/<date>/models/` directory.
2. Parse/compile the model in the MTSA GUI.
3. Run the relevant synthesis or update workflow from the GUI menus.
4. Compare the generated output with the files under the corresponding
   `Experiment/Experiment/<date>/result/` directory.

## Building From Source

Build the Maven project from the `mtsa` directory:

```bash
cd "Source Code/maven-root/mtsa"
mvn install -DskipTests=true
```

The executable shaded jar is produced at:

```text
Source Code/maven-root/mtsa/target/mtsa-1.0-SNAPSHOT.jar
```

Run the rebuilt tool with:

```bash
java -jar "Source Code/maven-root/mtsa/target/mtsa-1.0-SNAPSHOT.jar"
```

The build has been checked with a modern JDK. The Maven configuration targets
Java 10 because the source code uses Java 10 language features.

## Large Files

GitHub rejects normal Git files larger than 100 MB. For that reason:

- local dependency jars required for building are tracked with Git LFS;
- generated executable jars and very large experiment CSV files should be
  attached to GitHub Releases;
- `target/` build outputs are ignored and should be regenerated with Maven.

Recommended release assets include:

```text
Source Code/maven-root/mtsa/target/mtsa-1.0-SNAPSHOT.jar
Experiment/mtsa.jar
Experiment/Experiment/result/20260626/All/extracted_rows_full.csv
Experiment/Experiment/result/20260626/All/runs_compact.csv
```

