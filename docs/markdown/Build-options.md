---
short-description: Build options to configure project properties
...

# Build options

Most non-trivial builds require user-settable options. As an example a
program may have two different data backends that are selectable at
build time. Meson provides for this by having a option definition
file. Its name is `meson_options.txt` and it is placed at the root of
your source tree.

Here is a simple option file.

```meson
option('someoption', type : 'string', value : 'optval', description : 'An option')
option('other_one', type : 'boolean', value : false)
option('combo_opt', type : 'combo', choices : ['one', 'two', 'three'], value : 'three')
option('array_opt', type : 'stringarray', value : ['one', 'two'])
```

This demonstrates the four basic option types and their usage. String
option is just a free form string, a boolean option is,
unsurprisingly, true or false and the string array option contains an
array of strings.  The combo option can have any value from the
strings listed in argument `choices`. If `value` is not set, it
defaults to empty string for strings, `true` for booleans or the first
element in a combo. You can specify `description`, which is a free
form piece of text describing the option. It defaults to option name.

These options are accessed in Meson code with the `get_option`
function.

```meson
optval = get_option('opt_name')
```

This function also allows you to query the value of Meson's built-in
project options. For example, to get the installation prefix you would
issue the following command:

```meson
prefix = get_option('prefix')
```

It should be noted that you can not set option values in your Meson
scripts. They have to be set externally with the `meson configure`
command line tool. Running `meson configure` without arguments in a
build dir shows you all options you can set.

To change their values use the `-D`
option:

```console
$ meson configure -Doption=newvalue
```

Setting the value of arrays is a bit special. If you only pass a
single string, then it is considered to have all values separated by
commas. Thus invoking the following command:

```console
$ meson configure -Darray_opt=foo,bar
```

would set the value to an array of two elements, `foo` and `bar`.

If you need to have commas in your string values, then you need to
pass the value with proper shell quoting like this:

```console
$ meson configure "-Doption=['a,b', 'c,d']"
```

The inner values must always be single quotes and the outer ones
double quotes.

**NOTE:** If you cannot call `meson configure` you likely have a old
  version of Meson. In that case you can call `mesonconf` instead, but
  that is deprecated in newer versions
