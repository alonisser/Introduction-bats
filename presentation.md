class: center, middle

# Testing CLI oriented tools with BATS


[live presentation](http://alonisser.github.io/Introduction-bats) <br/>
[twitter](alonisser@twitter.com), [medium](https://medium.com/@alonisser/)

#####Shameless promotion: you can also read my [political blog](degeladom@wordpress.com)
---

# BATS

[BATS](https://github.com/sstephenson/bats)

Stands for: Bash Automated Testing System

---

# PROBLEM: Test driven development for CLI apps
 
--
* I would like to functional test (or integration, depends on lingo) the same as I do with webapps. from outside in

--

* Why? verification, reproducible results, regression safeguard.

--

* How? BATS to the rescue!

---

# BATS

* Allows setup/teardown
* test suites
* syntactic sugar around common CLI testing checks

---
class: center, middle

# Real Live example. testing "swamp" a process runner

---

# Simple test

```bash
@test "Check that the swamp client is available" {
    command -v swamp
}
```
* Would Simply fail if exit code is different from 0
* everything* in BATS is run with set -x


---
# Testing output

```bash
@test "Check swamp -h prints usage" {
   run swamp -h 
   [ "$status" -eq 0 ]
   [ "${lines[0]}" = "  Usage: swamp [options]" ]
}
```
* run - command to encapsulate calling our command (allows exiting with different value then 0)
* $status special variable with the status code
* $output
* ${lines[0]} special array contains the output lines and accessible as array


---
# Skipping


```bash
@test "Check Swamp respects boot order and wait for ready" {
 skip "not implemented yet! coming soon"

}
```


---
# Shebang

```bash
#! /usr/bin/env bats
```

* Needed in the beginning of the test suite


---
# Teardown and setup
```bash

teardown() {
  echo "Teardown test"
  echo "$BATS_TEST_NAME"
  run swamp -p bats/valid/ -H
  run swamp -p bats/invalid/ -H

}
```

---
# How would it look?

```bash
$ cd test && bats bats
1..12
ok 1 Check that the swamp client is available
ok 2 Check swamp -h prints usage
ok 3 Check swamp -d runs as daemon mode and swamp -H stops it
ok 4 Check stopping and starting a service actually starts the service
```

---
# Installation

```bash
sudo add-apt-repository ppa:duggan/bats
sudo apt-get update
sudo apt-get install bats

```

---
# More info

* Nice blog post: https://blog.engineyard.com/2014/bats-test-command-line-tools
* Github: https://github.com/sstephenson/bats


---

class: center, middle

#Open source rocks!

---

class: center, middle

#Thanks for listening!

---
