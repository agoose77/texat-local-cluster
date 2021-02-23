# TexAT Local Cluster
This repository contains the necessary scripts to launch a dask cluster using local shared storage.

## Usage
1. Ensure that this directory is accessible on all necessary hosts
   - Use rsync or a shared file system.
2. Launch the scheduler on a machine using `launch-scheduler`
3. Launch the worker(s) on each machine using `launch-scheduler`
   - See script for environment variable arguments.

## Requirements
* `texat-runtime.sif` on `PATH`
* `texat-sim-data` in `share/`

## Note
This repository uses direnv to set the PATH variable appropriately. If direnv is not available, use `export PATH="$PWD/bin:$PWD/scripts:$PATH"`.

### Architecture
In the `bin` directory, `python3` and `python` point to the `texat` shell script. This launches `texat-runtime.sif`, which is a Singularity image built from the texat Dockerfile `runtime` target. 

Singularity is used because it simplifies the commandline (shorter binding), runs as user (avoids UID mapping), is easily launched without importing, and is a requirement to launch on the SLURM cluster (therefore we use it here too).

The `shared` and `backend` directories are placeholders for the small and large file storage respectively. It is unlikely that these will be individually mapped instead of this entire directory being added to shared storage.

### SSHFS
 For local clusters, where bandwidth is large, we might disable compression:
```bash
sshfs -o cache=yes -o compression=no -o reconnect,kernel_cache,large_read,allow_other,default_permissions,follow_symlinks <HOST>:/var/lib/cluster /mnt/cluster
```
