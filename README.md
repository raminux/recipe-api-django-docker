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
    - such as: docker compose run --rm app sh -c  "python manage.py collectstatic"

## How to build Docker image
After creating a Dockerfile, requirements.txt, .gitignore, and .dockerignore files and app directory, it is time to use docker command to build the image:
```bash
$> docker build .
```
Now, creaete a docker-compose.yml file, then run the following command:
```bash
$ docker compose build
```

## What is Linting?

- How to check code formatting
- Highlights errors, typos, formatting issues

To handle linting, we use the following steps:
- Install **flake8** package
- run it through Docker Compose
    - docker compose run --rm app sh -c "flake8"
- How to configure?
    - create a requirements.dev.txt file.
    - add flake8>=3.9.2,< 3.10 to it
    - add args: - DEV=true to docker-compose.yml file
    - add the following lines to the Dockerfile:
        - COPY ./requirements.dev.txt /tmp/requirements.dev.txt
        - ARG DEV=false
        - if [ $DEV = "true" ]; \
        then /py/bin/pip install -r /tmp/requirements.dev.txt ;\
        fi && \
    - run the following command to rebuild the development server
        ```bash 
        $ docker compose build
        ```
    - add .flake8 file inside the app directory with the following content to exclude from linting:
        - [flake8]
            exclude = 
                migrations,
                __pychache__,
                manage.py,
                settings.py,
    - to run the linting tool, use the following command
        - docker compose run --rm app sh -c "flake8"

## For Testing

- Django test suite
- Setup tests per Django app
- Run test through Docker Compose
    - docker compose run --rm app sh -c "python manage.py test"

## How createe Django project
Just hit the following command
```bash
$ docker compose run --rm app sh -c "django-admin startproject configs ."
```
To run the Django server, just wake up the container
```bash
$ docker compose up
```

## GitHub Actions

- An automation tool
- Similar to Travis CI, GitLab CI/CD, Jenkins
- Run jobs when code changes
- Common uses
    - Deployment
    - Code linting
    - Unit tests
- How it works
    1. Trigger (Push to Github)
    2. Job (Run unit tests)
    3. Result (Success/Fail)
- Pricing
    - Charged per minutes
    - 2000 free minutes
    - Various plans available

## How we'll configure GitHub actions

- Create a config gile at .github/workflows/checks.yml
    - Set trigger
    - Add steps for running testing and linting
- Configure Docker Hub auth
    - Needed to pull base images
    - Has rate limits
        - anonymous users: 100 pulls / 6h
        - authenticated users: 200 pulls / 6h
    - GitHub actions uses shared IP addresses
        - limit applied to all users
    - So Authenticate with Docker Hub to have 200 pull
        - 200 pulls per 6h all to ourselves!
        - More than enough for most projects
    - Additional plans available
- How to authenticate with Docker Hub?
    - register account on https://hub.docker.com/
    - Use `docker login` dduring our job
    - Add secrets to GitHub project
        - DOCKERHUB_TOKEN
        - DOCKERHUB_USER
        - Secrets are encrypted
        - Decrypted when needed in actions

## Test Driven Development  (TDD) in Django

Django has its own way to TDD which is called Django test framework.
Keep in mind that in TDD, you first write tests and then add functionalities to pass those tests.

- based on the `unittest` library
- Django adds features
    - Test client - dummy web browser
    - Simulate authentication
    - Temporary database
- Django REST Framework adds features
    API test client
- Where do you put tests?
    - Placeholder tests added to each project
    - Or, create `tests/` subdirectory to split tests up
        - Keep in mind:
            - Only use `tests.py` or `tests/` directory (not both)
            - Test modules must start with `test_` when using `tests/` directory
            - Test directories must contain `__init__.py`
- Test Database
    - Test code that uses the DB
    - Specific databases for tests
    - Happens for every test (by default)
- Test Classes
    - SimpleTestCase
        - No database integration
        - Useful if no database is required for your test
        - Save time executing tests
    - TestCase
        - Database integration
        - Useful for testing code that uses the database
- Writing tests
    - Import test class
        - SimpleTestCase
        - TestCase
    - Import objects to test
    - Define test class
    - Add test method
        - Setup test inputs
        - Execute code to be tested
        - Check output using assert
