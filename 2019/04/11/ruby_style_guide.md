# ruby编码风格指南-持续更新

### 更多请参考：[Airbnb的Ruby代码风格指南](airbnb_ruby_style_guide.md)

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
* 不要用默认参数，用一个选项 hash 来做这个事。
```ruby
# 错误
def obliterate(things, gently = true, except = [], at = Time.now)
  ...
end

# 正确
def obliterate(things, options = {})
  default_options = {
    :gently => true, # obliterate with soft-delete
    :except => [], # skip obliterating these things
    :at => Time.now, # don't obliterate them until later
  }
  options.reverse_merge!(default_options)

  ...
end
```
* 请随意用 ||= 来初始化变量。
```ruby
# 把 name 赋值为 Bozhidar, 仅当 name 是 nil 或者 false 时
name ||= 'Bozhidar'
name ||= nil
```
* 把不用的变量名命名为 _
```ruby
payment, _ = Payment.complete_paypal_payment!(params[:token],
                                              native_currency,
                                              created_at)
```
* 避免使用类变量(@@)， 因为在继承的时候它们会有 "淘气" 的行为。
```ruby
class Parent
  @@class_var = 'parent'

  def self.print_class_var
    puts @@class_var
  end
end

class Child < Parent
  @@class_var = 'child'
end

Parent.print_class_var # => 会输出"child"
```
* 避免捕捉 Exception 这个大类的异常
```ruby
# 错误
begin
  # an exception occurs here
rescue Exception
  # exception handling
end

# 正确
begin
  # an exception occurs here
rescue StandardError
  # exception handling
end

# 可以接受
begin
  # an exception occurs here
rescue
  # exception handling
end
```
* 怎样抛出错误
```ruby
# 错误
raise RuntimeError, 'message'

# 正确一点 - RuntimeError 是默认的
raise 'message'

# 最好
class MyExplicitError < RuntimeError; end
raise MyExplicitError
# 尽量将异常的类和讯息两个分开作为 raise 的参数，而不是提供异常的实例。

# 错误
raise SomeException.new('message')
# 注意，提供异常的实例没办法做到 `raise SomeException.new('message'), backtrace`.

# 正确
raise SomeException, 'message'
# 可以达到 `raise SomeException, 'message', backtrace`.

# 避免使用 rescue 的变异形式。

# 错误
read_file rescue handle_error($!)

# 正确
begin
  read_file
rescue Errno:ENOENT => ex
  handle_error(ex)
end
```
* 尽量用数组和 hash 字面量来创建， 而不是用 new。 除非你需要传参数
```ruby
# 错误
arr = Array.new
hash = Hash.new

# 正确
arr = []
hash = {}

# 正确, 因为构造函数需要参数
x = Hash.new { |h, k| h[k] = {} }
```
* 用 符号(symbols) 而不是 字符串(strings) 作为 hash keys。
```ruby
# 错误
hash = { 'one' => 1, 'two' => 2, 'three' => 3 }

# 正确
hash = { :one => 1, :two => 2, :three => 3 }
```
* 尽量用 map 而不是 collect。

* 尽量用 detect 而不是 find。 find 容易和 ActiveRecord 的 find 搞混 - detect 则是明确的说明了 是要操作 Ruby 的集合, 而不是 ActiveRecord 对象。

* 尽量用 reduce 而不是 inject。

* 尽量用 size， 而不是 length 或者 count， 出于性能理由。

* 用多行 hashes 使得代码更可读, 逗号放末尾。
```ruby
hash = {
  :protocol => 'https',
  :only_path => false,
  :controller => :users,
  :action => :set_password,
  :redirect => @redirect_url,
  :secret => @secret,
}
# 如果是多行数组，用逗号结尾。
# 正确
array = [1, 2, 3]

# 正确
array = [
  "car",
  "bear",
  "plane",
  "zoo",
]
```
* 尽量使用字符串插值，而不是字符串拼接
```ruby
# 错误
email_with_name = user.name + ' <' + user.email + '>'

# 正确
email_with_name = "#{user.name} <#{user.email}>"
```
* 当定义 ActiveRecord 的模型 scopes 时， 把内容用大括号包起来。 如果不包的话, 在载入这个 class 时就会被强迫连接数据库。
```ruby
# 错误
scope :foo, where(:bar => 1)

# 正确
scope :foo, -> { where(:bar => 1) }

```
# 保持一致 (Be Consistent)
在你编辑某块代码时，看看周围的代码是什么风格。
如果它们在数学操作符两边都放了空格，那么你也应该这样做。
如果它们的代码注释用 # 井号包成了一个盒子, 那么你也应该这样做。

风格指南存在的意义是 让看代码时能关注 "代码说的是什么"。
而不是 "代码是怎么说这件事的"。这份整体风格指南就是帮助你做这件事的。
注意局部的风格同样重要。如果一个部分的代码和周围的代码很不一样。
别人读的时候思路可能会被打断。尽量避免这一点。