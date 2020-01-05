---
path: "/scad/quality"
title: "SCAD Container Quality"
date: "2020-01-04"
draft: false
author: Simon Anliker
tags:
  - scad
---

# Container Quality

## Docker Image Metadata

On Docker Hub
- Name
- Stars
- Publication date
- Short description
- Long description
- Comments
- Commenters
- Dates
- Content
- Image pull command
- Hardware/OS-specific characteristics (platform/os)


Docker Finder - multi attribute local search

    dfinder java:1.7 --sort -size --sort +stars
    

## Docker Image Quality

Dockerfile
- invalid/faulting shell commands
- invalid references to data sources for copy
- too many layers / no command aggregation

Docker image
- deprecated/vulnerable code or shared libs
- deprecated/vulnerable base image
- inappropriate size


Quality check tools: hadolint, dockerlint, anchore, dockscan


Best practices for Dockerfiles
- Pin version of used image explicitly
- Use label for maintainer
- Pin dependency versions explicitly
- Concat run commands with \ where possible
- Sort multi line arguments
- Use COPY instead of ADD because it is more transparent (no additional functionality like tar extraction)
- Use RUN with `wget` or `curl` instead of ADD so that unused files can be deleted to reduce image size (as part of the same RUN to not add any additional layers)
- Use multi-stages builds
- Use package trimming commands e.g. `--without-recommends` 

Best practices for builds
- In most cases the Dockerfile should be in an empty directory, if not use `.dockerignore` (`docker build .` uses `.` as the context which is tar'd and sent to the docker daemon).
- If no context is needed read from stdin: `docker build - < Dockerfile`
- Design for "cattle"


Strange stuff for files
- One layer per file - high ops cost (tons of layers)
- One layer per image - high storage cost (duplication)
- All files in a single layer - high network cost (pull all, always)

Wale: reduction of shared libraries by creating a core image with preprocessing

DIVA: Docker image vulnerability analysis - find problems in base images, avoid vulnerability propagation


## Runtime Quality

ChaosORCA architecture: fault injection at system call level



