# antedeguemon_checks

This is a collection of my personal (and highly experimental) Credo checks.

## 1. Consistency.ValidateDescribesArity

Checks if the test suite is following the describe/action spec from
[betterspecs](https://www.betterspecs.org/).

Example:

```elixir
defmodule ModuleTest do
  # ...

  describe "perform/1" do
    # this is fine
    test "is consistent" do
      Module.perform(1)
    end

    # this is going to raise a warning
    test "does not call perform/1" do
      Module.perform(1, 2, 3)
    end
  end

end
```

## 2. Warning.RedundantDelegateAlias

Checks if a `defdelegate` has a redundant `as` option.

Example:

```elixir
# this is fine
defdelegate perform(id), to: Module

# this is fine
defdelegate perform(id), to: Module, as: :perform_2

# this is going to raise a warning
defdelegate perform(id), to: Module, as: :perform

```

## 3. Warning.RejectTags

Checks if a module has a `@tag`, `@moduletag` or `@describetag`, that are
common left-over code.

## 4. Warning.UnspecifiedAsyncnessTestCase

Checks if a test module has a `(.*)Case` import that does not explicitly
defines its asyncness option.

```elixir
defmodule ModuleTest do
  # this is fine
  use OtherModule

  # this is fine
  use ExUnit.Case, async: false

  # this is fine
  use ExUnit.Case, async: true

  # this is going to raise a warning
  use ExUnit.Case

  # this is going to raise a warning
  use MyProject.DataCase
end
```
