<a name="readme-top"></a>

<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about-the-project">About The Project</a>
    </li>
    <li>
      <a href="#getting-started">Getting Started</a>
      <ul>
        <li><a href="#prerequisites">Prerequisites</a></li>
        <li><a href="#installation">Installation</a></li>
      </ul>
    </li>
    <li><a href="#what-is-included-out-of-the-box">What is included out-of-the-box</a></li>
    <li><a href="#how-to-code-like-a-pro">How to code like a PRO</a></li>
      <ul>
        <li><a href="#use-phpstan">Use PHPStan</a></li>
        <li><a href="#use-php_codeSniffer">Use PHP_CodeSniffer</a></li>
        <li><a href="#use-xdebug">Use Xdebug</a></li>
        <li><a href="#setup-alias-for-fast-start-of-development">Setup alias for fast start of development</a></li>
        <li><a href="#globally-gitignore-your-ide">Globally gitignore your IDE</a></li>
      </ul>
    <li><a href="#deploy-to-cloud">Deploy to cloud</a></li>
    <li><a href="#pictures">Pictures</a></li>
    <li><a href="#contributing">Contributing</a></li>
    <li><a href="#roadmap">Roadmap</a></li>
    <li><a href="#frequently-asked-questions">Frequently Asked Questions</a></li>
    <li><a href="#license">License</a></li>
  </ol>
</details>

<!-- ABOUT THE PROJECT -->
## About The Project
A template to jumpstart your new greenfield project. If you are a startup or an entrepreneur thinking to start a new project with php and symfony as a BE and Typescript and React as a FE, consider to use Symfony React skeleton and save weeks of development.

This project covers common (repetitive) parts of most greenfield projects. Fully functional and communicating BE and FE parts via REST, CI pipeline with robust tests and helm deployment.

On top of it you will receive BE, FE and DevOps best practices already implemented. You software development team can use these guidance to deliver sustainable and maintainable top quality product.

Don't forget to give the project a star!

<a href="https://34.76.96.12/">Live Demo</a>

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- GETTING STARTED -->
## Getting Started

### Prerequisites

* [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
* [Docker (and docker compose) installed.](https://docs.docker.com/engine/install/) You don't need Docker Desktop for this project. 
* [pnpm](https://pnpm.io/installation)
* OPTIONAL: [helm](https://helm.sh/docs/intro/install/) for deploying to kubernetes cluster 

### Installation

_Below is an example of how you can instruct your audience on installing and setting up your app. This template doesn't rely on any external dependencies or services._

1. Clone the repo
   ```sh
   git clone https://github.com/petrzivny/symfony-react-skeleton.git
   cd symfony-react-skeleton
   ```
2. Setup environmental variables by using prepared template
   ```sh
   mv api/.env.local.dist api/.env.local
   ```
3. Build BE docker images and run them as docker containers in dev mode
   ```sh
   cd .docker && docker compose --env-file ../api/.env.local up -d
   ```
4. Run FE react app in dev mode
   ```sh
   cd fe && pnpm run dev
   ```
5. Visit http://localhost:5173/
Next time you only need to perform points 3., 4. and 5. to start developing. You can set up alias for it, see <a href="#how-to-code-like-a-pro">How to code like a PRO</a> section.
<p align="right">(<a href="#readme-top">back to top</a>)</p>

## What is included out-of-the-box
1. Docker to run complete dev environment (php + nginx + PostgreSQL)
2. Symfony framework as a backed REST api
   - Independent on any used frontend. Communicating via REST (_best-practice_ 🎯)
   - [x] Symfony opcache preloading with JIT in prod (_performance_ ⏩).
   - [x] Xdebug setup to debug both html requests and CLI commands.
   - [x] Phpstan in a very strict level. Including [shipmonk-rules](https://github.com/shipmonk-rnd/phpstan-rules).
   - [x] PHP_CodeSniffer in a very strict level (lots of rules are my personal "taste", feel free to change/remove them) .Including [phpstan-strict-rules](https://github.com/phpstan/phpstan-strict-rules) and [slevomat-coding-standard](https://github.com/slevomat/coding-standard)
   - [x] Psalm.
   - [x] Roave/SecurityAdvisories to prevent using dependencies with known security vulnerabilities.
   - [x] PHPUnit Unit tests.
   - [x] PHPUnit Functional tests (including smoke tests).
   - [x] Other linters (Composer, Yaml, Symfony container).
   - [x] Php-fpm access proper logging (json format, GCP [LogEntry](https://cloud.google.com/logging/docs/reference/v2/rest/v2/LogEntry#httprequest) compatible, correct severity, trying to fix https://bugs.php.net/bug.php?id=73886)
   - [x] Symfony monolog proper logging (json format, GCP compatible using GoogleCloudLoggingFormatter)
3. React framework as a frontend SPA
   - Independent on any used backend. Communicating via REST (_best-practice_ 🎯)
   - [x] Typescript
   - [x] Eslint
   - [x] Vite
   - [x] React Query and Axios for proper data fetching including caching
   - [x] wsc (should be 20x faster than Babel but see the current [caveats](https://github.com/vitejs/vite-plugin-react-swc#caveats))
   - [x] Prettier
4. DevOps: CI pipeline to build both test and prod images, test them and push prod images to registry
   - [x] Run all tests from point 2 on final (test) docker image (_best-practice_ 👍).
   - [x] If everything passes there are php and nginx environment agnostic (_best-practice_ 👍) containers ready to be shipped into any environment (including prod of course).
   - [x] Pipeline expects self-hosted GitHub runner(s). [See](https://docs.github.com/en/actions/hosting-your-own-runners/managing-self-hosted-runners/adding-self-hosted-runners) for more information.
5. DevOps: Kubernetes deploy manifests
   - [x] Platform agnostic. As long as there is a Kubernetes you can use simple config files in `.deploy` dir to deploy to your environment (just a test, not production ready!).
   - [x] One pod for nginx, one pod for php for better scalability (_best-practice_ 👍).
   - [x] Ingress to connect your kubernetes cluster with outside world (you can use platform Load Balancer, but it is usually billed).
<p align="right">(<a href="#readme-top">back to top</a>)</p>

## How to code like a PRO
All the following setup information are **optional**, but highly **recommended**. It should not take you more than 10 minutes of setup time, and it will save you hours of your time in a future. Senior developer uses all of them. Period. 
### Use PHPStan
PHPStan is configured out of the box. For a better DX configure your IDE too. See [PHPStan usage](documentation%2Fphpstan%2FREADME.md) for more details.
### Use PHP_CodeSniffer
PHP_CodeSniffer is configured out of the box. For a better DX configure your IDE too. See [PHP_CodeSniffer usage](documentation%2Fphpcs%2FREADME.md) for more details.
### Use Xdebug
Xdebug is configured out of the box on the container side. You need to configure your IDE too. See [Xdebug usage](documentation%2Fxdebug%2FREADME.md) for more details.
### Setup alias for fast start of development
Setup alias in your shell to deliver points 3., 4. and 5. from <a href="#installation">Installation</a>
For example what I have in my `.zshrc` file: 
```shell
alias dcs="cd ~/Projects/personal/symfony-react-skeleton/.docker/ && docker compose --env-file ../api/.env.local up -d && cd ../fe && pnpm run dev"
alias des='docker exec -it symfony-react-skeleton_php sh -l'
```
I use `dcs` to start complete FE+BE dev environment and `des` to docker exec into php container. 
### Globally gitignore your IDE
Create a global `.gitignore` file in a parent directory for your project and add `.idea` line in it (.idea is for PhpStorm, if you use another editor, change the directory name accordingly). This directory created by PhpStorm in every project should not be versioned but should not be included in project's scope .gitignore file either (_best-practice_ 👍).

<p align="right">(<a href="#readme-top">back to top</a>)</p>



### Deploy to cloud
This example is configured out-of-the-box for [this infrastructure](https://github.com/petrzivny/infrastructure-skeleton)
1. Provision your infrastructure by using mentioned infrastructure skeleton. Save output values from terraform apply. You will use them in point 3. and 4. (or use your own infrastructure).
2. `cd .deploy`
3. Edit Chart.yaml and values.yaml files (use outputs from infrastructure terraform provisioning).
4. `helm install your-app-name . --namespace {app_k8_namespace} --create-namespace`

### Pictures
![ci-pipeline.png](documentation%2Fimages%2Fci-pipeline.png)

<!-- CONTRIBUTING -->
## Contributing
I **greatly appreciate** all suggestions and contributions. Contributions are what make the open source community such an amazing place to learn, inspire, and create.

If you have a suggestion that would make this better, please fork the repo and create a pull request. You can also simply open an issue with the tag "enhancement".
Don't forget to give the project a star! Thanks again!

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- ROADMAP -->
## Roadmap

- [x] Add Changelog
- [x] Add back to top links
- [ ] Add Additional Templates w/ Examples
- [ ] Add "components" document to easily copy & paste sections of the readme
- [ ] Multi-language Support
   - [ ] Chinese
   - [ ] Spanish

See the [open issues](https://github.com/othneildrew/Best-README-Template/issues) for a full list of proposed features (and known issues).

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Frequently Asked Questions
- [How to run CI tests locally?](#how-to-run-ci-tests-locally)
#### How to run CI tests locally?
A developer can run all BE tests at once `composer test` or only selected BE test can be ran e.g. `composer phpstan`. Commands should be run inside php container.
A developer can run all FE tests at once `pnpm run test` or only selected FE test can be ran e.g. `pnpm run lint`.
#### How to run BE application in prod mode locally?
`cd .docker && docker compose -f docker-compose-prod.yaml up -d`


<!-- LICENSE -->
## License
Distributed under the MIT License. Use it however you want. And if you like it, you can give me a star at GutHub. 

<p align="right">(<a href="#readme-top">back to top</a>)</p>
