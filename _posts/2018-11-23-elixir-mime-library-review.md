---
date: 2018-11-23T19:40:31Z
layout: post
title: Elixir's MIME library review
slug: elixir-mime-library-review
share: true
author: milmazz
tags:
- programming
- elixir
---

Elixir's [MIME][] is a read-only and immutable library that embeds the [MIME type
database][db], so, users can map MIME (Multipurpose Internet Mail Extensions)
types to extensions and vice-versa. It's a really compact project and includes
nice features, which I'll try to explain in detail in this article.

One of the goals, maybe the main one, of this library is to offer a _performant
lookup_ of the MIME database at runtime, that's why new MIME types can only be
added at compile-time via configuration, but we'll talk about this option
later. First, let's review its public API.

## API

MIME offers a short set of functions, which cover the most relevant cases when
you work with MIME types.

Let's review real quick the MIME library API, most of the examples were taken
from the MIME's [documentation page][hexdocs].

### `extensions(String.t()) :: [String.t()]`

Returns the extensions associated with the given MIME type.

```elixir
iex> MIME.extensions("application/json")
["json"]
iex> MIME.extensions("foo/bar")
[]
```

### `type(String.t()) :: String.t()`

Returns the MIME type related to the given file extension.

```elixir
iex> MIME.type("txt")
"text/plain"
```


### `from_path(Path.t()) :: String.t()`

Guesses the MIME type based on the path's extension.

```elixir
iex> MIME.from_path("index.html")
"text/html"
```

### `has_type?(String.t()) :: boolean`

Returns whether an extension has a MIME type associated.

```elixir
iex> MIME.has_type?("txt")
true
iex> MIME.has_type?("foobarbaz")
false
```

### `valid?(String.t()) :: boolean`

Returns whether a MIME type is registered.

```elixir
iex> MIME.valid?("text/plain")
true
iex> MIME.valid?("foo/bar")
false
```

## Who is using MIME library?

At the time of this writing, and according to the statistics available from the
[Hex package manager][hex], the [MIME library has 21 dependents
projects][stats], among those projects you can find: [Plug][plug],
[Phoenix][phoenix], [Tesla][tesla], [Swoosh][swoosh], etc., and have been
downloaded almost 6 million times. But more importantly, at least to me, is how
the MIME library is implemented, its code is really concise, it's around 200
SLOC (Source Lines Of Code) including comments, and embed captivating concepts.

## How was the MIME library built?

Now, let's start looking into how the MIME library was built.

Inside of the `MIME.Application.quoted/1` function you can find the following
section:

```elixir
# file: lib/mime/application.ex
mime_file = Application.app_dir(:mime, "priv/mime.types")
@compile :no_native
@external_resource mime_file
stream = File.stream!(mime_file)

mapping =
  Enum.flat_map(stream, fn line ->
    if String.starts_with?(line, ["#", "\n"]) do
      []
    else
      [type | exts] =
        line
        |> String.trim()
        |> String.split()

      [{type, exts}]
    end
  end)
```

You can notice that the `MIME` library transforms the data located in the
`priv/mime.types` file, which is a copy of the IANA (Internet Assigned Numbers
Authority) database in text format and describes what Internet media types are
sent to the client for the given file extension(s). Keep in mind that sending
the correct media type to the client is important so they know how to handle
the content of the file.

Here is an example of the `priv/mime.types` file content:

```
# file: priv/mime.types
# IANA types

# MIME type         Extensions
application/ATXML       atxml
application/atom+xml        atom
application/atomcat+xml       atomcat
application/octet-stream    bin lha lzh exe class so dll img iso
application/pdf         pdf
```

To do the transformation, the `MIME` library reads the file line by line (via
[`File.stream!/3`][stream]) at compile time, and ignores empty lines or lines
that start with a comment (`#`), and then creates a list of `{type,
extensions}` tuples. The result of this transformation is stored in a binding
called `mapping`.

Is important to note the usage of two Module attributes, the first one is
`@external_resource`, which as its name implies, specifies an external resource
for the given module, this attribute is used for tools like `Mix` to know if
the current module needs to be recompiled in the case that any external
resource is updated. Lastly, the `@compile` attribute defines options for the
module compilation, this is used to configure both Elixir and Erlang compilers.

Once the `MIME` library has transformed the data and stored the result in
`mapping`, it creates two private helper functions:

The first private helper function is `ext_to_mime/1`, which returns the MIME
type given an extension:

```elixir
@spec ext_to_mime(String.t()) :: String.t() | nil
defp ext_to_mime(type)

for {type, exts} <- mapping,
    ext <- exts do
  defp ext_to_mime(unquote(ext)), do: unquote(type)
end

defp ext_to_mime(_ext), do: nil
```

The `MIME` library creates thousands of `ext_to_mime/1` function clauses inside
of the `for` comprehension, this is a clear example of the power of
meta-programming and how the MIME library relies on _pattern matching_ to be
_performant_. And this is possible because of Elixir `quote` and `unquote`
mechanisms provide a feature called _unquote fragments_, that way is easy to
create function on-the-fly at compile time.

To give you a better idea, here is a section of the final result:

```elixir
defp ext_to_mime("atom"), do: "application/atom+xml"
defp ext_to_mime("pdf"), do: "application/pdf"
defp ext_to_mime("dll"), do: "application/octet-stream"
defp ext_to_mime("class"), do: "application/octet-stream"
# …
defp ext_to_mime(_ext), do: nil
```

To complete the function declaration you see a catch-all function clause, which
will be used if any match is not found.

The second private helper function declaration is `mime_to_ext/1`, this
function expects a MIME type and will return a list of extensions or `nil`.

```elixir
@spec mime_to_ext(String.t()) :: list(String.t()) | nil
defp mime_to_ext(type)

for {type, exts} <- mapping do
  defp mime_to_ext(unquote(type)), do: unquote(exts)
end

defp mime_to_ext(_type), do: nil
```

The result of the transformation should be similar to:

```elixir
defp mime_to_ext("application/atom+xml"), do: ["atom"]
defp mime_to_ext("application/octet-stream"),
  do:  ["bin", "lha", "lzh", "exe", "class", "so", "dll", "img", "iso"]
defp mime_to_ext("application/pdf"), do: ["pdf"]
# …
defp mime_to_ext(_type), do: nil
```

From here, is easy to build the public functions:

```elixir
@spec valid?(String.t()) :: boolean
def valid?(type) do
  is_list(mime_to_ext(type))
end

def extensions(type) do
  mime_to_ext(type) || []
end

@default_type "application/octet-stream"

@spec type(String.t()) :: String.t()
def type(file_extension) do
  ext_to_mime(file_extension) || @default_type
end

def has_type?(file_extension) do
  is_binary(ext_to_mime(file_extension))
end

def from_path(path) do
  case Path.extname(path) do
    "." <> ext -> type(downcase(ext, ""))
    _ -> @default_type
  end
end

defp downcase(<<h, t::binary>>, acc) when h in ?A..?Z,
  do: downcase(t, <<acc::binary, h + 32>>)

defp downcase(<<h, t::binary>>, acc), do: downcase(t, <<acc::binary, h>>)
defp downcase(<<>>, acc), do: acc
```

That's it!, at least with these functions, the MIME library cover the main
features. But wait, there is more, do you remember that at the beginning I
mentioned the following:

> One of the goals, maybe the main one, of this library is to be provide a
> _performant lookup_ of the MIME database at runtime, that's why new MIME types
> can only be added at compile-time via configuration, but we'll talk about
> this option later…

So, this means that we can add a MIME type like `application/wasm` for the
extension: `wasm`, which have been added to the [provisional standard media
type registry][provisional] but is not official yet.

Via configuration you can do the following:

```elixir
# file: config/config.exs
use Mix.Config

config :mime, :types, %{"application/wasm" => ["wasm"]}
```

Then, MIME **needs to be recompiled**, using `Mix` you can do the following:

```console
mix deps.clean mime --build
mix deps.get
```

You can test the result via `IEx`:

```elixir
iex> MIME.type("wasm")
"application/wasm"
iex> MIME.extensions("application/wasm")
["wasm"]
```

Now you may be wondering, how does it work? and that's an excellent question,
let's try to find an answer to that.

In the previous function declarations of `ext_to_mime/1` and `mime_to_ext/1`
I've omitted two function clauses on purpose, which are specifically related
with the custom types handling, let's see the whole declaration for those two
functions now:

```elixir
@spec ext_to_mime(String.t()) :: String.t() | nil
defp ext_to_mime(type)

for {type, exts} <- custom_types,
    ext <- List.wrap(exts) do
  defp ext_to_mime(unquote(ext)), do: unquote(type)
end

for {type, exts} <- mapping,
    ext <- exts do
  defp ext_to_mime(unquote(ext)), do: unquote(type)
end

defp ext_to_mime(_ext), do: nil

@spec mime_to_ext(String.t()) :: list(String.t()) | nil
defp mime_to_ext(type)

for {type, exts} <- custom_types do
  defp mime_to_ext(unquote(type)), do: unquote(List.wrap(exts))
end

for {type, exts} <- mapping do
  defp mime_to_ext(unquote(type)), do: unquote(exts)
end

defp mime_to_ext(_type), do: nil
```

Now you can see that these custom MIME types come first. But wait a minute,
where is `custom_types` binding coming from?, well that's the first argument of
the `MIME.Application.quoted/1` function.

There is another function that uses the `custom_types` binding, and that's
`compiled_custom_types/0`, which as its name implies, returns the custom types
compiled into the MIME library.

```elixir
def compiled_custom_types do
  unquote(Macro.escape(custom_types))
end
```

The last thing that I want to mention related to the function
`MIME.Application.quoted/1` is that this function returns a _quoted_
expression:

```elixir
def quoted(custom_types) do
  quote bind_quoted: [custom_types: Macro.escape(custom_types)] do
    mime_file = Application.app_dir(:mime, "priv/mime.types")
    @compile :no_native
    # …
  end
end
```

You can see here that the `bind_quoted` option is passing a binding to the
macro, please keep in mind that the `bind_quoted` option is recommended every
time you want to inject a value into the `quote`.

If you execute the function `MIME.Application.quoted/1` in a `IEx` session you
will get something like this:

```elixir
iex> MIME.Application.quoted(%{})
{:__block__, [],
 [
   {:=, [], [{:custom_types, [], MIME.Application}, {:%{}, [], []}]},
   {:__block__, [],
    [
      {:@, [context: MIME.Application, import: Kernel],
       [
         {:moduledoc, [context: MIME.Application],
          # …
```

So, our beloved `MIME.Application.quoted/1` function is actually returning an
Elixir data structure. But, who consumes that data structure? Let's check the
`lib/mime.ex` file contents:

```elixir
# file: lib/mime.ex
quoted = MIME.Application.quoted(Application.get_env(:mime, :types, %{}))
Module.create(MIME, quoted, __ENV__)
```

Believe me, that's all the content on `lib/mime.ex` at the moment. In the first
line you can see a call to `MIME.Application.quoted/1` passing as argument the
custom MIME types defined via configuration or an empty map as a fallback, the
result of that invocation is stored in the `quoted` binding. Then, the second
line will create a module with the given name of `MIME` and it will be defined
by the previous _quoted expression_, keep in mind that the function
`Module.create/3`, compared with `Kernel.defmodule/2`, is preferred when the
module body is a _quoted expression_ and another advantage is that
`Module.create/3` allow you to control the environment variables used when
defining the module.

## Automatic recompilation

When we started talking about how to add custom MIME types via configuration,
we also mentioned that **we need to recompile the library**. So, what happens
in the case you forget about doing that? Well, your changes will not take
effect until the dependency is manually recompiled, and before the release
`1.3.0` you didn't see any _warning_ about it.

Since version `1.3.0` of the MIME library, the recompilation process is
automatic if the compile-time database is out of date.

```elixir
# file: lib/mime/application.ex
defmodule MIME.Application do
  use Application
  require Logger

  def start(_, _) do
    app = Application.fetch_env!(:mime, :types)

    if app != MIME.compiled_custom_types() do
      Logger.error("""
      The :mime library has been compiled with the following custom types:

          #{inspect(MIME.compiled_custom_types())}

      But it is being started with the following types:

          #{inspect(app)}

      We are going to dynamically recompile it during boot,
      but please clean the :mime dependency to make sure it is recompiled:

          $ mix deps.clean mime --build

      """)

      Module.create(MIME, quoted(app), __ENV__)
    end

    Supervisor.start_link([], strategy: :one_for_one)
  end

  def quoted(custom_types) do
  # …
  end
end
```

So, what this means is that the `MIME` library at boot-time, will log an error
and will try to dynamically recompile the `MIME` module if the custom mime
types of the user environment are different from the ones returned by
`MIME.compiled_custom_types/0`, which is great! but, as the log messages
says, it's recommended to clean the `:mime` dependency to make sure it's
recompiled.

## Summary

Elixir `MIME` is a short but powerful library, its goal is clear, and in just
around 200 SLOC you can see a lot of nice concepts, like _meta-programming_,
_file streams_, _pattern matching_, _macros_, _unquote fragments_, _dynamic
module creation_, _dynamic recompilation at boot-time_, among other really cool
stuff.

That's all folks! Thanks for reading.

[MIME]: https://github.com/elixir-plug/mime
[db]: https://pagure.io/mailcap/blob/master/f/mime.types
[hex]: https://hex.pm/
[stats]: https://hex.pm/packages?search=depends%3Amime
[plug]: https://hex.pm/packages/plug
[phoenix]: https://hex.pm/packages/phoenix
[tesla]: https://hex.pm/packages/tesla
[swoosh]: https://hex.pm/packages/swoosh
[hexdocs]: https://hexdocs.pm/mime/MIME.html
[stream]: https://hexdocs.pm/elixir/File.html#stream!/3
[provisional]: https://www.iana.org/assignments/provisional-standard-media-types/provisional-standard-media-types.xhtml
