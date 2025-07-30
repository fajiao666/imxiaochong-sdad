# Contributing to imxiaochong-sdad

Thank you for your interest in contributing! This guide will help you get started and ensure your contributions align with our project standards.

## 🚀 Quick Start

1. **Fork the repository** on GitHub
2. **Clone your fork** locally
3. **Set up the development environment** (see [Development Environment](DEV_ENVIRONMENT.md))
4. **Create a feature branch** from `main`
5. **Make your changes** following our guidelines
6. **Test thoroughly** and ensure all checks pass
7. **Submit a pull request** with a clear description

## 📋 Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [Development Workflow](#development-workflow)
- [Coding Standards](#coding-standards)
- [Testing Requirements](#testing-requirements)
- [Documentation Guidelines](#documentation-guidelines)
- [Pull Request Process](#pull-request-process)
- [Issue Reporting](#issue-reporting)
- [Community Guidelines](#community-guidelines)

## 📜 Code of Conduct

This project follows the [Contributor Covenant](https://www.contributor-covenant.org/) Code of Conduct. By participating, you agree to uphold this code. Please report unacceptable behavior to [conduct@imxiaochong-sdad.com](mailto:conduct@imxiaochong-sdad.com).

### Our Standards

**Positive behavior includes:**
- Using welcoming and inclusive language
- Being respectful of differing viewpoints
- Gracefully accepting constructive criticism
- Focusing on what's best for the community
- Showing empathy towards other community members

**Unacceptable behavior includes:**
- Trolling, insulting/derogatory comments, and personal attacks
- Public or private harassment
- Publishing others' private information without explicit permission
- Other conduct which could reasonably be considered inappropriate

## 🛠️ Getting Started

### Prerequisites

- **Node.js** v16+ and npm/yarn/pnpm
- **Git** 2.20+
- **Code editor** with TypeScript support (VS Code recommended)
- **Docker** (optional, for containerized development)

### Development Setup

```bash
# 1. Fork and clone the repository
git clone https://github.com/YOUR_USERNAME/imxiaochong-sdad.git
cd imxiaochong-sdad

# 2. Install dependencies
npm install

# 3. Set up environment variables
cp .env.example .env.local
# Edit .env.local with your configuration

# 4. Start development server
npm run dev

# 5. Run tests
npm test

# 6. Build the project
npm run build
```

### Project Structure

```
imxiaochong-sdad/
├── src/                    # Source code
│   ├── components/         # React components
│   ├── hooks/             # Custom React hooks
│   ├── utils/             # Utility functions
│   ├── api/               # API layer
│   └── types/             # TypeScript definitions
├── docs/                  # Documentation
├── tests/                 # Test files
├── public/                # Static assets
└── scripts/               # Build and deployment scripts
```

## 🔄 Development Workflow

### Branching Strategy

We use **Git Flow** with the following branch types:

- `main` - Production-ready code
- `develop` - Integration branch for features
- `feature/*` - New features or enhancements
- `bugfix/*` - Bug fixes
- `hotfix/*` - Critical production fixes
- `release/*` - Release preparation

### Branch Naming Convention

```
feature/add-user-authentication
bugfix/fix-login-validation
hotfix/security-vulnerability
release/v2.1.0
```

### Commit Message Guidelines

We follow [Conventional Commits](https://www.conventionalcommits.org/):

```
type(scope): description

[optional body]

[optional footer(s)]
```

**Types:**
- `feat:` - New feature
- `fix:` - Bug fix
- `docs:` - Documentation changes
- `style:` - Code style changes (no logic changes)
- `refactor:` - Code refactoring
- `test:` - Adding or updating tests
- `chore:` - Maintenance tasks

**Examples:**
```
feat(auth): add social login integration

fix(api): resolve timeout issue in user endpoint

docs(readme): update installation instructions

test(components): add unit tests for Button component
```

## 📏 Coding Standards

### TypeScript Guidelines

```typescript
// Use explicit types for function parameters and return values
function formatUser(user: User): FormattedUser {
  return {
    id: user.id,
    name: user.name,
    email: user.email
  };
}

// Use interfaces for object shapes
interface User {
  id: string;
  name: string;
  email: string;
  createdAt: Date;
}

// Use enums for constants
enum UserRole {
  ADMIN = 'admin',
  USER = 'user',
  MODERATOR = 'moderator'
}
```

### React Component Guidelines

```tsx
// Use functional components with TypeScript
interface ButtonProps {
  variant?: 'primary' | 'secondary';
  size?: 'sm' | 'md' | 'lg';
  disabled?: boolean;
  onClick?: (event: React.MouseEvent) => void;
  children: React.ReactNode;
}

export function Button({ 
  variant = 'primary', 
  size = 'md', 
  disabled = false,
  onClick,
  children 
}: ButtonProps) {
  return (
    <button
      className={`btn btn--${variant} btn--${size}`}
      disabled={disabled}
      onClick={onClick}
    >
      {children}
    </button>
  );
}
```

### Code Style

- **Formatting**: Use Prettier (configured in `.prettierrc`)
- **Linting**: Use ESLint (configured in `.eslintrc.js`)
- **File naming**: Use kebab-case for files (`user-profile.tsx`)
- **Component naming**: Use PascalCase for components (`UserProfile`)
- **Function naming**: Use camelCase for functions (`getUserProfile`)

### Performance Guidelines

- Use `React.memo()` for expensive components
- Implement `useCallback()` and `useMemo()` appropriately
- Lazy load components with `React.lazy()`
- Optimize bundle size with tree shaking
- Use proper key props in lists

## 🧪 Testing Requirements

### Testing Strategy

All contributions must include appropriate tests:

1. **Unit tests** for utility functions
2. **Component tests** for React components
3. **Integration tests** for API endpoints
4. **E2E tests** for critical user flows

### Testing Guidelines

```javascript
// Unit test example
import { formatDate } from '../utils/date';

describe('formatDate', () => {
  test('formats date correctly', () => {
    const date = new Date('2024-01-15');
    expect(formatDate(date, 'YYYY-MM-DD')).toBe('2024-01-15');
  });

  test('handles invalid dates', () => {
    expect(() => formatDate('invalid', 'YYYY-MM-DD')).toThrow();
  });
});
```

```javascript
// Component test example
import { render, screen, fireEvent } from '@testing-library/react';
import { Button } from '../components/Button';

describe('Button', () => {
  test('renders correctly', () => {
    render(<Button>Click me</Button>);
    expect(screen.getByRole('button')).toBeInTheDocument();
  });

  test('calls onClick when clicked', () => {
    const handleClick = jest.fn();
    render(<Button onClick={handleClick}>Click me</Button>);
    
    fireEvent.click(screen.getByRole('button'));
    expect(handleClick).toHaveBeenCalledTimes(1);
  });
});
```

### Test Coverage Requirements

- **Minimum coverage**: 80% for new code
- **Critical paths**: 100% coverage required
- **Component props**: Test all prop variations
- **Error cases**: Test error handling paths

### Running Tests

```bash
# Run all tests
npm test

# Run tests in watch mode
npm run test:watch

# Run tests with coverage
npm run test:coverage

# Run E2E tests
npm run test:e2e
```

## 📚 Documentation Guidelines

### Code Documentation

```typescript
/**
 * Formats a date according to the specified format string
 * @param date - The date to format
 * @param format - The format string (e.g., 'YYYY-MM-DD')
 * @param options - Additional formatting options
 * @returns Formatted date string
 * @throws {Error} When date is invalid
 * @example
 * ```typescript
 * formatDate(new Date(), 'YYYY-MM-DD')
 * // Returns: "2024-01-15"
 * ```
 */
export function formatDate(
  date: Date,
  format: string,
  options?: FormatOptions
): string {
  // Implementation
}
```

### API Documentation

- Document all endpoints with OpenAPI/Swagger
- Include request/response examples
- Document error responses
- Provide authentication details

### Component Documentation

- Document all props with TypeScript interfaces
- Provide usage examples
- Include accessibility information
- Document styling options

## 🔍 Pull Request Process

### Before Submitting

- [ ] Code follows our style guidelines
- [ ] All tests pass
- [ ] New code has appropriate test coverage
- [ ] Documentation is updated
- [ ] No lint errors
- [ ] Build succeeds
- [ ] Self-review completed

### PR Description Template

```markdown
## Description
Brief description of the changes

## Type of Change
- [ ] Bug fix (non-breaking change that fixes an issue)
- [ ] New feature (non-breaking change that adds functionality)
- [ ] Breaking change (fix or feature that would cause existing functionality to not work as expected)
- [ ] Documentation update

## Testing
- [ ] Unit tests added/updated
- [ ] Integration tests added/updated
- [ ] Manual testing completed

## Screenshots (if applicable)

## Checklist
- [ ] My code follows the style guidelines
- [ ] I have performed a self-review
- [ ] I have commented my code, particularly in hard-to-understand areas
- [ ] I have made corresponding changes to the documentation
- [ ] My changes generate no new warnings
- [ ] I have added tests that prove my fix is effective or that my feature works
- [ ] New and existing unit tests pass locally with my changes
```

### Review Process

1. **Automated checks** must pass (CI/CD pipeline)
2. **Code review** by at least one maintainer
3. **Manual testing** if UI changes are involved
4. **Documentation review** for public API changes
5. **Security review** for security-related changes

### Review Criteria

**Code Quality:**
- Follows coding standards
- Properly structured and readable
- Includes appropriate error handling
- Performance considerations addressed

**Testing:**
- Adequate test coverage
- Tests are meaningful and cover edge cases
- All tests pass

**Documentation:**
- Code is well-commented
- Public APIs are documented
- Breaking changes are noted

## 🐛 Issue Reporting

### Bug Reports

Use the bug report template and include:

- **Environment details** (OS, browser, version)
- **Steps to reproduce** the issue
- **Expected behavior**
- **Actual behavior**
- **Screenshots** or error messages
- **Additional context**

### Feature Requests

Use the feature request template and include:

- **Problem description** you're trying to solve
- **Proposed solution**
- **Alternative solutions** considered
- **Use cases** and examples
- **Implementation suggestions** (optional)

### Security Issues

**Do not** open public issues for security vulnerabilities. Instead:

1. Email [security@imxiaochong-sdad.com](mailto:security@imxiaochong-sdad.com)
2. Include detailed vulnerability description
3. Provide steps to reproduce
4. Suggest potential fixes if known

## 🏆 Recognition

### Contributors

We recognize contributors in:

- **README.md** contributors section
- **CHANGELOG.md** release notes
- **GitHub releases** contributor highlights
- **Blog posts** for significant contributions

### Contribution Types

We value all types of contributions:

- **Code contributions** (features, bug fixes)
- **Documentation improvements**
- **Bug reports and testing**
- **Community support** and answering questions
- **Design and UX feedback**
- **Translations** and internationalization

## 🤝 Community Guidelines

### Getting Help

- **GitHub Discussions** for questions and ideas
- **Discord Server** for real-time chat
- **Stack Overflow** with `imxiaochong-sdad` tag
- **Documentation** for comprehensive guides

### Communication

- Be respectful and constructive
- Provide context and examples
- Search existing issues before creating new ones
- Use clear and descriptive titles
- Follow up on your contributions

### Best Practices

- Start with small contributions
- Ask questions if you're unsure
- Read existing code to understand patterns
- Follow the established conventions
- Be patient with the review process

## 📞 Contact

- **General questions**: [community@imxiaochong-sdad.com](mailto:community@imxiaochong-sdad.com)
- **Technical issues**: [GitHub Issues](https://github.com/imxiaochong-sdad/issues)
- **Security concerns**: [security@imxiaochong-sdad.com](mailto:security@imxiaochong-sdad.com)
- **Code of conduct**: [conduct@imxiaochong-sdad.com](mailto:conduct@imxiaochong-sdad.com)

---

Thank you for contributing to imxiaochong-sdad! Your efforts help make this project better for everyone. 🎉