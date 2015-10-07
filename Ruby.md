# Ruby

## OOP
```ruby
class Rectangle
  attr_reader :width, :height

  def initialize(width, height)
    @width = width
    @height = height
  end

  def area
    width*height
  end
end

class Square < Rectangle
  def initialize(length)
    @width = length
    @height = length
  end
end

Rectangle.new(2, 2).area # 4
Square.new(5).area # 25 
```

### Conventions
```ruby
obj.method? # returns Boolean
obj.method! # mutates object
```

## Functional programming
```ruby
(1..5).map{|x| x*x}
# [ 1, 4, 9, 16, 25 ]

(1..5).each_with_index.map{|x, i| x*i}
# [ 0, 2, 6, 12, 20 ]

(1..5).inject(0){|sum, x| sum+x}
# 15

["cat", "bob", "bro", "noon"].grep(/(.).+\1/)
# ["b", "c"]
```
