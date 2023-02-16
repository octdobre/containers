### Build context //todo move to build page

The Docker build context is the set of files and directories that are available to the docker build command when building a Docker image. It includes the Dockerfile and any other files that are needed to build the image. The build context is specified as the directory that contains the Dockerfile.

During the build process, Docker sends the entire build context to the Docker daemon, which then uses it to build the Docker image. The build context is sent over to the daemon, which includes all files in the directory, so it is important to make sure that only the necessary files are included, as sending unnecessary files can increase build time and network traffic.

For example, if you have a Dockerfile that specifies that files from the app directory should be copied into the image, the build context should be the parent directory of the app. By default, the docker build command uses the current directory as the build context, but it can be changed using the -f flag to specify a different Dockerfile, and the`.` (dot) to specify a different build context.