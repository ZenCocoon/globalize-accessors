# Globalize Accessors [![Build Status](https://travis-ci.org/globalize/globalize-accessors.png)](https://travis-ci.org/globalize/globalize-accessors)

## Introduction

Generator of accessor methods for models using Globalize. Use globalize-accessors with a list of translated fields you want easily access to and extra :locales array listing locales for which you want the accessors to be generated.

This way a single form can be used to edit given model fields with all anticipated translations.

`globalize-accessors` is compatible with both Rails 3.x and Rails 4.


## Installation

  gem install globalize-accessors

## Example

Definition like this:

````ruby
class Product
  translates :title, :description
  globalize_accessors :locales => [:en, :pl], :attributes => [:title]
end
````

Gives you access to methods: `title_pl`, `title_en`, `title_pl=`, `title_en=` (and a similar set of description_* methods). They work seamlessly with Globalize (not even touching the "core" title, title= methods used by Globalize itself).

`:locales` and `:attributes` are optional. Default values are:

````ruby
  :locales => I18n.available_locales
  :attributes => translated_attribute_names
````

which means that skipping all options will generate you accessor method for all translated fields and available languages.

You can also get the accessor locales for a class with the `globalize_locales` method:

````ruby
  Product.globalize_locales # => [:en, :pl]
````

## Always define accessors

If you wish to always define accessors and don't want to call the globalize_accessors method in every class, you can extend ActiveRecord::Base with a module:

````ruby
module TranslatesWithAccessors

  def translates(*params)
    options = params.extract_options!
    options.reverse_merge!(:globalize_accessors => true)
    accessors = options.delete(:globalize_accessors)
    super
    globalize_accessors if accessors
  end

end
````

## Licence

Copyright (c) 2009-2013 Tomek "Tomash" Stachewicz (http://tomash.wrug.eu),  Robert Pankowecki (http://robert.pankowecki.pl) released under the MIT license
