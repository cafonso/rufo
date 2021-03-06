# Rufo

[![Build Status](https://travis-ci.org/asterite/rufo.svg)](https://travis-ci.org/asterite/rufo)

**Ru**by **fo**rmatter

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'rufo'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install rufo

## Usage

### Format files or directories

```
$ rufo file names or dir names
```

### Format STDIN

```
$ cat file.rb | rufo
```

### Check that no formatting changes are produced

```
$ rufo --check file names or dir names
```

This will print one line for each file that isn't correctly formatted
according to **rufo**, and will exit with exit code 1.

## Editor support

- Sublime Text: [sublime-rufo](https://github.com/asterite/sublime-rufo)

## Configuration

Rufo follows most (if not all) of the conventions found in this [Ruby style guide](https://github.com/bbatsov/ruby-style-guide). It does a bit more than that, and it can also be configured a bit.

To configure it, place a `.rufo` file in your project. When formatting a file or a directory
via the `rufo` program, a `.rufo` file will try to be found in that directory or parent directories.

The `.rufo` file is a Ruby file that is evaluated in the context of the formatter. These are the
available configurations:

```ruby
# Whether to align successive comments (default: true)
align_comments      true

# Whether to align successive assignments (default: true)
align_assignments   true

# Whether to align successive hash keys (default: true)
align_hash_keys     true

# The indent size (default: 2)
indent_size         2
```

As time passes there might be more configurations available. Please open an
issue if you need something else to be configurable.

## Status

The formatter is able to format `rails` and other projects, so at this point
it's pretty mature. There might still be some bugs. Don't hesitate
to open an issue if you find something is not working well. In any case, if the formatter
chokes on some valid input you will get an error prompting you to submit a bug report here :-)

## How it works

Rufo is a **real** formatter, not a simple find and replace one. It works by employing
a Ruby parser and a Ruby lexer. The parser is used for the shape of the program. The program
is traversed and the lexer is used to sync this structure to tokens. This is why comments
can be handled well, because they are provided by the lexer (comments are not returned by
a parser).

To parse and lex, [Ripper](https://ruby-doc.org/stdlib-2.4.0/libdoc/ripper/rdoc/Ripper.html) is used.

As a reference, this was implemented in a similar fashion to [Crystal](https://github.com/crystal-lang/crystal)'s formatter.

And as a side note, rufo has **no dependencies**. It only depends on `rspec` for tests, but that's it.
That means it loads very fast (no need to read many Ruby files), and because `Ripper` is mostly written
in C (uses Ruby's lexer and parser) it formats files pretty fast too.

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake spec` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/asterite/rufo.

## License

The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).
