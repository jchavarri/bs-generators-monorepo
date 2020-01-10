# Issues when using BuckleScript generators in a monorepo

This repo illustrates the problem of BuckleScript generators not being
"transitive", which kind of breaks the point of having a monorepo setup,
where `bsb -make-world` is supposed to fully build an application and its
dependencies completely.

### To reproduce

```bash
git clone https://github.com/jchavarri/bs-generators-monorepo
cd bs-generators-monorepo
yarn
cd app
yarn build
```

The build will show error:

```
ninja: error: '/Users/javi/Development/github/bs-generators-monorepo/node_modules/atd-lib/src/meetup_bs.ml', needed by 'src/meetup_bs.mlast', missing and no known rule to make it
Failure: /Users/javi/Development/github/bs-generators-monorepo/node_modules/bs-platform/lib/ninja.exe
 Location: /Users/javi/Development/github/bs-generators-monorepo/node_modules/atd-lib/lib/bs
error Command failed with exit code 2.
```

### To fix it

We should build `atd-lib` first manually:

```bash
cd ../atd-lib
yarn build
cd ../app
yarn build
```
