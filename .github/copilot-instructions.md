# Mono Repo Security Patches Workflows Project

## Commit Message

- The commit message should be in the format of conventional commits.

## Copilot Chat Rules

- Every time you are asked for how to run the code, consider all the points described in the section "Required Secrets".

## Client Project functionality

This Client project is a simple microservice that provides a REST API for managing a list of users. It allows you to create, read, update, and delete users. The project is built using Spring WebFlux and uses R2DBC for database access. The project is designed to be a simple example of how to build a reactive microservice using Spring WebFlux and R2DBC. The main objective of this project is have a client for my library project, because we need to test the library project with the applied patches without deploying a new library release. The client uses the library project source code as a dependency, using the `implementation project(':library)`

## Workflows

The project contains two main workflows files located in the `.github/workflows` directory. These workflows are responsible for automating the process of checking for security patches in the library project and issues if there is any problem in the tests. The workflows are as follows:

1. **Identify, classify and patch vulnerable dependencies**: 
    - This workflow pulls the vulnerabilities reports from dependabot security alerts and creates a new branch with the patches applied.
    - The workflow is triggered on call from the other workflow.
2. **Test the library project with the applied patches**:
    - This workflow is triggered manually .
    - It runs the client project using Docker and runs the tests for the client project using K6 from Grafana.
    - If the tests fail, it creates a new issue with the details of the failure.
    - No matter if the tests pass or fail, it creates uploads the test report as artifact and deploy the test report to GitHub Pages.

## Required Secrets

1. **API_TOKEN_GH**: This secret is used to authenticate with the GitHub GraphQL API. It's used to fetch the vulnerabilities reports from dependabot security alerts. You can create a personal access token with the `repo` scope and add it as a secret in your GitHub repository.

2. **DB_URL** : This secret is used to connect to the SQLServer database. It should be in the format `r2dbc:pool:mssql://<host>:<port>/<database>?trustServerCertificate=false`. You can create a secret in your GitHub repository with this value.

3. **DB_USER**: This secret is used to connect to the SQLServer database. It should be the username for the database. You can create a secret in your GitHub repository with this value.

4. **DB_PASSWORD**: This secret is used to connect to the SQLServer database. It should be the password for the database. You can create a secret in your GitHub repository with this value.


