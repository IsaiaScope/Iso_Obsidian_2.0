---
tags:
---

# Handling Data Fetching in Tests

The whole goal of testing this component is to better understand how we deal with data fetching inside

of a testing environment.

So I want to take a look at the implementation of the use repositories hook right away.

As you might guess, this hook is going to eventually make a data fetching request.

possibles solution
![[Pasted image 20240320094434.png]]

```tsx
// useRepositories hook
import axios from 'axios';
import useSWR from 'swr';

async function repositoriesFetcher([url, searchQuery]) {
  const res = await axios.get(url, {
    params: {
      q: searchQuery || '',
    },
  });

  return res.data.items;
}

export default function useRepositories(searchQuery) {
  const { data, error, isLoading } = useSWR(
    searchQuery && ['/api/repositories', searchQuery],
    repositoriesFetcher
  );

  return {
    data,
    isLoading,
    error,
  };
}
```

```tsx
// HomeRoute
import Hero from '../components/Hero';
import RepositoriesTable from '../components/repositories/RepositoriesTable';
import useRepositories from '../hooks/useRepositories';

function HomeRoute() {
  const { data: jsRepos } = useRepositories('stars:>10000 language:javascript');
  const { data: tsRepos } = useRepositories('stars:>10000 language:typescript');
  const { data: rustRepos } = useRepositories('stars:>10000 language:rust');
  const { data: goRepos } = useRepositories('stars:>10000 language:go');
  const { data: pythonRepos } = useRepositories('stars:>10000 language:python');
  const { data: javaRepos } = useRepositories('stars:>10000 language:java');

  return (
    <div>
      <Hero />
      <div className="container mx-auto py-8 grid grid-cols-1 gap-4 md:grid-cols-2">
        <RepositoriesTable
          label="Most Popular Javascript"
          repositories={jsRepos}
        />
        <RepositoriesTable
          label="Most Popular Typescript"
          repositories={tsRepos}
        />

        <RepositoriesTable label="Most Popular Rust" repositories={rustRepos} />
        <RepositoriesTable label="Most Popular Go" repositories={goRepos} />
        <RepositoriesTable
          label="Most Popular Python"
          repositories={pythonRepos}
        />
        <RepositoriesTable label="Most Popular Java" repositories={javaRepos} />
      </div>
    </div>
  );
}

export default HomeRoute;
```

Very commonly used one is called mz W, which is short for mock service worker.

The goal of this library is in a test environment.

We're still going to use Axios or Fetch, but rather than letting the request that these things issue,

rather than sending that out to some outside API, the library is going to intercept the request.

It's going to say, Hey, don't actually send that out, it's going to grab the request and then automatically

respond to it.

So no request actually leaves our test environment, but we still get the benefit of testing a huge

amount of our code, everything from the component to the hook to the request itself.

So the request is still kind of issued.

It just doesn't actually get sent out.

We're going to say, Hey, if you ever intercept a request that has this route, then just go ahead

and immediately respond to it and resolve it with some data.

So the code that you and I have to write you and I have to write in this particular case, well, it's

not going to be far off from this.

We are pretty much going to say if there is ever a get request that is sent off to API slash repositories,

then just go ahead and respond, send back some JSON, respond with an array of objects where each object

represents a repository.

![[Pasted image 20240320094915.png]]


```tsx
// HomeRoute.test
import { render, screen } from '@testing-library/react';
import { setupServer } from 'msw/node';
import { rest } from 'msw';
import { MemoryRouter } from 'react-router-dom';
import HomeRoute from './HomeRoute';
import { createServer } from '../test/server';

createServer([
  {
    path: '/api/repositories',
    res: (req) => {
      const language = req.url.searchParams.get('q').split('language:')[1];
      return {
        items: [
          { id: 1, full_name: `${language}_one` },
          { id: 2, full_name: `${language}_two` },
        ],
      };
    },
  },
]);

test('renders two links for each language', async () => {
  render(
    <MemoryRouter>
      <HomeRoute />
    </MemoryRouter>
  );

  // Loop over each language
  const languages = [
    'javascript',
    'typescript',
    'rust',
    'go',
    'python',
    'java',
  ];

  for (let language of languages) {
    // For each language, make sure we see two links
    const links = await screen.findAllByRole('link', {
      name: new RegExp(`${language}_`),
    });



    expect(links).toHaveLength(2);
    expect(links[0]).toHaveTextContent(`${language}_one`);
    expect(links[1]).toHaveTextContent(`${language}_two`);
    expect(links[0]).toHaveAttribute('href', `/repositories/${language}_one`);
    expect(links[1]).toHaveAttribute('href', `/repositories/${language}_two`);
  }
});

const pause = () =>
  new Promise((resolve) => {
    setTimeout(resolve, 100);
  });
```

---
