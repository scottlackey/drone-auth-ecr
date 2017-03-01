drone-auth-ecr is a docker image that lives in your docker registry that can be pulled to auth against ECR from within drone, 
enabling drone to pull from ECR

**Author**: [Scott Lackey](https://github.com/scottlackey)

This image acts as a liazon between your [drone](http://readme.drone.io/0.5/) instance and [Amazon ECR](https://aws.amazon.com/ecr/). 
It has the AWS ECR client and the docker daemon installed so that it can pull an image from ECR. Since drone shares it's volumes the 
ECR image is available for all build steps.

To build
---------

- Ensure your drone CI node in AWS is configured with an IAM role with permissions to interact with ECR/ECS
- Ensure the config/config file has the correct AWS region.
- build the image with docker build, tag and push

example usage in Drone v.5 pipeline
-------------
```bash
pipeline:
  auth:
    image: drone-auth-ecr
    commands:
      - aws ecr get-login | bash 
      - docker pull 12345.dkr.ecr.us-west-2.amazonaws.com/my-image:latest
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock

  build:
    image: 12345.dkr.ecr.us-west-2.amazonaws.com/my-image:latest
    commands:
     - npm intstall
     - npm test
```
