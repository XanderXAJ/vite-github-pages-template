# Vite GitHub Pages deployment template

A simple but robust deployment of a Vite app to GitHub Pages. This project uses React + TypeScript as an example, but [you can easily switch to a different stack by following the instructions below](#switching-away-from-react--typescript).

[The Actions workflow](./.github/workflows/publish.yaml) is built from a combination of the [Vite documentation on static sites][vite-static], the documentation from the various GitHub Actions in use and my own wisdom.

[vite-static]: https://vitejs.dev/guide/static-deploy.html

## Create a new repo from this template

Press the _Use this template_ button (or equivalent for your language) near where the _Code_ button usually is.
[See GitHub's docs on creating a repository from a template][template-docs] for more details.

Once created, the repo's Pages configuration needs to be switched to GitHub Actions mode.
Go to the repo > _Settings_ > _Pages_ > and change _Source_ to **GitHub Actions**.
[See _Publishing with a custom GitHub Actions workflow_ in GitHub's documentation][pages-actions-publishing].

[template-docs]: https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-repository-from-a-template#creating-a-repository-from-a-template
[pages-actions-publishing]: https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site#publishing-with-a-custom-github-actions-workflow

## Switching away from React + TypeScript

1. Create a new project using the desired create-vite template by [following the Vite getting started guide][vite-get-started].
2. Copy the contents of the `.github` directory from this repo to your new project -- it'll work with any project generated from Vite's templates.

[vite-get-started]: https://vitejs.dev/guide/

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

[I've raised an MR to fix this](https://github.com/vitejs/vite/pull/12374) for anyone generating projects in the future. **Update 2023-03-11: Now merged!**
