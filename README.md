<b>Logz.io Docker Log Shipping Reference Architecture</b>

The recommended approach for shipping logs from Docker containers to utilize the Docker JSON File logging driver and FluentD (with a Docker metadata plugin for enriching data called fluent-plugin-docker_metadata_filter. Logz.io accepts many different methods of shipping, but provides more extended configuration references for the recommended approach below.

<b>Requirements

Docker (some version)

Getting Started<br /></b>
1 - Make sure the JSON-file logging driver is enabled (this is the default Docker logging driver):
https://docs.docker.com/config/containers/logging/json-file/<br />
2 - Docker build to build the package:<br />
docker build -t logziodockerrefarch:dev .<br />
3 - Docker run with a volume mount to the container log directory for the logs, volume mount to tmp to write the position file and a volume mount to the Docker socket to capture the metadata, as well as an environment variable to pass the url/token:<br />
ddocker run -v /var/lib/docker/containers:/var/lib/docker/containers -v /tmp:/tmp -v /var/run/docker.sock:/var/run/docker.sock -e "LOGZ_IO_URL_1=https://listener.logz.io:8071?token=xxxxxxx" -d --name=logziodockerrefarch logziodockerrefarch:dev<br />

<b>Configuration<br /></b>
The fluent.conf file contains configuration and comments which accomplish the following:<br />
1 - Sources the data from the volume mount to the host to capture logs<br />
2 - Uses the Docker metadata plugin to enrich the records<br />
3 - Copies the contents of the 'log' field to 'message' in order for enhanced visibility in Kibana discover page<br />
4 - Drops unnecessary fields (including the log field and unnecessary container metadata)<br />

<b>TODO<br /></b></b>
1 - Add more multi-line examples<br />