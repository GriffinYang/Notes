# Ajax

XMLHttpRequest (XHR) objects are used to interact with servers. You can retrieve data from a URL without having to do a full page refresh. This enables a Web page to update just part of a page without disrupting what the user is doing.XMLHttpRequest can be used to retrieve any type of data, not just XML.<br>
Firstly we need to create a new XMLHttpRequest object, and then we should invoke "open()" method, by using this method, the requst headers can be set using thie setRequestHeader() method.<br>
After we called open, the readyState turns to 1, then when send() has been called, and headers and status are available, state to be 2, after that, If responseType is "text" or empty string, responseText will have the partial text response as it loads. Finally, when the fetch operation is complete. This could mean that either the data transfer has been completed successfully or failed.The stae becomes 4;At this the the connection has been established and we could use method send to requst to the server.<br>
Here's a example of how we build a download request:

```javascript
document.querySelector('button').addEventListener('click', () => {
  const xhr = new XMLHttpRequest();
  xhr.open('GET', '../sample/VMware-workstation-full-16.2.3-19376536.exe');
  xhr.onreadystatechange = () => {
    if (xhr.readyState === 4 && xhr.status === 200) {
      console.log('VMware-workstation-full-16.2.3-19 Downloaded Comppleted!');
    } else {
      console.log({
        status: xhr.status,
        text: xhr.statusText,
      });
    }
  };
  xhr.send();
});
```

#### Fetch

Fetch is a built-in and promise-based function that can make us easier to set a Ajax, use the example above, we set a same Ajax with fetch():

```javascript
fetch('../sample/VMware-workstation-full-16.2.3-19376536.exe');
```
