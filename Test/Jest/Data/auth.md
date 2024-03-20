---
tags:
---

# auth

Now the next thing I want to point out is that for each of these different tests, we're going to have

to somehow fake an API response because our component when rendered, is going to make a request to

figure out whether or not the user is currently signed in.

So it's very clear right away that we need to understand how the authentication API works and how we're

going to actually fake that response.


So let's take a look at how we're going to limit or kind of scope down how long this first server is

running.

Make sure it's only running for the first two tests and same thing for the second server down here.

So we're going to use a technique called test nesting right above this first comment, I'm going to

add in a describe and I'm going to pass in as a first argument a string of when user is not signed in.

The second argument is going to be a function.

I'm going to take this first comment and everything down to the first two tests.

So we have set this up by using this describe function.

A described function lets us nest tests.

This has two important results.

First, it allows us to just organize our code a little bit better inside of a test file and describe

some overall conditions or just kind of mark a set of tests as being kind of about one precondition.

So in this case, if another engineer saw this described function, they would immediately understand

that all the tasks inside of this function, this describe block, is how we refer to it, are about

the factor, the case in which a user is signed in.

And then up here on the first describe, another engineer would see this and understand all the tests

inside of here are about when a user is not signed in.

So that's the first nice thing about the describe function.

The second nice thing is that it scopes the be for all the before each, the after each and the after

all functions.

So if these functions are called at the kind of top level of a file, they're going to apply to all

the tests inside that file.

But when these functions get called inside of a described block, they're going to only apply to the

tests inside of that describe.


Okay, So this technique is going to allow us to make sure that we create one server that gives us a

very specific kind of response for these tests and then a very different server that is going to give

us a totally different kind of response for these tests, which is exactly what we are looking for.




## Setting Up a Debugger
![[Pasted image 20240320150013.png]]
package json add test:debug script
now is possible to add debugger in test files just writing `debugger`
```shell
react-scripts --inspect-brk test --runInBand --no-cache
```
## note

describe.only() or test.only() make a test to run itself and ignore other ones 



```tsx
import { render, screen } from '@testing-library/react';
import { MemoryRouter } from 'react-router-dom';
import { SWRConfig } from 'swr';
import { createServer } from '../../test/server';
import AuthButtons from './AuthButtons';

async function renderComponent() {
  render(
    <SWRConfig value={{ provider: () => new Map() }}>
      <MemoryRouter>
        <AuthButtons />
      </MemoryRouter>
    </SWRConfig>
  );

  await screen.findAllByRole('link');
}



describe('when user is signed in', () => {
  // createServer() ---> GET '/api/user' ---> { user: { id: 3, email: 'asdf@a.com' }}
  createServer([
    {
      path: '/api/user',
      res: () => {
        return { user: { id: 3, email: 'asdf@asdf.com' } };
      },
    },
  ]);


  test('sign in and sign up are not visible', async () => {
    await renderComponent();

    const signInButton = screen.queryByRole('link', {
      name: /sign in/i,
    });

    const signUpButton = screen.queryByRole('link', {
      name: /sign up/i,
    });

    expect(signInButton).not.toBeInTheDocument();
    expect(signUpButton).not.toBeInTheDocument();

  });



  test('sign out is visible', async () => {
    await renderComponent();

    const signOutButton = screen.getByRole('link', {
      name: /sign out/i,
    });

    expect(signOutButton).toBeInTheDocument();
    expect(signOutButton).toHaveAttribute('href', '/signout');
  });
});



describe('when user is not signed in', () => {
  // createServer() ---> GET '/api/user' ---> { user: null }
  createServer([
    {
      path: '/api/user',
      res: () => {
        return { user: null };
      },
    },
  ]);

  test('sign in and sign up are visible', async () => {
    await renderComponent();

    const signInButton = screen.getByRole('link', {
      name: /sign in/i,
    });

    const signUpButton = screen.getByRole('link', {
      name: /sign up/i,
    });


    expect(signInButton).toBeInTheDocument();
    expect(signInButton).toHaveAttribute('href', '/signin');
    expect(signUpButton).toBeInTheDocument();
    expect(signUpButton).toHaveAttribute('href', '/signup');
  });



  test('sign out is not visible', async () => {
    await renderComponent();

    const signOutButton = screen.queryByRole('link', {
      name: /sign out/i,
    });

    expect(signOutButton).not.toBeInTheDocument();
  });
});
```

---
