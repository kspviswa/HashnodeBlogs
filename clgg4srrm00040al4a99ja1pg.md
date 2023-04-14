---
title: "Nuances of different types of `this` in Javascript"
seoTitle: "Nuances of different types of `this` in Javascript"
seoDescription: "Nuances of different types of `this` in Javascript"
datePublished: Fri Apr 14 2023 05:51:23 GMT+0000 (Coordinated Universal Time)
cuid: clgg4srrm00040al4a99ja1pg
slug: nuances-of-different-types-of-this-in-javascript
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/geM5lzDj4Iw/upload/f6ce00f41b55d262f12c9b9b66ba5f36.jpeg
tags: javascript

---

In this article, I will talk about the different nuances of `this` keyword in Javascript.

## **1) Global** `this`

If you access `this` simply without any object reference, then it would simply mean that you are referring to global `window` object.

For eg:

```javascript
f = function() { this.sampleAttr = 'Viz'; console.log('This object here is ' + this); }
```

Now executing function `f()`

```yaml
f()
VM815:1 This object here is [object Window]
```

## **2) Scoped** `this`

Now consider this example

```javascript
obj = {num1:0, add: function(num2) { this.num1 += num2; return this.num1 }}
obj.add(10)
10
obj.add(20)
30
```

As you can see, inside the `obj's` scope, `this` refers to `obj` object itself, not the global window object.

## **3)** `this` "outside" the function of object’s scope

Now consider this example.

```javascript
obj = {num1: 10, 
	   add: function(num2) { 
				console.log("This inside add is " + this); 
				h = function(num2) {
						console.log("This inside h is " + this); this.num1 += num2; }; 
		return h(num2); 
						} 
}

obj.add(20);

VM1973:1 This inside add is [object Object]
VM1973:1 This inside h is [object Window]

undefined
```

As you can see, inside the `add()`’s scope, `this` was pointing to `obj.this`. However inside the *inner* function `h()` ( even though the scope is within `obj` ), `this` has been reverted to the **global scope**.

### **How to solve this problem?**

Take a copy of `obj.this` as `that` and reuse inside `h()`. The complete example isgiven below.

```javascript
obj = {num1: 10, 
	   add: function(num2) { 
			console.log("This inside add is " + this); 
			var that = this; 
			h = function(num2) {
					console.log("That ( this)  inside h is " + that); 
					that.num1 += num2; 
					return that.num1; 
					}; 
			return h(num2); 
		} 
	}
```

```yaml
obj.add(20);
VM2045:1 This inside add is [object Object]
VM2045:1 That ( this)  inside h is [object Object]
30

obj.add(20);
VM2045:1 This inside add is [object Object]
VM2045:1 That ( this)  inside h is [object Object]
50
```

## **4) Explicit** `this` using `apply()` & `bind()`

If instead of taking a copy of local `this` and re-using it, you actually tell the inner function to get glued to the object itself using `apply()` & `bind()`.

```javascript
obj = {num1: 10, 
	   add: function(num2) { 
			console.log("This inside add is " + this); 
			h = function(num2) {
				console.log("That ( this)  inside h is " + this); 
				this.num1 += num2; 
				return this.num1; }.bind(this); 
			return h(num2); 
			} 
	}
```

```plaintext
obj.add(20);
VM2092:1 This inside add is [object Object]
VM2092:1 That ( this)  inside h is [object Object]
30

obj.add(20);
VM2092:1 This inside add is [object Object]
VM2092:1 That ( this)  inside h is [object Object]
50
```

## **5) Implicit** `this` using a constructor

By default, functions starting with capital letter will be treated like a constructor for that particular construct. In below example, I’m creating a new type called `P`.

```javascript
function P(num) { this.num = num;}

p = new P(10);
```

What I did? I simply created a constructor for `P` and assigned the incoming `num` to `P.num`. This is achieved implicitly by using `this` keyword. `p` is filled from prototype `P`.

```yaml
p.num
10
```

Now, let me omit the `new` keyword and call the constructor like a normal function, like this.

```yaml
p1 = P(10);

p1.num
VM2340:1 Uncaught TypeError: Cannot read property 'num' of undefined(…)(anonymous 
```

What just happened ?? There is no member named `num` for `p1` ??? So what’s type of `p` then???

```yaml
typeof p
"object"
```

Aha.. `p` is type of generic object, not our mighty `P`. So what about the instruction `this.num = num` would have done ?? You guessed it right. It just added `num` property to global `window` object. Or in other words, `this` pointed to `window` object.

```yaml
window.num
10
```