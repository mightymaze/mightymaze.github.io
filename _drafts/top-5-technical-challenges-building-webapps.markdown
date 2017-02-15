---
layout: post
title: Top 5 Technical Challenges building webapps
---

###No more gerbase coding
Use the standard coding pattern and always comment, document and indent your code. Follow your team leader's rules. Without following rules your code is nothing to the industry. Example:
```
/**
 * @name getCartAmount
 * @author Your Name
 * @desc this function will return the cart amount
 * 
 * @returns a float number
 */
function getCartAmount ()
{
	// total amount from the cart
	$total = $_SESSION ['cart_amount'];
	// get the discount
	$discount = $_SESSION ['discount'];
	// remove the discount from it
	$total -= $discount;
	// save it for future
	$_SESSION ['cart_amount'] = $total;
	// no more discount to perform
	$_SESSION ['discount'] = 0;
	// return the total now
	return $total;
}
```

###Modular-wise coding
You are no longer doing homework, you are making a part of a big universe. Suppose you need to close a modal window in javascript.
So, don't code like:
```
$('#modal').hide();
```
code like:
```
function closeModal() {
	$('#modal').hide();
}
```
It will be perfect if you make a modal window instance like:
```
// code here
modal.close = function() {
	$('#modal').hide();
};
```

###Thinking of Future
Always think before you code. Take more time thinking and less time typing. Before writing a one line code think for 1 minute. What you are making is not finished now, it will be bigger day by day. It's your responsibility to make the code future friendly.

So, while writing like
```
a = 10;
b = a * 299;
```
write like:
```
a = 10;
c = 299;
b = a * c;
```

###Using a Framework
Framework offers many pre-written and well written modules, libraries, helpers etc. So, using a framework always saves time and ensures security.

###Using a blueprint
Always start your project from a blueprint. A Blueprint is like boilerplate where many simple things are ready to use and the structure is defined well enough for a big application.
