```python
from ploomberutils import display_file
```

# Logging

This guide will show you how to log pipeline execution.

The *Summary* section provides a quick reference for configuring logging. If you want the details, continue reading.

<!-- #region -->
## Summary

### Function tasks

If you're using functions as tasks, configure logging like this:

```python
import logging


def some_task(product):
    # uncomment the next line if using Python >= 3.8 on macOS
    # logging.basicConfig(level=logging.INFO)

    logger = logging.getLogger(__name__)

    # to log a message, call logger.info
    logger.info('Some message')
```

### Scripts or notebooks

If using scripts/notebooks tasks, add this a the top of **each** one:

```python
import sys
import logging

logging.basicConfig(level=logging.INFO, stream=sys.stdout)
logger = logging.getLogger(__name__)

# to log a message, call logger.info
logger.info('Some message')
```

**and** add the following to **each** task definition:

```yaml
tasks:
  - source: scripts/script.py
    product: products/output.ipynb
    # add this
    papermill_params:
      log_output: True
```

Then, use the `--log` option when building the pipeline:

```sh
ploomber build --log info
```
<!-- #endregion -->

## Sample pipeline

The pipeline we'll be using for this guide contains two tasks (a script and a function):

```python
display_file('basic/pipeline.yaml')
```

Note that the script task contains:

```yaml
papermill_params:
    log_output: True
```

This extra configuration is required on each script/notebook task in your pipeline to enable logging. The code on each task isn't important, they contain a for loop and log a message on each iteration. Let's see it in action:

```sh
cd basic
ploomber build --log info --force
```

We can see that the logging statements appear in the console. If you want to take a look at the code, click here.


## Why not print?

Note that the snippets above use the `logging` module instead of `print`. Although `print` is a quick and easy way to display messages in the console, the `logging` module is more flexible. Hence, it is the recommended option.

<!-- #region -->
## Logging to a file (On Linux and macOS)

It's common to send all your log records to a file. You can easily do so on Linux and macOS with the following command:

```sh
ploomber build --log info > my.log 2>&1
```
<!-- #endregion -->

## Logging to a file from Python (Linux, macOS, and Windows)

Alternatively, you can configure logging from Python, which gives you more flexibility:

```python
# you may store the contents of this cell in a .py file and then call it from the command line
# e.g., python run_pipeline.py
import logging
from pathlib import Path

from ploomber.spec import DAGSpec

logging.basicConfig(filename='my.log', format='%(levelname)s:%(message)s', level=logging.INFO)

dag = DAGSpec('basic/pipeline.yaml').to_dag()
dag.build(force=True)
```

Let's look at the file contents:

```python
print(Path('my.log').read_text())
```

## Controlling logging level

The Python's [logging](https://docs.python.org/3/library/logging.html) module allows you to filter messages depending on their priority. For example, when running your pipeline, you may only want to display *regular* messages, but you may allow *regular* and *debugging* messages for more granularity when debugging. Since Ploomber runs tasks differently depending on their type (i.e., functions vs. scripts/notebooks), controlling the logging level requires a bit of extra work. Let's use the same pipeline in the `parametrized` directory:

```sh
cd parametrized
ploomber build --log info --env--logging_level info --force
```

Let's now run the pipeline but switch the logging level to debug, this will print the records we saw above, plus the ones with `debug` level:

```sh
cd parametrized
ploomber build --log debug --env--logging_level debug --force
```

<!-- #region -->
## Implementation details

To keep the tutorial short, we overlooked some technical details. However, if you want to customize logging, they are important to know.

### Function tasks and sub-processes

By default, Ploomber runs function tasks in a child process. However, beginning on version 3.8, [Python 3.8 switched to use spawn instead of fork on macOS](https://docs.python.org/3/library/multiprocessing.html#contexts-and-start-methods), this implies that child processes *do not* inherit the logging configuration of their parents. That's why you must configure a logger inside the function's body:

```python
import logging


def some_task(product):
    # the following line is required on Python>=3.8 if using macOS
    logging.basicConfig(level=logging.INFO)

    logger = logging.getLogger(__name__)

    # to log a message, call logger.info
    logger.info('Some message')
```

### Scripts and notebooks

Unlike function tasks, which can run in the same process that runs Ploomber, or in a child process, scripts and notebooks execute independently. Hence, any logging configuration made in the main process is lost, and We have to configure a separate logger at the top of the script/notebook.

### Parallel execution


Logging is currently unavailable when using the `Parallel` executor.

<!-- #endregion -->