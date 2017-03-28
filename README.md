# elpmaxe-fluffy-couscous

Example

* Build and run Sonatype Nexus OSS with Docker.

## Build

```
docker build ./nexus \
    --tag nexus:elpmaxe \
    --force-rm \
    --no-cache \
    --pull
```

## Persistent Data Storage

```
mkdir -p /opt/containers/nexus/nexus-data
chown -R 200 /opt/containers/nexus/nexus-data
chmod 777 /opt/containers/nexus/nexus-data
```

## Run

```
docker run \
    --publish 8081:8081 \
    --rm \
    --volume /opt/containers/nexus/nexus-data:/sonatype-work \
    --env MAX_HEAP=768m \
    nexus:elpmaxe
```

The `CONTEXT_PATH` [environment variable][1] can be used to control the Nexus Context Path
, passed as `-Dnexus-webapp-context-path`. Defaults to `/nexus`.

This [environment variable][2] is used to define the URL which Nexus is accessed.

## Permissions

- Use `docker exec` to run a command in a running container.

```
docker exec \
    --interactive \
    --tty \
    $(docker ps \
      --quiet \
      --filter=ancestor=nexus:elpmaxe) \
    /bin/bash
```

- Use `id` inside the container to check user permissions.

```
bash-4.2$ id
uid=200(nexus) gid=200(nexus) groups=200(nexus)
```

- Check user inside the container has sufficient permissions to mounted volume.

[1]: https://books.sonatype.com/nexus-book/reference/install-sect-proxy.html#nexus_webapp_context_path
[2]: https://github.com/sonatype/docker-nexus
