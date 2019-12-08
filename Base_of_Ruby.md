# Base_of_Ruby

## Ruby

루비는 동적 객체 지향 스크립트 프로그래밍 언어이다.  
루비는 순수 객체 지향 언어이며 정수, 문자열 등을 포함한 데이터 형식 등 모든 것이 객체이다.

## 루비의 철학

- DRY(Don't Repeat Yourself) : 같은 코드를 반복하지 말 것
- CoC(Convention over configuaration) : 설정보다 규약이 중요

## 기본 문법

### 출력

- puts
```ruby
puts "Hello World!"
puts 3*2
```

### 제어

### 함수

- 정의

```ruby
def h  
puts "Hello World!"  
end  
h  
#Hello World  
```

```ruby
def h(name)
puts Hello #{name}!"
end
h("Matz")
#Hello Matz!
```

```ruby
def h(name = "World")
puts "Hello #{name.capitalize}!"
end
h chris
# Hello Chris
h
# Hello World
```

### 클래스

- 정의

```ruby
class Greeter
	def initialize(name = "World")
		@name = name
	end
	def say_hi
		puts "Hi #{name}!"
	end
	def say)bye
		puts "bye ${@name}, come back soon."
	end
end
```

- 객체생성

```ruby
g = Greeter.new("Pat")
g.say_hi
#Hi Pat!
g.say_bye
#Bye Pat, come back soon.
puts g.@name
# compile error!!
# 직접 멤버 변수에 접근 불가
```

- 객체 들여다보기
```ruby
Greeter.instance_methods
# ["method", "send", "object_id", "singleton_methods",
#   "__send__", "equal?", "taint", "frozen?",
#     "instance_variable_get", "kind_of?", "to_a",
#     "instance_eval", "type", "protected_methods", "extend",
#     "eql?", "display", "instance_variable_set", "hash",
#     "is_a?", "to_s", "class", "tainted?", "private_methods",
#     "untaint", "say_hi", "id", "inspect", "==", "===",
#     "clone", "public_methods", "respond_to?", "freeze",
#     "say_bye", "__id__", "=~", "methods", "nil?", "dup",
#     "instance_variables", "instance_of?"]
```
Greeter 객체의 메서드 뿐만 아니라 상속된 메서드도 있다.

```ruby
Greeter.instance_methods(flase)
# => ["say_bye", "say_hi"]
```
flase를 인자로 넘기면 Greeter의 메소드만 볼 수 있다.

```ruby
g.respond_to?("name")
# => false
g.respond_to?("say_hi")
# => true
g.respond_to?("to_s")
# => true
```
메서드의 보유 유무 확인

- 클래스 정의 변경하기
```ruby
class greeter
    attr_accessor :name
end
g = Greeter.new("Andy")
g.respond_to?("name")
# => true
g.respond_to?("name=")
# => true
g.say_hi
# => Hi Andy!
g.name="Betty"
# => "Betty"
g.name
# => "Betty"
g.say_hi
# => Hi Betty
```
attr_accessor는 두개의 메서드를 새로 정의해준다.  
name은 인스턴스 변수의 값에 접근해주고 name=은 객체변수의 값을 변경하기 위한 것이다.

- 순환과 반복 - 이터레이션(Iteration)
```ruby
@names.each do |name|
    puts "Hello #{name}!"
end
```

```ruby
# Say hi to everybody
def say_hi
  if @names.nil?
    puts "..."
  elsif @names.respond_to?("each")
    # @names is a list of some kind, iterate!
    @names.each do |name|
      puts "Hello #{name}!"
    end
  else
    puts "Hello #{@names}!"
  end
end
```
@names객체가 each에 반응하면, 이 객체의 내용물을 순차적으로 접근할 수 있다.  
||사이의 변수는 each 코드블록에 넘겨지는 매개 변수이다.  
c언어의 for보다는 훨신 간단하고 우아하다.

```ruby
# Say bye to everybody
def say_bye
  if @names.nil?
    puts "..."
  elsif @names.respond_to?("join")
    # Join the list elements with commas
    puts "Goodbye #{@names.join(", ")}.  Come back soon!"
  else
    puts "Goodbye #{@names}.  Come back soon!"
  end
end
```
변수의 실제 타입은 신경쓰지 않고있다. 이를 덕 타이핑(Duck Typing)이라고 한다.  

- 스크립트 실행하기
```ruby
if __FILE__ == $0
```
__FILE__은 현재 파일의 이름이 저장되어 있는 특수 변수이다.  
$0은 현재의 프로그램을 시작한 파일의 이름이 저장된 변수이다.  
즉 이 팡ㄹ이 라이브러리로 사용될 때는 코드가 실행되지 않다가, 파일 자체가 실행될 때는 코드를 실행해주게 된다.  


```ruby
class MegaGreeter
  attr_accessor :names

  # Create the object
  def initialize(names = "World")
    @names = names
  end

  # Say hi to everybody
  def say_hi
    if @names.nil?
      puts "..."
    elsif @names.respond_to?("each")
      # @names is a list of some kind, iterate!
      @names.each do |name|
        puts "Hello #{name}!"
      end
    else
      puts "Hello #{@names}!"
    end
  end

  # Say bye to everybody
  def say_bye
    if @names.nil?
      puts "..."
    elsif @names.respond_to?("join")
      # Join the list elements with commas
      puts "Goodbye #{@names.join(", ")}.  Come back soon!"
    else
      puts "Goodbye #{@names}.  Come back soon!"
    end
  end

end

if __FILE__ == $0
  mg = MegaGreeter.new
  mg.say_hi
  mg.say_bye

  # Change name to be "Zeke"
  mg.names = "Zeke"
  mg.say_hi
  mg.say_bye

  # Change the name to an array of names
  mg.names = ["Albert", "Brenda", "Charles",
    "Dave", "Engelbert"]
  mg.say_hi
  mg.say_bye

  # Change to nil
  mg.names = nil
  mg.say_hi
  mg.say_bye
end

# Hello World!
# Goodbye World.  Come back soon!
# Hello Zeke!
# Goodbye Zeke.  Come back soon!
# Hello Albert!
# Hello Brenda!
# Hello Charles!
# Hello Dave!
# Hello Engelbert!
# Goodbye Albert, Brenda, Charles, Dave, Engelbert.  Come
# back soon!
# ...
# ...

```

