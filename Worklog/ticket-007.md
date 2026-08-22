# Day 007 — Docker & Docker Compose

## Sprint

Sprint 3

## Initiative

S3-I2 — Docker & Docker Compose

## Status

✅ Completed

## Objective

Containerize the Telemedicine HMS backend and establish a repeatable multi-service development environment using Docker and Docker Compose.

## What I Implemented

- Created Dockerfiles for Spring Boot services.
- Containerized the required backend services.
- Created Docker Compose orchestration.
- Configured service-to-service communication within the Docker environment.
- Configured environment-specific values where required.
- Containerized PostgreSQL where applicable.
- Verified application startup through Docker Compose.

## Technologies

- Docker
- Docker Compose
- Java
- Spring Boot
- PostgreSQL
- Maven

## Engineering Concepts Learned

- Docker images vs containers
- Dockerfile
- Multi-stage builds
- Container networking
- Environment variables
- Volumes
- Service dependencies
- Docker Compose orchestration

## Engineering Challenges

- Service startup dependencies
- Container networking
- Database connectivity
- Environment configuration
- JVM/Spring Boot container configuration

## Engineering Outcome

The Telemedicine HMS can now be deployed as a containerized multi-service application rather than requiring every backend service to be started manually.

## Next

S3-I3 — Unit Testing