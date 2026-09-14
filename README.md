# homebrew-amfaa

Homebrew tap for [All My Friends Are Agents](https://github.com/virusimmortal00/AllMyFriendsAreAgents) (`amfaa`) — a local-first, multi-agent chatroom for coding with AI models.

## Install

```sh
brew install --cask virusimmortal00/amfaa/amfaa
```

That one command taps this repository and installs `amfaa` in a single step. Using
the fully qualified `user/repo/cask` name like this trusts only this cask — modern
Homebrew (6.0+) requires taps and their contents to be explicitly trusted before
loading any code from them, and a fully qualified install grants that automatically
for the one item you asked for. See [Tap Trust](https://docs.brew.sh/Tap-Trust) for
why, and the alternative (`brew tap` then `brew trust`) if you'd rather trust the
whole tap up front — for example before installing by short name (`brew install
amfaa`) in a script.

Then run `amfaa` from the project directory you want agents to inspect. See the [Quick start](https://github.com/virusimmortal00/AllMyFriendsAreAgents#readme) in the main repository for first-time setup.

Upgrade and remove with the usual Homebrew lifecycle commands:

```sh
brew upgrade --cask amfaa
brew uninstall --cask amfaa
```

## Why a cask, not a formula

Every from-source Homebrew *Formula* install runs Homebrew's Mach-O linkage-fixing pass unconditionally. The published `amfaa` archive is a fully self-contained bundle (its own pinned Node.js runtime, the audited OpenCode runtime, and a full production `node_modules` tree), and rewriting the bundled `@rollup/rollup-darwin-arm64` native addon's dylib ID to this bundle's deeply nested install path overflows that addon's Mach-O header padding — verified with a real `brew install --formula` failure before switching to a *Cask*, which stages files as-is with no relinking.

## How this tap stays current

[`Casks/amfaa.rb`](Casks/amfaa.rb) is a generated mirror of
[`homebrew/Casks/amfaa.rb`](https://github.com/virusimmortal00/AllMyFriendsAreAgents/blob/main/homebrew/Casks/amfaa.rb)
in the main repository, which is the actual source of truth: a job at the end of that
repository's release-promotion workflow regenerates it from each published release's
manifest, validates it (lint, audit, a real `brew install --cask`, and running the
installed launcher), and merges it automatically once green.

[`.github/workflows/sync.yml`](.github/workflows/sync.yml) in *this* repository pulls
that file on a schedule, lints it, commits it here when it changes, and then verifies
the published tap actually installs and runs — using only this repo's own default
`GITHUB_TOKEN`; reading a public file from the main repository needs no
cross-repository credential. Run it manually (Actions → Sync cask from source repo →
Run workflow) to pick up a change immediately instead of waiting for the next
scheduled run.

Do not hand-edit `Casks/amfaa.rb` here — edit `homebrew/Casks/amfaa.rb` in the main
repository (or its generator, `scripts/update-homebrew-formula.ts`) instead; this tap's
copy is overwritten on the next sync.

## License

MIT, see [LICENSE](LICENSE). Same license as the application itself.
