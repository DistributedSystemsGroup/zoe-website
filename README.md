# Zoe website sources

The Zoe website has been created with Jekyll.

Jekyll is a static website generator written in Ruby that takes a collection of markdown files and generates a website.

The theme in use is the Doc Theme, go to [the website](https://aksakalli.github.io/jekyll-doc-theme/) for detailed information and demo.

To be able to modify the website, you will need to have Jekyll installed, then:

1. add or modify a post or a page
2. rebuild and run locally to test your changes
3. commit everything, including the generated files to have the updates published

## Running locally

You need Ruby (the one in Trusty is too old) and gem before starting, then:
```bash
# install bundler
gem install bundler

# clone the project
git clone https://github.com/DistributedSystemsGroup/zoe-website
cd zoe-website

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
