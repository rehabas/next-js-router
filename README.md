# Routing

When a file is added to the pages directory it's automatically available as a route.

First clone this repo: `git clone https://github.com/rehabas/next-js-router.git`, then run `npm i`

To run the server, Open your terminal and run: `npm run dev`

Create `src` folder, inside it create `pages` folder 
`src/pages/index.js`:

```js 
const Index = () => <h1>Index Page</h1>

export default Index;

```

you can also notice that you don't need to have import react from react, because next.js is using bubble to put that for us automatically.

Inside `pages` folder, create `details.js` file, and add this code:

```js
const Details = () => <h2>Details</h2>

export default Details;

```

Inside `pages` folder, create `device` folder, then create `john.js` file inside `device`, and add this code:

```js
const John = () => <h2>John's phone</h2>

export default John;

```

![](https://i.imgur.com/iHzYZmA.png)

Instead of device we can have a watch or a camera or phone so, instead of receiving a completely static name that device we put a square brackets and those square brackets are telling `next.js` that it is a variable called device in our URL and it's dynamic it:
`src/pages/[device]/[person].js`

```
http://localhost:3000/device/john
```
replace with:
```
http://localhost:3000/phone/Alex
```

Now from this dynamic routing how can I read the parameters from the URL? it's easy as well so let's do import `useRouter` from `next.js` router:
Inside `john.js` replace this code:
```js
const John = () => <h2>John's phone</h2>

export default John;
```
with this code:
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
We have router as something called query so all the parameters that we are having for example `device` and `person` will be inside this query:

![](https://i.imgur.com/O2g5bgZ.png)


Now edit code in `john.js`:
```javascript=
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

Now add this route in url par:

![](https://i.imgur.com/ivgXW1J.png)


After the browser refreshes you can see the device is `camera` and the person is `michael`

![](https://i.imgur.com/u4ocs4r.png)


If you edit url, and you have query parameters,for example:

![](https://i.imgur.com/P5U7I3b.png)


But if you have more than one query parameters, for example:
![](https://i.imgur.com/WOSAJ3p.png)


Now you can see that you have an array in query:

![](https://i.imgur.com/OGvwEIa.png)


So you received `sam` and `alex`, so if you have more than one you will receive an array,if you just have one you will receive just the single property.

## Linking between pages

Now you need to navigate to these pages you created because you are just going to your browser inputting the URLs there no user will be doing that, so in order to navigate for example from `details` to `person`: 

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
Open your browser again and go to `details` page and now let's see what's happening:

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

now:
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

inside `src` create `containers` folder
create `Homepage.js` and add this code:

```js
const Homepage = () => <h1>Hello</h1>

export default Homepage;
```

Inside `index.js` in `pages` folder put this code:

```js
import Homepage from '../containers/Homepage';

export default Homepage;

```

![](https://i.imgur.com/VpRlypv.png)


Inside `pages` folder, create `homepage.js` file and add this code:

```js
import Homepage from '../containers/Homepage';

export default Homepage;

```
![](https://i.imgur.com/VBG77mj.png)
