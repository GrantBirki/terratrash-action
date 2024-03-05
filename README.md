# terratrash-action 🗑️

This action is a simple wrapper around the [terratrash](https://github.com/GrantBirki/terratrash) Ruby Gem. It is designed to be used in a GitHub Actions workflow to clean up Terraform output text with ease to make it more human readable.

## Usage

```yaml
# you must run the checkout action first
- name: checkout
  uses: actions/checkout@v4

# somewhere in your workflow you might run terraform plan or apply (example here)
# this step shows an example of a terraform plan command where we pipe the output to a file
- name: Terraform plan
  id: plan
  continue-on-error: true
  run: |
    set -o pipefail
    terraform plan -no-color -compact-warnings | tee terraform-output.txt

# now you can use the terratrash-action to clean up the output
- name: terratrash
  uses: GrantBirki/terratrash-action@vX.X.X # replace with the latest version
  with:
    input_file_path: terraform-output.txt
    output_file_path: terraform-output-fmt.txt

# finally you can print the formatted output, save it as an artifact, post it as a PR comment, etc.
- name: Print formatted output
  run: cat terraform-output-fmt.txt
```

## Inputs

Check out the [`action.yml`](./action.yml) file for the most up-to-date list of inputs that this Action supports.

## branch-deploy compatibility

If you are using the [`github/branch-deploy`](https://github.com/github/branch-deploy) Action with Terraform and want some cleaned up Terraform deployment comments, here are some details that you might find useful:

- Live Example: [related workflow code](https://github.com/the-hideout/cloudflare/blob/0e579405af0422e80ae76f3e085001fd528045ec/.github/workflows/branch-deploy.yml#L60-L94) - [template file for pull request comments](https://github.com/the-hideout/cloudflare/blob/0e579405af0422e80ae76f3e085001fd528045ec/.github/deployment_message.md)
- [branch deploy custom comment docs](https://github.com/github/branch-deploy/blob/1f6516ef5092890ce75d9e97ca7cbdb628e38bdd/docs/custom-deployment-messages.md)
