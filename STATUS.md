# Status

We are applying for the **Available**, **Functional**, and **Reusable** badges.

## Functional

The artifact includes:

- The complete Speculate tool source code
- 15 Java benchmark repositories from the Respector dataset
- 4 Django benchmark repositories (mathesar, education-backend, treeherder, librephotos)
- Pre-compiled class files for all Java benchmarks
- Pre-computed generation results.
- Evaluation reports (RQ1) with endpoint, parameter, and constraint metrics
- A Dockerized workflow that reproduces the OpenAPI spec generation described
  in the paper for both Java and Django benchmarks

The reviewer can:

1. Pull the published Docker image and run the tool on any of the 15 Java or 4 Django
   bundled benchmarks without manual credential setup
2. Inspect pre-computed results and evaluation reports in `results/`
3. Compare fresh outputs against pre-computed results
4. Run the tool with different LLM models via `--spec-model`

## Reusable

Speculate is designed for reuse beyond the paper's benchmarks:

**Run on any Java or Python web project:**

- Any Spring Boot or Jersey repository can be analyzed by pulling the
  lightweight `krishannu/speculate-artifact:tool` image (~840 MB) and
  mounting the pre-compiled project.
- Any Django REST Framework project can be analyzed by installing the
  tool's dependencies into the project's existing virtualenv and running
  `gen_apidocs2.py` directly. A `--django-use-static-endpoints` fallback
  mode handles projects that cannot boot cleanly.

**Language-agnostic core with reusable language analyzers:**

Speculate's architecture separates a language-agnostic core (LLM
integration, prompt orchestration, OpenAPI validation) from language- and
framework-specific code analyzers. We have fully implemented analyzers for
two languages — Python and Java — covering a large fraction of real-world
REST API codebases. Because the language analyzer (AST parsing, symbol
resolution, class hierarchy traversal) is already built and battle-tested,
adding support for a new framework within these languages — such as FastAPI
or Flask for Python, or Spring WebFlux or Micronaut for Java — requires
only a thin framework-specific layer on top of the existing analyzer,
reusing the bulk of the existing code. The community norms and patterns
established by the Django, Spring Boot, and Jersey analyzers provide a
clear template for such extensions.

**Configurable:**

- Switch between LLM providers (Azure OpenAI, Gemini, DeepSeek) via a
  single `.env` file; the `.env.example` template documents all variables
- Control model selection (`--spec-model`, `--context-model`), concurrency,
  batch size, retry behavior, and generation stages via command-line flags

**Well-structured source:**

- ~12k lines of Python, split into a language-agnostic core and
  framework-specific modules
- Full source is included and inspectable; Docker image exposes it at
  `/artifact/tool/`
- Builds natively on `linux/amd64` and `linux/arm64`

## Available

The artifact is publicly accessible on archival and container registries:

- **Zenodo**: https://doi.org/10.5281/zenodo.19185245
- **Docker Hub**: `krishannu/speculate-artifact:latest`
- **GitHub**: https://github.com/practical-systems-iitd/speculate-artifact
