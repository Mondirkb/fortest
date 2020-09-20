Deployment

Before the deployment begins, checkout the file _config.yml and make sure the url is configured correctly. Furthermore, if you prefer the project site and don't use a custom domain, or you want to visit your website with a base url on a web server other than GitHub Pages, remember to change the baseurl to your project name that starting with a slash. For example, /project.

Assuming you have already gone through the initialization, you can now choose any of the following methods to deploy your website.
Deploy on GitHub Pages

For security reasons, GitHub Pages build runs on safe mode, which restricts us from using tool scripts to generate additional page files. Therefore, we can use GitHub Actions to build the site, store the built site files on a new branch, and use that branch as the source of the Pages service.

    Push any commit to origin/master to trigger the GitHub Actions workflow. Once the build is complete, a new remote branch called gh-pages will appear, which is used to store the built site files.
    Unless you prefer to project sites, rename your repository to <username>.github.io on GitHub.
    Choose branch gh-pages as the publishing source for your GitHub Pages site.
    Visit your website at the address indicated by GitHub.

Deploy on Other Platforms

On platforms other than GitHub, e.g. GitLab, we cannot enjoy the convenience of GitHub Actions. However, we have a tool to make up for this shortcoming.

Commit the changes of your repository first, then run the publish script:
