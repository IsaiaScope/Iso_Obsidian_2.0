# isonfirecli

> CLI tool to clone useful code from my repo

## Commands

### copy

Clone from GitHub repo, data is taken from _data-on-fire_ folder that is also the default value

#### Usage

```
npx isonfirecli copy <path>
```

### see

See all available paths from GitHub repository, giving you also _copy_ command for each possible path

#### Usage

```
npx isonfirecli see
```

### add

Add personal access token for GitHub API that gonna be sto on your local machine
it's need to raise the rate limit of GitHub API from 60 to 5000 requests per hour

- [Create your personal token](https://github.com/settings/tokens)
  - Personal access tokens - Tokens (classic)
  - enable all options
  - set no expiration

#### Usage

```
npx isonfirecli add -t <token>
```

## Thanks

This is my fist pack and it's far to be perfect, but _The first love is never forgotten_.

## License

Licensed under the [MIT license](https://github.com/shadcn/ui/blob/main/LICENSE.md).
