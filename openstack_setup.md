# Openstack Setup

download openrc.sh from openstack web console and source it

install openstack client

```bash
pip install python-openstackclient
```

## Generate private keys

```bash
ssh-keygen -t rsa -b 4096 -C "" -f ~/.ssh/id_rsa_jupyter
```

## determine specs

things we will need (`openstack help server create`)

```txt

usage: openstack server create [-h] [-f {json,shell,table,value,yaml}] [-c COLUMN] [--noindent] [--prefix PREFIX] [--max-width <integer>] [--fit-width]
                               [--print-empty] --flavor <flavor>
                               (--image <image> | --image-property <key=value> | --volume <volume> | --snapshot <snapshot>)
                               [--boot-from-volume <volume-size>] [--block-device-mapping <dev-name=mapping>] [--block-device ] [--swap <swap>]
                               [--ephemeral <size=size[,format=format]>] [--network <network>] [--port <port>]
                               [--nic <net-id=net-uuid,port-id=port-uuid,v4-fixed-ip=ip-addr,v6-fixed-ip=ip-addr,tag=tag,auto,none>] [--password <password>]
                               [--security-group <security-group>] [--key-name <key-name>] [--property <key=value>] [--file <dest-filename=source-filename>]
                               [--user-data <user-data>] [--description <description>] [--availability-zone <zone-name>] [--host <host>]
                               [--hypervisor-hostname <hypervisor-hostname>] [--hint <key=value>]
                               [--use-config-drive | --no-config-drive | --config-drive <config-drive-volume>|True] [--min <count>] [--max <count>] [--tag <tag>]
                               [--wait]

...

  --flavor <flavor>
                        Create server with this flavor (name or ID)
  --image <image>
                        Create server boot disk from this image (name or ID)
  --image-property <key=value>
                        Create server using the image that matches the specified property. Property must match exactly one property.
  --volume <volume>
                        Create server using this volume as the boot disk (name or ID)
                        This option automatically creates a block device mapping with a boot index of 0. On many hypervisors (libvirt/kvm for example) this will be device vda. Do not create a
                        duplicate mapping using --block-device-mapping for this volume.
  --snapshot <snapshot>
                        Create server using this snapshot as the boot disk (name or ID)
                        This option automatically creates a block device mapping with a boot index of 0. On many hypervisors (libvirt/kvm for example) this will be device vda. Do not create a
                        duplicate mapping using --block-device-mapping for this volume.
  --boot-from-volume <volume-size>
                        When used in conjunction with the ``--image`` or ``--image-property`` option, this option automatically creates a block device mapping with a boot index of 0 and tells the
                        volume_type=<volume_type>: type of volume to create (name or ID) when source if blank, image or snapshot and dest is volume (optional)
  --swap <swap>
                        Create and attach a local swap block device of <swap_size> MiB.
  --ephemeral <size=size[,format=format]>
                        Create and attach a local ephemeral block device of <size> GiB and format it to <format>.
  --network <network>
                        Create a NIC on the server and connect it to network. Specify option multiple times to create multiple NICs. This is a wrapper for the '--nic net-id=<network>' parameter
                        that provides simple syntax for the standard use case of connecting a new server to a given network. For more advanced use cases, refer to the '--nic' parameter.
  --port <port>
                        Create a NIC on the server and connect it to port. Specify option multiple times to create multiple NICs. This is a wrapper for the '--nic port-id=<port>' parameter that
                        provides simple syntax for the standard use case of connecting a new server to a given port. For more advanced use cases, refer to the '--nic' parameter.
  --nic <net-id=net-uuid,port-id=port-uuid,v4-fixed-ip=ip-addr,v6-fixed-ip=ip-addr,tag=tag,auto,none>
                        Create a NIC on the server.
                        NIC in the format:
                        net-id=<net-uuid>: attach NIC to network with this UUID,
                        port-id=<port-uuid>: attach NIC to port with this UUID,
                        v4-fixed-ip=<ip-addr>: IPv4 fixed address for NIC (optional),
                        v6-fixed-ip=<ip-addr>: IPv6 fixed address for NIC (optional),
                        tag: interface metadata tag (optional) (supported by --os-compute-api-version 2.43 or above),
                        none: (v2.37+) no network is attached,
                        auto: (v2.37+) the compute service will automatically allocate a network.
                        Specify option multiple times to create multiple NICs.
                        Specifying a --nic of auto or none cannot be used with any other --nic value.
                        Either net-id or port-id must be provided, but not both.
  --password <password>
                        Set the password to this server. This option requires cloud support.
  --security-group <security-group>
                        Security group to assign to this server (name or ID) (repeat option to set multiple groups)
  --key-name <key-name>
                        Keypair to inject into this server
  --property <key=value>
                        Set a property on this server (repeat option to set multiple values)
  --file <dest-filename=source-filename>
                        File(s) to inject into image before boot (repeat option to set multiple files)(supported by --os-compute-api-version 2.57 or below)
  --user-data <user-data>
                        User data file to serve from the metadata server
  --description <description>
                        Set description for the server (supported by --os-compute-api-version 2.19 or above)
  --availability-zone <zone-name>
                        Select an availability zone for the server. Host and node are optional parameters. Availability zone in the format <zone-name>:<host-name>:<node-name>, <zone-name>::<node-
                        name>, <zone-name>:<host-name> or <zone-name>
  --host <host>
                        Requested host to create servers. (admin only) (supported by --os-compute-api-version 2.74 or above)
  --hypervisor-hostname <hypervisor-hostname>
                        Requested hypervisor hostname to create servers. (admin only) (supported by --os-compute-api-version 2.74 or above)
  --hint <key=value>
                        Hints for the scheduler
  --use-config-drive    Enable config drive.
  --no-config-drive     Disable config drive.
  --config-drive <config-drive-volume>|True
                        **Deprecated** Use specified volume as the config drive, or 'True' to use an ephemeral drive. Replaced by '--use-config-drive'.
  --min <count>
                        Minimum number of servers to launch (default=1)
  --max <count>
                        Maximum number of servers to launch (default=1)
  --tag <tag>   Tags for the server. Specify multiple times to add multiple tags. (supported by --os-compute-api-version 2.52 or above)
  --wait                Wait for build to complete
```

### Source

image is `Pawsey - Ubuntu 20.04 - vGPU CUDA 11 - 2021-06`

also `--boot-from-volume` can specify volume size of 100gb

### Flavour

- 10 instances
- no more than 1TB ram, so ram < 100,000
- no more than 200vcpus total, so cpus < 20
- `n3.16c64r` looks good: 64GB RAM, 16 vCPUs

### Networks

public external

### Security Group

### Key Name



### Property

### The final command

```bash
export FLAVOR=$(openstack flavor list -f json | jq -r $'.[] | select(.Name == "n3.16c64r").ID' | tee /dev/stderr )
[ -z "${FLAVOUR}" ] \
    && echo "could not find flavour"
    && export ABORT=1
export IMAGE=$(openstack image list -f json | jq -r $'.[] | select(.Name == "Pawsey - Ubuntu 20.04 - vGPU CUDA 11 - 2021-06").ID' | tee /dev/stderr )
[ -z "${IMAGE}" ] \
    && echo "could not find image"
    && export ABORT=1
export DESCRIPTION="dev's jupyter notebook server for stderr, can be deleted after 2021-09-07"
export NAME="dev-stderr-notebook"
export VOLUME_SIZE = 100
export COUNT=10
[ -z "${ABORT}" ] \
    && openstack server create \
        --flavor "$FLAVOR" \
        --image "$IMAGE" \
        --boot-from-volume "$VOLUME_SIZE" \ 
        --min "$COUNT" --max "$COUNT" \
        $NAME

# for i in {0..10}; do
    # export NAME="dev-stderr-notebook-${i}"
    # echo "creating ${NAME}"
# done
```
