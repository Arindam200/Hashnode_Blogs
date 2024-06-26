---
title: "JavaScript XMLHttpRequest()"
datePublished: Sat Oct 28 2023 09:01:56 GMT+0000 (Coordinated Universal Time)
cuid: clo9tcn4w00010ajk434bdtue
slug: javascript-xml-http-request
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1691774280065/d1609c87-a640-4119-85eb-e6e813173e20.png
tags: javascript, web-development, apis, axios, wemakedevs

---

## Introduction:

XMLHttpRequest() is a crucial tool in web development, empowering your web applications to interact with servers and retrieve data without having to refresh the page.

In this article, We'll understand what is XMLHttpRequest,how it works, how to use it and many more!

So,Without delaying further,let's START!

## What is XMLHttpRequest?

![XML file can controll smart sockets NETIO by transfering .xml over HTTPs](https://imgs.search.brave.com/RjvFKdOLdi8hZQPQNc75v995Raufw1unVFBL4PQBGZM/rs:fit:860:0:0/g:ce/aHR0cHM6Ly93d3cu/bmV0aW8tcHJvZHVj/dHMuY29tL2ZpbGVz/L3N0eWxlcy9nbG9z/c2FyeV81NTJ4NDE0/L3B1YmxpYy9HbG9z/c2FyeV9YTUwtSFRU/UHMtY29udHJvbGxl/ZC1wb3dlci1zdHJp/cHNfMTEwNC5wbmc_/aXRvaz1hU1ZqMTln/aw align="left")

In JavaScript, the `XMLHttpRequest` method is used to make asynchronous requests to the server and load the information that is returned by the server onto the web pages.

XMLHttpRequest is mainly used in AJAX programming. With this method, we can update the parts of the web page without reloading the whole webpage.

Now, There's a much more optimized way to send `request` to the server using `fetch` method.

## How it Works?

When we use the XHR method, It creates

* XMLHttpRequest Object
    
* A callback function
    

With the `XMLHttpRequest` object, we can send a request to the server using the `open()` and `send()` methods respectively, and get data from the server.

## State Management:

Unlike the `fetch` method, XHR monitors the API requests by exposing states. These Different states are represented by integer values. And we can check the values using `readystate`property.

Here are the different states of an XHR:

| Value | State | Description |
| --- | --- | --- |
| `0` | `UNSENT` | Client has been created. `open()` not called yet. |
| `1` | `OPENED` | `open()` has been called. |
| `2` | `HEADERS_RECEIVED` | `send()` has been called, and headers and status are available. |
| `3` | `LOADING` | Downloading; `responseText` holds partial data. |
| `4` | `DONE` | The operation is complete. |

For more details check [MDN Docs](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/readyState).

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">Note: An <code>XMLHttpRequest</code> object travels them in the order <code>0</code> → <code>1</code> → <code>2</code> → <code>3</code> → … → <code>3</code> → <code>4</code>. State <code>3</code> repeats every time a data packet is received over the network.</div>
</div>

## How to use XMLHttpRequest?

Now you have the basic idea of XHR and How it works. So Let's understand how to use XHR to get data from the server.

In this part, we'll be sending a request to the Github server and getting one user's details.

So, Without delaying further, Let's Start!

1\. At first, we'll create an XHR object.

```javascript
const xhr = new XMLHttpRequest();
```

2\. After creating the object, We'll now send a request to the server.

```javascript
xhr.open('GET', 'https://api.github.com/users/Arindam200'
);
```

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">It's Better to declare a variable instead of using the hardcoded link!</div>
</div>

```javascript
const requestUrl = 'https://api.github.com/users/Arindam200'
xhr.open('GET', requestURL);
```

But the request Hasn't been sent yet! Because the open method can't call itself.

3\. To send the Request, we need to call the open Method.

```javascript
xhr.send()
```

This method opens the connection and sends the request to the server.

> Note: We can also pass the request body through the optional body parameter.
> 
> ```javascript
> xhr.send([body])
> ```
> 
> Some request methods like `GET` does not have a body. And some of them like `POST` use `body` to send the data to the server4.

We can terminate the request at any time. The call to `xhr.abort()` does that:

```javascript
xhr.abort(); // terminate the request
```

That triggers `abort` event, and `xhr.status` becomes `0`.

4\. Now, Let's track the State and How they are changing. For that, We have to create one Call back function.

```javascript
xhr.onreadystatechange = function() {
 console.log(xhr.readyState)
}
```

After executing this, We'll get the following.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691766969652/9dafd87a-c1a7-46e2-891c-b82c430919b2.png align="center")

This callback function executes every time the state is changing.

5\. Now, Let's see the response we are getting. We'll get the Response when the state change completes, which means the `readyState` is 4.

```javascript
xhr.onreadystatechange = function(){
        console.log(xhr.readyState);
        if (xhr.readyState === 4) {
            console.log(responseText);            
        }
}
```

Let's see the console now.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691767627648/7449703f-63c7-4898-9d1d-fa55573bb2ec.png align="center")

Okay! So we got an error here!

Can you guess why?

Simply because We haven't referenced it properly. In our code snippet, we're logging `responseText` without specifying where it comes from.

To fix this error, We have to provide the correct context of the variable. So the Correct code will be:

```javascript
xhr.onreadystatechange = function(){
        console.log(xhr.readyState);
        if (xhr.readyState === 4) {
            console.log(this.responseText);            
        }
}
```

After Defining the Correct context, we'll get the desired response! It'll look like this:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691768232706/b14f174b-26fa-4300-b8d8-cd9ad269a047.png align="center")

6\. Now, we'll store the response in a variable and access its property! As an example, I'm Accessing the `name` property.

```javascript
xhr.onreadystatechange = function(){
        console.log(xhr.readyState);
        if (xhr.readyState === 4) {
            const data = this.responseText
            console.log(data.name);
        }
    }
```

Let's see the Output:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691768657698/2d6a62a3-7349-4408-bfd3-197e450aaf82.png align="center")

Wait! We have done everything right! But why we can't access the name property?

Here's the catch!

The response we are getting is of type `String` , not `Object` . Let's ensure this let's check the type of the function:

```javascript
console.log(typeof data);
```

And we got:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691768959150/6e526349-15d7-4088-9a83-74e95919a855.png align="center")

7\. For the above-mentioned reason, We aren't able to access the properties.

But How to fix this?

To fix this, We have to convert the string into `JSON` data!

```javascript
const data = JSON.parse(this.responseText)
```

Now If we check the type of data, We will get as `Object` .

And with that, we can access the desired properties!

8\. Here's the complete Code:

```javascript
const requestUrl = "https://api.github.com/users/Arindam200";
    const xhr = new XMLHttpRequest();
    xhr.open("GET", requestUrl);
    xhr.onreadystatechange = function () {
      console.log(xhr.readyState);
      if (xhr.readyState === 4) {
        const data = JSON.parse(this.responseText);
        console.log(typeof data);
        console.log(data.name);
      }
    };
    xhr.send();
```

## Conclusion

If you found this blog post helpful, please consider sharing it with others who might benefit. You can also follow me for more content on Javascript, React, and other web Development topics.

For Paid collaboration mail me at : [arindammajumder2020@gmail.com](mailto:arindammajumder2020@gmail.com)

Connect with me on [Twitter](https://twitter.com/intent/follow?screen_name=Arindam_1729), [LinkedIn](https://www.linkedin.com/in/arindam2004/), [Youtube](https://www.youtube.com/channel/@Arindam_1729) and [GitHub](https://github.com/Arindam200).

Thank you for Reading :)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691770163363/0d85c069-19de-4b0e-af4a-8d55a9e864dd.png align="center")