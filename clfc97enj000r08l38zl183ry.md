---
title: "Best Practices for Writing a Dockerfile"
seoDescription: "Optimize Your Docker Images for Performance and Efficiency"
datePublished: Fri Mar 17 2023 08:03:57 GMT+0000 (Coordinated Universal Time)
cuid: clfc97enj000r08l38zl183ry
slug: best-practices-for-writing-a-dockerfile
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1678970625075/cd74bc7f-3f74-4336-8105-2d3da1c38f08.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1679040202629/39206e3b-956d-4e16-83ef-c7f6bc6ef58e.png
tags: docker, devops, best-practices, containers, dockerfile

---

Docker has transformed the way containers are used since the time it was in use by the developer's community, primarily due to its user-friendly approach. Dockerfiles are the core building blocks of Docker, providing a set of instructions for creating a Docker image. A well-written Dockerfile can help simplify the deployment process, improve the overall performance and security of the application, and reduce the likelihood of errors or issues.

Optimizing Docker Images can be a time-consuming process that requires experience and practice. However, having worked with Docker for a considerable amount of time, I'm excited to share my knowledge on how to create better containers right from the start. In this article, I'll be discussing the best practices that you can follow to ensure optimal Docker image development.

### Determine the units that can be cached

To optimize the performance and efficiency of your Docker image build process, it's important to identify **cacheable units**. Installing packages using multiple `RUN` commands can slow down the build process. Instead, using a single `RUN` command to apply all dependency packages creates a single cacheable unit, avoiding the creation of multiple ones.

```bash
RUN apt-get update && apt-get install -y \
    python3 \
    py3-pip \
    git \
    openssh-client \
    bash \
    openssl \
    ca-certificates \
    gcc \
    musl-dev \
    libffi-dev \
    libxml2-dev \
    libxslt-dev \
    make
```

The above is an example of grouping multiple packages into a single `RUN` command, which allows us to take advantage of Docker's caching mechanism to speed up the build process. Additionally, this helps to reduce the size of the final Docker image.

### Make the images lightweight

> The image size plays an essential role in creating a good Dockerfile. Using smaller images will result in faster deployments and less attack surface.

When creating an image it's always a good idea to only install tools that you really need and not ones that you don't. This helps keep the image small and efficient. Also, when using a package manager make sure to only install necessary packages and not ones that are just recommended. This will prevent unnecessary dependencies from being installed and will help keep the image streamlined.

```bash
RUN apt-get update && apt-get -y install --no-install-recommends
```

This above command is a Unix command commonly used to update the package list and install packages without installing any recommended packages.

The `--no-install-recommends` option, when used with the `apt-get install` command, tells the package manager to install only the specified packages and their dependencies, but not any recommended packages that may not be necessary for the system to function properly.

Overall this command updates the package list and installs packages without any recommended packages, which can help keep the image lean and avoid installing unnecessary packages.

### Maintainance of the image

> Choosing the right base image for the application is essential.

**Use the official Docker images**

By utilizing the official Docker image, you can avoid the hassle of including unnecessary dependencies that would make the image bigger. There are three key benefits to opting for the official image:

1. You get an image that follows the best practices.
    
2. The image is reliable and maintained well.
    
3. You can trust the image more, ensuring better security.
    

Here's an example Dockerfile:

```bash
# Use official Python image as base
FROM python:3.9-slim-buster

# Set working directory
WORKDIR /app

# Copy the requirements file and install dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy the rest of the application code to the container
COPY . .

# Expose the port on which the application will run
EXPOSE 8080

# Set the command to run the application
CMD ["python", "app.py"]
```

This Dockerfile uses the official Python 3.9 image as its base and sets the working directory to `/app`. It then copies the `requirements.txt` file to the container and installs the dependencies using `pip`. The rest of the application code is also copied to the container. Finally, it exposes port `8080` on which the application will run and sets the command to run the [`app.py`](http://app.py) file using `python`.

**Use the correct tags**  
To ensure stability and avoid issues, it's best to choose a specific tag when selecting a base image instead of using the `latest` tag. The `latest` tag may have updates that cause problems or conflicts with your project over time.

```bash
#Pull official base image

FROM python:3.9-slim-buster
```

**Use minimal flavors of the image**

Using minimal flavors (variants) of an image can make it smaller and easier to deploy. This means that applications can be launched faster and with better security.

The Alpine flavor is a good example of a lightweight image, with a base size of just 2MB, which can help to significantly reduce the overall image size.

### Replicability

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679037237825/dba04c26-1753-4692-a9b1-1c2f46a53241.jpeg align="center")

**Build your application in a stable environment**

When creating a Docker application, it's essential to use a controlled environment to build it consistently. It's not advisable to build the application in your local environment and then upload it to the registry. Doing so can lead to inconsistency in the application's image due to the various packages you may have installed on your local machine. This can compromise the essential advantage of Docker, which is the ability to execute applications consistently across various environments.

**Implement multi-stage builds**

When deploying applications, it's best to use multi-stage builds. This helps to avoid the need to build dependencies in the running container.

To do this, you can create a build image with all the necessary development dependencies to build your application. Then, you can transfer the compiled binaries to a separate container image specifically designed to run the application. This way, you won't have to worry about including any unnecessary dependencies in the running container.

```bash
# Stage 1: Build the frontend code
FROM node:14.17-alpine AS builder

WORKDIR /app

COPY package*.json ./
RUN npm install --only=prod

COPY . .
RUN npm run build

# Stage 2: Copy the compiled code to an Nginx image
FROM nginx:1.21-alpine

COPY --from=builder /app/build /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

The above Dockerfile has two stages:

* **Stage 1 (called** `builder`**):** This stage is based on the official Node.js `14.17-alpine image`, which includes Node.js and `npm`. It sets the working directory to `/app`, copies the `package.json` and `package-lock.json` files, and runs `npm install --only=prod` to install only the production dependencies. Then, it copies the rest of the code into the container and runs `npm run build` to build the frontend code. The resulting compiled code is stored in the `/app/build` directory.
    
* **Stage 2:** This stage is based on the official Nginx `1.21-alpine image`, which includes the Nginx web server. It copies the compiled frontend code from Stage 1 by using the `COPY --from=builder` syntax. Finally, it exposes port 80 and starts the Nginx server.
    

By using multi-stage builds, this Dockerfile ensures that the final container only includes the compiled frontend code and the minimal dependencies required to run it. This results in a smaller, more secure container that's optimized for production use.

### Conclusion

That's all for this article. If you're new to Docker, I suggest using these best practices when building your first image. It will help you understand them better and use Docker more efficiently right from the beginning.

If you have any other practices to share, feel free to leave a comment below. Thanks for reading!