# bash_cmd_lib (WIP)
Common bash script library, with better error handling, status reporting and logging

## Why you need this
- Automatic error handling
- Composable logging
- Auto resource cleanup
- Make errexit really work

## Bash Pitfalls
- [When Bash scripts bite](https://news.ycombinator.com/item?id=14321213)
- [Why is bash errexit not behaving as expected in function calls](https://stackoverflow.com/questions/19789102/why-is-bash-errexit-not-behaving-as-expected-in-function-calls)
- [Safe ways to do things in bash](https://github.com/anordal/shellharden/blob/master/how_to_do_things_safely_in_bash.md)

## Example

```bash
#!/bin/bash
. cmd_lib.sh

error_command() {
    do_something_failed
}

function bad_greeting() {
    local name="$1"

    info "Running error_command ..."
    error_command
    output "hello, ${name}!"
}

function good_greeting() {
    local name="$1"

    info "Running good_command ..."
    output "hello, ${name}!"
}

main() {
    local bad_ans
    bad_ans=$(_call bad_greeting "rust-shell-script")
    info "${bad_ans}"
    local good_ans
    good_ans=$(_call good_greeting "rust-shell-script")
    info "${good_ans}"
    return 0
}

main "$@"
```

ouput:
```
Running error_command ...
/bin/bash: line 219: do_something_failed: command not found
FATAL: Running command (bad_ans=$(_call bad_greeting "rust-shell-script")) near script hello.sh:main():1 failed (ret=127)
```

## Related
See [rust-shell-script](https://github.com/rust-shell-script/rust-shell-script/)
