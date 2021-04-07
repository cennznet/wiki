# Running a development node

A development node gives you a local sandbox environment for testing your features.

* Install [Docker](https://www.docker.com/)
* Modify the following command. Replace `<version>` with the [latest release](https://github.com/cennznet/cennznet/releases) version number, Eg, 1.4.0. 
* Run the command:
```bash
docker run -p 9944:9944 -it --rm  cennznet/cennznet:<version> --dev --ws-external
```