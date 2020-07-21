# Routing

When a file is added to the pages directory it's automatically available as a route.

First clone this repo: `git clone https://github.com/rehabas/next-js-router.git`, then run `npm i`

Create `src` folder, inside it create `pages` folder, then create `index.js` file inside `pages` folder and add this code inside it: (`src/pages/index.js`)

```js 
const Index = () => <h1>Index Page</h1>

export default Index;

```

To run the server, open your terminal and run: `npm run dev`

**Note:** With `Next.js` you don’t need to import `React` because `Next.js` does this for you automatically.

Inside `pages` folder, create `details.js` file, and add this code inside it: (`src/pages/details.js`)

```js
const Details = () => <h2>Details</h2>

export default Details;

```

Inside `pages` folder, create `device` folder, then create `john.js` file inside `device` folder and add this code inide it: (`src/pages/device/john.js`)

```js
const John = () => <h2>John's phone</h2>

export default John;

```

Now, go to: (`http://localhost:3000/device/john`)

![](https://i.imgur.com/iHzYZmA.png)

Instead of `device`, you can have a watch or a camera or a phone. So, instead of receiving a completely static name that device,  put square brackets and those square brackets are telling `next.js` that it is a variable called `device` in URL and it's dynamic:
`src/pages/[device]/[person].js`

![Selection_216](https://user-images.githubusercontent.com/49806841/87908270-c7788880-ca6e-11ea-860c-5c76764399c0.png)

Try to replace this URL: `http://localhost:3000/device/john` with `http://localhost:3000/phone/Alex`

Now from this dynamic routing how can you read the parameters from the URL? it's easy as well so let's do import `useRouter` from `next.js` router:
Inside `john.js` add this code:

```js
import { useRouter } from 'next/router';

const Person = () => {
    const router = useRouter();
    console.log(router.query);

    return (
    <h2>John's phone</h2>
    )
}

export default Person;

```
We have a router as something called query so all the parameters that we are having, for example, `device` and `person` will be inside this query:

![](https://i.imgur.com/O2g5bgZ.png)


Now edit code in `john.js`:
```js
import { useRouter } from 'next/router';

const Person = () => {
    const router = useRouter();
    console.log(router.query);

    return (
    <h2>{router.query.person}'s {router.query.device}</h2>
    )
}

export default Person;
```

Now, go to: (`http://localhost:3000/camera/michael`)

![](https://i.imgur.com/ivgXW1J.png)


After the browser refreshes, you can see the device is `camera` and the person is `michael`

![](https://i.imgur.com/u4ocs4r.png)


Edit url and add query parameters,for example: (`http://localhost:3000/camera/michael?query1=sam`)

![](https://i.imgur.com/P5U7I3b.png)


But if you have more than one query parameters, you can see that you have an array in query: (`http://localhost:3000/camera/michael?query1=sam&query1=alex`)

![](https://i.imgur.com/OGvwEIa.png)


You received `sam` and `alex`, so if you have more than one you will receive an array, if you just have one you will receive just the single property.

## Linking between pages

Now you need to navigate to these pages you created because you are just going to your browser inputting the URLs and there is no user will be doing that, so to navigate, for example, from `details` to `person`: 

In `detail.js`

```js
import Link from 'next/link';

const Details = () => 
    <div>
        <Link href="/phone/John">
            <a>Navigate to John's phone</a>
        </Link>
    </div>

export default Details;

```
Open your browser again and go to (`http://localhost:3000/details`) and now let's see what's happening:

![](https://i.imgur.com/RfstcZD.png)


When you click you should just navigate there:

![](https://i.imgur.com/6LCxl3f.png)


But if you go back to `http://localhost:3000/details` and open the development tools and look at what will happen, you will see that it will be a completely fully page reload, that's not what we want, we want to navigate and the router to do that transition.
so you have to provide `href` and `as` to make sure the router knows which JavaScript file to load.

**href** - The name of the page in the pages directory. For example `/[device]/[person]`.

**as** - The url that will be shown in the browser. For example `/phone/John`.
```js
import Link from 'next/link';

const Details = () => 
    <div>
        <Link as="/phone/John" href="/[device]/[person]">
            <a>Navigate to John's phone</a>
        </Link>
    </div>

export default Details;

```

Now:
```js
import Link from 'next/link';

const people = [
    {device: 'phone', name: 'John'},
    {device: 'camera', name: 'Alex'},
    {device: 'watch', name: 'Mick'}
]

const Details = () => 
    <div>
        {people.map(person => 
            <div>
                <Link as={`/${person.device}/${person.name}`} href="/[device]/[person]">
                    <a>Navigate to {person.name}'s {person.device}</a>
                </Link>
            </div>
        )}
    </div>

export default Details;

```

Inside `src` create `containers` folder, then create `Homepage.js` file inside `containers` folder and add this code inside it: (`src/containers/Homepage.js`)

```js
const Homepage = () => <h1>Hello</h1>

export default Homepage;
```

Inside `index.js` in `pages` folder add this code:

```js
import Homepage from '../containers/Homepage';

export default Homepage;

```

![](https://i.imgur.com/VpRlypv.png)


Inside `pages` folder, create `homepage.js` file and add this code inside it:

```js
import Homepage from '../containers/Homepage';

export default Homepage;

```
![](https://i.imgur.com/VBG77mj.png)


# Resources

* https://nextjs.org/docs/basic-features/pages
* https://nextjs.org/docs/routing/introduction
