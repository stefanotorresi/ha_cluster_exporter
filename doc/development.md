# Developer notes

1. [Makefile](#makefile)
2. [OBS packaging](#obs-packaging)


## Makefile

Most development tasks can be accomplished via [make](../Makefile).

For starters, you can run the default target with just `make`.

The default target will clean, analyse, test and build the amd64 binary into the `build/bin` directory.

You can also cross-compile to the various architectures we support with `make build-all`.


## OBS Packaging

The CI will automatically publish GitHub releases to SUSE's Open Build Service: to perform a new release, just publish a new GH release. Tags must always follow the [SemVer](https://semver.org/) scheme.

If you wish to produce an OBS working directory locally, having configured [`osc`](https://en.opensuse.org/openSUSE:OSC) already, you can run:
```
make exporter-obs-workdir
```
This will checkout the OBS project and prepare a new OBS commit in the `build/obs` directory.

Note that, by default, the current Git working directory HEAD reference is used to download the sources from the remote, so this reference must have been pushed beforehand.
  
You can use the `OSB_PROJECT`, `REPOSITORY` and `REVISION` environment variables to change the behaviour of OBS-related make targets.

For example, if you were on a feature branch of your own fork, you may want to change these variables, so:
```bash
git push feature/yxz # don't forget to make changes remotely available
export OBS_PROJECT=home:JohnDoe
export REPOSITORY=johndoe/my_forked_repo
export REVISION=feature/yxz
make exporter-obs-workdir
``` 
will prepare to commit on OBS into `home:JohnDoe/prometheus-ha_cluster_exporter` by checking out the branch `feature/yxz` from `github.com/johndoe/my_forked_repo`.

At last, to actually perform the commit into OBS, run `make exporter-obs-commit`. 
