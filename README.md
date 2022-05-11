[![Build Status](https://app.travis-ci.com/Montana/travis-regula.svg?branch=master)](https://app.travis-ci.com/Montana/travis-regula)

## Regula + Travis CI

We're gonna get Regula up and running, with something I thought of which is using a `mock_key.json` file I created, this is so you can sample it first, once you get a final `main.tf` (Terraform), you can edit as you please. Once Terraform calls Regula then Travis picks up the /POST and sees if it gets a response from Regula. This is a working example, and Travis is in my opinion the easiest development tool, to set Regula up on and get up and running with max uptime. 

## Terraform

We can grab Terraform by putting this in our `script` hook in our `travis.yml` file:

```bash
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
sudo apt-get update && sudo apt-get install terraform
```

## Kubernetes

There is a compliant Kubernetes pod in this repo I've also banged out, since we're using Travis there's not _alot_ of need for it, but I'm just trying to give you the most feasible option.

## Swagger 

There's a full `swagger.yaml` file within the repository.

## Travis CI 

All of the `.travis.yml` file was made by me (Montana Mendy). Just some crucial points to go over for the `.travis.yml`: 


```yaml
branches:
  only:
    - master
    - /^(cherry-pick-)?backport-\d+-to-/
addons:
  apt:
    packages:
      - moreutils
env:
  global:
    - 'PATH="$HOME/.local/bin:$PATH"'
    - REGULA_VERSION=1.6.0
install:
  - >-
    if [[ "${TRAVIS_COMMIT_MESSAGE}" = *"[Build latest]"* ]]; then export
    BUILD_VERSION="$(cat packaging/version | cut -d'-' -f1,2 | sed -e
    's/-/./g').latest"; fi;
before_script:
  - mkdir "$HOME/.local/bin"
  - >-
    curl -L
    "https://github.com/fugue/regula/releases/download/v${REGULA_VERSION}/regula_${REGULA_VERSION}_Linux_x86_64.tar.gz"
    | tar -xvz -C "$HOME/.local/bin"
  - 'RANGE1=`echo "$TRAVIS_COMMIT_RANGE" | awk ''{n=split($1,a,".");print a[1]}''`'
script:
  - REGULA_OUTPUT="$(mktemp)"
  - (regula run -f json || true) | tee "$REGULA_OUTPUT"
  - REGULA_RULES_PASSED="$(jq -r '.summary.rule_results.PASS' "$REGULA_OUTPUT")"
  - REGULA_RULES_FAILED="$(jq -r '.summary.rule_results.FAIL' "$REGULA_OUTPUT")"
  - regula -v
  - regula init
  - regula run
  - regula run --format table
  - >-
    echo "${REGULA_RULES_PASSED} rules passed, ${REGULA_RULES_FAILED} rules
    failed" >&2
  - 'if [[ "$REGULA_RULES_FAILED" -gt 0 ]]; then exit 1; fi'
after_deploy:
  - >-
    if [ -n "${BUILDER_NAME}" ]; then rm -rf /home/${BUILDER_NAME}/* && echo
    "Cleared /home/${BUILDER_NAME} directory" || echo "Failed to clean
    /home/${BUILDER_NAME} directory"; fi;
  - 'if [ -d "${PACKAGES_DIRECTORY}" ]; then rm -rf "${PACKAGES_DIRECTORY}"; fi;'
  - >-
    if: "((branch IN (master, develop) && type = push) OR branch =~ /.*env.*/ OR
    commit_message =~ /\\[recreate env\\]/) AND commit_message !~ /\\[delete
    env\\]/ AND type != cron AND commit_message !~ /\\[execute .*. test\\]/ AND
    commit_message !~ /\\[start recreate scheduler\\]/"
  - >-
  - regula run mock_key.json
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
