#### (This README file is still in the making...)

# *Bootcamp Final Project: Building and Deploying Cloud-Based Application on AWS Architecture with Efficient CI/CD Pipelines.*
**(The source code for the application originates from the following GitHub repository: https://github.com/status-page/status-page).**

Our project had three primary objectives in mind, which were to prioritize:
- efficiency
- cost-effectiveness
- high availability and scalability

## The architecture we built on AWS:
![arch](https://user-images.githubusercontent.com/117725271/224322465-470708e5-7b32-4497-b987-f4a9099d25e5.png)

#### Security:
- We separated the database, application, and public subnets, and placed the application servers in private subnets that were only reachable through a bastion host. This enhanced the overall security posture of the system and reduced the attack surface.
- we implemented specific security rules for each type of instance in order to isolate them and minimize the chances of opening unnecessary ports. This further increased the security of the architecture and helped to mitigate potential threats.
- To further strengthen our security measures, we implemented a Web Application Firewall (WAF) with our load balancer. The WAF provided protection against SQL injection attacks and anonymous connections to our application (such as VPN), while also defending against bot attacks. Additionally, we incorporated a CAPTCHA feature to make our system even more resistant to unauthorized access attempts. 

- *Due to time constraints and a deadline, we made some compromises with regards to the security of our CI and test servers. Specifically, we were not able to fully isolate the test environment and had to place the test and CI server in public subnets.*

#### Availability:
- To ensure maximum availability of our system, we have adopted a multi-AZ deployment approach. This approach involves replicating our infrastructure across multiple Availability Zones (AZs) in a given region, providing resiliency and redundancy in the event of a single AZ failure.
- In order to distribute traffic evenly across various instances running in our two Availability Zones (AZs), we implemented an Application Load Balancer (ALB). It allowed us to effectively manage traffic, ensure high availability, and optimize performance by routing requests to the most appropriate target instances.

#### Scalability:
- By deploying our application in an Auto Scaling Group, we gained the benefits of flexibility and scalability as needed. The ASG allowed us to automatically adjust the number of instances running our application in response to changes in demand, ensuring that we could meet our performance and capacity requirements at all times. Additionally, the ASG ensured that we had a reliable and fault-tolerant architecture, by automatically replacing unhealthy instances.

#### Cost-Effectiveness:
- Our project's infrastructure costs around $200 monthly, with the RDS being the main expense. Our focus is on cost-effectiveness and simplicity, utilizing t3.medium or t3.small hardware to handle the workload, with assistance from the LB and ASG. The RDS expense is around $50 per month, with the smallest maintainable option used for production, while ElastiCache (with a replica) adds another ~ $50. Other expenses come from sources such as WAF, ELB traffic, instances, and more. Although we considered the free tier, we decided it wasn't suitable for production needs.

## Our CI/CD pipeline:
![cicd](https://user-images.githubusercontent.com/117725271/225620292-737e0010-a868-4c02-b784-9bf88ca76507.png)

#### CI:
- Here is an overview of the CI process:
  1. Trigger: Any push event on the main branch triggers the build process.
  2. Build: Our runner (CI server) builds the image using the Dockerfile we have created.
  3. Push: The built image is then pushed from the runner to our Amazon Elastic Container Registry (ECR) with a 'latest' tag.

- Our runner server has a scheduled job that automatically removes redundant containers and images every three hours to free up space and reduce its workload from previous workflows.

#### CD:
- Our CD process is triggered whenever a new image is added to the Amazon Elastic Container Registry (ECR). Here is an overview of the CD process:
  1. The latest image is deployed to the test server through the AWS CodeDeploy service. This stage ensures that the deployment process completes successfully without any errors.
  2. During the testing stage, we verify that the deployed app is able to communicate with the internet and is functioning correctly without any issues.
  3. Once the testing stage is completed, the process is repeated in the production environment. The latest image is deployed to the production servers, targeting our production Auto Scaling Group (ASG), and updating the instances running the app with the new feature/update. Finally, CodeDeploy verifies that the app is running smoothly in the production environment.
  
## Project Wrap-Up:
During the course of the project, we made significant changes to our architecture which made us better look at the big picture of things. We shifted from a Docker Compose-based setup (which would be great to use as a test env) to cloud-based resources such as RDS and ElastiCache. While the first approach was simpler and cheaper, we realized it wasn't suitable for a production environment. Instead, we switched to using RDS for a managed PostgreSQL DB and Elasticache for Redis, making the project more compatible for production use. Though this approach is more expensive, it offers better scalability and security.
In conclusion, we learned that different environments require different infrastructure setups and consideration of all aspects, and we're confident that our final architecture will best serve both developers and users.
