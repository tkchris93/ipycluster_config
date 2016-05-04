# ipycluster_config
The required files to configure the IPython Cluster in the Nullspace Lab

### Enabling the Clusters tab
`ipcluster nbextension enable`

### Setting up MPI profile
1. Have MPI installed
- Configured to use mpiexec
- Create profile: `ipython profile create --parallel --profile=mpi`
- Edit ~/.ipython/profile_mpi/ipcluster_config.py
- Add `c.IPClusterEngines.engine_launcher_class = 'MPIEngineSetLauncher'` to the MPILauncher section of the config.


`
