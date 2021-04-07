# Running nodes from snapshots

Fresh nodes may take days to complete synchronisation. To make the process faster, the CENNZnet team takes snapshots of the nodes.

The [Archive(Validator) Node archives can be found here](https://s3-ap-southeast-1.amazonaws.com/cennznet-snapshots.centralityapp.com/azalea/1.2.2/validator/index.html).

The [Full Node archives can be found here](https://s3-ap-southeast-1.amazonaws.com/cennznet-snapshots.centralityapp.com/azalea/1.2.2/fullnode/index.html).

To run a node with an archive,
* Download the latest archive
* Unzip the archive
* Add the argument  '-v </path/to/dir/containing/the/unzipped/archive>:/mnt/cennznet' to 'docker run'. This mounts the volume to the docker container, so that the container has access to the archive.
* Add the argument '--base-path /mnt/cennznet' to 'cennznet/cennznet'. This puts the archive in use.
* Modify the following command. Replace `<version>` with the [latest release](https://github.com/cennznet/cennznet/releases) version number, Eg, 1.4.0. edit the the path to archive and the node name.
* Run the command

```bash
docker run -p 9944:9944 -it -v </path/to/dir/containing/the/unzipped/archive>:/mnt/cennznet \
 cennznet/cennznet:<version>
 --base-path /mnt/cennznet \
 --chain=/cennznet/genesis/azalea.raw.json \
 --unsafe-ws-external \
 --unsafe-rpc-external --rpc-cors all \
 --telemetry-url 'ws://cennznet-telemetry.centrality.me:8000/submit 0' \
 --name <MY_NODE>
```
