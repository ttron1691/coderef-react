# coderef-react

# React.js Reference Cheat Sheet

## Table of Contents
- [Core Concepts](#core-concepts)
- [Component Types](#component-types)
- [JSX Syntax](#jsx-syntax)
- [Props](#props)
- [State Management](#state-management)
- [Hooks](#hooks)
- [Advanced Hooks](#advanced-hooks)
- [Lifecycle Methods](#lifecycle-methods)
- [Event Handling](#event-handling)
- [Forms](#forms)
- [Conditional Rendering](#conditional-rendering)
- [Lists and Keys](#lists-and-keys)
- [Context API](#context-api)
- [Refs](#refs)
- [Performance Optimization](#performance-optimization)
- [Error Handling](#error-handling)
- [Testing](#testing)
- [Routing](#routing)
- [State Management Libraries](#state-management-libraries)
- [Server-Side Rendering](#server-side-rendering)
- [Best Practices](#best-practices)

## Core Concepts

### Components
React apps are built using components - reusable, self-contained pieces of code that return markup.

### Virtual DOM
React uses a virtual representation of the DOM for performance. It only updates the actual DOM when necessary.

### Unidirectional Data Flow
Data flows down from parent to child components via props.

## Component Types

### Function Components
```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

### Class Components
```jsx
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

### Pure Components
```jsx
class PureGreeting extends React.PureComponent {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

### Memo Components
```jsx
const MemoizedComponent = React.memo(function MyComponent(props) {
  return <div>{props.name}</div>;
});
```

## JSX Syntax

### Basic Syntax
```jsx
const element = <h1>Hello, world!</h1>;
```

### Expressions in JSX
```jsx
const name = 'John';
const element = <h1>Hello, {name}</h1>;
```

### JSX Attributes
```jsx
const element = <img src={user.avatarUrl} alt="User avatar" />;
```

### Children in JSX
```jsx
const element = (
  <div>
    <h1>Hello!</h1>
    <p>Welcome to React</p>
  </div>
);
```

### JSX Fragments
```jsx
function ListItems() {
  return (
    <>
      <li>Item 1</li>
      <li>Item 2</li>
    </>
  );
}
```

## Props

### Basic Props
```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

// Usage
<Welcome name="Sara" />
```

### Default Props
```jsx
function Button({ text = "Click me" }) {
  return <button>{text}</button>;
}

// Class component
Button.defaultProps = {
  text: "Click me"
};
```

### Props Destructuring
```jsx
function Profile({ name, age, location }) {
  return (
    <div>
      <h2>{name}</h2>
      <p>Age: {age}</p>
      <p>Location: {location}</p>
    </div>
  );
}
```

### Props Spreading
```jsx
const buttonProps = {
  type: "submit",
  className: "primary",
  disabled: false
};

<Button {...buttonProps} />
```

### PropTypes
```jsx
import PropTypes from 'prop-types';

function User({ name, age }) {
  return <div>{name} is {age} years old</div>;
}

User.propTypes = {
  name: PropTypes.string.isRequired,
  age: PropTypes.number
};
```

## State Management

### Class Component State
```jsx
class Counter extends React.Component {
  constructor(props) {
    super(props);
    this.state = { count: 0 };
  }
  
  increment = () => {
    this.setState({ count: this.state.count + 1 });
  }
  
  render() {
    return (
      <div>
        <p>Count: {this.state.count}</p>
        <button onClick={this.increment}>Increment</button>
      </div>
    );
  }
}
```

### State Updates with Previous State
```jsx
increment = () => {
  this.setState(prevState => ({
    count: prevState.count + 1
  }));
}
```

### Function Component State with Hooks
```jsx
import { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

## Hooks

### useState
```jsx
import { useState } from 'react';

function Form() {
  const [name, setName] = useState('');
  
  return (
    <input 
      value={name} 
      onChange={e => setName(e.target.value)} 
    />
  );
}
```

### useEffect
```jsx
import { useState, useEffect } from 'react';

function DataFetcher() {
  const [data, setData] = useState(null);
  
  useEffect(() => {
    fetch('https://api.example.com/data')
      .then(response => response.json())
      .then(data => setData(data));
      
    return () => {
      // Cleanup function
    };
  }, []); // Empty dependency array = run once on mount
  
  return <div>{data ? JSON.stringify(data) : 'Loading...'}</div>;
}
```

### useContext
```jsx
import { createContext, useContext } from 'react';

const ThemeContext = createContext('light');

function ThemedButton() {
  const theme = useContext(ThemeContext);
  
  return <button className={theme}>Themed Button</button>;
}

// Provider in parent component
<ThemeContext.Provider value="dark">
  <ThemedButton />
</ThemeContext.Provider>
```

### useReducer
```jsx
import { useReducer } from 'react';

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    default:
      throw new Error();
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, { count: 0 });
  
  return (
    <div>
      <p>Count: {state.count}</p>
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
    </div>
  );
}
```

### useRef
```jsx
import { useRef, useEffect } from 'react';

function FocusInput() {
  const inputRef = useRef(null);
  
  useEffect(() => {
    // Focus the input on component mount
    inputRef.current.focus();
  }, []);
  
  return <input ref={inputRef} />;
}
```

## Advanced Hooks

### useCallback
```jsx
import { useState, useCallback } from 'react';

function ParentComponent() {
  const [count, setCount] = useState(0);
  
  // This function is memoized and only changes if count changes
  const handleClick = useCallback(() => {
    console.log(`Clicked, count is: ${count}`);
  }, [count]);
  
  return <ChildComponent onClick={handleClick} />;
}
```

### useMemo
```jsx
import { useState, useMemo } from 'react';

function ExpensiveCalculation({ list }) {
  // This calculation is memoized and only re-computed when list changes
  const sortedList = useMemo(() => {
    console.log('Sorting list...');
    return [...list].sort();
  }, [list]);
  
  return <div>{sortedList.join(', ')}</div>;
}
```

### useLayoutEffect
```jsx
import { useLayoutEffect, useRef } from 'react';

function Tooltip() {
  const tooltipRef = useRef(null);
  
  useLayoutEffect(() => {
    // This runs synchronously after DOM mutations but before browser paint
    const { height } = tooltipRef.current.getBoundingClientRect();
    tooltipRef.current.style.top = `-${height}px`;
  }, []);
  
  return <div ref={tooltipRef}>Tooltip content</div>;
}
```

### useImperativeHandle
```jsx
import { useRef, useImperativeHandle, forwardRef } from 'react';

const FancyInput = forwardRef((props, ref) => {
  const inputRef = useRef();
  
  useImperativeHandle(ref, () => ({
    focus: () => {
      inputRef.current.focus();
    },
    reset: () => {
      inputRef.current.value = '';
    }
  }));
  
  return <input ref={inputRef} />;
});

// Parent component
function Form() {
  const fancyInputRef = useRef();
  
  return (
    <>
      <FancyInput ref={fancyInputRef} />
      <button onClick={() => fancyInputRef.current.focus()}>Focus Input</button>
      <button onClick={() => fancyInputRef.current.reset()}>Reset Input</button>
    </>
  );
}
```

### Custom Hooks
```jsx
// Custom hook for form handling
function useFormInput(initialValue) {
  const [value, setValue] = useState(initialValue);
  
  function handleChange(e) {
    setValue(e.target.value);
  }
  
  return {
    value,
    onChange: handleChange
  };
}

// Usage
function Form() {
  const nameInput = useFormInput('');
  const emailInput = useFormInput('');
  
  return (
    <form>
      <input {...nameInput} placeholder="Name" />
      <input {...emailInput} placeholder="Email" />
      <p>Current values: {nameInput.value}, {emailInput.value}</p>
    </form>
  );
}
```

## Lifecycle Methods

### Class Component Lifecycle

#### Mounting
```jsx
class Example extends React.Component {
  constructor(props) {
    super(props);
    // Initialize state
  }
  
  static getDerivedStateFromProps(props, state) {
    // Return updated state based on props
    return null;
  }
  
  componentDidMount() {
    // Called after component is inserted into the DOM
    // Good place for API calls
  }
  
  render() {
    // Required method to return React elements
    return <div />;
  }
}
```

#### Updating
```jsx
class Example extends React.Component {
  shouldComponentUpdate(nextProps, nextState) {
    // Return false to prevent rendering
    return true;
  }
  
  getSnapshotBeforeUpdate(prevProps, prevState) {
    // Capture information from the DOM before it changes
    return null;
  }
  
  componentDidUpdate(prevProps, prevState, snapshot) {
    // Called after component updates
    // Good place to operate on the DOM after updates
  }
}
```

#### Unmounting
```jsx
class Example extends React.Component {
  componentWillUnmount() {
    // Clean up resources, timers, subscriptions
  }
}
```

#### Error Handling
```jsx
class Example extends React.Component {
  static getDerivedStateFromError(error) {
    // Update state to show fallback UI
    return { hasError: true };
  }
  
  componentDidCatch(error, info) {
    // Log error information
    console.error(error, info);
  }
}
```

### Hooks Lifecycle Equivalents
```jsx
import { useState, useEffect } from 'react';

function Component() {
  const [data, setData] = useState(null);
  
  // componentDidMount
  useEffect(() => {
    fetchData();
    
    // componentWillUnmount
    return () => {
      // Cleanup
    };
  }, []);
  
  // componentDidUpdate (when dependencies change)
  useEffect(() => {
    console.log('data updated', data);
  }, [data]);
  
  const fetchData = async () => {
    const response = await fetch('https://api.example.com/data');
    const result = await response.json();
    setData(result);
  };
  
  return <div>{data ? JSON.stringify(data) : 'Loading...'}</div>;
}
```

## Event Handling

### Basic Event Handling
```jsx
function Button() {
  const handleClick = (e) => {
    e.preventDefault();
    console.log('Button clicked');
  };
  
  return <button onClick={handleClick}>Click me</button>;
}
```

### Passing Arguments to Event Handlers
```jsx
function ItemList() {
  const handleDelete = (id, e) => {
    e.preventDefault();
    console.log(`Delete item ${id}`);
  };
  
  return (
    <ul>
      <li><button onClick={(e) => handleDelete(1, e)}>Delete Item 1</button></li>
      <li><button onClick={(e) => handleDelete(2, e)}>Delete Item 2</button></li>
    </ul>
  );
}
```

### Binding in Class Components
```jsx
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = { isOn: false };
    
    // Binding in constructor
    this.handleClick = this.handleClick.bind(this);
  }
  
  handleClick() {
    this.setState(prevState => ({
      isOn: !prevState.isOn
    }));
  }
  
  // Alternative: use class property with arrow function (no binding needed)
  toggleWithArrow = () => {
    this.setState(prevState => ({
      isOn: !prevState.isOn
    }));
  }
  
  render() {
    return (
      <div>
        <button onClick={this.handleClick}>
          {this.state.isOn ? 'ON' : 'OFF'}
        </button>
        <button onClick={this.toggleWithArrow}>
          Toggle with arrow function
        </button>
      </div>
    );
  }
}
```

### Common React Events
```jsx
// Click events
<button onClick={handleClick}>Click</button>

// Form events
<form onSubmit={handleSubmit}>...</form>
<input onChange={handleChange} />

// Focus events
<input onFocus={handleFocus} onBlur={handleBlur} />

// Keyboard events
<input onKeyDown={handleKeyDown} onKeyPress={handleKeyPress} onKeyUp={handleKeyUp} />

// Mouse events
<div 
  onMouseEnter={handleMouseEnter}
  onMouseLeave={handleMouseLeave}
  onMouseMove={handleMouseMove}
/>
```

## Forms

### Controlled Components
```jsx
import { useState } from 'react';

function ControlledForm() {
  const [formData, setFormData] = useState({
    username: '',
    email: '',
    message: ''
  });
  
  const handleChange = (e) => {
    const { name, value } = e.target;
    setFormData(prevData => ({
      ...prevData,
      [name]: value
    }));
  };
  
  const handleSubmit = (e) => {
    e.preventDefault();
    console.log('Form submitted:', formData);
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <div>
        <label htmlFor="username">Username:</label>
        <input
          type="text"
          id="username"
          name="username"
          value={formData.username}
          onChange={handleChange}
        />
      </div>
      
      <div>
        <label htmlFor="email">Email:</label>
        <input
          type="email"
          id="email"
          name="email"
          value={formData.email}
          onChange={handleChange}
        />
      </div>
      
      <div>
        <label htmlFor="message">Message:</label>
        <textarea
          id="message"
          name="message"
          value={formData.message}
          onChange={handleChange}
        />
      </div>
      
      <button type="submit">Submit</button>
    </form>
  );
}
```

### Uncontrolled Components with Refs
```jsx
import { useRef } from 'react';

function UncontrolledForm() {
  const nameRef = useRef(null);
  const emailRef = useRef(null);
  
  const handleSubmit = (e) => {
    e.preventDefault();
    console.log('Name:', nameRef.current.value);
    console.log('Email:', emailRef.current.value);
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <div>
        <label htmlFor="name">Name:</label>
        <input type="text" id="name" ref={nameRef} defaultValue="Default name" />
      </div>
      
      <div>
        <label htmlFor="email">Email:</label>
        <input type="email" id="email" ref={emailRef} />
      </div>
      
      <button type="submit">Submit</button>
    </form>
  );
}
```

### Form Validation
```jsx
import { useState } from 'react';

function FormWithValidation() {
  const [formData, setFormData] = useState({
    username: '',
    email: '',
    password: ''
  });
  
  const [errors, setErrors] = useState({});
  
  const handleChange = (e) => {
    const { name, value } = e.target;
    setFormData(prevData => ({
      ...prevData,
      [name]: value
    }));
  };
  
  const validate = () => {
    let tempErrors = {};
    
    if (!formData.username) tempErrors.username = 'Username is required';
    if (!formData.email) tempErrors.email = 'Email is required';
    else if (!/\S+@\S+\.\S+/.test(formData.email)) tempErrors.email = 'Email is invalid';
    if (!formData.password) tempErrors.password = 'Password is required';
    else if (formData.password.length < 6) tempErrors.password = 'Password must be at least 6 characters';
    
    setErrors(tempErrors);
    return Object.keys(tempErrors).length === 0;
  };
  
  const handleSubmit = (e) => {
    e.preventDefault();
    
    if (validate()) {
      console.log('Form submitted successfully:', formData);
      // Process form data, e.g., API call
    } else {
      console.log('Form validation failed');
    }
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <div>
        <label htmlFor="username">Username:</label>
        <input
          type="text"
          id="username"
          name="username"
          value={formData.username}
          onChange={handleChange}
        />
        {errors.username && <p className="error">{errors.username}</p>}
      </div>
      
      <div>
        <label htmlFor="email">Email:</label>
        <input
          type="email"
          id="email"
          name="email"
          value={formData.email}
          onChange={handleChange}
        />
        {errors.email && <p className="error">{errors.email}</p>}
      </div>
      
      <div>
        <label htmlFor="password">Password:</label>
        <input
          type="password"
          id="password"
          name="password"
          value={formData.password}
          onChange={handleChange}
        />
        {errors.password && <p className="error">{errors.password}</p>}
      </div>
      
      <button type="submit">Submit</button>
    </form>
  );
}
```

## Conditional Rendering

### If/Else with Element Variables
```jsx
function Greeting({ isLoggedIn }) {
  let greeting;
  
  if (isLoggedIn) {
    greeting = <h1>Welcome back!</h1>;
  } else {
    greeting = <h1>Please sign in</h1>;
  }
  
  return <div>{greeting}</div>;
}
```

### Inline If with Logical && Operator
```jsx
function Notifications({ messages }) {
  return (
    <div>
      {messages.length > 0 && (
        <h2>You have {messages.length} unread messages</h2>
      )}
    </div>
  );
}
```

### Inline If-Else with Conditional (Ternary) Operator
```jsx
function LoginStatus({ isLoggedIn }) {
  return (
    <div>
      {isLoggedIn 
        ? <LogoutButton /> 
        : <LoginButton />
      }
    </div>
  );
}
```

### Switch Statement in Render
```jsx
function StatusMessage({ status }) {
  let message;
  
  switch (status) {
    case 'loading':
      message = <LoadingSpinner />;
      break;
    case 'success':
      message = <SuccessMessage />;
      break;
    case 'error':
      message = <ErrorMessage />;
      break;
    default:
      message = null;
  }
  
  return <div>{message}</div>;
}
```

### Preventing Component from Rendering
```jsx
function WarningBanner({ warn }) {
  if (!warn) {
    return null;
  }
  
  return <div className="warning">Warning!</div>;
}
```

## Lists and Keys

### Rendering Lists
```jsx
function NumberList({ numbers }) {
  const listItems = numbers.map((number) =>
    <li key={number.toString()}>{number}</li>
  );
  
  return <ul>{listItems}</ul>;
}

// Usage
<NumberList numbers={[1, 2, 3, 4, 5]} />
```

### Keys in Lists
```jsx
function TodoList({ todos }) {
  return (
    <ul>
      {todos.map((todo) =>
        <li key={todo.id}>
          {todo.text}
        </li>
      )}
    </ul>
  );
}
```

### Extracting Components with Keys
```jsx
function TodoList({ todos }) {
  return (
    <ul>
      {todos.map((todo) =>
        <TodoItem key={todo.id} todo={todo} />
      )}
    </ul>
  );
}

function TodoItem({ todo }) {
  return <li>{todo.text}</li>;
}
```

### Index as a Key (use cautiously)
```jsx
function ListWithoutIds({ items }) {
  return (
    <ul>
      {items.map((item, index) =>
        // Only use index as a key when items have no stable ID and list won't change
        <li key={index}>{item}</li>
      )}
    </ul>
  );
}
```

## Context API

### Creating Context
```jsx
import { createContext } from 'react';

// Create a context with a default value
const ThemeContext = createContext('light');
```

### Provider and Consumer
```jsx
import { createContext, useState } from 'react';

const ThemeContext = createContext();

function App() {
  const [theme, setTheme] = useState('light');
  
  const toggleTheme = () => {
    setTheme(prevTheme => prevTheme === 'light' ? 'dark' : 'light');
  };
  
  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      <MainContent />
    </ThemeContext.Provider>
  );
}

function MainContent() {
  return (
    <div>
      <Header />
      <Content />
    </div>
  );
}

function Header() {
  // Consumer using render props pattern
  return (
    <ThemeContext.Consumer>
      {({ theme }) => (
        <header className={`header-${theme}`}>
          <h1>My App</h1>
        </header>
      )}
    </ThemeContext.Consumer>
  );
}

function Content() {
  // Consumer using hook
  const { theme, toggleTheme } = useContext(ThemeContext);
  
  return (
    <div className={`content-${theme}`}>
      <p>Current theme: {theme}</p>
      <button onClick={toggleTheme}>Toggle Theme</button>
    </div>
  );
}
```

### Multiple Contexts
```jsx
const ThemeContext = createContext('light');
const UserContext = createContext(null);

function App() {
  const [theme, setTheme] = useState('light');
  const [user, setUser] = useState({ name: 'Guest' });
  
  return (
    <ThemeContext.Provider value={theme}>
      <UserContext.Provider value={user}>
        <Layout />
      </UserContext.Provider>
    </ThemeContext.Provider>
  );
}

function ProfilePage() {
  const theme = useContext(ThemeContext);
  const user = useContext(UserContext);
  
  return (
    <div className={theme}>
      <h2>{user.name}'s Profile</h2>
      {/* ... */}
    </div>
  );
}
```

## Refs

### Creating and Accessing Refs
```jsx
import { useRef, useEffect } from 'react';

function TextInput() {
  const inputRef = useRef(null);
  
  useEffect(() => {
    // Focus the input on component mount
    inputRef.current.focus();
  }, []);
  
  return <input ref={inputRef} type="text" />;
}
```

### Refs in Class Components
```jsx
class AutoFocusInput extends React.Component {
  constructor(props) {
    super(props);
    this.inputRef = React.createRef();
  }
  
  componentDidMount() {
    this.inputRef.current.focus();
  }
  
  render() {
    return <input ref={this.inputRef} />;
  }
}
```

### Forwarding Refs
```jsx
import { forwardRef } from 'react';

const FancyButton = forwardRef((props, ref) => (
  <button ref={ref} className="fancy-button" {...props}>
    {props.children}
  </button>
));

// Parent component
function Form() {
  const buttonRef = useRef(null);
  
  useEffect(() => {
    console.log('Button element:', buttonRef.current);
  }, []);
  
  return <FancyButton ref={buttonRef}>Click me!</FancyButton>;
}
```

### Callback Refs
```jsx
function MeasureNode() {
  const [height, setHeight] = useState(0);
  
  const measuredRef = useCallback(node => {
    if (node !== null) {
      setHeight(node.getBoundingClientRect().height);
    }
  }, []);
  
  return (
    <>
      <h1 ref={measuredRef}>Hello, world</h1>
      <p>The above header is {Math.round(height)}px tall</p>
    </>
  );
}
```

## Performance Optimization

### React.memo
```jsx
import { memo } from 'react';

const ExpensiveComponent = memo(function ExpensiveComponent({ value }) {
  console.log('Rendering ExpensiveComponent');
  
  // Expensive calculation or rendering
  const result = expensiveCalculation(value);
  
  return <div>{result}</div>;
});

// Custom comparison function
const areEqual = (prevProps, nextProps) => {
  return prevProps.value === nextProps.value;
};

const MemoizedWithCustomComparison = memo(ExpensiveComponent, areEqual);
```

### useCallback
```jsx
function ParentComponent() {
  const [count, setCount] = useState(0);
  const [text, setText] = useState('');
  
  // Without useCallback, this function would be recreated on every render
  // causing ChildComponent to re-render unnecessarily
  const handleClick = useCallback(() => {
    console.log('Button clicked');
  }, []); // Empty dependency array means function is created once
  
  return (
    <div>
      <input value={text} onChange={e => setText(e.target.value)} />
      <button onClick={() => setCount(count + 1)}>Count: {count}</button>
      <ChildComponent onButtonClick={handleClick} />
    </div>
  );
}

const ChildComponent = memo(function ChildComponent({ onButtonClick }) {
  console.log('ChildComponent rendered');
  return <button onClick={onButtonClick}>Click me</button>;
});
```

### useMemo
```jsx
function FilteredList({ items, filterTerm }) {
  // Memoize the filtered list so it only recalculates when items or filterTerm changes
  const filteredItems = useMemo(() => {
    console.log('Filtering items');
    return items.filter(item => 
      item.toLowerCase().includes(filterTerm.toLowerCase())
    );
  }, [items, filterTerm]);
  
  return (
    <ul>
      {filteredItems.map((item, index) => (
        <li key={index}>{item}</li>
      ))}
    </ul>
  );
}
```

### Lazy Loading with React.lazy and Suspense
```jsx
import { lazy, Suspense } from 'react';

// Lazy load a component
const LazyComponent = lazy(() => import('./LazyComponent'));

function App() {
  return (
    <div>
      <Suspense fallback={<div>Loading...</div>}>
        <LazyComponent />
      </Suspense>
    </div>
  );
}
```

### Code Splitting with React Router
```jsx
import { BrowserRouter as Router, Routes, Route } from 'react-router-dom';
import { lazy, Suspense } from 'react';

const Home = lazy(() => import('./Home'));
const About = lazy(() => import('./About'));
const Dashboard = lazy(() => import('./Dashboard'));

function App() {
  return (
    <Router>
      <Suspense fallback={<div>Loading...</div>}>
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/about" element={<About />} />
          <Route path="/dashboard" element={<Dashboard />} />
        </Routes>
      </Suspense>
    </Router>
  );
}
```

## Error Handling

### Error Boundaries
```jsx
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }
  
  static getDerivedStateFromError(error) {
    // Update state so the next render will show the fallback UI
    return { hasError: true };
  }
  
  componentDidCatch(error, errorInfo) {
    // Log the error to an error reporting service
    console.error('Error caught by boundary:', error, errorInfo);
  }
  
  render() {
    if (this.state.hasError) {
      // You can render any custom fallback UI
      return <h1>Something went wrong.</h1>;
    }
    
    return this.props.children;
  }
}

// Usage
function App() {
  return (
    <div>
      <ErrorBoundary>
        <MyComponent />
      </ErrorBoundary
