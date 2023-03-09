### This README file is still in the making

# *Bootcamp Final Project - Building and deploying an application on the cloud (on an AWS architecture), and building effective CI/CD pipelines.*
## Some technologies I used: AWS Services and tools, Docker and Docker Compose, Github actions.
**(The source code for the application originates from the following GitHub repository: https://github.com/status-page/status-page).**

Our project had three primary objectives in mind, which were to prioritize:
- efficiency
- cost-effectiveness
- high availability

#### Initially, we deployed the application manually and established the required containers to enable deployment as a containerized application.
![containers](https://user-images.githubusercontent.com/117725271/224085012-c5b7b764-9f8f-456f-8a73-3437ff4109e6.png)
 

#### The architecture we built on aws looks like this:
![project architecture](https://user-images.githubusercontent.com/117725271/224083368-ac110178-f94b-419e-865b-163a60bdc536.png)

To ensure maximum availability, we implemented a multi-AZ deployment strategy and also ensured security by isolating the DB, application, and public subnets.


