# Git collaboration with Azure DevOps

Modern software development demands robust version control practices, automated workflows, and stringent security measures. Azure DevOps provides a comprehensive platform that combines enterprise-grade security with powerful collaboration features, making it an ideal choice for teams seeking to implement safe and secure development practices. This article presents a complete workflow for Git collaboration using Azure DevOps, incorporating industry best practices for branching strategies, continuous integration, and deployment automation.

## Azure DevOps as the foundation for secure code storage

Azure DevOps represents Microsoft's mature, enterprise-focused DevOps platform that excels in secure code storage through its comprehensive security architecture and deep ecosystem integration. The platform provides **unlimited private Git repositories** with enterprise-grade protection features that set it apart from competitors.

The security foundation of Azure DevOps builds on multiple layers of protection. All data receives **encryption at rest** using Azure Blob Storage service-side encryption and Azure SQL Database transparent data encryption. Communication channels employ **HTTPS/TLS encryption** for all data in transit. The platform maintains **geographic data residency** across 8 global regions with geo-replication, ensuring both availability and compliance with regional data sovereignty requirements.

What distinguishes Azure DevOps from GitHub, GitLab, and Bitbucket is its **integrated DevOps approach** that combines source control, work management, CI/CD pipelines, and testing in a unified platform. While GitHub excels at community collaboration and GitLab offers strong CI/CD capabilities, Azure DevOps provides superior enterprise features with native Azure cloud integration and comprehensive work item tracking through Azure Boards. The platform's **99.9% SLA guarantee** with financial backing ensures reliability for mission-critical applications.

## The main-staging branching strategy

The branching strategy forms the backbone of safe collaboration, establishing clear pathways for code to flow from development through production. The recommended approach employs a **three-environment pattern** that balances simplicity with enterprise requirements for quality assurance and deployment control.

The **main branch** serves as the single source of truth for production code. This branch remains protected at all times, accepting changes only through approved pull requests. Direct commits are prohibited, ensuring every change undergoes proper review and testing. The main branch reflects exactly what runs in production, providing a reliable rollback point if issues arise.

The **staging branch** acts as the pre-production environment where integration happens. All development work merges first into staging, where comprehensive testing validates changes before production deployment. This branch represents the next version of the application, accumulating features and fixes that will eventually promote to main. Staging provides a buffer that prevents unstable code from reaching production while maintaining a realistic testing environment.

Developers create **working branches** from staging for all development activities. These branches follow strict naming conventions that enhance traceability and organization. Feature branches use the pattern `feature/<ado_card_number_and_title>`, such as `feature/452-user-authentication`, clearly indicating both the work type and the associated Azure DevOps work item. For critical production issues requiring immediate attention, `hotfix/<ado_card_number_and_title>` branches created from main allow rapid deployment while maintaining the review process.

This naming structure provides multiple benefits: automatic linking between code changes and work items, clear visual distinction between different work types in branch listings, simplified automation rules based on branch prefixes, and improved team communication about ongoing work. The inclusion of ADO card numbers creates bidirectional traceability, enabling developers to navigate from code to requirements and back.

## Daily commits and the rebase workflow

Security through regular backups forms a critical component of the development workflow. The requirement for **daily commits** ensures that no developer loses more than 24 hours of work if hardware fails or other disasters occur. This practice transcends simple backup functionality—it promotes incremental development, enables early problem detection, and maintains team synchronization.

Developers should start each day by pulling the latest changes from staging: `git pull origin staging`. Throughout the day, they commit completed work units with meaningful messages that reference the ADO card: `git commit -m "#452 Implement user authentication middleware"`. At day's end, they push their working branch to the remote repository: `git push origin feature/452-user-authentication`. This rhythm creates natural checkpoints that facilitate code reviews and collaboration.

The **git rebase workflow** maintains a clean, linear project history that simplifies debugging and code archaeology. Before creating a pull request, developers must update their branch with the latest staging changes using rebase instead of merge. The process begins with fetching the latest staging branch: `git fetch origin staging`. Then, the developer rebases their working branch: `git rebase origin/staging`.

During rebase, Git replays each commit from the working branch on top of the updated staging branch. If conflicts arise, Git pauses the rebase, allowing developers to resolve conflicts commit by commit. This granular approach ensures each commit remains functional and maintains the logical progression of changes. After resolving conflicts, developers continue the rebase: `git rebase --continue`.

**Interactive rebase** provides powerful history-cleaning capabilities before pull request creation. Using `git rebase -i HEAD~n` where n represents the number of commits to review, developers can squash related commits, reword commit messages, and remove unnecessary changes. The interactive editor presents options including `pick` to keep commits as-is, `squash` to combine with the previous commit while preserving messages, and `fixup` to combine while discarding the message.

Best practice suggests squashing work-in-progress commits while maintaining logical separation between distinct changes. For example, commits implementing a feature might squash together, but the bug fix discovered during implementation should remain separate. This approach creates a readable history that tells the story of the code evolution without overwhelming reviewers with implementation details.

## Pull request workflow and approval requirements

Pull requests serve as the quality gate between individual development and shared code. The workflow begins when developers push their updated, rebased branch and create a pull request targeting the staging branch. The pull request title should be descriptive yet concise: "Add OAuth2 authentication for API endpoints #452". The description must include the ADO card link, a summary of changes, testing performed, and any deployment considerations.

Azure DevOps enforces **branch policies** that prevent bypassing the review process. The staging and main branches require a minimum of two approvals before merging, with at least one approval from a senior developer. The platform tracks reviewer expertise, suggesting appropriate reviewers based on code ownership and past contributions. All build validations must pass before merge becomes available, ensuring that proposed changes don't break existing functionality.

The code review process emphasizes constructive feedback and knowledge sharing. Reviewers examine code for correctness, adherence to team standards, security considerations, and architectural alignment. Comments should be specific and actionable, with suggestions for improvement rather than mere criticism. The **comment resolution requirement** ensures all feedback receives attention before merge.

Research shows that pull requests under **200-400 lines of code** receive more thorough reviews and ship 40% faster than larger changes. Teams should decompose large features into smaller, reviewable chunks that can merge independently. This practice reduces cognitive load on reviewers, decreases the likelihood of bugs, and accelerates the feedback cycle.

When reviewers request changes, developers address feedback through additional commits to their working branch. These commits trigger automatic re-execution of build validations and notify reviewers of updates. Once all policies are satisfied and approvals secured, the pull request merges using a **squash merge strategy** that combines all commits into a single, clean commit in the staging branch.

## ADO card state management

Work items in Azure DevOps progress through a carefully orchestrated series of states that reflect the development lifecycle. This state management provides visibility into work progress and enables automation of routine tasks. The journey begins when a work item enters the **"Ready for development"** state, indicating that requirements are clear, acceptance criteria defined, and dependencies resolved.

When developers begin work, they transition the card to **"In development"**. This state change can trigger automated actions such as branch creation, capacity updates, and stakeholder notifications. The Azure DevOps integration automatically links commits and branches to the work item when developers include the card number in branch names and commit messages.

Upon completing initial development, the developer moves the card to **"In review"**, signaling readiness for peer feedback. This informal review happens before the formal pull request, allowing early detection of issues. Team members provide feedback on approach, identify potential problems, and suggest improvements. This practice reduces rework and improves final code quality.

After incorporating initial feedback, the state advances to **"Ready for PR"**. The developer creates the pull request, linking it to the work item through the Azure DevOps UI or by including `#452` in the pull request description. The work item's Development section now shows the associated pull request, providing complete traceability from requirement to implementation.

The **"In final review"** state indicates that the pull request is under formal review. Senior developers and technical leads examine the code for architectural compliance, security considerations, and production readiness. The work item remains in this state until all pull request policies are satisfied and approvals obtained.

Successful merge to staging triggers transition to **"On staging"**. Automated pipelines deploy the code to the staging environment where integration testing begins. The card's state provides clear communication about where code resides in the deployment pipeline, enabling coordination between development and QA teams. Testers execute test cases, perform exploratory testing, and validate acceptance criteria. Any issues discovered create new bug work items linked to the original card, maintaining relationship tracking. Successful QA validation moves the card to **"Go live"**, indicating production deployment readiness.

The final transition to **"Live"** occurs when code deploys to production. This state change can trigger automated actions including release note generation, stakeholder notifications, and metric collection. The complete state history provides an audit trail of the work item's journey from concept to production.

## Testing requirements and automation

Every branch must maintain comprehensive test coverage to ensure code quality and prevent regressions. The testing strategy encompasses multiple layers, each serving a specific purpose in the quality assurance process. **Unit tests** form the foundation, validating individual components in isolation. These tests must achieve **minimum 80% code coverage**, with critical business logic requiring 95% or higher coverage.

Integration tests validate component interactions, ensuring that modules work correctly together. These tests run against the staging branch after each merge, catching integration issues before they affect other developers. End-to-end tests simulate user workflows, validating complete feature functionality from the user interface through to data persistence.

Security testing integrates directly into the development workflow through **static application security testing (SAST)** that analyzes code for vulnerabilities during the build process. The GitHub Advanced Security for Azure DevOps integration provides secret scanning for over 200 token types, dependency scanning for vulnerable components, and CodeQL analysis for security vulnerabilities. **Dynamic application security testing (DAST)** tools like OWASP ZAP scan running applications for runtime vulnerabilities.

Test execution occurs at multiple points in the development lifecycle. Local pre-commit hooks run unit tests for modified code, preventing broken commits. Pull request validations execute the complete test suite, blocking merge if tests fail. Deployment pipelines run environment-specific tests, with smoke tests validating basic functionality and comprehensive regression suites ensuring system stability.

## Azure Pipelines and automated testing

Azure Pipelines orchestrates the continuous integration and deployment process through YAML-defined workflows stored alongside application code. The pipeline configuration begins with trigger definitions that specify when builds execute.

The build stage compiles code, runs tests, and produces deployment artifacts. Modern pipelines use **multi-stage YAML** that defines the entire deployment lifecycle in a single file.

Security scanning integrates seamlessly into the pipeline through the Microsoft Security DevOps task.

For pull request validation, **branch policies** configure build validation requirements. Every pull request targeting staging or main must pass all pipeline stages before merge becomes available. Failed builds block merge, requiring developers to fix issues before proceeding.

## Preview environments and deployment

Pull requests trigger creation of **temporary preview environments** that enable stakeholders to review changes in a running application. These ephemeral environments provision automatically using Azure's infrastructure-as-code capabilities.

The preview environment receives a **temporary domain** following the pattern `https://pr-452-appname.azurewebsites.net`. This URL appears as a comment in the pull request, enabling reviewers to test functionality directly. Preview environments include test data and integrate with non-production versions of external services, providing a realistic but safe testing environment.

Azure App Service **deployment slots** facilitate zero-downtime deployments to staging and production. The staging slot receives deployments first, allowing final validation before swapping with production.

The **swap operation** promotes staging to production with zero downtime. Azure performs a warm-up of the staging slot, then swaps the routing rules to direct traffic to the new version. The previous production version remains in the staging slot, enabling instant rollback if issues arise.

## Secrets management

**Azure Key Vault** centralizes secrets management, eliminating hardcoded credentials from code and configuration. Pipeline integration retrieves secrets at runtime.

Managed identities eliminate password management for service-to-service authentication. Applications use their assigned identity to access Azure resources, with permissions managed through role-based access control. This approach improves security by eliminating credential storage and enabling fine-grained access control.

Key Vault references in application configuration automatically retrieve secrets at runtime. Connection strings, API keys, and certificates remain secure while being easily rotatable without code changes. Access policies restrict secret access to specific identities, with audit logs tracking all access attempts.

## Security considerations and compliance

Security integrates throughout the development lifecycle rather than being bolted on at the end. **Pre-commit hooks** prevent sensitive data from entering the repository.

Branch protection rules enforce security requirements before code reaches production. The main branch requires successful security scans, approved pull requests, and up-to-date status checks. Administrator override requires additional approval and generates audit events.

Compliance policies enforce organizational standards through Azure Policy integration. These policies prevent deployment of non-compliant resources, enforce encryption requirements, and ensure proper tagging. Policy violations block deployments and create compliance work items for remediation.

## Best practices for team adoption

Successful adoption of this Git collaboration workflow requires careful planning and gradual implementation. Teams should begin with basic branching and pull request requirements, adding automation and advanced features as comfort grows. Initial focus should be on establishing consistent naming conventions, implementing code review processes, and ensuring reliable test coverage.

Documentation must be comprehensive yet accessible. README files should explain project setup, development workflows, and deployment processes. Wiki pages detail architectural decisions, coding standards, and troubleshooting guides. Process documentation captures team agreements and evolves based on feedback.

## Conclusion

Safe and secure Git collaboration through Azure DevOps combines robust version control practices with comprehensive automation and security measures. The main-staging branching strategy provides clear code promotion paths while maintaining production stability. Daily commits with rebase workflows create clean history while ensuring regular backups. Pull request reviews with strict approval requirements maintain code quality and share knowledge across the team.

Azure DevOps work item states provide visibility into development progress and enable process automation. Comprehensive testing at every level prevents defects from reaching production. Azure Pipelines orchestrate the entire deployment lifecycle with security scanning and compliance validation integrated throughout. Preview environments enable thorough validation while deployment slots ensure zero-downtime releases.

This workflow scales from small teams to large enterprises, adapting to varying complexity and compliance requirements. The combination of process, automation, and tooling creates an environment where developers can work confidently, knowing that multiple safety nets prevent mistakes from impacting users. By following these practices, teams deliver high-quality software rapidly while maintaining security and compliance standards.

The journey to DevOps excellence is iterative. Teams should implement these practices gradually, measuring impact and adjusting based on experience. With commitment to continuous improvement and proper tool support, organizations can achieve the seemingly contradictory goals of increased velocity and improved quality. Azure DevOps provides the platform; this workflow provides the path.
