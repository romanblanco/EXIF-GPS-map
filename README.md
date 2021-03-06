# Graffiti

Repository provides a tool for graffiti community to keep track and preserving street artwork in their areas.

<img src="./docs/map.png" width="100%" />

## Features

### Distributed storage

The core of the project are geotagged photos stored in [IPFS](), meaning they are not stored in a central database, allowing users to only maintain the photos they are interested in. The data will be reachable as long as there is someone sharing them.

### Metadata

The maintained graffiti collection can be annotated by the user in [metadata](https://github.com/romanblanco/graffiti/blob/master/collection/metadata.json), specifying OpenStreetMap node as a surface and tags for photos for better searchability.

## Requirements

- [Docker Compose](https://docs.docker.com/compose/install/)

## Getting started

```
docker-compose up -d --build
```

In Docker log you should see the process is waiting for getting IPFS content:

```
graffiti-collection | 05:12:52.059 main — INFO 001 parsing graffiti metadata JSON file
graffiti-collection | 05:12:52.065 main — DEBU 002 parsed JSON with 545 elements
graffiti-collection | 05:12:52.065 main — INFO 003 getting IPFS content
```

This could take a few minutes to the fresh IPFS setup to discover the content source and start downloading images. On `localhost`, you can monitor your IPFS instance running at https://localhost:5001/webui.

When the downloading starts, you should observe the following log:

```
graffiti-collection | 05:16:05.238 main — DEBU 004 got metadata of 554 photos from IPFS
graffiti-collection | 05:16:06.207 main — DEBU 005 got raw tar of photos from IPFS: *shell.trailerReader: &{0xc000092120}
graffiti-collection | 05:16:06.209 main — INFO 006 parsing EXIF data from photos
graffiti-collection | 05:16:06.209 main — DEBU 007 parsing EXIF data for photo: IMG_20180216_000759.jpg
graffiti-collection | 05:16:06.211 main — DEBU 008 parsing EXIF data for photo: IMG_20180217_190142.jpg
...
graffiti-collection | 05:58:55.457 main — INFO 25c serving complete content at :8083/api
graffiti-collection | 05:58:55.457 main — INFO 25d serving geotagged content at :8083/geojson
```

When the data are served, the working map should be available on http://localhost:3000/.

## Troubleshooting

If the `graffiti-collection` container is hanging on `getting IPFS content`, it is probably because the route to content has not been discovered. To help the discovery, by adding the peer distributing IPFS content:

```bash
docker exec -it graffiti-ipfs sh
#ipfs bootstrap add <peer>
ipfs bootstrap add /ip4/192.168.10.1/tcp/4001/ipfs/QmQVvZEmvjhYgsyEC7NvMn8EWf131EcgTXFFJexample3
exit
```

If you have problem sharing your content and your device is behind NAT, configure port forwarding: https://docs.ipfs.io/how-to/nat-configuration/#port-forwarding.

If issues still persist, refer to https://github.com/ipfs/go-ipfs/blob/master/docs/file-transfer.md#troubleshooting.

## Learn more

- [Wiki](https://github.com/romanblanco/graffiti/wiki)
- [Task board](https://github.com/romanblanco/graffiti/projects)
- [#graffiti:matrix.org](https://view.matrix.org/room/!WVlsowqbtSqMWutaCX:matrix.org/)
