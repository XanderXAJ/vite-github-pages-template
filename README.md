# Vite GitHub Pages deployment template

A simple but robust deployment of a Vite app to GitHub Pages.

[The Actions workflow](./.github/workflows/publish.yaml) is built from a combination of the [Vite documentation on static sites][vite-static], the documentation from the various GitHub Actions in use and my own wisdom.

[vite-static]: https://vitejs.dev/guide/static-deploy.html

## Create a new repo from this template

Press the _Use this template_ button (or equivalent for your language) near where the _Code_ button usually is.
[See GitHub's docs on creating a repository from a template][template-docs] for more details.

[template-docs]: https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-repository-from-a-template#creating-a-repository-from-a-template

## Fixing the broken Vite logo (fix now merged upstream!)

When production building the app for GitHub Pages, you'll notice the that the Vite logo ends up broken.

[I'm not the only one to have noticed](https://github.com/vitejs/vite/issues/10601). [A fix was suggested here](https://github.com/vitejs/vite/issues/7358), which I've implemented in this project.

This occurs because the `img` `src` directly refers to the build-generated `/vite.svg` and React does not preprocess `src` attributes to add the configured `base`.
The fix is to import `/vite.svg` just like any other resource in React:

```ts
import viteLogo from '/vite.svg'
// ...
<img src={viteLogo} className="logo" alt="Vite logo" />
```

To make things a bit more tricky, this issues does not show up when running `npm run dev` -- only when running `npm run build` (followed by `npm run preview` for testing locally):

```shell
# This will replicate the issue
npm run build -- --base /test
npm run preview -- --base /test

# This will NOT replicate the issue
npm run dev -- --base /test
```

[I've raised an MR to fix this](https://github.com/vitejs/vite/pull/12374) for anyone generating projects in the future. **Update 2022-03-11: Now merged!**
