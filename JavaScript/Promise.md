## Promise
### Why we use promise?
To help us get out from the callback hell, we use promise. Promise simplifies our codes and makes it more readable.
### What is callback hell?
When there are multiple nested callback functions in the form where 2nd one depends on the execution of first one, 3rd one depends on the second one, this scenerio is called callback hell. An example of callback hell is given below:

```javascript
// node js code
const fs = require('fs');
const superagent = require('superagent');

fs.readFile(`${__dirname}/dog.txt`, (err, data) => {
  superagent
    .get(`https://dog.ceo/api/breed/${data}/images/random`)
    .then(res => {
      fs.writeFile('dog-img.txt', res.body.message, err => {
        console.log('Random dog image saved to file!!!');
      });
    })
    .catch(err => {
      console.log(err);
    });
});

```
### Promise:
The Promise method contains two calbacks provided by javascript as arguments, i.e. resolve and reject.

``` javascript
let promise = new Promise(function(resolve, reject) {
  // code here
});

```
The function passed to `new Promise` is called the `executor`. When `new Promise` is created, it runs automatically. It contains the producing code, that should eventually produce a result.

* `resolve(value)` — if the job finished successfully, with result `value`.
* `reject(error)` — if an error occurred, error is the `error` object.

### Promise to solve callback hell:
We can write the privious code in another way using `Promie`.

``` javascript
// read file function using promise

const readFilePro = file => {
  return new Promise((resolve, reject) => {
    fs.readFile(file, (err, data) => {
      if (err) reject('Failed to locate file!');
      resolve(data);
    });
  });
};

```

``` javascript
// write file function using promise

const writeFilePro = (file, data) => {
  return new Promise((resolve, reject) => {
    fs.writeFile(file, data, err => {
      if (err) reject('Failed to write file!');
      resolve('Random dog image saved to file!!!');
    });
  });
};

```

Now, we can rewrite our original funtion call:

``` javascript

readFilePro(`${__dirname}/dog.txt`)
  .then(data => {
    return superagent.get(`https://dog.ceo/api/breed/${data}/images/random`);
  })
  .then(res => {
    return writeFilePro('dog-img.txt', res.body.message);
  })
  .catch(err => {
    console.log(err);
  });
  
  ```
Itn't this amazing :stuck_out_tongue_closed_eyes:

### Async-Await function
We can also use async await function to write asyncronous function in a syncronous manner. Let's rewrite the privious codes using async - await function.

``` javascript
//async funtion body
const getDogPic = async () => {
  try {
    const data = await readFilePro(`${__dirname}/dog.txt`);
    
    const res = await superagent.get(
      `https://dog.ceo/api/breed/${data}/images/random`
    );
    console.log(res.body.message);

    await writeFilePro('dog-img.txt', res.body.message);
    console.log('data has been written!!');
  } catch (err) {
    throw err;
  }
};

// asynction getDogPic function call
(async () => {
  try {
    getDogPic();
  } catch (err) {
    console.log(err);
  }
})();

```

### Handle multiple Promise

Using `Promise.all()`, we can handle multiple promise request,

``` javascript

const getDogPic = async () => {
  try {
    const data = await readFilePro(`${__dirname}/dog.txt`);

    const promise1 = superagent.get(
      `https://dog.ceo/api/breed/${data}/images/random`
    );
    const promise2 = superagent.get(
      `https://dog.ceo/api/breed/${data}/images/random`
    );
    const promise3 = superagent.get(
      `https://dog.ceo/api/breed/${data}/images/random`
    );

    const all = await Promise.all([promise1, promise2, promise3]);
    const imgs = all.map(el => el.body.message);

    await writeFilePro('dog-img.txt', imgs.join('\n'));
    console.log('data has been written!!');
  } catch (err) {
    throw err;
  }
};

(async () => {
  try {
    getDogPic();
  } catch (err) {
    console.log(err);
  }
})();

```

