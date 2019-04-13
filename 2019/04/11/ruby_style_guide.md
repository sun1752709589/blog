# ruby编码风格指南-持续更新

* 避免使用 class << self 除非非常必要, 比如 single accessors and aliased attributes
```ruby
class TestClass
  # bad
  class << self
    def first_method
      ...
    end

    def second_method_etc
      ...
    end
  end

  # good
  class << self
    attr_accessor :per_page
    alias_method :nwo, :find_by_name_with_owner
  end

  def self.first_method
    ...
  end

  def self.second_method_etc
    ...
  end
end
```

