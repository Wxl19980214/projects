# Intermediate ML project

This example shows how to build an ML pipeline with integration testing (using
the `on_finish` key). When the pipeline takes a lot of time to run end-to-end
it is a good idea to test with a sample, take a look at the `pipeline.yaml`,
`env.yaml` to see how this parametrization happens and how this affects the
`get` function defined in `tasks.py`.

## Setup

```bash tags=["bash"]
# same instructions as the other version
git clone https://github.com/ploomber/projects
cd ml-basic

conda env create --file environment.yml
conda activate ml-basic
```

## Execute the pipeline

```bash
ploomber build
```

## Integration testing with a sample

To see available parameters:

```bash
ploomber build --help
```

Run with a sample:

```bash
ploomber build --env__sample true 
```
