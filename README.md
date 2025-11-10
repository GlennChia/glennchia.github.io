# Glenn's site

# 1. Local Setup

Step 1: Follow instructions [here](https://dev.to/luizgadao/easy-way-to-change-ruby-version-in-mac-m1-m2-and-m3-16hl) to upgrade the ruby version.

```bash
brew install rbenv ruby-build
ruby -v
rbenv install -l
rbenv install 3.4.7
rbenv global 3.4.7
ruby -v
```

Then modify `.zprofile` with the following before running `source .zprofile`

```bash
export PATH="$HOME/.rbenv/bin:$PATH"
eval "$(rbenv init - zsh)"
```

Step 2: Install Jekyll

```bash
sudo gem install bundler jekyll
```

Step 3: Install dependencies:

```bash
bundle install
```

Step 4: Run locally:

```bash
bundle exec jekyll serve
```

Step 5: Visit `http://localhost:4000`
