# Hotwired ATS

## Application Setup with esbuild

The first step in building Hotwired ATS is to set up a new Rails application using the Rails application generator. This chapter will guide you through setting up all the essential tools needed to build a modern Rails application. By the end of this setup, your Rails 7 application will be configured with:

- Postgres as the primary database
- esbuild for bundling JavaScript, configured to automatically refresh the browser on code changes
- Tailwind and postcss for styling
- The Hotwire stack (Stimulus and Turbo) for faster page loads, partial page updates, and frontend interactivity
- CableReady and StimulusReflex for server-powered frontend interactivity and reactive page updates
- Mrujs to enhance Rails/UJS functionality and for its powerful Cable Car plugin
- UUIDs for primary keys

## Setting up your Environment

Before starting, ensure you have Ruby, Rails, Postgres, Redis, Node, and Yarn installed. This book is built for Rails 7 and Ruby 3.0 (3.1 will work fine too). 

## Creating a Rails Application with `rails new`

Start by using `rails new` to generate your application. Rails 7 ships with Stimulus and Turbo by default, and you can use the newly released `jsbundling-rails` and `cssbundling-rails` gems to install esbuild and Tailwind with a single command:

```bash
rails new hotwired_ats -T -d postgresql --css=tailwind --javascript=esbuild
cd hotwired_ats
rails db:create
```

This command creates a new Rails application with Stimulus and Turbo installed, and sets up esbuild and Tailwind.

## Heads up!

Before proceeding, ensure `turbo-rails` version 1.x is installed. If not, you can fix this by running `gem uninstall turbo-rails` and then deleting `Gemfile.lock` and running `bundle install`.

## Configure Tailwind

Customize your Tailwind setup to allow importing other CSS files into `application.tailwind.css`. Also, add the Tailwind Forms plugin:

```bash
yarn add postcss-import @tailwindcss/forms
```

Update `postcss.config.js` and `tailwind.config.js` accordingly.

## Update esbuild config

Create a custom configuration for esbuild to enable source maps, minify bundles, and automatically rebuild and refresh the page on changes. 

Create `esbuild.config.js` and update it as per the provided example.

## Update bin/dev Scripts

Update the scripts in `package.json` to use the new configurations.

```json
"scripts": {
  "build": "node esbuild.config.js",
  "build:css": "tailwindcss --postcss -i ./app/assets/stylesheets/application.tailwind.css -o ./app/assets/builds/application.css"
}
```

Update `Procfile.dev` to pass in the `--rebuild` argument to the yarn build command.

## Install CableReady, StimulusReflex, and Mrujs

Install and configure StimulusReflex, CableReady, and Mrujs for server-powered frontend interactivity and reactive page updates.

```bash
bundle add stimulus_reflex --version 3.5.0.pre8
yarn add stimulus_reflex@3.5.0-pre8 mrujs
```

Follow the provided instructions to complete the installation.

## Use uuids by default

Enable UUIDs for primary keys and update Rails model generator configuration accordingly.

```bash
rails g migration EnableUUID
```

Update `config/initializers/generators.rb` and `app/models/application_record.rb` as per the provided examples.

## Create an empty dashboard

Generate a Dashboard controller and set it as the root route.

```bash
rails g controller Dashboard show
```

Update `routes.rb` to set the root route to the Dashboard’s show action.

```ruby
root to: 'dashboard#show'
```

Your application is now set up with the necessary tools and configurations to start building features! 🚀