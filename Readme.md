# Control a local podman/docker/OCI registery

We run a very simple **insecure** OCI registry using the docker registry 
version 2 image. Since we run this container using **rootless** podman, the 
registry container itself has no more privileges than the user who runs 
it. 

The following shell scripts can be used to control the registry:

- **`runRegistry`** runs the registry in the current terminal (so you can 
  see all activity logged to the terminal AND you can stop the registry by 
  typing ^C).

- **`removeRegistry`** deletes the podman-registry container created (but 
  not deleted by the runRegistry command).

- **`clearRegistry`** clears out ALL images contained in the current file 
  system registry (using our example `registryConfig` (see below), this is 
  located in $HOME/.local/share/containers/registry). 

- **`listContainers`** uses `podman container ls --all` to list all known 
  containers. 

- **`listRegistry`** uses `podman search` to list all images in the current 
  podman-registry. 

The `registryConfig` script, which is sourced by all of the above scripts, 
contains the following configuration parameters: 

- `$REGISTRY_DIR` This is the (local) file system directory used to store 
  the uploaded images. 

- `$REGISTRY_HOST` This is the DNS name of the host which runs this 
  registry. It must be configured in your (local) DNS service *or*, 
  alternatively in the /etc/hosts files on each machine which will access 
  this registry. 

- `$REGISTRY_PORT` This is the port on which the registry will listen. The 
  firewall on the machine which runs this registry *must* allow inbound 
  traffic on this port. 

The `registryConfigExample` script provides an example. Rename it to 
`registryConfig` and update the values to suit your needs.

## Usage

You can use the standard `podman push`, `podman pull` and `podman search` 
tools to push, pull and search this podman-registry. You may need to append 
the switch `--tls-verify=false` to allow `podman` to use `http` instead of 
(the default) `https` transport. 

Using our example `registryConfig`, the registry prefix is: 
`podman-registry:5000`. This will need to be appended to `podman tag` as 
well as any image names you might want to push or pull from this local 
registry. 

## Setup

Using our example `registryConfig`, to be accessible from all machines on 
the LAN, you need to ensure the ufw firewall allows traffic to/from tcp 
port 5000. To do this type: 

```
  sudo ufw allow 5000
```

## Resources

See:

- [Docker Registry](https://docs.docker.com/registry/)

- [About Registry](https://docs.docker.com/registry/introduction/)

- [Deploy a registry server](https://docs.docker.com/registry/deploying/)

- [Configuring a registry](https://docs.docker.com/registry/configuration/)

- [RedHat: How to implement a simple personal/private Linux container 
   image registry for internal 
   use](https://www.redhat.com/sysadmin/simple-container-registry) 
