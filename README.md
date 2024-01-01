### CI/CD Project With Circle CI

#### Overview

This project demonstrates the usage of Docker to build a container image of an application and utilize CircleCI for automated image builds and pushing to Docker Hub.

#### Prerequisites

- Docker installed on your local machine: [Docker Install Guide](https://docs.docker.com/get-docker/)
- A Docker Hub account: [Docker Hub](https://hub.docker.com/)
- Access to CircleCI with a connected GitHub/Bitbucket repository

#### Getting Started

1. **Clone the Repository**

   Clone the repository to your local machine:

   ```bash
   git clone https://github.com/Tijani891/CI-CD-project.git
   ```

2. **Navigate to the Project Directory**

   ```bash
   cd CI-CD-project
   ```

3. **Build the Docker Image**

   Use the provided Dockerfile to build the Docker image. Replace `<image-name>` with your desired image name and tag:

   ```bash
   docker build -t <image-name>:<tag> .
   ```

4. **Verify the Image**

   Verify that the Docker image was successfully created:

   ```bash
   docker images
   ```

5. **Set Up CircleCI**

   - Push the code changes to your GitHub/Bitbucket repository.
   - Go to the CircleCI dashboard (https://app.circleci.com) and sign in.
   - Add your repository to CircleCI and set up the project.

6. **Configure CircleCI**

   Create a `.circleci/config.yml` file in your repository root with the following content:

   ```yaml
    version: 2.1
    jobs:
      build_and_push_image:
        docker:
          - image: docker:latest
        steps:
          - checkout
          - setup_remote_docker:
              docker_layer_caching: false
          - run:
              name: Build Docker image
              command: |
                docker build -t acme-web-app -t $DOCKERHUB_USERNAME/acme-web-app .

          - run:
              name: Login to Docker Hub
              command: |
                echo $DOCKERHUB_PASSWORD | docker login -u $DOCKERHUB_USERNAME --password-stdin
    
          - run:
              name: Push Docker image to Docker Hub
              command: |
                docker push $DOCKERHUB_USERNAME/acme-web-app:latest

    workflows:
      build_and_push_image:
        jobs:
          - build_and_push_image

   ```

    Ensure that the `DOCKERHUB_PASSWORD` and `DOCKERHUB_USERNAME` is stored securely as a CircleCI environment variable.

7. **Commit and Push Changes**

   Commit the `.circleci/config.yml` file and push it to your repository:

   ```bash
   git add .circleci/config.yml
   git commit -m "Add CircleCI configuration"
   git push origin master
   ```

#### Continuous Integration with CircleCI

Once you've set up CircleCI with the configuration file, every push to the repository will trigger CircleCI to run the build job, which will build the Docker image and push it to your Docker Hub repository automatically.

### Additional Notes

- Make sure to replace placeholders with your actual details (image name, Docker Hub credentials, etc.).
- Customize the Dockerfile as per your application requirements.
- Review CircleCI and Docker Hub documentation for further configuration and best practices.

