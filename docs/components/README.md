# Component Library

Complete documentation for all UI components, including props, events, styling, and accessibility guidelines.

## 📚 Component Categories

### Core Components
- [Button](Button.md) - Interactive buttons with various styles and states
- [Input](Input.md) - Text inputs, textareas, and form controls
- [Modal](Modal.md) - Overlays, dialogs, and popup windows
- [Card](Card.md) - Content containers with headers, bodies, and actions

### Navigation Components
- [Header](Header.md) - Page headers with navigation and branding
- [Sidebar](Sidebar.md) - Side navigation panels and menus
- [Breadcrumb](Breadcrumb.md) - Navigation breadcrumb trails
- [Pagination](Pagination.md) - Page navigation controls

### Data Display Components
- [Table](Table.md) - Data tables with sorting, filtering, and pagination
- [List](List.md) - Ordered and unordered lists with custom styling
- [Avatar](Avatar.md) - User profile images and placeholders
- [Badge](Badge.md) - Status indicators and labels

### Form Components
- [Form](Form.md) - Form containers with validation
- [Select](Select.md) - Dropdown selections and multi-select
- [Checkbox](Checkbox.md) - Binary choice controls
- [Radio](Radio.md) - Single choice from multiple options

### Feedback Components
- [Alert](Alert.md) - Success, warning, error, and info messages
- [Toast](Toast.md) - Temporary notification messages
- [Loading](Loading.md) - Loading spinners and progress indicators
- [Tooltip](Tooltip.md) - Contextual help and information

## 🚀 Quick Start

### Installation

```bash
# npm
npm install @imxiaochong-sdad/components

# yarn
yarn add @imxiaochong-sdad/components

# pnpm
pnpm add @imxiaochong-sdad/components
```

### Basic Usage

```jsx
import { Button, Card, Input } from '@imxiaochong-sdad/components';
import '@imxiaochong-sdad/components/dist/style.css';

function App() {
  return (
    <Card>
      <Card.Header>
        <h2>Welcome</h2>
      </Card.Header>
      <Card.Body>
        <Input placeholder="Enter your name" />
        <Button variant="primary">Submit</Button>
      </Card.Body>
    </Card>
  );
}
```

## 🎨 Design System

### Theme Configuration

```javascript
// theme.js
export const theme = {
  colors: {
    primary: '#007bff',
    secondary: '#6c757d',
    success: '#28a745',
    warning: '#ffc107',
    danger: '#dc3545',
    info: '#17a2b8',
    light: '#f8f9fa',
    dark: '#343a40'
  },
  spacing: {
    xs: '0.25rem',
    sm: '0.5rem',
    md: '1rem',
    lg: '1.5rem',
    xl: '3rem'
  },
  breakpoints: {
    sm: '576px',
    md: '768px',
    lg: '992px',
    xl: '1200px'
  },
  typography: {
    fontFamily: '"Inter", sans-serif',
    fontSize: {
      xs: '0.75rem',
      sm: '0.875rem',
      base: '1rem',
      lg: '1.125rem',
      xl: '1.25rem',
      '2xl': '1.5rem'
    }
  }
};
```

### Using the Theme

```jsx
import { ThemeProvider } from '@imxiaochong-sdad/components';
import { theme } from './theme';

function App() {
  return (
    <ThemeProvider theme={theme}>
      {/* Your app components */}
    </ThemeProvider>
  );
}
```

## 📋 Component Standards

### Props Interface
All components follow consistent prop patterns:

```typescript
interface BaseComponentProps {
  /** Component CSS class name */
  className?: string;
  /** Inline styles */
  style?: React.CSSProperties;
  /** Component variant */
  variant?: 'primary' | 'secondary' | 'success' | 'warning' | 'danger';
  /** Component size */
  size?: 'sm' | 'md' | 'lg';
  /** Disabled state */
  disabled?: boolean;
  /** Test ID for testing */
  testId?: string;
  /** Children elements */
  children?: React.ReactNode;
}
```

### Event Handlers
Common event patterns across components:

```typescript
interface ComponentEvents {
  /** Click event handler */
  onClick?: (event: React.MouseEvent) => void;
  /** Focus event handler */
  onFocus?: (event: React.FocusEvent) => void;
  /** Blur event handler */
  onBlur?: (event: React.FocusEvent) => void;
  /** Change event handler */
  onChange?: (value: any, event: React.ChangeEvent) => void;
}
```

### Accessibility
All components implement WCAG 2.1 AA standards:

- **Keyboard Navigation**: Full keyboard support
- **Screen Readers**: Proper ARIA labels and roles
- **Focus Management**: Visible focus indicators
- **Color Contrast**: Minimum 4.5:1 contrast ratio
- **Semantic HTML**: Proper HTML elements and structure

## 🔧 Customization

### CSS Custom Properties

```css
:root {
  --component-primary-color: #007bff;
  --component-border-radius: 0.375rem;
  --component-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
  --component-transition: all 0.2s ease-in-out;
}
```

### Component Overrides

```jsx
// Custom button with override styles
<Button 
  className="custom-button"
  style={{ backgroundColor: '#custom-color' }}
>
  Custom Button
</Button>
```

### Styled Components

```jsx
import styled from 'styled-components';
import { Button } from '@imxiaochong-sdad/components';

const CustomButton = styled(Button)`
  background: linear-gradient(45deg, #fe6b8b 30%, #ff8e53 90%);
  border: 0;
  border-radius: 3px;
  box-shadow: 0 3px 5px 2px rgba(255, 105, 135, 0.3);
  color: white;
  height: 48px;
  padding: 0 30px;
`;
```

## 📱 Responsive Design

### Breakpoint Usage

```jsx
import { useBreakpoint } from '@imxiaochong-sdad/components';

function ResponsiveComponent() {
  const { isMobile, isTablet, isDesktop } = useBreakpoint();
  
  return (
    <div>
      {isMobile && <MobileLayout />}
      {isTablet && <TabletLayout />}
      {isDesktop && <DesktopLayout />}
    </div>
  );
}
```

### Responsive Props

```jsx
<Card
  padding={{ sm: 'sm', md: 'md', lg: 'lg' }}
  columns={{ sm: 1, md: 2, lg: 3 }}
>
  Content
</Card>
```

## 🧪 Testing

### Component Testing

```javascript
import { render, screen, fireEvent } from '@testing-library/react';
import { Button } from '@imxiaochong-sdad/components';

describe('Button Component', () => {
  test('renders with correct text', () => {
    render(<Button>Click me</Button>);
    expect(screen.getByText('Click me')).toBeInTheDocument();
  });

  test('calls onClick when clicked', () => {
    const handleClick = jest.fn();
    render(<Button onClick={handleClick}>Click me</Button>);
    
    fireEvent.click(screen.getByText('Click me'));
    expect(handleClick).toHaveBeenCalledTimes(1);
  });

  test('is accessible', async () => {
    const { container } = render(<Button>Accessible Button</Button>);
    const results = await axe(container);
    expect(results).toHaveNoViolations();
  });
});
```

### Visual Testing

```javascript
import { ComponentStory, ComponentMeta } from '@storybook/react';
import { Button } from '@imxiaochong-sdad/components';

export default {
  title: 'Components/Button',
  component: Button,
  argTypes: {
    variant: {
      control: { type: 'select' },
      options: ['primary', 'secondary', 'success', 'warning', 'danger']
    }
  }
} as ComponentMeta<typeof Button>;

const Template: ComponentStory<typeof Button> = (args) => <Button {...args} />;

export const Primary = Template.bind({});
Primary.args = {
  variant: 'primary',
  children: 'Primary Button'
};

export const AllVariants = () => (
  <div style={{ display: 'flex', gap: '1rem' }}>
    <Button variant="primary">Primary</Button>
    <Button variant="secondary">Secondary</Button>
    <Button variant="success">Success</Button>
    <Button variant="warning">Warning</Button>
    <Button variant="danger">Danger</Button>
  </div>
);
```

## 🚀 Performance

### Bundle Size Optimization

```javascript
// Import only what you need
import { Button } from '@imxiaochong-sdad/components/Button';

// Or use tree shaking
import { Button, Card } from '@imxiaochong-sdad/components';
```

### Lazy Loading

```jsx
import { lazy, Suspense } from 'react';

const LazyModal = lazy(() => import('@imxiaochong-sdad/components/Modal'));

function App() {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <LazyModal />
    </Suspense>
  );
}
```

## 🔗 Related Resources

- [Design Tokens](../guides/design-tokens.md)
- [Accessibility Guidelines](../guides/accessibility.md)
- [Contribution Guide](../guides/CONTRIBUTING.md)
- [Storybook Documentation](https://storybook.imxiaochong-sdad.com)
- [Figma Design System](https://figma.com/imxiaochong-sdad)

## 📞 Support

- **Component Issues**: [GitHub Issues](https://github.com/imxiaochong-sdad/components/issues)
- **Design Questions**: [Design Team](mailto:design@imxiaochong-sdad.com)
- **Documentation**: [Component Docs](https://components.imxiaochong-sdad.com)
- **Community**: [Discord Server](https://discord.gg/imxiaochong-sdad)