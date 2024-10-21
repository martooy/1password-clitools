# 1password-clitools
Follow along with [https://developer.1password.com/docs/cli/get-started/](https://developer.1password.com/docs/cli/get-started/)

##Install the thing and make it work

#### Install with:

`brew install 1password-cli`

#### Check version

`op --version`

Current as of 2024-10-21 is 2.30.0

### Signin
`eval $(op signin)`

### List Vaults
`op vault ls`

## Make it autocomplete

For bash:

`source <(op completion bash)`

or if you use ZSH

`eval "$(op completion zsh)"; compdef _op op`


## Nifty Tricks to try...

**How many things in my vault?**

`echo $(($(op item list | wc -l) - 1))`

**How may of each kind of thing do I have?**

`op item list --format=json | jq '.[] | .category' | sort | uniq -c | sort -n`

**Show me my items sorted by how old they are:**

op item list --format=json | jq -c '.[] | [.updated_at, .category, .title]'| sort -nr

**Show me stuff that has a stored username (experimental - the additional_information field is username for the cases I’ve seen):**

`op item list --format=json | jq -c '.[] | [.updated_at, .category, .title, .additional_information]'| sort -nr`

**You can reorder the fields in the jq field list and I guess sort by username, which might help find things by email address, if that’s the username:**

`op item list --format=json | jq -c '.[] | [.additional_information, .updated_at, .category, .title]'| sort -nr`

**And exclude your “usual” email addresses to see outliers:**

`op item list --format=json | jq -c '.[] | [.additional_information, .updated_at, .category, .title]'| sort -nr | grep \@ | egrep -vi '(foobar@gmail.com|foobaz@gmail.com|barfoo@gmail.com)'`
