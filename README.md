
---
NOVA — Safe State, Explicit Rules.

NOVA is an experimental, local-first programming language.
It makes state changes explicit, checked at runtime, and impossible to hide.

> If state changes, you see it. If a rule breaks, it fails loudly.

Why NOVA?

JavaScript:
user.isAdmin = true // oops, mutated anywhere
NOVA:
state User {
  isAdmin: bool @immutable
  name: string @mutable
}

rule only_admin_can_promote {
  check: caller.isAdmin == true
}

safe change promote(user) {
  enforce only_admin_can_promote
  user.isAdmin = true
}
- `state` = data with explicit mutability
- `rule` = explicit invariant, not hidden if-else
- `safe` = only place where mutation is allowed

Try in 10 seconds
git clone https://github.com/yahia7556y-prog/nova-lang
cd nova-lang
cargo run examples/hello.nova
Example: Bank Account
state Account {
  balance: i64 @mutable @min(0)
}

rule no_overdraft {
  check: Account.balance >= 0
}

safe change withdraw(acc: Account, amount: i64) {
  enforce no_overdraft
  acc.balance = acc.balance - amount
}
If balance would go below 0, NOVA panics with rule name, file, and line.

Roadmap

- Lexer & Parser
- Runtime checks for `rule`
- Local-first package manager
- WASM backend for AURIX

---
Built by @yahia7556y-prog — feedback welcome in Issues.
---
