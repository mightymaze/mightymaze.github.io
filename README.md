www.mazharahmed.me
====================

My Personal blog in Jekyll

## Requirements

- Ruby
- Gem
- Bundler
- Jekyll
- Algolia
- Jekyll Admin
- Python 2 or 3

## How to run

- Install Ruby 2.5.1
- Keep `ruby-dev` package installed
- Just run `sudo gem install bundler` first
- If you face any problem run `sudo gem install bundler -v '1.17.3'`
- Run `bundle install` to install all dependencies
- Run `jekyll serve` to start the server on 4000 port
- Browse `http://localhost:4000/admin` to access jekyll-admin
- After writing articles run `python3 tag_genarator.py` to generate tags pages
- Also run `ALGOLIA_API_KEY=... jekyll algolia` to update the indexes on algolia
- Commit and see the update on the site

## Credits

- Mazhar Ahmed [twitter.com/humblemaze](https://twitter.com/humblemaze)
