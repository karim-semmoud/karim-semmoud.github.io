+++
author = "Karim Semmoud"
title = "Phoenix Gettext & Crowdin"
date = "2023-08-02"
description = "Manage Phoenix Gettext translations with Crowdin"
tags = [
    "Phoenix",
    "Elixir",
    "Gettext",
    "Crowdin",
]
+++

# PhoenixCrowdin

This reposiory is an example on managed translations using

- Phoenix framework
- Gettext
- Plugs
- Tests

## Prerequise

- Elixir and Phoenix installed
- ASDF
- NPM
- Github Account
- Crowdin Account

## Run Locally

```
# Clone this repository
git clone https://github.com/karim-semmoud/phoenix-crowdin.git

# Enter the project folder 
cd phoenix-crowdin

# install Erlang and Elixir correct versions 
asdf install

# install and setup dependencies
mix deps.get

# install Flowbite
cd assets
npm install

# run tests
mix test

# Start Phoenix endpoint with
mix phx.server
# or inside IEx with 
iex -S mix phx.server


```

Now you can visit [`localhost:4000`](http://localhost:4000) from your browser.
You can use the language switch on the top right or visit
[`localhost:4000/?locale=fr`](http://localhost:4000/?locale=fr)

The logic is as follow

Default language is English en
Additional language is French fr

- If URL parameter (?locale=fr)exist and is valid, use it
- Else application language cookie exist use this language
- Else if browser language exist use this language
- Else use default language

## Connect your Crowdin project

## Feeling like doing it from scratch ?

### Create Phoenix app

```
mix phx.new phoenix-crowdin

#Oups ... So we could use mix phx.new phoenix_crowdin but lets be honest, who use underscores in github repository names ? Lets fix that

mix phx.new phoenix-crowdin --app phoenix_crowdin

#The folder name will be phoenix-crowdin andt the app PhenixCrowdin

```

That is great we have our Phoenix app setup  

Before anything else lets create our `tools-versions`

```
#Setup tools.versions copy the result
asdf install

#Create new file
sudo nano .tool-versions

#Paste your versions here
elixir 1.15.2-otp-26
erlang 26.0.2
nodejs 20.4.0

```

Lets create our Github reposotory

```
git init
git commit
git remote add origin git@github.com:username/phoenix-crowdin
git push -u origin master
```

Start app

```
mix phx.server
```

### Setup Gettext

For more details see [[Phoenix Framework Tips for beginners]] for more details

```

  

mix gettext.extract

# Create other languages

mix gettext.merge priv/gettext --locale fr

mix gettext.merge priv/gettext --locale ar

  

#Merge all lang

mix gettext.merge priv/gettext

```

Setup Crowdin

Create an account at ( only one free private project but as many public projects )

Go to Integration and search for Github

Select your github repo, your master branch and click on the edit button next to the " Service branch"

As a source file path enter

`/priv/gettext/en/LC_MESSAGES/*.po`

As a translated path

`/priv/gettext/%two_letters_code%/LC_MESSAGES/%original_file_name%`

The syntax is important and you can verify visually the folders on the right

Click save

Push Options

For testing purpose you can enable this options. It will create pull request each time there are new translations. However is a real life scenario it would create way too many pull request and would probably not fit the translation workflow. For example you may wish to wait until the language is translated a 100% before creating a pull request

Crowdin will automatically add a crowdin.yml confi file to your repo.

You should pull the new file to your machine

```

files:

- source: /priv/gettext/en/LC_MESSAGES/*.po

translation: /priv/gettext/%two_letters_code%/LC_MESSAGES/%original_file_name%

```

Let make some translations

Got go to Crowdin Translation select french and default

Translate the 2 strings ( actions is the same)

You can make more translations to the error file also

Once saved

It create also a pull request

Lets now create some new string

Open `lib/phoenix_crowdin_web/controllers/page_html/home.html.heex`

and modify `Phoenix Framework` by

```

<%= gettext("Phoenix Framework") %>

```

By default this string will be save in default.po file.

To save it in errors file use

```

<%= dgettext("errors", "Phoenix Framework") %>

```

Lets replace all the texts on the page

We then run

```

mix gettext.extract

#Compiling 15 files (.ex)

#Extracted priv/gettext/default.pot

  

mix gettext.merge priv/gettext

#Wrote priv/gettext/en/LC_MESSAGES/default.po (11 new messages, 0 removed, 2 #unchanged, 0 reworded (fuzzy), 0 marked as obsolete)

#Wrote priv/gettext/en/LC_MESSAGES/errors.po (0 new messages, 0 removed, 24 #unchanged, 0 reworded (fuzzy), 0 marked as obsolete)

#Wrote priv/gettext/fr/LC_MESSAGES/errors.po (0 new messages, 0 removed, 24 #unchanged, 0 reworded (fuzzy), 0 marked as obsolete)

#Wrote priv/gettext/fr/LC_MESSAGES/default.po (11 new messages, 0 removed, 2 # # #unchanged, 0 reworded (fuzzy), 0 marked as obsolete)

  

```

Let commit our changes to GitHub

Crowdin automatically check for new commits and update on `priv/gettext` files

### Language Switch

#### Locale Plug

This Plug uses the following ressources

<https://github.com/smeevil/set_locale/tree/master>

<https://phrase.com/blog/posts/set-and-manage-locale-data-in-your-phoenix-l10n-project/>

#### Flowbite Dropdown

Follow this guide in order to add Flowbite CSS and JS

<https://flowbite.com/docs/getting-started/phoenix/>

## Learn more

- Official website: <https://www.phoenixframework.org/>

- Guides: <https://hexdocs.pm/phoenix/overview.html>

- Docs: <https://hexdocs.pm/phoenix>

- Forum: <https://elixirforum.com/c/phoenix-forum>

- Source: <https://github.com/phoenixframework/phoenix>
