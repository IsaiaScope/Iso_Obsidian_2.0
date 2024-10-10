---
tags:
  - Fincons/Filmbank
---

# Testing Shaka build

## Set Up Local testing (Changing WordPress configuration)

- [ ] Create/Open the *dds-repository-frontend-iframeplayer* with this origin `https://eu-west-1.console.aws.amazon.com/codesuite/codecommit/repositories/dds-repository-frontend-iframeplayer`, this repo is situated in CodeCommit section on AWS of Filmbank at Repositories List
- [ ] Run (to open a local server)

```bash
npm run dev:app
```

- [ ] _WordPress CASE_: Accessing by Client (instance Education/Quality Check)
  - [ ] Use Podman for Local container/Go to test environment
  - [ ] WordPress Dashboard of the instance changing config `Iframe player URL` option with `http://localhost:3000/player.html`
  - [ ] Now the iframe points to our local one

> console log need to switch console on the browser even selecting insted `top` to `player.html`

---

### Test New shaka builds

if you want to test shaka build from the node module just update the npm package in _dds-repository-frontend-iframeplayer_ and edit _src\app\app-player\shared\player\shaka.js_

```
import * as shaka from 'shaka-player/dist/shaka-player.ui.debug';

export default shaka;
```

now you are using that version

---
