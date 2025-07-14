---
title: Debugging Shell Startup Performance
subtitle: Cutting the time it takes for my shell to start in half by getting rid of a tool I have been relying on for the last couple of years.
date: 2025-07-09
toc: false
language: en
tags: ["Python", "Shell", "dotfiles"]
---

It has been a couple of years since I switched from oh-my-zsh to [zsh4humans]. Since then, I never had to worry about any performance related issues when starting my shell. It always felt instant. Until now.

## The Problem

A couple of months ago, I noticed considerable lag when starting a new instance of my shell. I'm not really sensitive to shell startup time, since I'm not a heavy shell user (mostly inside VSCode when coding and sometimes in iTerm to trigger one-off commands or navigate something buried in a hidden folder). But startup times (more specifically, time to first command or `first_command_lag` in [zsh-bench] lingo) measured in seconds instead of milliseconds is too much. So I digged into my [`./dotfiles`] to find the culprit.

## Finding the culprit

I commented out half of my [`.zshrc`] and checked again. And repeated that a couple of times until I was pretty sure the issue was with my python shell config, specifically with those two lines:

```sh
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
```

They alone were responsible for ~300ms or half of the total first command lag time. And I [was][1] [not][2] [alone][3].

## Is there a workaround?

Some people seem to be happy when shell detection and rehashing is skipped

```sh
eval "$(pyenv init - --no-rehash zsh)"
eval "$(pyenv virtualenv-init -)"
```

but for me, the improvement (around ~50ms) was marginal at best.

When I removed all the environments I created over the last year (n=68), I gained ~150ms, but `pyenv` was still using ~170ms to load (and now it wasn't even doing anything anymore for me).

## Time to switch

So it is probably time to find another tool that provides similar utility to `pyenv` with less impact on my shell performance. And whenever I hear performance, I think to myself: "maybe somebody has rewritten this in Rust".

And, of course, [Astral] has. It is called [`uv`]. And in the next article, I will describe how that switch went.

[`./dotfiles`]: https://github.com/jannismain/.dotfiles
[`.zshrc`]: https://github.com/jannismain/.dotfiles/blob/c5a39a30b3cbf22ee5f0150b51c1ef9d759b619f/src/.zshrc
[zsh4humans]: https://github.com/romkatv/zsh4humans
[zsh-bench]: https://github.com/romkatv/zsh-bench
[1]: https://github.com/pyenv/pyenv/issues/2918
[2]: https://github.com/pyenv/pyenv/issues/784
[3]: https://cpajr.com/pyenv-slow-shell/
[Astral]: https://github.com/astral-sh
[`uv`]: https://github.com/astral-sh/uv
