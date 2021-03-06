mruby-merb
==========

This is a mruby gem provinding an ERB-like mechanism for converting template files with embedded mruby code.

Installation
------------

Simply add the following line to your `build_config.rb` file in mruby root:

    conf.gem :github => "pbosetti/mruby-merb", :branch => "master"

and then recompile mruby with `make clean; make`.

Usage
-----

Due to the fact that mruby lacks the `Binding` class, values must be passed from toplevel (or calling scope) to the code within the template by using global variables, as in the following example:

    merb = MERB.new "The value of $x is: <%= $x -%>"
    $x = 2.3
    result = merb.run
    puts result #=> The value of $x is: 2.3

Of course you can load the template string from an external file:

    merb = Merb.new
    result = merb.convert("test.erb")

Supported erb tags
------------------

* `<% ... %>`: standard ruby code execution (no printout)
* `<%= ... %>`: execute and insert the resulting value as a string; adds a newline
* `<%= ... -%>`: execute and insert the resulting value as a string; no newline added (note the dash in the closing tag!)

An arbitrary pair of opening and closing tags can be specified instead of the `<%` and `%>` default:

    merb.tags[:open] = '{-'
    merb.tags[:close] = '-}'
