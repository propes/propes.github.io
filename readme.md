## Tech Stack

This blog uses jekyll because of it's easy integration with github pages.

## Setup for local development

### Ubuntu 20.04

Install ruby:

```sh
sudo snap install ruby --classic
```

Install gcc and make:

```sh
sudo apt install build-essential
```

Install jekyll and bundler gems:

```sh
gem install jekyll bundler
```

Install/update the project gems:

```sh
bundle update
```

### Running locally

```sh
bundle exec jekyll serve
```

## How this project was set up

This project was initially set up with the default theme using `jekyll new blog`. The theme files were then copied from the gem to the project directory to be overwritten and customised.

```sh
# Get the path to the theme gem
gempath=$(bundle info --path minima)

cp $gempath/assets .
cp $gempath/_includes .
cp $gempath/_layouts .
cp $gempath/_sass .
```

The following plugins were added to the Gemfile:

```rb
# ./Gemfile

group :jekyll_plugins do
  gem "jekyll-feed", "~> 0.12"
  gem "jekyll-seo-tag", "~> 2.6"
end
```

References to the theme gem in Gemfile and configuration were removed.

- In Gemfile, `gem "minima", "~> 2.5"` removed
- In `_config.yml`, `theme: minima` removed
