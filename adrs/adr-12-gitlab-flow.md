# ADR 12: Use GitLab Flow Merge strategy

**Date**: 2023-09-15

## Status
Proposed 

## Context
Choosing a code merge strategy is important because it directly impacts the development process, code quality, collaboration among team members, and the overall success of a software project. A well-chosen merge strategy ensures that code changes are integrated smoothly, reducing conflicts, errors, and potential disruptions.

## Decision
Due to constraints on our mobile app release schedule, we've opted to implement **[GitLab Flow](development_viewpoint/README.md#gitlab-flow)** for both frontend and backend development. This choice aims to streamline joint releases and simplify our overall workflow.

## Consequences
 
### Positive
- **Simplicity**: GitLab Flow is designed to be straightforward and easy to understand. It doesn't have as many branches and complex rules as GitFlow, making it accessible to developers of varying experience levels.

- **Frequent Releases**: By merging code into the main branch more frequently, GitLab Flow can support a release early and often approach, allowing you to deliver new features and bug fixes to users more quickly.

- **Alignment with DevOps Practices**: GitLab Flow aligns with DevOps principles by emphasizing automation, collaboration, and continuous improvement. This can help teams adopt a DevOps culture more easily.

### Negative
- **Risk of Integration Issues**: Frequent merges into the main branch can increase the risk of integration issues. It's crucial to have a robust CI/CD pipeline and automated testing in place to catch problems early.

- **Less Granular Control**: Some teams may require more granular control over their release process, which GitLab Flow does not provide by default. You may need to supplement it with additional practices or tools for release management.

- **Scalability**: While GitLab Flow can work well for small to medium-sized teams and projects, larger projects may require additional workflows and branching strategies to maintain scalability.
