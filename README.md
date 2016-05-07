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

### Verifying that engines are on multiple computers
```
import socket
import ipyparallel as ipp
rc = ipp.Client()
rc[:].apply_sync(socket.gethostname)
```
`
### Basics of `ipyparallel`
##### Call a function across all engines in a view. 
In the example below, `view` is a View containing all engines and `one` is a View containing just engine 1.
```
import ipyparallel as ipp
rc = ipp.Client()
view = rc[:]
one = rc[1]

def foo():
    return 1
    
a = view.apply(foo)
print a.get()

b = one.apply(foo)
print b.get()
```
##### Send a time-consuming process to one engine
```
def wait(n):
    import time
    time.sleep(n)
    return n
    
a = view.apply_async(wait, 5)

# The following will block execution till it's done
a.get()

# To check to see if it's done executing (non-blocking)
a.ready()

# Try to store result, but only block execution for n seconds
a.get(n)
```

### When connecting
To avoid updating the connection info all the time, start the `ipcontroller` with:
```
ipcontroller --reuse --profile=distributed --ip=<ip address>
```

### Start multiple engines
```
ipcluster engines --n X --profile=distributed
```

