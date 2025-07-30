# formatDate Function

Format dates according to specified format strings with locale and timezone support.

## Signature

```typescript
function formatDate(
  date: Date | string | number,
  format: string,
  options?: {
    locale?: string;
    timezone?: string;
    strict?: boolean;
  }
): string
```

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `date` | `Date \| string \| number` | Yes | The date to format |
| `format` | `string` | Yes | Format string using tokens |
| `options` | `object` | No | Additional formatting options |
| `options.locale` | `string` | No | Locale for formatting (default: 'en-US') |
| `options.timezone` | `string` | No | Timezone for formatting (default: local) |
| `options.strict` | `boolean` | No | Strict parsing mode (default: false) |

## Format Tokens

### Year
| Token | Description | Example |
|-------|-------------|---------|
| `YYYY` | 4-digit year | 2024 |
| `YY` | 2-digit year | 24 |
| `Y` | Year with any number of digits | 2024 |

### Month
| Token | Description | Example |
|-------|-------------|---------|
| `MMMM` | Full month name | January |
| `MMM` | Short month name | Jan |
| `MM` | Month number (01-12) | 01 |
| `M` | Month number (1-12) | 1 |

### Day
| Token | Description | Example |
|-------|-------------|---------|
| `DDDD` | Full day name | Monday |
| `DDD` | Short day name | Mon |
| `DD` | Day of month (01-31) | 01 |
| `D` | Day of month (1-31) | 1 |

### Hour
| Token | Description | Example |
|-------|-------------|---------|
| `HH` | Hour (00-23) | 13 |
| `H` | Hour (0-23) | 13 |
| `hh` | Hour (01-12) | 01 |
| `h` | Hour (1-12) | 1 |

### Minute & Second
| Token | Description | Example |
|-------|-------------|---------|
| `mm` | Minutes (00-59) | 30 |
| `m` | Minutes (0-59) | 30 |
| `ss` | Seconds (00-59) | 45 |
| `s` | Seconds (0-59) | 45 |
| `SSS` | Milliseconds (000-999) | 123 |

### Meridiem
| Token | Description | Example |
|-------|-------------|---------|
| `A` | Uppercase AM/PM | AM |
| `a` | Lowercase am/pm | am |

## Basic Usage

```javascript
import { formatDate } from '@imxiaochong-sdad/utils';

const date = new Date('2024-01-15T14:30:45.123Z');

// Basic formatting
formatDate(date, 'YYYY-MM-DD');
// Returns: "2024-01-15"

formatDate(date, 'MM/DD/YYYY');
// Returns: "01/15/2024"

formatDate(date, 'MMMM D, YYYY');
// Returns: "January 15, 2024"
```

## Advanced Examples

### Time Formatting

```javascript
const date = new Date('2024-01-15T14:30:45.123Z');

// 24-hour format
formatDate(date, 'HH:mm:ss');
// Returns: "14:30:45"

// 12-hour format
formatDate(date, 'h:mm:ss A');
// Returns: "2:30:45 PM"

// With milliseconds
formatDate(date, 'HH:mm:ss.SSS');
// Returns: "14:30:45.123"
```

### Full Date-Time Formats

```javascript
const date = new Date('2024-01-15T14:30:45Z');

// ISO-like format
formatDate(date, 'YYYY-MM-DD HH:mm:ss');
// Returns: "2024-01-15 14:30:45"

// Human-readable format
formatDate(date, 'DDDD, MMMM D, YYYY at h:mm A');
// Returns: "Monday, January 15, 2024 at 2:30 PM"

// Short format
formatDate(date, 'MMM D, YY');
// Returns: "Jan 15, 24"
```

### Locale Support

```javascript
const date = new Date('2024-01-15T14:30:45Z');

// French locale
formatDate(date, 'DDDD, MMMM D, YYYY', { locale: 'fr-FR' });
// Returns: "lundi, janvier 15, 2024"

// German locale
formatDate(date, 'DD.MM.YYYY', { locale: 'de-DE' });
// Returns: "15.01.2024"

// Japanese locale
formatDate(date, 'YYYY年MM月DD日', { locale: 'ja-JP' });
// Returns: "2024年01月15日"
```

### Timezone Support

```javascript
const date = new Date('2024-01-15T14:30:45Z');

// UTC timezone
formatDate(date, 'YYYY-MM-DD HH:mm:ss', { timezone: 'UTC' });
// Returns: "2024-01-15 14:30:45"

// New York timezone
formatDate(date, 'YYYY-MM-DD HH:mm:ss', { timezone: 'America/New_York' });
// Returns: "2024-01-15 09:30:45"

// Tokyo timezone
formatDate(date, 'YYYY-MM-DD HH:mm:ss', { timezone: 'Asia/Tokyo' });
// Returns: "2024-01-15 23:30:45"
```

### Input Flexibility

```javascript
// Date object
formatDate(new Date(), 'YYYY-MM-DD');

// ISO string
formatDate('2024-01-15T14:30:45Z', 'MM/DD/YYYY');

// Timestamp
formatDate(1705329045000, 'MMMM D, YYYY');

// Custom date string
formatDate('January 15, 2024', 'YYYY-MM-DD');
```

## Error Handling

```javascript
try {
  // Valid usage
  const result = formatDate(new Date(), 'YYYY-MM-DD');
  console.log(result);
} catch (error) {
  console.error('Format failed:', error.message);
}

// Handle invalid dates
const result = formatDate('invalid-date', 'YYYY-MM-DD');
if (result === 'Invalid Date') {
  console.log('Date parsing failed');
}

// Strict mode
try {
  formatDate('2024-13-45', 'YYYY-MM-DD', { strict: true });
} catch (error) {
  console.error('Strict parsing failed:', error.message);
}
```

## Common Patterns

### Current Date/Time

```javascript
// Today's date
const today = formatDate(new Date(), 'YYYY-MM-DD');

// Current time
const now = formatDate(new Date(), 'HH:mm:ss');

// Timestamp
const timestamp = formatDate(new Date(), 'YYYY-MM-DD HH:mm:ss');
```

### Relative Dates

```javascript
const now = new Date();

// Yesterday
const yesterday = new Date(now.getTime() - 24 * 60 * 60 * 1000);
formatDate(yesterday, 'YYYY-MM-DD');

// Next week
const nextWeek = new Date(now.getTime() + 7 * 24 * 60 * 60 * 1000);
formatDate(nextWeek, 'MMMM D, YYYY');
```

### File Naming

```javascript
// Safe filename with timestamp
const filename = formatDate(new Date(), 'YYYY-MM-DD_HH-mm-ss');
console.log(`backup_${filename}.sql`);
// Returns: "backup_2024-01-15_14-30-45.sql"

// Log file naming
const logFile = formatDate(new Date(), 'YYYY-MM-DD');
console.log(`app_${logFile}.log`);
// Returns: "app_2024-01-15.log"
```

### API Integration

```javascript
// Query parameter formatting
const apiCall = (startDate, endDate) => {
  const start = formatDate(startDate, 'YYYY-MM-DD');
  const end = formatDate(endDate, 'YYYY-MM-DD');
  
  return fetch(`/api/data?start=${start}&end=${end}`);
};

// Response formatting
const formatApiResponse = (data) => {
  return data.map(item => ({
    ...item,
    created_at: formatDate(item.created_at, 'MMMM D, YYYY'),
    updated_at: formatDate(item.updated_at, 'MM/DD/YY')
  }));
};
```

## React Integration

```jsx
import { formatDate } from '@imxiaochong-sdad/utils';

function DateDisplay({ date, format = 'MMMM D, YYYY' }) {
  return (
    <time dateTime={formatDate(date, 'YYYY-MM-DD')}>
      {formatDate(date, format)}
    </time>
  );
}

// Usage
<DateDisplay date={new Date()} />
<DateDisplay date="2024-01-15" format="MM/DD/YYYY" />
```

### Custom Hook

```jsx
import { useState, useEffect } from 'react';
import { formatDate } from '@imxiaochong-sdad/utils';

function useFormattedDate(format = 'YYYY-MM-DD HH:mm:ss') {
  const [formattedDate, setFormattedDate] = useState('');

  useEffect(() => {
    const timer = setInterval(() => {
      setFormattedDate(formatDate(new Date(), format));
    }, 1000);

    return () => clearInterval(timer);
  }, [format]);

  return formattedDate;
}

// Usage
function Clock() {
  const time = useFormattedDate('h:mm:ss A');
  return <div>{time}</div>;
}
```

## Performance

### Caching

```javascript
// Cache formatted dates for performance
const dateCache = new Map();

function cachedFormatDate(date, format, options) {
  const key = `${date}-${format}-${JSON.stringify(options)}`;
  
  if (dateCache.has(key)) {
    return dateCache.get(key);
  }
  
  const result = formatDate(date, format, options);
  dateCache.set(key, result);
  
  return result;
}
```

### Memoization

```javascript
import { memo } from 'react';
import { formatDate } from '@imxiaochong-sdad/utils';

const MemoizedDateComponent = memo(({ date, format }) => (
  <span>{formatDate(date, format)}</span>
));
```

## Testing

```javascript
import { formatDate } from '@imxiaochong-sdad/utils';

describe('formatDate', () => {
  const testDate = new Date('2024-01-15T14:30:45.123Z');

  test('formats basic date patterns', () => {
    expect(formatDate(testDate, 'YYYY-MM-DD')).toBe('2024-01-15');
    expect(formatDate(testDate, 'MM/DD/YYYY')).toBe('01/15/2024');
    expect(formatDate(testDate, 'DD.MM.YY')).toBe('15.01.24');
  });

  test('formats time patterns', () => {
    expect(formatDate(testDate, 'HH:mm:ss')).toBe('14:30:45');
    expect(formatDate(testDate, 'h:mm A')).toBe('2:30 PM');
  });

  test('handles different input types', () => {
    const timestamp = testDate.getTime();
    const isoString = testDate.toISOString();
    
    expect(formatDate(timestamp, 'YYYY-MM-DD')).toBe('2024-01-15');
    expect(formatDate(isoString, 'YYYY-MM-DD')).toBe('2024-01-15');
  });

  test('respects timezone options', () => {
    const utc = formatDate(testDate, 'HH:mm', { timezone: 'UTC' });
    const tokyo = formatDate(testDate, 'HH:mm', { timezone: 'Asia/Tokyo' });
    
    expect(utc).toBe('14:30');
    expect(tokyo).toBe('23:30');
  });

  test('handles invalid dates', () => {
    expect(() => formatDate('invalid', 'YYYY-MM-DD')).toThrow();
    expect(formatDate(null, 'YYYY-MM-DD')).toBe('Invalid Date');
  });
});
```

## Migration Guide

### From Moment.js

```javascript
// Moment.js
moment().format('YYYY-MM-DD');
moment().format('MMMM Do YYYY, h:mm:ss a');

// Our formatDate
formatDate(new Date(), 'YYYY-MM-DD');
formatDate(new Date(), 'MMMM D, YYYY h:mm:ss a');
```

### From date-fns

```javascript
// date-fns
import { format } from 'date-fns';
format(new Date(), 'yyyy-MM-dd');

// Our formatDate
formatDate(new Date(), 'YYYY-MM-DD');
```

## Browser Support

- **Modern Browsers**: Full support
- **IE11**: Requires Intl polyfill for locale support
- **Node.js**: v12+ (full timezone support in v14+)
- **React Native**: Supported

## Related Functions

- [parseDate](parseDate.md) - Parse date strings
- [addDays](addDays.md) - Add/subtract days
- [isValid](isValid.md) - Validate dates
- [getTimezone](getTimezone.md) - Get user timezone