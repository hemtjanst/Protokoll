# Protokoll [![Build Status](https://travis-ci.org/hemtjanst/protokoll.svg?branch=master)](https://travis-ci.org/hemtjanst/protokoll)

Hemtjänst documentation.

View it at [hemtjan.st/protokoll](https://hemtjan.st/protokoll).

Powered by:

* [Jekyll](https://jekyllrb.com/)
* [Just The Docs](https://pmarsceill.github.io/just-the-docs/)

## Development

In order to run the site locally, you'll need Ruby and Bundler:

```sh
$ bundle install
$ bundle exec jekyll serve -w --config _config.yml,_config-dev.yml
Configuration file: _config.yml
Configuration file: _config-dev.yml
            Source: github.com/hemtjanst/protokoll
       Destination: github.com/hemtjanst/protokoll/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
                    done in 2.354 seconds.
 Auto-regeneration: enabled for 'github.com/hemtjanst/protokoll'
    Server address: http://127.0.0.1:4000/
  Server running... press ctrl-c to stop.
```

A number of Rake tasks are provided:

```sh
$ bundle exec rake -vT
rake build     # Build the site with Jekyll
rake clean     # Remove generated site
rake doctor    # Check for Jekyll deprecation issues
rake test      # Build and validate the site
rake validate  # Validate _site/ with html-proofer
```

CI will run `rake test` on PRs.
