## Regula

We're gonna get Regula up and running, with something I thought of which is using a `mock_key.json` file I created, this is so you can sample it first, once you get a final `main.tf` (Terraform), you can edit as you please. Once Terraform calls Regula then Travis picks up the /POST and sees if it gets a response from Regula. This is a working example, and Travis is in my opinion the easiest development tool, to set Regula up on and get up and running with max uptime. 

## Kubernetes

There is a compliant Kubernetes pod in this repo I've also banged out, since we're using Travis there's not _alot_ of need for it, but I'm just trying to give you the most feasible option.

## Swagger 

There's a full `swagger.yaml` file within the repository.

## Travis CI 

All of the `.travis.yml` file was made by me (Montana Mendy). Just some crucial points to go over for the `.travis.yml`: 


```yaml
global:
    - 'PATH="$HOME/.local/bin:$PATH"'
    - REGULA_VERSION=1.6.0
before_script:
  - mkdir "$HOME/.local/bin"
  - >-
    curl -L
    "https://github.com/fugue/regula/releases/download/v${REGULA_VERSION}/regula_${REGULA_VERSION}_Linux_x86_64.tar.gz"
    | tar -xvz -C "$HOME/.local/bin"
```

We are going to change the `PATH` and use `cURL` to fetch the latest version of Regula, make sure you have it by going to https://regula.dev/. You can also make sure by running: 

```bash
regula version
``` 

In the CLI, but it will print out something like this: 


<img width="587" alt="Screen Shot 2022-05-07 at 7 09 41 AM" src="https://user-images.githubusercontent.com/20936398/167257899-26f89d71-dd03-43e5-89c2-a317a5323b04.png">

If the `regula` command isn't working you need to install Regula and the binary. Make sure you run the following:

```
brew tap fugue/regula
```

Once brew has symlinked `fugue/regula`, you can now start the install process:

```
brew install regula
```

If you want to upgrade regula, just run:

```
brew upgrade regula
```

## Usage

The usage does get pretty complex from the Regula side - I've been thinking about writing a bash script to help users and make it easier, if you think this is a good idea and want to see it just email me at [montana@linux.com](mailto:montana@linux.com), and or need any help in the mean time.
