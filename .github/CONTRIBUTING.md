# Contributing to HQ Operations

Thank you for your interest in contributing to the HQ Operations system! This document provides guidelines and instructions for contributing.

## Code of Conduct

This project adheres to a Code of Conduct. By participating, you are expected to uphold this code.

## Getting Started

1. Fork the repository
2. Clone your fork locally
3. Create a feature branch
4. Make your changes
5. Submit a pull request

## Development Setup

```bash
pnpm install
cp .env.example .env.local
pnpm db:migrate
pnpm dev
```

## Commit Messages

We follow [Conventional Commits](https://www.conventionalcommits.org/):

- `feat:` A new feature
- `fix:` A bug fix
- `docs:` Documentation only changes
- `style:` Changes that don't affect code meaning
- `refactor:` Code change that neither fixes a bug nor adds a feature
- `perf:` Code change that improves performance
- `test:` Adding missing tests or correcting existing tests
- `chore:` Changes to build process, dependencies, etc.

Example:
```
feat: add real-time notifications to battle board
```

## Pull Request Process

1. Update relevant documentation
2. Add tests for new functionality
3. Ensure all tests pass: `pnpm test`
4. Ensure linting passes: `pnpm lint`
5. Create a clear PR description
6. Link related issues
7. Wait for review and address feedback

## Code Style

- Use TypeScript
- Follow ESLint rules
- Format code with Prettier
- Use meaningful variable names
- Add comments for complex logic

```bash
pnpm lint --fix
pnpm format
```

## Testing

Write tests for new features:

```bash
pnpm test
pnpm test --watch
pnpm test --coverage
```

## Documentation

- Update README if adding features
- Document complex logic
- Update CHANGELOG
- Add JSDoc comments to public APIs

## Questions?

Feel free to open an issue or discussion for questions.
