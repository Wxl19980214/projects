# Ploomber sample projects

![](https://github.com/ploomber/projects/workflows/ci/badge.svg)
[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/ploomber/projects/master)

This repository contains sample pipelines developed using [Ploomber](github.com/ploomber/ploomber)

## Basic

1. `spec-api-directory/` Simple example showing how to build a pipeline out of a directory with scripts. This is the example used in the JupyterCon 2020 demo
2. `spec-api-python/` Python pipeline defined using a `pipeline.yaml` spec file
3. `spec-api-r/` R pipeline defined using a `pipeline.yaml` spec file

## Intermediate

4. `spec-api-sql/` Pipeline with SQL and Python tasks, using the Spec API
5. `python-api/` Pipeline using the Python API
6. `parametrized/` Pipeline with input parameters
7. `debugging/` Pipeline to demonstrate debugging capabilities
8. `testing/` Pipeline with SQL and Python tasks showing how to test pipelines
9. `ml-basic/` Simple Machine Learning pipeline using the both APIs (Python and Spec). It also shows how to test pipelines using `pytest` and how to package your pipeline so you can install it with `pip install`

## Advanced

10. `sql-templating` SQL pipeline showing how to use macros to write concise SQL scripts
11. `etl` Pipeline with a dozen tasks demonstrating how to extract data form a SQL database and then process it with Python