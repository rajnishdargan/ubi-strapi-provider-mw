# Contributing Guidelines

Guidelines for contributing to the project.

## Getting Started

### Setup
```bash
# Fork and clone
git clone https://github.com/your-username/ubi-strapi-provider-mw.git
cd ubi-strapi-provider-mw

# Install dependencies
npm install

# Setup pre-commit hooks
npm run prepare
```

## Code Standards

### Branch Naming
```bash
# Features
feature/add-benefit-search
feature/file-upload

# Bug fixes
fix/auth-error
fix/database-issue

# Documentation
docs/update-api-guide
```

### Commit Messages
Use clear, descriptive commit messages:

```bash
feat: add benefit search endpoint
fix: resolve JWT validation error
docs: update API documentation
refactor: clean up file upload logic
```

### Code Style
```typescript
// Use meaningful names
const applicationData = await this.getApplication(id);

// Use async/await
async function processApplication(data: ApplicationData): Promise<Result> {
  try {
    const result = await this.validateData(data);
    return result;
  } catch (error) {
    this.logger.error('Processing failed', error);
    throw error;
  }
}

// Use proper types
interface ApplicationRequest {
  benefitId: string;
  applicantData: ApplicantData;
}
```

## Pull Request Process

### Before Creating PR
1. Sync with main: `git pull origin main`
2. Run linting: `npm run lint`
3. Test locally: Verify functionality works

### PR Guidelines
- Keep PRs small and focused
- Write clear descriptions
- Update documentation if needed
- Test your changes

## Database Changes

### Schema Updates
```bash
# 1. Modify prisma/schema.prisma
# 2. Create migration
npx prisma migrate dev --name descriptive_name

# 3. Test migration
npx prisma migrate reset
```

## Development Commands

```bash
# Development
npm run start:dev        # Start dev server

# Database
npx prisma generate     # Generate client
npx prisma migrate dev  # Run migrations
npx prisma studio       # Database browser
```

## File Structure

```bash
src/
├── applications/       # Application management
├── benefits/          # ONDC benefits
├── auth/             # Authentication
├── common/           # Shared utilities
└── main.ts          # App entry point
```

## Security Guidelines

- Never commit secrets
- Validate all user input
- Use environment variables for config
- Encrypt sensitive data

## Getting Help

- Check existing documentation first
- Search GitHub issues
- Ask questions in pull request comments

## Quick Reference

```bash
# Common workflow
git checkout -b feature/my-feature
# Make changes
npm run lint
npm run format
git add .
git commit -m "feat: add feature"
git push origin feature/my-feature
# Create PR
```