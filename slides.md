## i :heart: Promises in Ruby
Promises we can keep.

"I promise that there will be no cat pics."

Dinshaw Gobhai | [dgobhai@constantcontact.com](mailto:dgobhai@constantcontact.com)

@tallfriend

[github.com/dinshaw/promises-in-ruby](github.com/dinshaw/promises-in-ruby)

[github.com/dinshaw/promises](github.com/dinshaw/promises)

***

## Promises, eh?
"direct correspondence between synchronous and asynchronous functions"

[Callbacks, Promises, and Coroutines](https://www.youtube.com/watch?v=V2Q13hzTGmA) - Domenic Denicola

[You’re missing the point of Promises](https://blog.domenic.me/youre-missing-the-point-of-promises/) - Domenic Denicola

[Promises/A+](https://promisesaplus.com/) - Lots of very smart people

[github.com/lgierth/promise.rb](https://github.com/lgierth/promise.rb)

***

### Getting cat pics with JS

```js
getCat("Garfield", function(err, cat) {
  if (err) {
    getCatError(err)
  } else {
    getCatPics(cat, function(err, pics) {
      if (err) {
        getCatPicsError(err)
      } else {
        showCatPic(pics)
...
```

---

### Getting cat pics with a Promise

```js
getCat("Garfield")
  .then(getCatPics, getCatError)
  .then(showCatPic, getCatPicsError)
```

***

## In regular ruby

```ruby
  ...
    cat = get_cat 'Garfield'
    pics = get_cat_pics cat
    show_cat_pic pics.first
  rescue GetCat::Error => e
    handle_get_cat_error
  rescue GetCatPics::Error => e
    handle_get_cat_pics_error
  ...
```

---

## Ruby Promise

```ruby
  get_cat('Garfield').
    then(get_cat_pics, handle_get_cat_error).
    then(show_cat_pic, handle_get_cat_pics_error)

```

[Block, Proc, and Lambda](http://awaxman11.github.io/blog/2013/08/05/what-is-the-difference-between-a-block/)

---

## Ruby Promise

```ruby
  Promise.start(Cat.get('Garfield')).
    then(get_cat_pics, handle_get_cat_error).
    then(show_cat_pic, handle_get_cat_pics_error)

```

***

## Code

***

## Fulfill and Reject

```ruby
    Promise.new { |fulfill, reject|
      if my_method?
        fulfill.call my_instance.value
      else
        reject.call my_instance.error
      end
    }
```

***

## Initialize

```ruby
  def initialize
    @state = :pending
    @value = nil
    begin
      yield method(:fulfill), method(:reject)
    rescue Exception => e
      reject(e)
    end
  end
```

***

## All

```ruby
  def self.all(*promises)
    Promise.new do |fulfill, reject|
      results = []
      on_success = ->(result) do
        results << result
        fulfill.call(results) if results.size == promises.size
      end
      promises.each do |promise|
        promise.then(on_success, reject)
      end
    end
  end
```

***

## Any

```ruby
  def self.any(*promises)
    Promise.new do |fulfill, reject|
      count = promises.size
      on_error = ->(*) do
        count -= 1
        reject.call if count == 0
      end
      promises.each do |promise|
        promise.then(fulfill, on_error)
      end
    end
  end
```

***

## Promises Recap
<p class='fragment'>* Return a promise, immediately</p>
<p class='fragment'>* Fulfills or Rejects</p>
<p class='fragment'>* Chainable / "thenable"</p>

***

## Source code

[github.com/dinshaw/promises](github.com/dinshaw/promises)

***

## Thanks!
Brian Mitchel explaining this stuff to me.

Mike Davis for helping me write the talk.

Jed Northbridge for his [Reavel-CK](https://github.com/jedcn/reveal-ck) gem that I used to make this presentation.

***

## Thoughts
* EM

