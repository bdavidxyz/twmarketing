# Marketing Tw classes

## Getting started

Make sure that you have Node (>= 12.3.0) and NPM locally installed. Also make sure that you have installed [HUGO](https://gohugo.io/getting-started/quick-start/) After you've unzipped the folder, go to the root of the project and run:

```
npm install
```

Then run `npm run start` to start the development server.

## Building for production

If you want to generate the production files you have to run `npm run build`.

## Deploying to GitHub Pages

To deploy the site to GitHub Pages, you can use the provided GitHub Actions workflow.

1. Create a new GitHub repository for your project
2. Add the GitHub Actions workflow by creating `.github/workflows/gh-pages.yml` with the content from `DEPLOYMENT.md`
3. Enable GitHub Pages in your repository settings:
   - Go to Settings > Pages
   - Select "GitHub Actions" as the source
4. Push your changes to the main branch
5. The workflow will automatically build and deploy your site to GitHub Pages

For detailed deployment instructions, see [DEPLOYMENT.md](DEPLOYMENT.md).
