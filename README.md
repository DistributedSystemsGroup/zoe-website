# Jekyll Doc Theme

Go to [the website](https://aksakalli.github.io/jekyll-doc-theme/) for detailed information and demo.

## Running locally

You need Ruby (the one in Trusty is too old) and gem before starting, then:
```bash
# install bundler
gem install bundler

# clone the project
git clone https://github.com/aksakalli/jekyll-doc-theme.git
cd jekyll-doc-theme

# Install dependencies
bundle install

# rebuild and run a test webserver
bundle exec jekyll serve  --host 0.0.0.0 --port 5555

# just rebuild
bundle exec jekyll build
```

## Adding posts

Posts are under the `_posts` directory.

## License

Released under [the MIT license](LICENSE).
