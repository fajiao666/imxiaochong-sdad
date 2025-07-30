# Button Component

A flexible button component with multiple variants, sizes, and states.

## Import

```jsx
import { Button } from '@imxiaochong-sdad/components';
```

## Basic Usage

```jsx
<Button>Click me</Button>
```

## Props

### Required Props

None - all props are optional.

### Optional Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `variant` | `'primary'` \| `'secondary'` \| `'success'` \| `'warning'` \| `'danger'` \| `'ghost'` \| `'link'` | `'primary'` | Visual style variant |
| `size` | `'sm'` \| `'md'` \| `'lg'` | `'md'` | Button size |
| `disabled` | `boolean` | `false` | Whether the button is disabled |
| `loading` | `boolean` | `false` | Whether to show loading state |
| `icon` | `ReactNode` | `undefined` | Icon to display before text |
| `iconPosition` | `'left'` \| `'right'` | `'left'` | Position of the icon |
| `fullWidth` | `boolean` | `false` | Whether button takes full width |
| `type` | `'button'` \| `'submit'` \| `'reset'` | `'button'` | HTML button type |
| `href` | `string` | `undefined` | If provided, renders as a link |
| `target` | `string` | `undefined` | Link target (when href is provided) |
| `className` | `string` | `undefined` | Additional CSS class names |
| `style` | `CSSProperties` | `undefined` | Inline styles |
| `testId` | `string` | `undefined` | Test identifier |
| `children` | `ReactNode` | `undefined` | Button content |

### Event Props

| Prop | Type | Description |
|------|------|-------------|
| `onClick` | `(event: MouseEvent) => void` | Click event handler |
| `onFocus` | `(event: FocusEvent) => void` | Focus event handler |
| `onBlur` | `(event: FocusEvent) => void` | Blur event handler |
| `onMouseEnter` | `(event: MouseEvent) => void` | Mouse enter event handler |
| `onMouseLeave` | `(event: MouseEvent) => void` | Mouse leave event handler |

## Examples

### Variants

```jsx
<div style={{ display: 'flex', gap: '1rem', flexWrap: 'wrap' }}>
  <Button variant="primary">Primary</Button>
  <Button variant="secondary">Secondary</Button>
  <Button variant="success">Success</Button>
  <Button variant="warning">Warning</Button>
  <Button variant="danger">Danger</Button>
  <Button variant="ghost">Ghost</Button>
  <Button variant="link">Link</Button>
</div>
```

### Sizes

```jsx
<div style={{ display: 'flex', gap: '1rem', alignItems: 'center' }}>
  <Button size="sm">Small</Button>
  <Button size="md">Medium</Button>
  <Button size="lg">Large</Button>
</div>
```

### With Icons

```jsx
import { 
  PlusIcon, 
  DownloadIcon, 
  ArrowRightIcon 
} from '@heroicons/react/24/outline';

<div style={{ display: 'flex', gap: '1rem', flexWrap: 'wrap' }}>
  <Button icon={<PlusIcon />}>Add Item</Button>
  <Button icon={<DownloadIcon />} variant="secondary">
    Download
  </Button>
  <Button 
    icon={<ArrowRightIcon />} 
    iconPosition="right"
    variant="ghost"
  >
    Next
  </Button>
</div>
```

### States

```jsx
<div style={{ display: 'flex', gap: '1rem', flexWrap: 'wrap' }}>
  <Button>Normal</Button>
  <Button disabled>Disabled</Button>
  <Button loading>Loading</Button>
  <Button loading disabled>Loading Disabled</Button>
</div>
```

### Full Width

```jsx
<div style={{ width: '300px' }}>
  <Button fullWidth>Full Width Button</Button>
</div>
```

### As Link

```jsx
<div style={{ display: 'flex', gap: '1rem' }}>
  <Button href="https://example.com">External Link</Button>
  <Button href="/dashboard" variant="secondary">
    Internal Link
  </Button>
  <Button href="https://example.com" target="_blank">
    Open in New Tab
  </Button>
</div>
```

### Form Usage

```jsx
<form onSubmit={(e) => { e.preventDefault(); console.log('Submitted'); }}>
  <div style={{ display: 'flex', gap: '1rem' }}>
    <Button type="submit" variant="primary">
      Submit
    </Button>
    <Button type="reset" variant="secondary">
      Reset
    </Button>
    <Button type="button" variant="ghost">
      Cancel
    </Button>
  </div>
</form>
```

## Advanced Usage

### Custom Styling

```jsx
<Button
  className="custom-button"
  style={{
    background: 'linear-gradient(45deg, #fe6b8b 30%, #ff8e53 90%)',
    color: 'white',
    border: 'none',
    borderRadius: '25px'
  }}
>
  Custom Styled Button
</Button>
```

### With Custom Loading Indicator

```jsx
import { Spinner } from '@imxiaochong-sdad/components';

<Button
  loading
  icon={<Spinner size="sm" />}
  variant="primary"
>
  Processing...
</Button>
```

### Async Click Handler

```jsx
function AsyncButton() {
  const [loading, setLoading] = useState(false);

  const handleClick = async () => {
    setLoading(true);
    try {
      await new Promise(resolve => setTimeout(resolve, 2000));
      console.log('Operation completed');
    } catch (error) {
      console.error('Operation failed:', error);
    } finally {
      setLoading(false);
    }
  };

  return (
    <Button 
      onClick={handleClick} 
      loading={loading}
      disabled={loading}
    >
      {loading ? 'Processing...' : 'Start Operation'}
    </Button>
  );
}
```

## Accessibility

### Keyboard Support

| Key | Function |
|-----|----------|
| `Enter` | Activates the button |
| `Space` | Activates the button |
| `Tab` | Moves focus to/from the button |

### ARIA Attributes

The Button component automatically includes:

- `role="button"` (when rendered as non-button element)
- `aria-disabled="true"` (when disabled)
- `aria-busy="true"` (when loading)
- `aria-label` (can be provided via props)

### Screen Reader Support

```jsx
<Button 
  aria-label="Add new item to the list"
  icon={<PlusIcon />}
>
  Add
</Button>
```

### Focus Management

```jsx
function FocusExample() {
  const buttonRef = useRef(null);

  const focusButton = () => {
    buttonRef.current?.focus();
  };

  return (
    <div>
      <Button onClick={focusButton}>Focus Next Button</Button>
      <Button ref={buttonRef}>Target Button</Button>
    </div>
  );
}
```

## Styling

### CSS Classes

The Button component uses these CSS classes:

```css
.btn {
  /* Base button styles */
}

.btn--primary {
  /* Primary variant styles */
}

.btn--secondary {
  /* Secondary variant styles */
}

.btn--sm {
  /* Small size styles */
}

.btn--md {
  /* Medium size styles */
}

.btn--lg {
  /* Large size styles */
}

.btn--disabled {
  /* Disabled state styles */
}

.btn--loading {
  /* Loading state styles */
}

.btn--full-width {
  /* Full width styles */
}
```

### CSS Custom Properties

```css
.btn {
  --btn-border-radius: var(--border-radius-md);
  --btn-font-weight: var(--font-weight-medium);
  --btn-transition: var(--transition-all);
  
  /* Primary variant */
  --btn-primary-bg: var(--color-primary-500);
  --btn-primary-border: var(--color-primary-500);
  --btn-primary-text: var(--color-white);
  
  /* Sizes */
  --btn-sm-padding: var(--spacing-1) var(--spacing-3);
  --btn-sm-font-size: var(--font-size-sm);
  --btn-md-padding: var(--spacing-2) var(--spacing-4);
  --btn-md-font-size: var(--font-size-base);
  --btn-lg-padding: var(--spacing-3) var(--spacing-6);
  --btn-lg-font-size: var(--font-size-lg);
}
```

## Testing

### Unit Testing

```javascript
import { render, screen, fireEvent } from '@testing-library/react';
import { Button } from '@imxiaochong-sdad/components';

describe('Button', () => {
  test('renders correctly', () => {
    render(<Button>Test Button</Button>);
    expect(screen.getByRole('button')).toBeInTheDocument();
    expect(screen.getByText('Test Button')).toBeInTheDocument();
  });

  test('handles click events', () => {
    const handleClick = jest.fn();
    render(<Button onClick={handleClick}>Click me</Button>);
    
    fireEvent.click(screen.getByRole('button'));
    expect(handleClick).toHaveBeenCalledTimes(1);
  });

  test('shows loading state', () => {
    render(<Button loading>Loading Button</Button>);
    expect(screen.getByRole('button')).toHaveAttribute('aria-busy', 'true');
  });

  test('is disabled when disabled prop is true', () => {
    render(<Button disabled>Disabled Button</Button>);
    expect(screen.getByRole('button')).toBeDisabled();
  });
});
```

### Integration Testing

```javascript
import { render, screen, fireEvent, waitFor } from '@testing-library/react';
import { Button } from '@imxiaochong-sdad/components';

test('button in form submission', async () => {
  const handleSubmit = jest.fn();
  
  render(
    <form onSubmit={handleSubmit}>
      <Button type="submit">Submit Form</Button>
    </form>
  );

  fireEvent.click(screen.getByRole('button'));
  
  await waitFor(() => {
    expect(handleSubmit).toHaveBeenCalled();
  });
});
```

## Performance

### Bundle Size
- Gzipped: ~2.3KB
- Dependencies: React

### Optimization Tips

1. **Tree Shaking**: Import only the Button component
   ```jsx
   import { Button } from '@imxiaochong-sdad/components/Button';
   ```

2. **Memoization**: Use `React.memo` for expensive renders
   ```jsx
   const MemoizedButton = React.memo(Button);
   ```

3. **Event Handler Optimization**: Use `useCallback` for event handlers
   ```jsx
   const handleClick = useCallback(() => {
     // Handle click
   }, []);
   ```

## Migration Guide

### From v1.x to v2.x

```jsx
// v1.x
<Button color="primary" size="small">Button</Button>

// v2.x
<Button variant="primary" size="sm">Button</Button>
```

Breaking changes:
- `color` prop renamed to `variant`
- `small` size renamed to `sm`
- `large` size renamed to `lg`

## Related Components

- [IconButton](IconButton.md) - Buttons with only icons
- [ButtonGroup](ButtonGroup.md) - Groups of related buttons
- [ToggleButton](ToggleButton.md) - Toggle state buttons
- [FloatingActionButton](FloatingActionButton.md) - Primary action buttons