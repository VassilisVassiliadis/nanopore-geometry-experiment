# Nanopore Geometry Experiment

Automated virtual experiment that calculates geometric properties of nanoporous materials using the Zeo++ software package.

## Quick links

- [Launching the virtual experiment](#launching-the-virtual-experiment)
- [Using custom database of CIF files](#using-custom-database-of-cif-files)
- [Help and Support](#help-and-support)
- [Contributing](#contributing)
- [License](#license)

## Launching the virtual experiment

### On a local machine

## Instructions

If you have a container runtime such as docker, podman, rancher desktop, etc available on your system you can execute the experiment by:

1. creating a python virtual environment, activating it, and installing the python module `st4sd-runtime-core[develop]>=2.2.0`
2. cloning this repository
3. launching the experiment

For example:

```bash
#!/usr/bin/env sh

: # Download virtual experiment
git clone https://github.com/st4sd/nanopore-geometry-experiment.git

: # Setup ST4SD runtime-core
python3 -m venv --copies venv
. venv/bin/activate
python3 -m pip install "st4sd-runtime-core[develop]>=2.2.0"

: # Go inside the directory of this repository. 
: # The next command executes the experiment in there
cd nanopore-geometry-experiment

: # Run the experiment
elaunch.py -i docker-example/cif_files.dat \
      --applicationDependencySource="nanopore-database=cif:copy" \
      nanopore-geometry-experiment.package
```

**Note**: Make sure you run the `git clone` command in a directory that your container runtime (e.g.`docker`, `podman`, etc) can mount later when you execute the experiment.

**Note**: Using a container runtime is intended for small scale experiments and/or debugging your experiments. Use the `kubernetes` example below to launch larger experiments.

### On Kubernetes

Follow our end to end [example notebook](nanopore-geometry-experiment.ipynb) to launch our `nanopore-geometry-experiment` experiment on your ST4SD Cloud instance.


## Using custom database of CIF files

You may download the CIF files of your choosing to a PVC inside your OpenShift cluster (below we use the name `nanopore-database-pvc`), mount it as a volume and ask the virtual experiment instance to use the contents of the PVC as the contents of the `nanopore-database` application-dependency.

```Python
file_names = [""]
payload = {
  "volumes": [{
        "type": {"persistentVolumeClaim": "nanopore-database-pvc"},
        "applicationDependency": "nanopore-database"
    }],
  # other fields
}
rest_uid = api.api_experiment_start("nanopore-geometry-experiment", payload)
```

**Note**: The [example notebook](nanopore-geometry-experiment.ipynb) shows a full example.

## Help and Support

Please feel free to create an issue and alert the maintainers listed in the [MAINTAINERS.md](MAINTAINERS.md) page.

## Contributing

We always welcome external contributions. Please see our [guidance](CONTRIBUTING.md) for details on how to do so.

## License

This project is licensed under the Apache 2.0 license. Please [see details here](LICENSE.md).
