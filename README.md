## Regula

We're gonna get Regula up and running, with something I thought of which is using a `mock_json` file I created, this is called by Regula, then Travis picks up the /POST and sees if it gets a response from Regula. This is a working example. 

## Travis CI 

Just some crucial points to go over for the `.travis.yml`: 


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

The usage does get pretty complex from the Regula side - I've been thinking about writing a bash script to help users and make it easier, if you think this is a good idea and want to see it just email me at [montana@linux.com](mailto:montana@linux.com).
