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

## How to create Docker Access Token and Set it up in GitHub

1. Go to accounts settings in docker hub
2. Security
3. New Access Token
4. Go to settings of your repository in GitHub
5. Go to Secrets/actions
6. Add new repository secret
7. Add the following secrets
    - DOCKERHUB_USER --> Docker hub user name
    - DOCKERHUB_TOKEN --> Docker hub token in step 3

## Using Docker with Django

1. Many benefits
    - Consistent development and production environments
    - Easier collaboration
    - Capture all dependencies as code
    - Easier cleanup
    - save a lot of time
2. Drawbacks
    - VSCode is unable to access interpreter
    - More difficult to use integrated features such as Linting tools and Interactive debugger
    - suggest using terminal

## How to configure Docker to use Django

- Create a Dockerfile
- List steps for creating image
    - Choose a base image (Python)
    - Install dependencies
    - Setup users
- Setup Docker Compose
    - How our Docker images(s) should be used
    - Define our "services"
        - Name(eg:app)
        - Port mappings
        - Volum mappings
- Run all commands using Docker Compose
    - such as: docker-compose run --rm app sh -c  "python manage.py collectstatic"

## How to build Docker image
After creating a Dockerfile, requirements.txt, .gitignore, and .dockerignore files and app directory, it is time to use docker command to build the image:
```bash
$> docker build .
```
Now, creaete a docker-compose.yml file, then run the following command:
```bash
$ docker compose up -d
```

## What is Linting?

- How to check code formatting
- Highlights errors, typos, formatting issues

To handle linting, we use the following steps:
- Install **flake8** package
- run it through Docker Compose
    - docker compose run --rm app sh -c "flake8"

## For Testing

- Django test suite
- Setup tests per Django app
- Run test through Docker Compose
    - docker compose run --rm app sh -c "python manage.py test"

