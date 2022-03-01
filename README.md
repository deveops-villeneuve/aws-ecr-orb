# AWS ECR Orb [![CircleCI Build Status](https://circleci.com/gh/CircleCI-Public/aws-ecr-orb.svg?style=shield "CircleCI Build Status")](https://circleci.com/gh/CircleCI-Public/aws-ecr-orb) [![CircleCI Orb Version](https://img.shields.io/badge/endpoint.svg?url=https://badges.circleci.io/orb/circleci/aws-ecr)](https://circleci.com/orbs/registry/orb/circleci/aws-ecr) [![GitHub license](https://img.shields.io/badge/license-MIT-blue.svg)](https://raw.githubusercontent.com/circleci-public/aws-ecr-orb/master/LICENSE) [![CircleCI Community](https://img.shields.io/badge/community-CircleCI%20Discuss-343434.svg)](https://discuss.circleci.com/c/ecosystem/orbs)
CircleCI orb for interacting with Amazon's Elastic Container Registry (ECR).
## Usage

See the [orb registry listing](https://circleci.com/orbs/registry/orb/circleci/aws-ecr) for usage guidelines.

## Examples
See below for both simple and complete examples of this orb's `build_and_push_image` job. For details, see the [listing in the Orb Registry](https://circleci.com/orbs/registry/orb/circleci/aws-ecr).

### Simple
```yaml
version: 2.1

orbs:
  aws-ecr: circleci/aws-ecr@x.y.z

workflows:
  simple_build_and_push:
    jobs:
      # with default parameter values, the following would be sufficient to build and push an image to ECR
      - aws-ecr/build-and-push-image:
          repo: myRepositoryName

```

# Complete
```yaml
version: 2.1

orbs:
  aws-ecr: circleci/aws-ecr@x.y.z
  
executors:
  arm64:
    machine:
      image: ubuntu-2004:202101-01
      docker_layer_caching: true
    resource_class: arm.medium

workflows:
  complete_build_and_push:
    jobs:
      # build and push image to ECR
      - aws-ecr/build-and-push-image:

            # select exectuor defined above
            executor: arm64

            # name of your ECR repository
            repo: myECRRepository

            # set this to true to create the repository if it does not already exist, defaults to "false"
            create-repo: true

            # required if any necessary secrets are stored via Contexts
            context: myContext

            # AWS profile name, defaults to "default"
            profile-name: myProfileName

            # name of env var storing your AWS Access Key ID, defaults to AWS_ACCESS_KEY_ID
            aws-access-key-id: ACCESS_KEY_ID_ENV_VAR_NAME

            # name of env var storing your AWS Secret Access Key, defaults to AWS_SECRET_ACCESS_KEY
            aws-secret-access-key: SECRET_ACCESS_KEY_ENV_VAR_NAME

            # Name of new profile associated with role arn.
            new-profile-name: newProfileName

            # Source profile containing credentials to assume the role with role-arn.
            source-profile: sourceProfileName

            # Role ARN that new profile should take
            role-arn: arn:aws:iam::123456789012:role/some-role

            #Your AWS region
            region: AWS_REGION

            # name of env var storing your ECR Registry ID
            registry-id: AWS_ECR_REGISTRY_ID

            # ECR image tags (comma separated string), defaults to "latest"
            tag: latest,myECRRepoTag

            # name of Dockerfile to use, defaults to "Dockerfile"
            dockerfile: myDockerfile

            # path to Dockerfile, defaults to . (working directory)
            path: pathToMyDockerfile

            # Select a specific version of the AWS v2 CLI. By default the latest version will be used.
            aws-cli-version: latest

            # Boolean value if pushing to public registry. Defaults to true.
            public-registry: false

            # Security scans repository on push.  Defaults to true.
            repo-scan-on-push: true

            # The amount of time to allow the docker build command to run before timing out, defaults to "10m"
            no-output-timeout: 20m

            # Extra docker buildx build arguments
            extra-build-args: --compress

            # Specify platform targets for built docker image.
            platform: linux/amd64

            # Push image to repository after building.  Defaults to true
            push-image: true

            # Set to true if you don't want to build the image if it already exists in the ECR repo, for example when
            # you are tagging with the git commit hash. Specially useful for faster code reverts.
            skip-when-tags-exist: false
```

## Contributing
We welcome [issues](https://github.com/CircleCI-Public/aws-ecr-orb/issues) to and [pull requests](https://github.com/CircleCI-Public/aws-ecr-orb/pulls) against this repository! For further questions/comments about this or other orbs, visit [CircleCI's Orbs discussion forum](https://discuss.circleci.com/c/orbs).
