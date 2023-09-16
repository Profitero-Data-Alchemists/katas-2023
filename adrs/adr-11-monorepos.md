# ADR 11: Use separate Monorepo for Frontend and Backend

**Date**: 2023-09-15

## Status
Proposed

## Context
Choosing a code structure approach is crucial in software development because it directly impacts the organization, readability, maintainability, and scalability of your codebase. The code structure defines how your code is organized, where files and directories are placed, and how different components interact. 

## Decision
We will begin our app with two mono repositories: one for the frontend (covering both web and mobile apps) and another for all the backend microservices. This approach allows us to efficiently manage development in the early stages and maximize code reuse. It's important to note that a well-structured monorepo can always be divided into smaller repositories if needed.
To ensure flexibility and minimize direct code dependencies, we'll implement a strategy where code sharing occurs through libraries or plugins managed as separate artifacts. This way, we maintain modularity and can quickly adapt to changing requirements in the future.

## Consequences
 
### Positive
- **Code Sharing and Reusability**: Monorepos promote code sharing and reusability across projects within the same repository. Developers can easily reference and reuse code, libraries, and components, which can lead to faster development and fewer duplicated efforts.

- **Simplified Dependency Management**: In a monorepo, managing dependencies between different components is simplified. You can use a single package manager to handle dependencies across all projects, ensuring compatibility and reducing versioning conflicts.

- **Consistency**: A monorepo enforces consistency in coding standards, code style, and development practices across all projects. This consistency can lead to higher code quality and easier code reviews.

- **Improved Testing and Continuous Integration**: With all code in one place, it's easier to set up comprehensive testing and CI/CD pipelines that cover all projects. This ensures better code quality and more robust automation.

### Negative
- **Build Times**: Building and deploying all components in a monorepo can be time-consuming, especially if changes in one component trigger rebuilds and testing across the entire repository.

- **Team Size**: Monorepos are generally more suitable for smaller to medium-sized teams. Large teams may experience coordination challenges and scalability issues.

- **Pull Request Complexity**: Pull requests (or merge requests) can be more complex in a monorepo because they often involve changes across multiple projects. This requires thorough testing and code review processes.
