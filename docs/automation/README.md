# Documentation Automation

Automated tools and processes to keep documentation up-to-date, accurate, and comprehensive.

## 🤖 Automation Overview

Our documentation automation includes:

- **Auto-generation** from code comments and TypeScript definitions
- **Link checking** to prevent broken references
- **Content validation** for consistency and accuracy
- **Deployment automation** for seamless updates
- **API documentation** sync with OpenAPI specs
- **Component documentation** from Storybook stories

## 📁 Automation Tools

### Code Documentation Generation

#### TypeDoc Configuration

```json
// typedoc.json
{
  "entryPoints": ["src/index.ts"],
  "out": "docs/api-reference",
  "theme": "default",
  "includeVersion": true,
  "excludePrivate": true,
  "excludeProtected": true,
  "categorizeByGroup": true,
  "categoryOrder": [
    "Components",
    "Hooks",
    "Utils",
    "Types",
    "*"
  ],
  "plugin": ["typedoc-plugin-markdown"],
  "readme": "none",
  "hideGenerator": true
}
```

#### JSDoc Integration

```javascript
// scripts/generate-docs.js
const jsdoc2md = require('jsdoc-to-markdown');
const fs = require('fs');
const path = require('path');

// Generate function documentation
const templates = jsdoc2md.render({
  files: 'src/utils/**/*.js',
  template: fs.readFileSync('templates/function.hbs', 'utf8'),
  'heading-depth': 2
});

fs.writeFileSync('docs/functions/generated.md', templates);
```

#### Component Documentation

```javascript
// scripts/generate-component-docs.js
const docgen = require('react-docgen-typescript');
const glob = require('glob');

const options = {
  savePropValueAsString: true,
  shouldExtractLiteralValuesFromEnum: true,
  propFilter: (prop) => {
    if (prop.declarations !== undefined && prop.declarations.length > 0) {
      const hasPropAdditionalDescription = prop.declarations.find((declaration) => {
        return !declaration.fileName.includes('node_modules');
      });
      return Boolean(hasPropAdditionalDescription);
    }
    return true;
  },
};

// Parse component files
const files = glob.sync('src/components/**/*.tsx');
const docs = docgen.parse(files, options);

// Generate markdown documentation
docs.forEach((component) => {
  const markdown = generateComponentMarkdown(component);
  fs.writeFileSync(`docs/components/${component.displayName}.md`, markdown);
});
```

### API Documentation Sync

#### OpenAPI Documentation

```yaml
# openapi.yml
openapi: 3.0.0
info:
  title: imxiaochong-sdad API
  version: 1.0.0
  description: Comprehensive API for the imxiaochong-sdad project
servers:
  - url: https://api.imxiaochong-sdad.com/v1
    description: Production server
  - url: http://localhost:3000/api/v1
    description: Development server

# Auto-generated from code annotations
paths:
  /auth/login:
    post:
      summary: User authentication
      # ... rest of specification
```

#### Swagger Integration

```javascript
// scripts/sync-api-docs.js
const swaggerJsdoc = require('swagger-jsdoc');
const fs = require('fs');

const options = {
  definition: {
    openapi: '3.0.0',
    info: {
      title: 'imxiaochong-sdad API',
      version: '1.0.0',
    },
  },
  apis: ['src/api/**/*.js', 'src/routes/**/*.js'],
};

const specs = swaggerJsdoc(options);
fs.writeFileSync('docs/api/openapi.json', JSON.stringify(specs, null, 2));
```

### Content Validation

#### Link Checker Configuration

```javascript
// scripts/check-links.js
const linkCheck = require('link-check');
const glob = require('glob');
const fs = require('fs');

const checkLinks = async () => {
  const files = glob.sync('docs/**/*.md');
  const errors = [];

  for (const file of files) {
    const content = fs.readFileSync(file, 'utf8');
    const links = extractLinks(content);

    for (const link of links) {
      try {
        const result = await linkCheck(link);
        if (result.status === 'dead') {
          errors.push({ file, link, error: result.statusCode });
        }
      } catch (error) {
        errors.push({ file, link, error: error.message });
      }
    }
  }

  if (errors.length > 0) {
    console.error('Dead links found:', errors);
    process.exit(1);
  }
};

checkLinks();
```

#### Content Linting

```javascript
// .markdownlint.json
{
  "default": true,
  "MD007": { "indent": 2 },
  "MD013": { "line_length": 120 },
  "MD024": { "allow_different_nesting": true },
  "MD033": false,
  "MD041": false
}
```

```bash
# package.json scripts
{
  "scripts": {
    "lint:docs": "markdownlint docs/**/*.md",
    "lint:docs:fix": "markdownlint docs/**/*.md --fix"
  }
}
```

### Deployment Automation

#### GitHub Actions Workflow

```yaml
# .github/workflows/docs.yml
name: Documentation

on:
  push:
    branches: [main, develop]
    paths: ['docs/**', 'src/**']
  pull_request:
    paths: ['docs/**', 'src/**']

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
          cache: 'npm'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Generate documentation
        run: npm run docs:generate
      
      - name: Validate links
        run: npm run docs:check-links
      
      - name: Lint documentation
        run: npm run lint:docs
      
      - name: Build documentation site
        run: npm run docs:build

  deploy:
    if: github.ref == 'refs/heads/main'
    needs: validate
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./docs/dist
```

#### Documentation Site Build

```javascript
// scripts/build-docs-site.js
const { execSync } = require('child_process');
const fs = require('fs');
const path = require('path');

// Build documentation with VitePress/Docusaurus/etc.
const buildDocs = () => {
  // Copy generated files
  execSync('cp -r docs/api-reference docs/site/api');
  execSync('cp -r docs/components docs/site/components');
  execSync('cp -r docs/functions docs/site/functions');
  
  // Build the site
  execSync('cd docs/site && npm run build');
  
  // Copy to distribution directory
  execSync('cp -r docs/site/dist docs/dist');
};

buildDocs();
```

## 📋 Automation Scripts

### Package.json Scripts

```json
{
  "scripts": {
    "docs:generate": "npm run docs:api && npm run docs:components && npm run docs:functions",
    "docs:api": "typedoc && swagger-jsdoc -d swaggerDef.js src/api/**/*.js -o docs/api/openapi.json",
    "docs:components": "node scripts/generate-component-docs.js",
    "docs:functions": "node scripts/generate-function-docs.js",
    "docs:check-links": "node scripts/check-links.js",
    "docs:validate": "npm run docs:check-links && npm run lint:docs",
    "docs:build": "node scripts/build-docs-site.js",
    "docs:serve": "cd docs/site && npm run serve",
    "docs:dev": "cd docs/site && npm run dev"
  }
}
```

### Pre-commit Hooks

```yaml
# .pre-commit-config.yaml
repos:
  - repo: local
    hooks:
      - id: generate-docs
        name: Generate documentation
        entry: npm run docs:generate
        language: system
        files: '^src/.*\.(ts|tsx|js|jsx)$'
        
      - id: validate-docs
        name: Validate documentation
        entry: npm run docs:validate
        language: system
        files: '^docs/.*\.md$'
```

### Husky Configuration

```javascript
// .husky/pre-commit
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

# Generate and validate documentation
npm run docs:generate
npm run docs:validate

# Add generated docs to commit
git add docs/
```

## 🔄 Automated Workflows

### Documentation Update Workflow

1. **Code Changes Detected**
   - Monitor file changes in source code
   - Trigger documentation regeneration
   - Update component/function docs

2. **Validation Pipeline**
   - Check for broken links
   - Validate markdown syntax
   - Ensure code examples work
   - Verify API consistency

3. **Build and Deploy**
   - Generate static documentation site
   - Deploy to GitHub Pages/Netlify/Vercel
   - Update search indices
   - Notify team of changes

### API Documentation Sync

```javascript
// scripts/sync-api-changes.js
const { execSync } = require('child_process');

// Watch for API changes
const watchApiChanges = () => {
  const watcher = chokidar.watch('src/api/**/*.js');
  
  watcher.on('change', (path) => {
    console.log(`API file changed: ${path}`);
    
    // Regenerate OpenAPI spec
    execSync('npm run docs:api');
    
    // Validate against existing spec
    const hasBreakingChanges = validateApiChanges();
    
    if (hasBreakingChanges) {
      console.warn('Breaking API changes detected!');
      // Notify maintainers
    }
    
    // Update documentation
    execSync('npm run docs:build');
  });
};
```

### Component Documentation Sync

```javascript
// scripts/sync-component-changes.js
const chokidar = require('chokidar');

const syncComponentDocs = () => {
  const watcher = chokidar.watch('src/components/**/*.tsx');
  
  watcher.on('change', (filepath) => {
    const componentName = path.basename(filepath, '.tsx');
    
    // Regenerate component documentation
    generateComponentDoc(filepath, componentName);
    
    // Update Storybook stories if needed
    updateStorybookStories(componentName);
    
    // Regenerate examples
    updateExamples(componentName);
  });
};
```

## 📊 Documentation Metrics

### Analytics Configuration

```javascript
// scripts/docs-analytics.js
const analytics = require('./utils/analytics');

const trackDocumentationMetrics = () => {
  // Track page views
  analytics.track('docs_page_view', {
    page: window.location.pathname,
    timestamp: new Date().toISOString()
  });
  
  // Track search queries
  document.getElementById('search').addEventListener('input', (e) => {
    analytics.track('docs_search', {
      query: e.target.value,
      timestamp: new Date().toISOString()
    });
  });
  
  // Track broken links
  document.addEventListener('click', (e) => {
    if (e.target.tagName === 'A' && e.target.href.includes('404')) {
      analytics.track('docs_broken_link', {
        url: e.target.href,
        page: window.location.pathname
      });
    }
  });
};
```

### Quality Metrics

```javascript
// scripts/quality-metrics.js
const calculateDocQuality = () => {
  const metrics = {
    coverage: calculateCoverage(),
    freshness: calculateFreshness(),
    accuracy: calculateAccuracy(),
    completeness: calculateCompleteness()
  };
  
  // Generate quality report
  generateQualityReport(metrics);
  
  return metrics;
};

const calculateCoverage = () => {
  const sourceFiles = glob.sync('src/**/*.{ts,tsx,js,jsx}');
  const documentedFiles = glob.sync('docs/**/*.md');
  
  return {
    total: sourceFiles.length,
    documented: documentedFiles.length,
    percentage: (documentedFiles.length / sourceFiles.length) * 100
  };
};
```

## 🛠️ Maintenance Tasks

### Automated Maintenance

```javascript
// scripts/maintenance.js
const scheduledTasks = {
  daily: [
    'checkBrokenLinks',
    'validateExamples',
    'updateMetrics'
  ],
  weekly: [
    'auditDependencies',
    'updateScreenshots',
    'reviewOutdatedContent'
  ],
  monthly: [
    'comprehensiveReview',
    'performanceAudit',
    'accessibilityCheck'
  ]
};

// Run maintenance tasks
const runMaintenanceTasks = (frequency) => {
  const tasks = scheduledTasks[frequency];
  
  tasks.forEach(async (task) => {
    try {
      await require(`./tasks/${task}`)();
      console.log(`✅ ${task} completed`);
    } catch (error) {
      console.error(`❌ ${task} failed:`, error);
    }
  });
};
```

### Documentation Health Checks

```javascript
// scripts/health-check.js
const healthCheck = async () => {
  const checks = [
    { name: 'Links', fn: checkAllLinks },
    { name: 'Images', fn: checkImages },
    { name: 'Code Examples', fn: validateCodeExamples },
    { name: 'API Consistency', fn: checkApiConsistency },
    { name: 'Navigation', fn: validateNavigation }
  ];
  
  const results = await Promise.all(
    checks.map(async (check) => {
      try {
        const result = await check.fn();
        return { name: check.name, status: 'pass', result };
      } catch (error) {
        return { name: check.name, status: 'fail', error: error.message };
      }
    })
  );
  
  generateHealthReport(results);
  
  return results;
};
```

## 📞 Support & Configuration

### Getting Started

1. **Install dependencies**:
   ```bash
   npm install --save-dev typedoc react-docgen-typescript swagger-jsdoc
   ```

2. **Configure automation**:
   ```bash
   npm run setup-docs-automation
   ```

3. **Run initial generation**:
   ```bash
   npm run docs:generate
   ```

### Customization

Customize automation by modifying:
- `scripts/` directory for custom generators
- `.github/workflows/` for CI/CD pipelines
- `docs/templates/` for documentation templates
- `docs/config/` for build configurations

### Troubleshooting

Common issues and solutions:
- **Build failures**: Check Node.js version compatibility
- **Broken links**: Review internal link structure
- **Missing content**: Verify source code annotations
- **Deployment issues**: Check GitHub Pages configuration

---

*Documentation automation ensures our docs stay current, accurate, and helpful. Contribute improvements to make the system even better!*