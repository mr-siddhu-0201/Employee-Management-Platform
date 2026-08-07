# Contributing to Employee Management Full-Stack Application

Thank you for considering contributing to this project! We welcome contributions from the community. This document provides guidelines and instructions on how to contribute.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [How to Contribute](#how-to-contribute)
- [Reporting Bugs](#reporting-bugs)
- [Suggesting Enhancements](#suggesting-enhancements)
- [Pull Request Process](#pull-request-process)
- [Development Setup](#development-setup)
- [Coding Standards](#coding-standards)
- [Commit Messages](#commit-messages)

## Code of Conduct

This project adheres to the Contributor Covenant code of conduct. By participating, you are expected to uphold this code. See [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md) for details.

## How to Contribute

### Reporting Bugs

Before creating bug reports, please check the issue list to avoid duplicates. When creating a bug report, include:

- **Title**: A clear, descriptive title
- **Description**: A clear description of what the bug is
- **Steps to Reproduce**: Detailed steps to reproduce the behavior
- **Expected Behavior**: What you expected to happen
- **Actual Behavior**: What actually happened
- **Screenshots**: If applicable
- **Environment**: Your OS, browser, Node.js version, Java version, etc.

### Suggesting Enhancements

Enhancement suggestions are always welcome. When creating an enhancement suggestion, include:

- **Title**: A clear, descriptive title
- **Description**: A detailed description of the suggested enhancement
- **Current Behavior**: Describe the current behavior
- **Proposed Behavior**: Describe the expected behavior
- **Rationale**: Explain why this enhancement would be useful
- **Alternatives**: Describe any alternative solutions

## Pull Request Process

1. **Fork the Repository**: Click the "Fork" button on GitHub to create your own copy
2. **Clone Your Fork**: Clone your forked repository locally
3. **Create a Branch**: Create a new branch for your feature or bug fix
   ```bash
   git checkout -b feature/your-feature-name
   ```
4. **Make Changes**: Make your changes to the codebase
5. **Follow Coding Standards**: Ensure your code follows the project's coding standards (see below)
6. **Test Your Changes**: Thoroughly test your changes
7. **Commit**: Commit your changes with a clear commit message (see Commit Messages below)
8. **Push**: Push your changes to your forked repository
9. **Create Pull Request**: Create a pull request from your branch to the main repository
10. **Respond to Reviews**: Address any feedback from reviewers

## Development Setup

### Backend Setup

1. Clone the repository
   ```bash
   git clone https://github.com/TEJA6777/WorkHub-Modern-Employee-Management-Platform.git
   cd WorkHub-Modern-Employee-Management-Platform/backend
   ```

2. Ensure Java 25 and Maven are installed

3. Install dependencies
   ```bash
   mvn install
   ```

4. Configure the application (see README.md for details)

5. Start the development server
   ```bash
   mvn spring-boot:run
   ```

### Frontend Setup

1. Navigate to frontend directory
   ```bash
   cd WorkHub-Modern-Employee-Management-Platform/frontend
   ```

2. Ensure Node.js and npm are installed

3. Install dependencies
   ```bash
   npm install
   ```

4. Set up environment variables (see .env.example)

5. Start the development server
   ```bash
   npm start
   ```

## Coding Standards

### Java/Backend

- Follow the [Google Java Style Guide](https://google.github.io/styleguide/javaguide.html)
- Use meaningful variable and method names
- Add Javadoc comments for public methods and classes
- Keep methods focused and reasonably sized
- Use proper exception handling

### JavaScript/Frontend

- Follow the [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript)
- Use meaningful variable and function names
- Add comments for complex logic
- Use ESLint for code quality
- Format code using Prettier

## Commit Messages

Please follow these guidelines for commit messages:

- Use the imperative mood ("add feature" not "added feature")
- Limit the first line to 72 characters
- Reference issues and pull requests liberally after the first line
- Use a blank line to separate the subject from the body
- Explain what and why, not how

**Example:**
```
Add user authentication feature

- Implement JWT-based authentication
- Add login and logout endpoints
- Add authentication middleware

Fixes #123
```

## Testing

- Write tests for new features
- Ensure all existing tests pass
- Maintain or improve code coverage
- Test edge cases and error scenarios

### Running Tests

**Backend:**
```bash
mvn test
```

**Frontend:**
```bash
npm test
```

## Documentation

- Update README.md if needed
- Add comments to complex code sections
- Update .env.example if adding new environment variables
- Keep documentation up to date

## License

By contributing to this project, you agree that your contributions will be licensed under the MIT License.

## Questions?

Feel free to open an issue or contact the maintainers at https://github.com/TEJA6777

---

Thank you for contributing!
