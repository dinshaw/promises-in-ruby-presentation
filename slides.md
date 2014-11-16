## i :heart: Promises in Ruby
No cat pics, I promise.

Dinshaw Gobhai | [dgobhai@constantcontact.com](mailto:dgobhai@constantcontact.com)

Twitter :  @tallfriend

Slides : [github.com/dinshaw/promises-in-ruby](http://dinshaw.github.io/promises-sildes)

Code : [github.com/dinshaw/promises](http://github.com/dinshaw/promises)

---

## Promises, eh?
> direct correspondence between synchronous and asynchronous functions

[Callbacks, Promises, and Coroutines](https://www.youtube.com/watch?v=V2Q13hzTGmA) - Domenic Denicola

[You’re missing the point of Promises](https://blog.domenic.me/youre-missing-the-point-of-promises/) - Domenic Denicola

[Promises/A+](https://promisesaplus.com/) - Lots of very smart people

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
1|  getCat("Garfield")
2|    .then(getCatPics, getCatError)
3|    .then(showCatPic, getCatPicsError)
```

***
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
1|    get_cat('Garfield').
2|      then(get_cat_pics, handle_get_cat_error).
3|      then(show_cat_pic, handle_get_cat_pics_error)
```

[Block, Proc, and Lambda](http://awaxman11.github.io/blog/2013/08/05/what-is-the-difference-between-a-block/) - Adam Waxman

---

## Ruby Promise

```ruby
 1|  Promise.start(Cat.get('Garfield')).
 2|    then(get_cat_pics, handle_get_cat_error).
 3|    then(show_cat_pic, handle_get_cat_pics_error)
```

***

## Code

---

## Fulfill and Reject

```ruby
  Promise.new { |fulfill, reject|
    cat_pics = get_cat_pics
    if cat_pics.size &gt; 0
      fulfill.call cat_pics
    else
      reject.call 'No cat pics!'
    end
  }
```

---

## Initialize

```ruby
1|  def initialize
2|    @state = :pending
3|    @value = nil
4|    begin
5|      yield method(:fulfill), method(:reject)
6|    rescue Exception => e
7|      reject(e)
8|    end
9|  end
```

---

## More code

---

## All

```ruby
1 |  def self.all(promises)
2 |    Promise.new do |fulfill, reject|
3 |      results = []
4 |      on_success = ->(result) do
5 |        results &lt;&lt; result
6 |        if results.size == promises.size
7 |          fulfill.call(results)
8 |        end
9 |      end
10|      promises.each do |promise|
11|        promise.then(on_success, reject)
12|      end; end; end
```

---

## Any

```ruby
1 |  def self.any(promises)
2 |    Promise.new do |fulfill, reject|
3 |      count = promises.size
4 |      on_error = ->(*) do
5 |        count -= 1
6 |        reject.call if count == 0
7 |      end
8 |      promises.each do |promise|
9 |        promise.then(fulfill, on_error)
10|      end; end; end
```

---

## Promises Recap
<blockquote>
  Wouldn't it be nice to see these flows the way we think about them? - @mcdavis
</blockquote>
<p class='fragment'>* Return a promise, immediately</p>
<p class='fragment'>* Fulfills or Rejects</p>
<p class='fragment'>* Chainable / "thenable"</p>


---

## Source code

[github.com/dinshaw/promises](github.com/dinshaw/promises)

---

## Thanks!
Brian Mitchel explaining this stuff to me.

Mike Davis for helping me write the talk.

Jed Northbridge for his [Reavel-CK](https://github.com/jedcn/reveal-ck) gem that I used to make this presentation.

Constant Contact for sending me here.

### Questions?
