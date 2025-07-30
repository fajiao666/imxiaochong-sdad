# Function Documentation

Complete reference for all utility functions, hooks, and helper methods.

## 📚 Function Categories

### Utility Functions
- [formatDate](formatDate.md) - Date formatting utilities
- [validateEmail](validateEmail.md) - Email validation function
- [debounce](debounce.md) - Debouncing function execution
- [throttle](throttle.md) - Throttling function execution
- [deepClone](deepClone.md) - Deep object cloning
- [generateId](generateId.md) - Unique ID generation

### String Functions
- [capitalize](capitalize.md) - String capitalization
- [truncate](truncate.md) - String truncation with ellipsis
- [slugify](slugify.md) - Convert strings to URL-safe slugs
- [sanitizeHtml](sanitizeHtml.md) - HTML sanitization
- [parseMarkdown](parseMarkdown.md) - Markdown parsing

### Array Functions
- [groupBy](groupBy.md) - Group array items by property
- [sortBy](sortBy.md) - Sort arrays by multiple criteria
- [chunk](chunk.md) - Split arrays into chunks
- [unique](unique.md) - Remove duplicate items
- [flatten](flatten.md) - Flatten nested arrays

### Object Functions
- [pick](pick.md) - Pick specific properties from objects
- [omit](omit.md) - Omit properties from objects
- [merge](merge.md) - Deep merge objects
- [isEmpty](isEmpty.md) - Check if object/array is empty
- [hasProperty](hasProperty.md) - Safe property checking

### Math Functions
- [clamp](clamp.md) - Clamp values within range
- [random](random.md) - Generate random numbers
- [round](round.md) - Round numbers to precision
- [lerp](lerp.md) - Linear interpolation
- [percentage](percentage.md) - Calculate percentages

### React Hooks
- [useLocalStorage](useLocalStorage.md) - Local storage hook
- [useDebounce](useDebounce.md) - Debounced value hook
- [useFetch](useFetch.md) - Data fetching hook
- [useMediaQuery](useMediaQuery.md) - Media query hook
- [useIntersection](useIntersection.md) - Intersection observer hook

### API Functions
- [apiRequest](apiRequest.md) - HTTP request wrapper
- [uploadFile](uploadFile.md) - File upload utility
- [downloadFile](downloadFile.md) - File download utility
- [cacheResponse](cacheResponse.md) - Response caching
- [retryRequest](retryRequest.md) - Request retry logic

## 🚀 Quick Start

### Installation

```bash
# npm
npm install @imxiaochong-sdad/utils

# yarn
yarn add @imxiaochong-sdad/utils

# pnpm
pnpm add @imxiaochong-sdad/utils
```

### Basic Usage

```javascript
import { 
  formatDate, 
  debounce, 
  validateEmail,
  useLocalStorage 
} from '@imxiaochong-sdad/utils';

// Utility functions
const formattedDate = formatDate(new Date(), 'YYYY-MM-DD');
const isValidEmail = validateEmail('user@example.com');

// Debounced function
const debouncedSearch = debounce((query) => {
  console.log('Searching for:', query);
}, 300);

// React hook
function MyComponent() {
  const [value, setValue] = useLocalStorage('key', 'default');
  return <div>{value}</div>;
}
```

## 📋 Function Standards

### TypeScript Support
All functions include complete TypeScript definitions:

```typescript
/**
 * Format a date according to the specified format string
 * @param date - The date to format
 * @param format - The format string (e.g., 'YYYY-MM-DD')
 * @param options - Additional formatting options
 * @returns Formatted date string
 * @example
 * formatDate(new Date(), 'YYYY-MM-DD HH:mm:ss')
 * // Returns: "2024-01-01 12:30:45"
 */
function formatDate(
  date: Date | string | number,
  format: string,
  options?: {
    locale?: string;
    timezone?: string;
  }
): string;
```

### Error Handling
Functions include comprehensive error handling:

```javascript
function validateEmail(email) {
  try {
    if (typeof email !== 'string') {
      throw new TypeError('Email must be a string');
    }
    
    if (!email.trim()) {
      return { valid: false, error: 'Email is required' };
    }
    
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    const valid = emailRegex.test(email);
    
    return { 
      valid, 
      error: valid ? null : 'Invalid email format' 
    };
  } catch (error) {
    return { 
      valid: false, 
      error: error.message 
    };
  }
}
```

### Performance Optimization
Functions are optimized for performance:

- **Memoization**: Expensive calculations are cached
- **Lazy Evaluation**: Values computed only when needed
- **Tree Shaking**: Functions can be imported individually
- **Type Guards**: Runtime type checking for safety

## 🔧 Advanced Usage

### Custom Configuration

```javascript
// Configure global settings
import { configure } from '@imxiaochong-sdad/utils';

configure({
  dateFormat: 'YYYY-MM-DD',
  locale: 'en-US',
  timezone: 'UTC',
  debounceDelay: 300,
  maxRetries: 3
});
```

### Function Composition

```javascript
import { pipe, compose } from '@imxiaochong-sdad/utils';

// Pipe functions left to right
const processData = pipe(
  (data) => data.filter(item => item.active),
  (data) => data.map(item => ({ ...item, processed: true })),
  (data) => data.sort((a, b) => a.name.localeCompare(b.name))
);

// Compose functions right to left
const transformText = compose(
  capitalize,
  trim,
  sanitizeHtml
);

const result = transformText('<script>alert("xss")</script>  hello world  ');
// Returns: "Hello world"
```

### Currying Support

```javascript
import { curry } from '@imxiaochong-sdad/utils';

// Curry functions for partial application
const add = curry((a, b, c) => a + b + c);
const addFive = add(5);
const addFiveAndTen = addFive(10);
const result = addFiveAndTen(15); // 30

// Practical example
const formatCurrency = curry((locale, currency, amount) => 
  new Intl.NumberFormat(locale, {
    style: 'currency',
    currency
  }).format(amount)
);

const formatUSD = formatCurrency('en-US', 'USD');
const formatEUR = formatCurrency('en-GB', 'EUR');

console.log(formatUSD(1234.56)); // "$1,234.56"
console.log(formatEUR(1234.56)); // "€1,234.56"
```

## 🧪 Testing

### Unit Testing

```javascript
import { formatDate, validateEmail } from '@imxiaochong-sdad/utils';

describe('formatDate', () => {
  test('formats date correctly', () => {
    const date = new Date('2024-01-01T12:30:45Z');
    expect(formatDate(date, 'YYYY-MM-DD')).toBe('2024-01-01');
    expect(formatDate(date, 'HH:mm:ss')).toBe('12:30:45');
  });

  test('handles invalid dates', () => {
    expect(() => formatDate('invalid', 'YYYY-MM-DD')).toThrow();
  });
});

describe('validateEmail', () => {
  test('validates correct emails', () => {
    expect(validateEmail('user@example.com')).toEqual({
      valid: true,
      error: null
    });
  });

  test('rejects invalid emails', () => {
    expect(validateEmail('invalid-email')).toEqual({
      valid: false,
      error: 'Invalid email format'
    });
  });
});
```

### Integration Testing

```javascript
import { debounce, apiRequest } from '@imxiaochong-sdad/utils';

test('debounced API search', async () => {
  const mockApi = jest.fn().mockResolvedValue({ data: [] });
  const debouncedSearch = debounce(mockApi, 300);

  // Trigger multiple calls
  debouncedSearch('query1');
  debouncedSearch('query2');
  debouncedSearch('query3');

  // Wait for debounce delay
  await new Promise(resolve => setTimeout(resolve, 350));

  // Only the last call should execute
  expect(mockApi).toHaveBeenCalledTimes(1);
  expect(mockApi).toHaveBeenCalledWith('query3');
});
```

## 🚀 Performance

### Bundle Analysis

| Category | Functions | Gzipped Size |
|----------|-----------|--------------|
| Utility | 6 | 1.2KB |
| String | 5 | 0.8KB |
| Array | 5 | 1.1KB |
| Object | 5 | 0.9KB |
| Math | 5 | 0.6KB |
| React Hooks | 5 | 2.1KB |
| API | 5 | 1.4KB |
| **Total** | **36** | **8.1KB** |

### Tree Shaking

```javascript
// Import only what you need
import { formatDate } from '@imxiaochong-sdad/utils/formatDate';
import { debounce } from '@imxiaochong-sdad/utils/debounce';

// Or use path imports
import formatDate from '@imxiaochong-sdad/utils/lib/formatDate';
import debounce from '@imxiaochong-sdad/utils/lib/debounce';
```

### Performance Benchmarks

```javascript
// Benchmark utilities
import { benchmark } from '@imxiaochong-sdad/utils/test';

const results = benchmark([
  { name: 'formatDate', fn: () => formatDate(new Date(), 'YYYY-MM-DD') },
  { name: 'validateEmail', fn: () => validateEmail('test@example.com') },
  { name: 'debounce', fn: () => debounce(() => {}, 100) }
]);

console.log(results);
// {
//   formatDate: { ops: 1250000, time: 0.8 },
//   validateEmail: { ops: 2100000, time: 0.48 },
//   debounce: { ops: 950000, time: 1.05 }
// }
```

## 📱 Browser Support

### Compatibility
- **Modern Browsers**: Full support
- **IE11**: Polyfills required for some functions
- **Node.js**: v14+ supported
- **React Native**: Compatible

### Polyfills
For older browser support:

```javascript
// Install polyfills
npm install core-js

// Import in your entry file
import 'core-js/stable';
import '@imxiaochong-sdad/utils/polyfills';
```

## 🔗 Migration Guides

### From Lodash

```javascript
// Lodash
import _ from 'lodash';
_.debounce(fn, 300);
_.groupBy(array, 'property');
_.isEmpty(obj);

// Our utils
import { debounce, groupBy, isEmpty } from '@imxiaochong-sdad/utils';
debounce(fn, 300);
groupBy(array, 'property');
isEmpty(obj);
```

### From Moment.js

```javascript
// Moment.js
import moment from 'moment';
moment().format('YYYY-MM-DD');
moment().add(1, 'day');

// Our utils
import { formatDate, addDays } from '@imxiaochong-sdad/utils';
formatDate(new Date(), 'YYYY-MM-DD');
addDays(new Date(), 1);
```

## 🔧 Contributing

### Adding New Functions

1. Create function file in appropriate category
2. Add TypeScript definitions
3. Write comprehensive tests
4. Add documentation
5. Update exports

### Function Template

```javascript
/**
 * [Function description]
 * @param {Type} param - Parameter description
 * @returns {Type} Return value description
 * @example
 * functionName(example)
 * // Returns: expected result
 */
export function functionName(param) {
  // Validation
  if (!param) {
    throw new Error('Parameter is required');
  }

  // Implementation
  try {
    return result;
  } catch (error) {
    throw new Error(`Function failed: ${error.message}`);
  }
}
```

## 📞 Support

- **Function Issues**: [GitHub Issues](https://github.com/imxiaochong-sdad/utils/issues)
- **Feature Requests**: [GitHub Discussions](https://github.com/imxiaochong-sdad/utils/discussions)
- **Documentation**: [Utils Docs](https://utils.imxiaochong-sdad.com)
- **API Reference**: [TypeDoc](https://typedoc.imxiaochong-sdad.com)