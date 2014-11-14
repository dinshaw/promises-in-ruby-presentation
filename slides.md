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
      if something
        fulfill.call something.value
      else
        reject.call something.error
      end
    }
```

***

## Promises Recap
<p class='fragment'>* Return a promise, immediately</p>
<p class='fragment'>* Fulfills or Rejects</p>
<p class='fragment'>* Chainable / "thenable"</p>

***

## Thanks!
Brian Mitchel and Mike Davis for explaining this stuff to me.
Jed Northbridge for his Reavel-CK gem that I used to make this presentation

***

## Thoughts
* EM

