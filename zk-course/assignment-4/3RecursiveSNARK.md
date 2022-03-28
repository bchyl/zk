# why, solve what and security drawbacks
## why
recursion-friendly cryptography isn’t actually possible on the EVM.
it’s possible to implement, but there are no pre-compiles issued in the protocol.
## solve what
naive SNARK drawbacks:
in case that we want to prove the correctness of a function after t iterated
execution on it.

recursive SNARK such as PLONK solution.

apply SNARKs at each iterated execution to prove that execution and the correctness of prior
proof. 
Hence, don’t need to wait to aggregate all executions to prove at once. 
With this, the election problem becomes a lot easier since people can submit the proof of their vote and the vote-counter can just aggregate the vote and verify the intermediary result of voting.

## security drawbacks
- not Turing-complete
- can't implement jumps or recursion, similar to a shader or other parallel operations

# Kimchi and improve PLONK
plonkish: After PLONK, There’s been fflonk, turbo PLONK, ultra PLONK, plonkup, and recently plonky2.

Kimchi is a collection of improvements, optimizations, and alterations made on top of PLONK:
- overcomes the trusted setup limitation of PLONK by using a bulletproof-style polynomial commitment inside of the protocol. (adds 12 registers to the 3 register)
- a gate can directly write its output on the registers used by the next gate
- lookups: Sometimes, some operations can be written as a table

brings several optimizations and quality-of-life improvements to circuit builders. This should allow for faster provers, larger circuits, and potentially shorter proof sizes!

## ref
[Kimchi: The latest update to Mina’s proof system](https://minaprotocol.com/zh-hans/blog/kimchi-the-latest-update-to-minas-proof-system)

# snapp with 3 fields
when try array fields it get runtime err `Error: @state fields must have type State<A> for some type A`. can you help me please?

then have to use 3 field elements hard code.
the snapp code commit at:
[snapp with 3 fields](https://github.com/bchyl/snarkyjs-workshop/commit/9e6b9e60abc9a3b14d2cb6eb6102c777fb18136c)

the UT testcase result:
```
% npm install

added 6 packages, and audited 8 packages in 2s

found 0 vulnerabilities

% npx tsc

% node dist/1_plus_exercise_3f.js
second update failed
Exercise 1
final state value 1
```
