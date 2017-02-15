---
layout: post
title: Getting query values from PUT, DELETE, PATCH requests in Django
---

I assume what you're asking is if you can have a method like this:

<pre class="brush:python">
def restaction(request, id):
    if request.method == "PUT":
        someparam = request.PUT["somekey"]
</pre>

The answer is no, you can't. Django doesn't construct such dictionaries for PUT, OPTIONS and DELETE requests, the reasoning being explained [here](https://groups.google.com/forum/#!msg/django-developers/dxI4qVzrBY4/m_9IiNk_p7UJ).

To summarise it for you, the concept of REST is that the data you exchange can be much more complicated than a simple map of keys to values. For example, PUTting an image, or using json. A framework can't know the many ways you might want to send data, so it does the obvious thing - let's you handle that bit. See also the answer to this question where the same response is given.

Now, where do you find the data? Well, according to the docs, django 1.2 features request.raw_post_data. As a heads up, it looks like django 1.3 will support request.read() i.e. file-like semantics.

<pre class="brush:python">
from django.http import QueryDict
put = QueryDict(request.body)
description = put.get('description')
</pre>