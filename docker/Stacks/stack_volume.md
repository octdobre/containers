# :electric_plug: Stacks Volumes

## Basic

* Basic Volume
* Read Only
* Read Write
```
services:
  agent1:                       
    image: mssql           
    volumes:
      - db-data:/etc/data
      - db-data-readonly:/etc/data-readonly:ro
      - db-data-readwrite:/etc/data-readonly:rw
volumes:
  db-data:
  db-data-readonly:
  db-data-readwrite:
```

## Options

* Options
```
services:
  agent1:                       
    image: mssql           
    volumes:
      - db-data:/etc/data
volumes:
  db-data:
    driver_opts:
      type: "nfs"
      o: "addr=10.40.0.199,nolock,soft,rw"
      device: ":/docker/example"
```

Options are volume driver dependent.


## Using external volumes


* External

External volumes decouple volume management from the compose file.
Volumes persist when creating or deleting infrastructure with compose.

```
services:
  agent1:                       
    image: mongodb           
    volumes:
      - db-data-notmanaged:/etc/data
volumes:
  db-data-notmanaged:
    external: true
```

## :books: Documentation

:point_right::link:[Volume Storage Drivers](https://docs.docker.com/storage/storagedriver/select-storage-driver/)