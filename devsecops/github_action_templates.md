# Github action templates

Depending on how the project is structured, other actions or tools may have to be used.

The shown actions are suitable for a JavaScript project, for example React.js with npm.

The used tools are all open source and free of charge. If available, Github Advanced Security Features can also be used.

Basic understanding of Github actions is necessary.

## Pre-Commit Hooks

Catch bugs or vulnerabilities before they get into the repo.

Steps:

- Check for secrets

- Quality checks: code formatting, commit messages

One way to use git hooks locally in a Node environment is by using a library called husky. Install husky as dev-dependency.

Put these lines of code into your package.json.

```
"husky": {
    "hooks": {
      "pre-commit": "gitleaks detect -v -s ./src/ --no-git -c gitleaks.toml"
    }
  },
```

It may happen that the hooks do not work - two solutions:

- Remove node_modules and then install the packages again

- Remove the git hooks folder, uninstall husky and install it again

## Upload build artifact

Github provides a default action for uploading artifacts.

```
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Build Projects
        if: github.event_name == 'push'
        run: npm run build

      - name: Upload Build Folder
        if: github.event_name == 'push'
        uses: actions/upload-artifact@v1
        with:
          name: build
          path: build
```

## Static Application Security Testing (SAST)

<b>Semgrep</b> is a fast, open-source, static analysis tool for finding bugs and enforcing code standards at editor, commit, and CI time.

- Template Github actions config: https://semgrep.dev/docs/semgrep-ci/sample-ci-configs/#github-actions

- https://semgrep.dev/

- Semgrep rule sets: https://semgrep.dev/explore

```
  semgrep:
    runs-on: ubuntu-latest
    needs: [Build]
    steps:
      # Fetch project source
      - uses: actions/checkout@v2

      - uses: returntocorp/semgrep-action@v1
        id: semgrep
        continue-on-error: true
        with:
          config: >- # more at semgrep.dev/explore
            p/security-audit
            p/owasp-top-ten
            p/xss
            p/r2c
          generateSarif: "1"

        # == Optional settings in the `with:` block

        # Instead of `config:`, use rules set in Semgrep App.
        # Get your token from semgrep.dev/manage/settings.
        #   publishToken: ${{ secrets.SEMGREP_APP_TOKEN }}

        # Never fail the build due to findings on pushes.
        # Instead, just collect findings for semgrep.dev/manage/findings
        #   auditOn: push

        # Upload findings to GitHub Advanced Security Dashboard [step 1/2]
        # See also the next step.
        #   generateSarif: "1"

        # Change job timeout (default is 1800 seconds; set to 0 to disable)
        # env:
        #   SEMGREP_TIMEOUT: 300

      # Upload findings to GitHub Advanced Security Dashboard [step 2/2]
      # - name: Upload SARIF file for GitHub Advanced Security Dashboard
      #   uses: github/codeql-action/upload-sarif@v1
      #   with:
      #     sarif_file: semgrep.sarif
      #   if: always()

      - name: Upload semgrep output
        uses: actions/upload-artifact@v1
        with:
          name: semgrep.sarif
          path: semgrep.sarif

      - name: Check on failures
        if: steps.semgrep.outcome != 'success'
        uses: actions/github-script@v3
        with:
          script: |
            core.setFailed('Detected semgrep findings')
```

## Software Composition Analysis (SCA)

It typically looks for a lock file then performs a build to fetch upstream dependency information. In the case of containers, Dependency Scanning uses the compatible manifest and reports only these declared software dependencies (and those installed as a sub-dependency). Dependency Scanning can not detect software dependencies that are pre-bundled into the container’s base image.

To identify pre-bundled dependencies, use Container Scanning.

<b>OWASP Dependency-Check</b> will be used to check for vulnerable dependencies.

- https://github.com/jeremylong/DependencyCheck

```
  dependency-check:
    runs-on: ubuntu-latest
    needs: [Build]
    steps:
      - uses: actions/checkout@v2

      - name: Dependency Check
        uses: dependency-check/Dependency-Check_Action@main
        id: dependencyCheck
        continue-on-error: true
        with:
          project: "test"
          path: "."
          format: "HTML"
          args: >
            --failOnCVSS 7
            --enableRetired

      - name: Dependency Check Results Upload
        uses: actions/upload-artifact@master
        id: dependencyCheckResults
        continue-on-error: true
        with:
          name: dependency-check-report
          path: ${{ github.workspace }}/reports

      - name: Check on failures
        if: steps.dependencyCheck.outcome != 'success' || steps.dependencyCheckResults.outcome != 'success'
        uses: actions/github-script@v3
        with:
          script: |
            core.setFailed('Detected critical and high vulnerabilities')
```

The <b>CycloneDX</b> module for Node.js is used to generate a Software Bill-of-Materials (BOM) containing an aggregate of all project dependencies.

- Github Action: https://github.com/CycloneDX/gh-node-module-generatebom

- Dev dependency: https://www.npmjs.com/package/@cyclonedx/bom

Only production dependencies are part of the BOM. The result is a JSON file called "bom.json".

## Secret scanning

<b>Gitleaks</b> is used to detect secrets. Gitleaks can also be installed locally and used in a pre-commit hook.

Add a gitleaks.toml file to configure secret patterns and to allow secrets ("[allowlist]").

<details>
  <summary>See gitleaks.toml example</summary>
  
    ```
    title = "gitleaks config"

    # Gitleaks rules are defined by regular expressions and entropy ranges.
    # Some secrets have unique signatures which make detecting those secrets easy.
    # Examples of those secrets would be Gitlab Personal Access Tokens, AWS keys, and Github Access Tokens.
    # All these examples have defined prefixes like `glpat`, `AKIA`, `ghp_`, etc.
    #
    # Other secrets might just be a hash which means we need to write more complex rules to verify
    # that what we are matching is a secret.
    #
    # Here is an example of a semi-generic secret
    #
    #   discord_client_secret = "8dyfuiRyq=vVc3RRr_edRk-fK__JItpZ"
    #
    # We can write a regular expression to capture the variable name (identifier),
    # the assignment symbol (like '=' or ':='), and finally the actual secret.
    # The structure of a rule to match this example secret is below:
    #
    #                                                           Beginning string
    #                                                               quotation
    #                                                                   │            End string quotation
    #                                                                   │                      │
    #                                                                   ▼                      ▼
    #    (?i)(discord[a-z0-9_ .\-,]{0,25})(=|>|:=|\|\|:|<=|=>|:).{0,5}['\"]([a-z0-9=_\-]{32})['\"]
    #
    #                   ▲                              ▲                                ▲
    #                   │                              │                                │
    #                   │                              │                                │
    #              identifier                  assignment symbol
    #                                                                                Secret
    #
    [[rules]]
    id = "gitlab-pat"
    description = "GitLab Personal Access Token"
    regex = '''glpat-[0-9a-zA-Z\-]{20}'''

    [[rules]]
    id = "aws-access-token"
    description = "AWS TEST"
    regex = '''(A3T[A-Z0-9]|AKIA|AGPA|AIDA|AROA|AIPA|ANPA|ANVA|ASIA)[A-Z0-9]{16}'''

    # Cryptographic keys
    [[rules]]
    id = "PKCS8-PK"
    description = "PKCS8 private key"
    regex = '''-----BEGIN PRIVATE KEY-----'''

    [[rules]]
    id = "RSA-PK"
    description = "RSA private key"
    regex = '''-----BEGIN RSA PRIVATE KEY-----'''

    [[rules]]
    id = "OPENSSH-PK"
    description = "SSH private key"
    regex = '''-----BEGIN OPENSSH PRIVATE KEY-----'''

    [[rules]]
    id = "PGP-PK"
    description = "PGP private key"
    regex = '''-----BEGIN PGP PRIVATE KEY BLOCK-----'''

    [[rules]]
    id = "github-pat"
    description = "Github Personal Access Token"
    regex = '''ghp_[0-9a-zA-Z]{36}'''

    [[rules]]
    id = "github-oauth"
    description = "Github OAuth Access Token"
    regex = '''gho_[0-9a-zA-Z]{36}'''

    [[rules]]
    id = "SSH-DSA-PK"
    description = "SSH (DSA) private key"
    regex = '''-----BEGIN DSA PRIVATE KEY-----'''

    [[rules]]
    id = "SSH-EC-PK"
    description = "SSH (EC) private key"
    regex = '''-----BEGIN EC PRIVATE KEY-----'''


    [[rules]]
    id = "github-app-token"
    description = "Github App Token"
    regex = '''(ghu|ghs)_[0-9a-zA-Z]{36}'''

    [[rules]]
    id = "github-refresh-token"
    description = "Github Refresh Token"
    regex = '''ghr_[0-9a-zA-Z]{76}'''

    [[rules]]
    id = "shopify-shared-secret"
    description = "Shopify shared secret"
    regex = '''shpss_[a-fA-F0-9]{32}'''

    [[rules]]
    id = "shopify-access-token"
    description = "Shopify access token"
    regex = '''shpat_[a-fA-F0-9]{32}'''

    [[rules]]
    id = "shopify-custom-access-token"
    description = "Shopify custom app access token"
    regex = '''shpca_[a-fA-F0-9]{32}'''

    [[rules]]
    id = "shopify-private-app-access-token"
    description = "Shopify private app access token"
    regex = '''shppa_[a-fA-F0-9]{32}'''

    [[rules]]
    id = "slack-access-token"
    description = "Slack token"
    regex = '''xox[baprs]-([0-9a-zA-Z]{10,48})?'''

    [[rules]]
    id = "stripe-access-token"
    description = "Stripe"
    regex = '''(?i)(sk|pk)_(test|live)_[0-9a-z]{10,32}'''

    [[rules]]
    id = "pypi-upload-token"
    description = "PyPI upload token"
    regex = '''pypi-AgEIcHlwaS5vcmc[A-Za-z0-9-_]{50,1000}'''

    [[rules]]
    id = "gcp-service-account"
    description = "Google (GCP) Service-account"
    regex = '''\"type\": \"service_account\"'''

    [[rules]]
    id = "heroku-api-key"
    description = "Heroku API Key"
    regex = ''' (?i)(heroku[a-z0-9_ .\-,]{0,25})(=|>|:=|\|\|:|<=|=>|:).{0,5}['\"]([0-9A-F]{8}-[0-9A-F]{4}-[0-9A-F]{4}-[0-9A-F]{4}-[0-9A-F]{12})['\"]'''
    secretGroup = 3

    [[rules]]
    id = "slack-web-hook"
    description = "Slack Webhook"
    regex = '''https://hooks.slack.com/services/T[a-zA-Z0-9_]{8}/B[a-zA-Z0-9_]{8,12}/[a-zA-Z0-9_]{24}'''

    [[rules]]
    id = "twilio-api-key"
    description = "Twilio API Key"
    regex = '''SK[0-9a-fA-F]{32}'''

    [[rules]]
    id = "age-secret-key"
    description = "Age secret key"
    regex = '''AGE-SECRET-KEY-1[QPZRY9X8GF2TVDW0S3JN54KHCE6MUA7L]{58}'''

    [[rules]]
    id = "facebook-token"
    description = "Facebook token"
    regex = '''(?i)(facebook[a-z0-9_ .\-,]{0,25})(=|>|:=|\|\|:|<=|=>|:).{0,5}['\"]([a-f0-9]{32})['\"]'''
    secretGroup = 3

    [[rules]]
    id = "twitter-token"
    description = "Twitter token"
    regex = '''(?i)(twitter[a-z0-9_ .\-,]{0,25})(=|>|:=|\|\|:|<=|=>|:).{0,5}['\"]([a-f0-9]{35,44})['\"]'''
    secretGroup = 3

    [[rules]]
    id = "adobe-client-id"
    description = "Adobe Client ID (Oauth Web)"
    regex = '''(?i)(adobe[a-z0-9_ .\-,]{0,25})(=|>|:=|\|\|:|<=|=>|:).{0,5}['\"]([a-f0-9]{32})['\"]'''
    secretGroup = 3

    [[rules]]
    id = "adobe-client-secret"
    description = "Adobe Client Secret"
    regex = '''(p8e-)(?i)[a-z0-9]{32}'''

    [[rules]]
    id = "alibaba-access-key-id"
    description = "Alibaba AccessKey ID"
    regex = '''(LTAI)(?i)[a-z0-9]{20}'''

    [[rules]]
    id = "alibaba-secret-key"
    description = "Alibaba Secret Key"
    regex = '''(?i)(alibaba[a-z0-9_ .\-,]{0,25})(=|>|:=|\|\|:|<=|=>|:).{0,5}['\"]([a-z0-9]{30})['\"]'''
    secretGroup = 3

    [[rules]]
    id = "asana-client-id"
    description = "Asana Client ID"
    regex = '''(?i)(asana[a-z0-9_ .\-,]{0,25})(=|>|:=|\|\|:|<=|=>|:).{0,5}['\"]([0-9]{16})['\"]'''
    secretGroup = 3

    [[rules]]
    id = "asana-client-secret"
    description = "Asana Client Secret"
    regex = '''(?i)(asana[a-z0-9_ .\-,]{0,25})(=|>|:=|\|\|:|<=|=>|:).{0,5}['\"]([a-z0-9]{32})['\"]'''
    secretGroup = 3

    [[rules]]
    id = "atlassian-api-token"
    description = "Atlassian API token"
    regex = '''(?i)(atlassian[a-z0-9_ .\-,]{0,25})(=|>|:=|\|\|:|<=|=>|:).{0,5}['\"]([a-z0-9]{24})['\"]'''
    secretGroup = 3

    [[rules]]
    id = "bitbucket-client-id"
    description = "Bitbucket client ID"
    regex = '''(?i)(bitbucket[a-z0-9_ .\-,]{0,25})(=|>|:=|\|\|:|<=|=>|:).{0,5}['\"]([a-z0-9]{32})['\"]'''
    secretGroup = 3

    [[rules]]
    id = "bitbucket-client-secret"
    description = "Bitbucket client secret"
    regex = '''(?i)(bitbucket[a-z0-9_ .\-,]{0,25})(=|>|:=|\|\|:|<=|=>|:).{0,5}['\"]([a-z0-9_\-]{64})['\"]'''
    secretGroup = 3

    [[rules]]
    id = "beamer-api-token"
    description = "Beamer API token"
    regex = '''(?i)(beamer[a-z0-9_ .\-,]{0,25})(=|>|:=|\|\|:|<=|=>|:).{0,5}['\"](b_[a-z0-9=_\-]{44})['\"]'''
    secretGroup = 3

    [[rules]]
    id = "clojars-api-token"
    description = "Clojars API token"
    regex = '''(CLOJARS_)(?i)[a-z0-9]{60}'''

    [[rules]]
    id = "contentful-delivery-api-token"
    description = "Contentful delivery API token"
    regex = '''(?i)(contentful[a-z0-9_ .\-,]{0,25})(=|>|:=|\|\|:|<=|=>|:).{0,5}['\"]([a-z0-9\-=_]{43})['\"]'''
    secretGroup = 3

    [[rules]]
    id = "contentful-preview-api-token"
    description = "Contentful preview API token"
    regex = '''(?i)(contentful[a-z0-9_ .\-,]{0,25})(=|>|:=|\|\|:|<=|=>|:).{0,5}['\"]([a-z0-9\-=_]{43})['\"]'''
    secretGroup = 3

    [[rules]]
    id = "databricks-api-token"
    description = "Databricks API token"
    regex = '''dapi[a-h0-9]{32}'''

    [[rules]]
    id = "discord-api-token"
    description = "Discord API key"
    regex = '''(?i)(discord[a-z0-9_ .\-,]{0,25})(=|>|:=|\|\|:|<=|=>|:).{0,5}['\"]([a-h0-9]{64})['\"]'''
    secretGroup = 3

    [[rules]]
    id = "discord-client-id"
    description = "Discord client ID"
    regex = '''(?i)(discord[a-z0-9_ .\-,]{0,25})(=|>|:=|\|\|:|<=|=>|:).{0,5}['\"]([0-9]{18})['\"]'''
    secretGroup = 3

    [[rules]]
    id = "discord-client-secret"
    description = "Discord client secret"
    regex = '''(?i)(discord[a-z0-9_ .\-,]{0,25})(=|>|:=|\|\|:|<=|=>|:).{0,5}['\"]([a-z0-9=_\-]{32})['\"]'''
    secretGroup = 3

    [[rules]]
    id = "doppler-api-token"
    description = "Doppler API token"
    regex = '''['\"](dp\.pt\.)(?i)[a-z0-9]{43}['\"]'''

    [[rules]]
    id = "dropbox-api-secret"
    description = "Dropbox API secret/key"
    regex = '''(?i)(dropbox[a-z0-9_ .\-,]{0,25})(=|>|:=|\|\|:|<=|=>|:).{0,5}['\"]([a-z0-9]{15})['\"]'''

    [[rules]]
    id = "dropbox--api-key"
    description = "Dropbox API secret/key"
    regex = '''(?i)(dropbox[a-z0-9_ .\-,]{0,25})(=|>|:=|\|\|:|<=|=>|:).{0,5}['\"]([a-z0-9]{15})['\"]'''

    [[rules]]
    id = "dropbox-short-lived-api-token"
    description = "Dropbox short lived API token"
    regex = '''(?i)(dropbox[a-z0-9_ .\-,]{0,25})(=|>|:=|\|\|:|<=|=>|:).{0,5}['\"](sl\.[a-z0-9\-=_]{135})['\"]'''

    [[rules]]
    id = "dropbox-long-lived-api-token"
    description = "Dropbox long lived API token"
    regex = '''(?i)(dropbox[a-z0-9_ .\-,]{0,25})(=|>|:=|\|\|:|<=|=>|:).{0,5}['\"][a-z0-9]{11}(AAAAAAAAAA)[a-z0-9\-_=]{43}['\"]'''

    [[rules]]
    id = "duffel-api-token"
    description = "Duffel API token"
    regex = '''['\"]duffel_(test|live)_(?i)[a-z0-9_-]{43}['\"]'''

    [[rules]]
    id = "dynatrace-api-token"
    description = "Dynatrace API token"
    regex = '''['\"]dt0c01\.(?i)[a-z0-9]{24}\.[a-z0-9]{64}['\"]'''

    [[rules]]
    id = "easypost-api-token"
    description = "EasyPost API token"
    regex = '''['\"]EZAK(?i)[a-z0-9]{54}['\"]'''

    [[rules]]
    id = "easypost-test-api-token"
    description = "EasyPost test API token"
    regex = '''['\"]EZTK(?i)[a-z0-9]{54}['\"]'''

    [[rules]]
    id = "fastly-api-token"
    description = "Fastly API token"
    regex = '''(?i)(fastly[a-z0-9_ .\-,]{0,25})(=|>|:=|\|\|:|<=|=>|:).{0,5}['\"]([a-z0-9\-=_]{32})['\"]'''
    secretGroup = 3

    [[rules]]
    id = "finicity-client-secret"
    description = "Finicity client secret"
    regex = '''(?i)(finicity[a-z0-9_ .\-,]{0,25})(=|>|:=|\|\|:|<=|=>|:).{0,5}['\"]([a-z0-9]{20})['\"]'''
    secretGroup = 3

    [[rules]]
    id = "finicity-api-token"
    description = "Finicity API token"
    regex = '''(?i)(finicity[a-z0-9_ .\-,]{0,25})(=|>|:=|\|\|:|<=|=>|:).{0,5}['\"]([a-f0-9]{32})['\"]'''
    secretGroup = 3

    [[rules]]
    id = "flutterweave-public-key"
    description = "Flutterweave public key"
    regex = '''FLWPUBK_TEST-(?i)[a-h0-9]{32}-X'''

    [[rules]]
    id = "flutterweave-secret-key"
    description = "Flutterweave secret key"
    regex = '''FLWSECK_TEST-(?i)[a-h0-9]{32}-X'''

    [[rules]]
    id = "flutterweave-enc-key"
    description = "Flutterweave encrypted key"
    regex = '''FLWSECK_TEST[a-h0-9]{12}'''

    [[rules]]
    id = "frameio-api-token"
    description = "Frame.io API token"
    regex = '''fio-u-(?i)[a-z0-9-_=]{64}'''

    [[rules]]
    id = "gocardless-api-token"
    description = "GoCardless API token"
    regex = '''['\"]live_(?i)[a-z0-9-_=]{40}['\"]'''

    [[rules]]
    id = "grafana-api-token"
    description = "Grafana API token"
    regex = '''['\"]eyJrIjoi(?i)[a-z0-9-_=]{72,92}['\"]'''

    [[rules]]
    id = "hashicorp-tf-api-token"
    description = "Hashicorp Terraform user/org API token"
    regex = '''['\"](?i)[a-z0-9]{14}\.atlasv1\.[a-z0-9-_=]{60,70}['\"]'''

    [[rules]]
    id = "hubspot-api-token"
    description = "Hubspot API token"
    regex = '''(?i)(hubspot[a-z0-9_ .\-,]{0,25})(=|>|:=|\|\|:|<=|=>|:).{0,5}['\"]([a-h0-9]{8}-[a-h0-9]{4}-[a-h0-9]{4}-[a-h0-9]{4}-[a-h0-9]{12})['\"]'''
    secretGroup = 3

    [[rules]]
    id = "intercom-api-token"
    description = "Intercom API token"
    regex = '''(?i)(intercom[a-z0-9_ .\-,]{0,25})(=|>|:=|\|\|:|<=|=>|:).{0,5}['\"]([a-z0-9=_]{60})['\"]'''
    secretGroup = 3

    [[rules]]
    id = "intercom-client-secret"
    description = "Intercom client secret/ID"
    regex = '''(?i)(intercom[a-z0-9_ .\-,]{0,25})(=|>|:=|\|\|:|<=|=>|:).{0,5}['\"]([a-h0-9]{8}-[a-h0-9]{4}-[a-h0-9]{4}-[a-h0-9]{4}-[a-h0-9]{12})['\"]'''
    secretGroup = 3

    [[rules]]
    id = "ionic-api-token"
    description = "Ionic API token"
    regex = '''(?i)(ionic[a-z0-9_ .\-,]{0,25})(=|>|:=|\|\|:|<=|=>|:).{0,5}['\"](ion_[a-z0-9]{42})['\"]'''

    [[rules]]
    id = "linear-api-token"
    description = "Linear API token"
    regex = '''lin_api_(?i)[a-z0-9]{40}'''

    [[rules]]
    id = "linear-client-secret"
    description = "Linear client secret/ID"
    regex = '''(?i)(linear[a-z0-9_ .\-,]{0,25})(=|>|:=|\|\|:|<=|=>|:).{0,5}['\"]([a-f0-9]{32})['\"]'''
    secretGroup = 3

    [[rules]]
    id = "lob-api-key"
    description = "Lob API Key"
    regex = '''(?i)(lob[a-z0-9_ .\-,]{0,25})(=|>|:=|\|\|:|<=|=>|:).{0,5}['\"]((live|test)_[a-f0-9]{35})['\"]'''
    secretGroup = 3

    [[rules]]
    id = "lob-pub-api-key"
    description = "Lob Publishable API Key"
    regex = '''(?i)(lob[a-z0-9_ .\-,]{0,25})(=|>|:=|\|\|:|<=|=>|:).{0,5}['\"]((test|live)_pub_[a-f0-9]{31})['\"]'''
    secretGroup = 3

    [[rules]]
    id = "mailchimp-api-key"
    description = "Mailchimp API key"
    regex = '''(?i)(mailchimp[a-z0-9_ .\-,]{0,25})(=|>|:=|\|\|:|<=|=>|:).{0,5}['\"]([a-f0-9]{32}-us20)['\"]'''
    secretGroup = 3

    [[rules]]
    id = "mailgun-private-api-token"
    description = "Mailgun private API token"
    regex = '''(?i)(mailgun[a-z0-9_ .\-,]{0,25})(=|>|:=|\|\|:|<=|=>|:).{0,5}['\"](key-[a-f0-9]{32})['\"]'''
    secretGroup = 3

    [[rules]]
    id = "mailgun-pub-key"
    description = "Mailgun public validation key"
    regex = '''(?i)(mailgun[a-z0-9_ .\-,]{0,25})(=|>|:=|\|\|:|<=|=>|:).{0,5}['\"](pubkey-[a-f0-9]{32})['\"]'''
    secretGroup = 3

    [[rules]]
    id = "mailgun-signing-key"
    description = "Mailgun webhook signing key"
    regex = '''(?i)(mailgun[a-z0-9_ .\-,]{0,25})(=|>|:=|\|\|:|<=|=>|:).{0,5}['\"]([a-h0-9]{32}-[a-h0-9]{8}-[a-h0-9]{8})['\"]'''
    secretGroup = 3

    [[rules]]
    id = "mapbox-api-token"
    description = "Mapbox API token"
    regex = '''(?i)(pk\.[a-z0-9]{60}\.[a-z0-9]{22})'''

    [[rules]]
    id = "messagebird-api-token"
    description = "MessageBird API token"
    regex = '''(?i)(messagebird[a-z0-9_ .\-,]{0,25})(=|>|:=|\|\|:|<=|=>|:).{0,5}['\"]([a-z0-9]{25})['\"]'''
    secretGroup = 3

    [[rules]]
    id = "messagebird-client-id"
    description = "MessageBird API client ID"
    regex = '''(?i)(messagebird[a-z0-9_ .\-,]{0,25})(=|>|:=|\|\|:|<=|=>|:).{0,5}['\"]([a-h0-9]{8}-[a-h0-9]{4}-[a-h0-9]{4}-[a-h0-9]{4}-[a-h0-9]{12})['\"]'''
    secretGroup = 3

    [[rules]]
    id = "new-relic-user-api-key"
    description = "New Relic user API Key"
    regex = '''['\"](NRAK-[A-Z0-9]{27})['\"]'''

    [[rules]]
    id = "new-relic-user-api-id"
    description = "New Relic user API ID"
    regex = '''(?i)(newrelic[a-z0-9_ .\-,]{0,25})(=|>|:=|\|\|:|<=|=>|:).{0,5}['\"]([A-Z0-9]{64})['\"]'''
    secretGroup = 3

    [[rules]]
    id = "new-relic-browser-api-token"
    description = "New Relic ingest browser API token"
    regex = '''['\"](NRJS-[a-f0-9]{19})['\"]'''

    [[rules]]
    id = "npm-access-token"
    description = "npm access token"
    regex = '''['\"](npm_(?i)[a-z0-9]{36})['\"]'''

    [[rules]]
    id = "planetscale-password"
    description = "Planetscale password"
    regex = '''pscale_pw_(?i)[a-z0-9\-_\.]{43}'''

    [[rules]]
    id = "planetscale-api-token"
    description = "Planetscale API token"
    regex = '''pscale_tkn_(?i)[a-z0-9\-_\.]{43}'''

    [[rules]]
    id = "postman-api-token"
    description = "Postman API token"
    regex = '''PMAK-(?i)[a-f0-9]{24}\-[a-f0-9]{34}'''

    [[rules]]
    id = "pulumi-api-token"
    description = "Pulumi API token"
    regex = '''pul-[a-f0-9]{40}'''

    [[rules]]
    id = "rubygems-api-token"
    description = "Rubygem API token"
    regex = '''rubygems_[a-f0-9]{48}'''

    [[rules]]
    id = "sendgrid-api-token"
    description = "Sendgrid API token"
    regex = '''SG\.(?i)[a-z0-9_\-\.]{66}'''

    [[rules]]
    id = "sendinblue-api-token"
    description = "Sendinblue API token"
    regex = '''xkeysib-[a-f0-9]{64}\-(?i)[a-z0-9]{16}'''

    [[rules]]
    id = "shippo-api-token"
    description = "Shippo API token"
    regex = '''shippo_(live|test)_[a-f0-9]{40}'''

    [[rules]]
    id = "linedin-client-secret"
    description = "Linkedin Client secret"
    regex = '''(?i)(linkedin[a-z0-9_ .\-,]{0,25})(=|>|:=|\|\|:|<=|=>|:).{0,5}['\"]([a-z]{16})['\"]'''
    secretGroup = 3

    [[rules]]
    id = "linedin-client-id"
    description = "Linkedin Client ID"
    regex = '''(?i)(linkedin[a-z0-9_ .\-,]{0,25})(=|>|:=|\|\|:|<=|=>|:).{0,5}['\"]([a-z0-9]{14})['\"]'''
    secretGroup = 3

    [[rules]]
    id = "twitch-api-token"
    description = "Twitch API token"
    regex = '''(?i)(twitch[a-z0-9_ .\-,]{0,25})(=|>|:=|\|\|:|<=|=>|:).{0,5}['\"]([a-z0-9]{30})['\"]'''
    secretGroup = 3

    [[rules]]
    id = "typeform-api-token"
    description = "Typeform API token"
    regex = '''(?i)(typeform[a-z0-9_ .\-,]{0,25})(=|>|:=|\|\|:|<=|=>|:).{0,5}(tfp_[a-z0-9\-_\.=]{59})'''
    secretGroup = 3

    [[rules]]
    id = "generic-api-key"
    description = "Generic API Key"
    regex = '''(?i)((key|api|token|secret|password)[a-z0-9_ .\-,]{0,25})(=|>|:=|\|\|:|<=|=>|:).{0,5}['\"]([0-9a-zA-Z\-_=]{8,64})['\"]'''
    entropy = 3.7
    secretGroup = 4

    [allowlist]
    description = "Allowlisted files"
    regexes = [
        '''ghu_InstallationUserToServer000000000000''',
        '''ghs_InstallallationOrActionToken00000000''',
        '''ghp_PersonalAccessToken01245678900000000'''
    ]
    paths = [
        '''.github/actions/node_modules/@octokit/auth-token/README.md''',
        '''.github/actions/node_modules'''
    ]
    ```

</details>

- https://github.com/zricethezav/gitleaks

- Gitleaks Github action: https://github.com/zricethezav/gitleaks-action

```
  secret-scanning:
    runs-on: ubuntu-latest
    needs: [Build]
    steps:
      - uses: actions/checkout@v2
        with:
          fetch-depth: "0"

      - name: Detect secrets
        id: detectSecrets
        uses: zricethezav/gitleaks-action@master
        continue-on-error: true
        with:
          config-path: gitleaks.toml

      - name: Check on failures
        if: steps.detectSecrets.outcome != 'success'
        uses: actions/github-script@v3
        with:
          script: |
            core.setFailed('Detected code secrets')
```

Local gitleaks installation:

1. Clone repo https://github.com/zricethezav/gitleaks

2. Install Go <b>1.16+</b> (not mentioned in the official documentation!)

3. Create executalbe with "make build"

4. Copy gitleaks executable to /usr/local/bin (on debian based systems)

## Infrastructure as Code (IaC)

Currently, <b>KICS</b> scanning supports configuration files for Terraform, Ansible, AWS CloudFormation, Helm, Docker, and Kubernetes.

- https://kics.io/

- KICS Github action: https://github.com/Checkmarx/kics-github-action

Semgrep also has IaC capabilities.

## Compliance as Code (CaC)

TODO - in progress

## Release management

You can create a Github release to package software, along with release notes and links to workflow artifacts.

Official github documentation:

- https://docs.github.com/en/repositories/releasing-projects-on-github/about-releases

<b>semantic-release</b> automates the whole package release workflow including: determining the next version number, generating the release notes, and publishing the package.

- https://github.com/semantic-release/semantic-release

semantic-release uses the commit messages to determine the consumer impact of changes in the codebase. Following formalized conventions for commit messages, semantic-release automatically determines the next semantic version number, generates a changelog and publishes the release.

By default, semantic-release uses Angular Commit Message Conventions.

Tools such as commitizen or commitlint can be used to help contributors and enforce valid commit messages.

The commit messages are checked for quality. The quality guidelines are determined by https://www.conventionalcommits.org/.

With the devDependency packages <b>commitlint/cli</b> and <b>commitlint/config-conventional</b> enforce the quality.

The Commitizen tool is used to create quality commit messages. Commitizen must be installed locally:

- https://github.com/commitizen/cz-cli

- npm install commitizen -g

- Initialize project to use the cz-conventional-changelog: commitizen init cz-conventional-changelog --save-dev --save-exact

## Dynamic Application Security Testing (DAST)

Dynamic Application Security Testing (DAST) examines applications for vulnerabilities like these in deployed environments.

<b>OWASP Zed Attack Proxy</b>:

- https://www.zaproxy.org/

- Fast / baseline scan (passive): https://www.zaproxy.org/docs/docker/baseline-scan/

- Full scan (active): https://www.zaproxy.org/docs/docker/full-scan/

DAST can analyze applications in two ways:

- Passive scan only (smoke tests). DAST executes ZAP’s Baseline Scan and doesn’t actively attack your application.

- Active scan - can be configured to also perform an active scan to attack your application and produce a more extensive security report.

## Fuzzing

Because the effort of fuzzing is sometimes enormous, it usually makes sense to perform fuzzing in a separate workflow.

Usually, the input or payload must comply with a certain format or protocol. Lists can help here:

- https://github.com/danielmiessler/SecLists

- https://wordlists.assetnote.io/

- https://github.com/fuzzdb-project/fuzzdb

- https://github.com/swisskyrepo/PayloadsAllTheThings

- https://www.kali.org/tools/seclists/

Whatever the target is, the attack vectors are within it’s I/O.

Tools:

- https://github.com/xmendez/wfuzz

- https://github.com/google/AFL

- https://pypi.org/project/APIFuzzer/
