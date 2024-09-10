1. [[react-test.pdf]]
2. [jest](https://jestjs.io/docs/tutorial-react) => already installed when create-react-app is used, check package json
3. examples

```jsx 
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import Greeting from './Greeting';

describe('Greeting component', () => {
  test('renders "Hello World" as a text', () => {
    // Arrange
    render(<Greeting />);

    // Act
    // ... nothing

    // Assert
    const helloWorldElement = screen.getByText('Hello World!');
    expect(helloWorldElement).toBeInTheDocument();
  });

  test('renders "good to see" you if the button was NOT clicked', () => {
    render(<Greeting />);

    const outputElement = screen.getByText('good to see you', { exact: false });
    expect(outputElement).toBeInTheDocument();
  });

  test('renders "Changed!" if the button was clicked', () => {
    // Arrange
    render(<Greeting />);

    // Act
    const buttonElement = screen.getByRole('button');
    userEvent.click(buttonElement)

    // Assert
    const outputElement = screen.getByText('Changed!');
    expect(outputElement).toBeInTheDocument();
  });

  test('does not render "good to see you" if the button was clicked', () => {
     // Arrange
     render(<Greeting />);

     // Act
     const buttonElement = screen.getByRole('button');
     userEvent.click(buttonElement)
 
     // Assert
     const outputElement = screen.queryByText('good to see you', { exact: false });
     expect(outputElement).toBeNull();
  });
});
```

---

1. for async code its possible to create a fake api call (mock) to simulate success or errors
```jsx
import { render, screen } from '@testing-library/react';
import Async from './Async';

describe('Async component', () => {
  test('renders posts if request succeeds', async () => {
    window.fetch = jest.fn();
    window.fetch.mockResolvedValueOnce({
      json: async () => [{ id: 'p1', title: 'First post' }],
    });
    render(<Async />);

    const listItemElements = await screen.findAllByRole('listitem');
    expect(listItemElements).not.toHaveLength(0);
  });
});
```