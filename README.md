# Apify base Docker images

Public Docker images for Apify Actor serverless platform (https://docs.apify.com/actor)

The sources for the images are present in subdirectories that are named as the corresponding
Docker image. For example, the `node` directory corresponds to the
[apify/actor-node](https://hub.docker.com/r/apify/actor-node/) Docker image.

The images are using the following tags:

| Tag      | Description                                  |
| -------- | -------------------------------------------- |
| `latest` | Well-tested production version of the image. |
| `beta`   | Development version of the image.            |

## Maintenance

The process of building and publishing new images is automated using GitHub Actions, and a set of scripts that are stored in `.github/actions/version-matrix`. We recommend reading the [`README.md`](.github/actions/version-matrix/README.md) in that directory to understand how the scripts work.

Manual releases can also be done by triggering the specified workflows manually. At minimum, you **must** specify the `release_tag` input, which is the tag for the image. Every other variable is optional, and if not specified, the matrix generator will fetch the latest version of the corresponding package from the registry.

> [!CAUTION]
> When manually building a `latest` image, please try to avoid specifying certain library versions unless absolutely necessary.

### Adding a new actor image

To create a new image, which is not yet published in Apify DockerHub organization.
You need access to the organization and rights to create a new repository.
After, you need to follow these steps:

1. Create a new folder with the same name as the package you want to create without the prefix `actor-`.
   For image `apify/actor-node`, create folder `node`.

2. Create a source of the image in that folder. Remember to create a test that is runnable using `docker run` to be able to test in the image in CI/CD.

3. Create a new repository in the Apify DockerHub organization, use the name with `actor-` prefix, e.g. `apify/actor-node`.

4. Give permission Read & Write to create image for devs groups in the Apify DockerHub organization.

5. Create a GitHub workflow which builds, tests and publishes the image into the DockerHub.

### Testing images locally

You will need the following tools installed: docker, make, jq, git

1. Clone this repository

1. Run `make all` to test out all images (this will take a long while, be patient). You can use `make test-node` or `make test-python` to run a specific collection of tests.

You can overwrite the node version you build the images for by specifying `NODE_VERSION=xx` environment variable.
You can overwrite the playwright version you build the images for by specifying `PLAYWRIGHT_VERSION=vx.x.x-` environment variable. You must respect the format of `v<full-semver-version>-`
You can overwrite the puppeteer version you build the images for by specifying `PUPPETEER_VERSION=x.x.x` environment variable. You must respect the format of `<full-semver-version>`

1. If you want to run a specific test, call it with `make <test name>`. Run `make what-tests` to see what tests are available.
