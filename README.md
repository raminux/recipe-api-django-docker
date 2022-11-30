# Recipe API using Django + Docker

## Why using Docker?

- Consistent development and production servers
- Easier collaboration with other developers: different developers may have different versions of Python, database, and SDK, which will result in inconsistency between the requirements of a project. Using Docker will resolve this issue, becasuse all developers can use the same docekr image for their development
- Capture all dependencies as code: such as Python requirements, and OS level dependencies
- Easier cleanup: just delete the docker image

## How to use Docker

- Define Dockerfile: containes all the operating system level dependencies that our project depends on
- Create Docker Compose configuration: tells Docker how to run the images that are created from our Docker file configuration
- Run all commands via Docker Compose

## Docker on GitHub actions

- Docker Hub has rate limit
    - 100 pulls/6hr for unauthenticated users
    - 200 pulls/6hr for authenticated users
- GitHub Actions is a shared service
    - 100 pulls/6hr applied for all users
- Authenticate with Docker Hub
    - Create account (both Docker Hub and GitHub)
    - Setup credentials (Add Docker Hub credentials in GitHub)
    - Login before running job