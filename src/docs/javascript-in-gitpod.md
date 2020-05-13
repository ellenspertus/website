# JavaScript in Gitpod

Gitpod comes with great built-in support for JavaScript and tools like Node, Npm and Yarn pre-installed. Still, depending on your project you might want to further optimize the experience.

## Examples

Here are a few JavaScript example projects that are automated with Gitpod:

Repository | Description | Try it
---|---|---
[Mozilla pdf.js](https://github.com/mozilla/pdf.js) | PDF.js is a Portable Document Format (PDF) viewer that is built with HTML5. | [![Open in Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#https://github.com/mozilla/pdf.js)
[Tesseract.js](https://github.com/naptha/tesseract.js) | Pure Javascript OCR for more than 100 Languages. | [![Open in Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#https://github.com/naptha/tesseract.js)

## Start tasks

Almost the entirety of JavaScript projects these days use some sort of build tool for things like bundling, linting, code-splitting and so on and they also use a package manager, either it is Npm or Yarn for managing dependencies. 

You can automate the process of installing dependencies and starting any tasks like `build`, `lint`, `test` and so on at the workspace startup, for doing so please create a `.gitpod.yml` file in the root of your project and add the tasks you want to be automated. An example might look like this:

```yaml
tasks:
    - init: npm install && npm run build
      command: npm run dev
```

<span aria-hidden="true">👆</span> In the above example, we are telling Gitpod to run what is in the `init` phase at the time of workspace initialization and then afterwards run whatever is in the `command` phase. 

You can read more about start tasks [here](/docs/config-start-tasks/). 

## Node Versions

Gitpod comes with node version 12.16.3 pre-installed but let's say your project uses a different version of node(let say 8 for example), well the good news is that Gitpod also comes with nvm(a tool used to manage multiple active Node versions) installed. To install and configure the desired version of node create a `.gitpod.Dockerfile` and add the following to it:

```dockerfile
FROM gitpod/workspace-full-vnc:latest

RUN bash -c ". .nvm/nvm.sh \
    && nvm install 8 \
    && nvm use 8 \
    && nvm alias default 8"

RUN echo "nvm use default >/dev/null" >> /home/gitpod/.bashrc.d/51-nvm-fix
```

and then in your `.gitpod.yml` reference the image as shown below:

```yaml
image: 
    file: .gitpod.Dockerfile
```

and then after commiting your changes start a new workspace, the version for that workspace will be what you've specified in `.gitpod.Dockerfile`.

## Using Eslint for linting

If your project's `package.json` does not mention Eslint as a dependency then you have to install it first. For installing it add the following to the end of the `- init` phase of your `.gitpod.yml` as shown:

```yaml
tasks:
    - init: npm install && npm run build && npm install -g eslint
```

and then also add the following in .gitpod.yml as well:

```yaml
vscode:
  extensions:
    - dbaeumer.vscode-eslint@2.0.15:/v3eRFwBI38JLZJv5ExY5g==
```



