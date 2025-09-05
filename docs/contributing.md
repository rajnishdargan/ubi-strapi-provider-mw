# Contributing Guidelines

Guidelines for contributing to the UBI Strapi Provider Middleware project.

## Development Workflow

### Getting Started
Before contributing, ensure you have followed the [Setup Guide](setup.md) to get the project running locally.

### Branching Strategy
- Use descriptive feature branches for new work
- Base your branch off `main`

### Commit Messages
- Use clear, descriptive commit messages
- Follow conventional commit format when possible

### Code Style
- Follow existing code style and patterns
- The project uses ESLint and Prettier for consistency

### Pull Requests
- Target the `main` branch
- Provide clear description of changes
- Reference related issues if applicable

## Making Changes

### Backend (NestJS)
- Follow NestJS best practices (modules, controllers, services, DTOs)
- Ensure proper error handling and validation
- Document complex logic with comments

### Database (Prisma)
- If schema changes are needed, update `prisma/schema.prisma`
- Generate and apply new migrations: `npx prisma migrate dev --name your_migration_name`
- Avoid direct database modifications in production

### Environment Variables
- If new environment variables are introduced, add them to `env.sample` with descriptions
- Update `docs/configuration/environment-variables.md` with the new variable details

## Code Quality

### Standards
- Maintain TypeScript type safety
- Follow existing patterns and conventions
- Test changes before submitting

## Review Process

- Review your own code before submitting
- Address feedback from reviewers
- Ensure changes work as expected

## Need Help?

- Refer to the [Setup Guide](setup.md) for local development issues
- Check [Environment Variables](configuration/environment-variables.md) for configuration problems
- Consult the [Prisma Documentation](https://www.prisma.io/docs) for database-related questions