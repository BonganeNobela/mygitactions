# Contributing

We welcome contributions to GlobeTalk! Follow these guidelines to contribute effectively.

## Branching Strategy

- **Main Branch (`main`)**: Stable, production-ready code.
- **Testing Branch (`testing`)**: For integrating and validating features.
- **Feature Branches (`feature/xyz`)**: For new features or bug fixes.

## Workflow

1. **Create a Feature Branch**
   - Name: `feature/<short-description>`
   - Base: `testing`
   - Example: `git checkout -b feature/add-profile-endpoint testing`

2. **Implement and Commit**
   - Use clear commit messages: `type(scope): message`
   - Example: `feat(api): add profile update endpoint`

3. **Push to GitHub**
   - Push your feature branch: `git push origin feature/add-profile-endpoint`

4. **Create a Pull Request (PR)**
   - Base: `testing`
   - Compare: Your feature branch
   - Include a descriptive title and testing notes.

5. **Code Review**
   - At least one reviewer must approve.
   - Reviewers check code quality, functionality, and no breaking changes.

6. **Testing**
   - Run tests locally: `npm test`
   - Ensure 80% code coverage with Jest.
   - CI pipeline runs unit and integration tests.

7. **Merging**
   - Merge `feature` to `testing` after approval and CI checks.
   - Merge `testing` to `main` after validation.
   - Delete merged feature branches.

## Scrum Methodology

GlobeTalk uses Scrum for agile development across four sprints:

- **Sprint Planning**: Select features and tasks for each sprint.
- **Daily Stand-ups**: Discuss progress and blockers (15 minutes daily).
- **Sprint Review**: Demonstrate features to stakeholders.
- **Sprint Retrospective**: Reflect on what worked and what to improve.

### Team Roles

- **Product Owner**: Shaneel Kassen (defines vision, prioritizes features)
- **Scrum Master**: Paballo Lipopo (facilitates events, removes obstacles)
- **Development Team**: Nfundwenhle Ezra Nkontwana, Kuhlula Mambane, Tiyani Mabunda, Bongani Nobela, Paballo Lipopo

## Getting Started

1. Fork the repository.
2. Clone your fork: `git clone https://github.com/your-username/globetalk.git`
3. Follow [Installation](./installation.md) to set up the project.
4. Create a feature branch and start coding!

## Code Style

- Use ESLint for JavaScript: `npx eslint .`
- Follow Firebase best practices for Functions and Firestore.

## Feedback

Report issues or suggest improvements via [GitHub Issues](https://github.com/example/globetalk/issues).