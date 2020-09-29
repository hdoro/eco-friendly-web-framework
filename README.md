# Sustainable web framework design</h1>

**A set of open ideas for how we could build more sustainable websites and apps**.

## For context

💡 Feel free to [skip to the features](#the-features-and-directives) if you're just browsing by.

### Why is this relevant

The web has a _big_ carbon footprint. Every code we create and push live has an impact on the planet that can be reduced through conscious design and code decisions. I won't get into much detail about it, but I suggest either watching Jack Lenox's talk on ["How better performing websites can help save the planet"](https://blog.jacklenox.com/2019/09/23/talk-how-better-performing-websites-can-help-save-the-planet/) or, if you want to dive deeper, reading through [Principles of Sustainable Software Engineering](https://principles.green/). 

I've also collected a bunch of resources on this topic that you can access here: [hdoro.dev/sustainability/](https://hdoro.dev/sustainability/).

### What I mean by "web framework"

As for the usage of **"web framework"**, this could into many directions. Here we'll focus on a developer tool for building content-driven websites and web-apps, with a focus on the front-end. Its job will be to **generate HTML, CSS, JS and image** files from templates and data defined by the user. It'll provide some back-end functionality but it won't go deep in databases, permissions and complex APIs. It also won't cover edge cases and super complex apps such as Twitter and Github.

This is still broad, but being broad is the idea. We're covering ~95% of the use cases out there, from marketing sites to admin dashboards and small social apps. (Yes, 95% is an invention 😋)

So, a summary of what we want with this thought experiment is to conceive a developer tool that can materially reduce the carbon footprint of our websites and apps, while still achieving economic goals and being inclusive and accessible.

### Guiding questions

Three questions stand out in our quest for a more accessible web:

1. How can we power our work with renewable energy?
2. How can we make our products and services — and the user experiences they offer — as efficient as possible?
3. And better, yet, how can we avoid building unnecessary stuff that have no business or usage case?

Questions #1 and #2 are taken word by word from Tim Frick's book, [Designing for Sustainability
](https://www.oreilly.com/library/view/designing-for-sustainability/9781491935767/).

### Areas of improvement that satisfy these questions

First, we have the 4 directives provided by Tim Frick and the Mightybytes team on their Designing for Sustainability book:
- **Findability** - if our creations can't be found, our efforts will be rendered useless, which isn't very effective. Plus, if users have a hard time finding our content, they'll have to load more pages to do what they want, increasing bandwidth consumed and server work.
- **Performance optimization** - "When your website runs more efficiently, you use less processing power, which means that your site uses less energy and will have a lower carbon footprint."
- **Design & user experience** - again, if your user has a hard time doing what they came for, they'll use more bandwidth. This also includes recognizing business goals and always striving for efficiency, reducing waste wherever possible. For example, lazy-loading images and serving them at different sizes according to the user's device is a common design pattern that contributes to more sustainable websites.
- **Green hosting** - data centers consume __a lot__ of energy to run 24/7, so if we choose hosting companies that run on renewable energy we're severely reducing this impact.

That doesn't encompass everything that we can work on, however, so I'm also drawing on [Principles of Sustainable Software Engineering](https://principles.green/) to get two major aspects of a sustainable web:
- Even when we use green hosting, we should **minimize server usage**, reducing the [carbon intensity of our applications](https://principles.green/principles/carbon-intensity/)
- Also, we should work on the [energy proportionality of our servers](https://principles.green/principles/energy-proportionality/) by running servers at **high rates of utilization**. Imagine the amount of websites that are running servers all-day long only to receive a couple of visits in a day.

Finally, I'd also like to add a few other considerations to call our framework truly sustainable
- Make a **strong business case** - a niche developer tool that only reaches hackers and fails to materially help businesses may be a cool experiment and potential case study, but that's it. It won't pick up adoption to make a true dent on the web landscape.
- **Increased developer productivity, with a caveat** - every human has a personal carbon footprint, and developers are no different - in fact, they might even have it worse with all their fancy gear. This means that if we can reduce the amount of work they need to get things done, then we're reducing our footprint. That said, we must be careful about not reducing friction on actions that may hurt the other pillars, such as making it way too easy to degrade performance with external libraries and packages (for more on this, see [Tim Kadlec's post on "Building with Friction"](https://timkadlec.com/remembers/2020-03-18-building-with-friction/))
- **Easy maintainability** - if fixing bugs and adding new features takes an absurd amount of time, then we're hurting the two pillars above. In the past I've went all in into making clients' sites' performant and low-impact, only to later regret some extreme decisions I made that made it hard to adapt the original code.
- **Easy to get started** and attractive to developers - Javascript land always has a new sexy tool drawing attention. If we can't compete with them on attention or make it too hard for newcomers to hop-in, then we're back to no adoption and low impact.

## The features and directives

With these 10 areas in mind, I'll go through the features that this "green framework" could bring to the table to concretely help.

ℹ **Big disclaimer #1**: In order to keep my sanity while writing this and to avoid this document from being a huge report, I won't go too deep into the technical reasoning behind web performance choices, how "serverless" works and some other potentially confusing parts. Also, I realize that this really needs code examples to become clearer, I just haven't had the time to do so.

ℹ **Big disclaimer #2**: You'll notice I don't talk about implementation here, that's intentional. **This is a dream-list of features** that would make sustainability easier to build right into our apps and sites. Some aren't feasible yet, others are incompatible. That's fine, just like in the rest of our lives, we'll have to make trade-offs, but it helps to have all of the options laid out in front of us to make better decisions 😉

### A hybrid operating mode - the best of pre-rendered (static), the best of server-rendered and cautionary client-side rendering

By default, the framework would have a **build step to pre-render every page that can be pre-rendered**, making them into static assets that are ready to go as soon as an user makes the request, dramatically speeding up the performance and lowering server usage. 

Instead of having to use a beefy machine, a cheap VM could handle hundreds of thousands of requests due to how little it'd have to do. Pages that don't change often would fall into this category, such as an about page or a blog post. If that's all a website has, then we'd generate a `dist` folder that could be simply dropped in an FTP server and be ready to go. Ideally, though, we'd suggest modern services such as [Netlify](https://netlify.com/) and [Vercel](http://vercel.co/) that integrate the continuous integration/delivery (CI/CD) with the static hosting.

Builds would be incremental meaning we'd only rebuild pages that had their content changed. This is a big challenge and there aren't many great examples of it out there yet.Gatsby claims to do this super smartly, but that's far from my experience. [Trio](https://gettriossg.com/) promises to do that, but it's very small and not (yet, maybe?) relevant.

Increasing the speed of these builds would be a priority. A good reference is [Zola](https://github.com/getzola/zola), a Rust static site generator that supposedly can create 10.000 pages in 10 seconds.

In my biased experience as an agency owner and now freelancer, most clients don't need to go further than that. Marketing/simple content websites are the vast majority of web pages out there, and we'd be drastically greenifying and speeding up the web with this static choice.

However, many websites have pages that change at all times and need to be publicly accessible for SEO, requiring the need for **server-side rendering (SSR)**. In these cases, you could make a choice of either hosting your server 24/7 or delegate rendering these pages to ["serverless" functions](https://www.serverless.com/learn/manifesto/) - a misleading name for server code that runs on-demand inside infrastructure managed by cloud vendors such as AWS and Microsoft Azure.

Choosing one or the other would depend on your use case. If you have high traffic and very dynamic data, then hosting your own server would be ideal, else you can count on "serverless" to minimize server usage (it wouldn't run at all times), make maintainability easier and only use servers at high rates of utilization. Regardless, you could effortlessly configure caching behavior on a per-route basis.

[Next.js](https://nextjs.org/) is already allowing this behavior (when coupled with Vercel, the hosting provider), take a look at the example below ([full documentation here](https://nextjs.org/docs/basic-features/data-fetching#incremental-static-regeneration)):

```js
export async function getStaticProps() {
  const res = await fetch(API_ENDPOINT)
  const data = await res.json()

  return {
    data, // this data will be used to render the HTML in the corresponding template
    // Next.js will attempt to re-generate the page:
    // - When a request comes in
    // - At most once every hour
    revalidate: 3600, // In seconds
  }
}
```

In the example above, it will run a "serverless" function at maximum once every hour to re-build the page. During this period, it'll have a cached version of the HTML for it ready to go, as if it was static. This way, you don't have to wait for the website to rebuild in order for the page to be updated nor use tons of energy at every request.

Finally, for admin dashboards and other non-public content, you'd be able to use client-side rendering (CSR) with front-end frameworks such as [Vue](https://vuejs.org/), [Svelte](https://svelte.dev/) and [Choo.js](https://github.com/choojs/choo).

As this can potentially lead to very JS-bloated sites that drain users' batteries and require huge amounts of scripts to be transferred, the framework would audit your bundle and provide suggestions to minimize it, such as switching from [React](http://reactjs.org/) to [Preact](https://preactjs.com/). Also, if applicable you could couple static pre-rendering of parts of a page, with client-side rendering being used solely for the dynamic parts.

[ElderJS](https://elderguide.com/tech/elderjs/) is a great example of how to enable client-side rendering without letting go of performance: you have to explicitly mark components as dynamic for it to include them in the Javascript bundle and fire them off when the page loads. Otherwise, it ships with 0 runtime JS, making fast and energy efficient the default.

> "Build-rendered beats server-rendered beats client-rendered."
> [Heydon Pickering](https://heydonworks.com/article/the-random-link/)

### It'd be based in Javascript/Node for reach

Yes, Javascript has its flaw and npm is yucky, but it's still the most accessible (_reads least inaccessible_) server language out there. It's also shared by the browser and has been growing in popularity, so with it we'd have the best chances to make a true difference through increased reach.

Plus, with WebAssembly (WASM) and the fact that the code would run server-side, we could still port things over from Rust, Go, C++, what have you. Many of the features listed below would probably have to rely on these lower-level languages due to their performance gains anyways!

Finally, NodeJS is widely accepted by platforms and hosting providers, making deployment easy peasy and potentially free and light for most use cases (see the static section below).

### Template engine agnostic

Want to use [Handlebars](https://handlebarsjs.com/), [Nunjucks](https://mozilla.github.io/nunjucks/), [React](http://reactjs.org/), [Svelte](https://svelte.dev/), [Blade](https://laravel.com/docs/8.x/blade), plain JS template strings, [Go Templates](https://golang.org/pkg/text/template/), simple markdown, [Liquid](https://shopify.github.io/liquid/), [Twig](https://twig.symfony.com/), [Tera](https://tera.netlify.app/), or your own thing™? You got it!

Again, for reach's sake we want to be as inclusive as possible.

**✨ Real life reference:** This is quite the technical challenge, indeed. [Eleventy](http://11ty.dev/) may be a good reference of API design, supporting 9 common templating languages besides regular Javascript. With some WASM, maybe this could be pulled off?

### Bring your data from anywhere

By now you're probably screaming at the screen asking "WHERE'S THE CONTENT?". Again, for reach's sake and also to enable very advanced and simple use cases, you could use pretty much what you want.

Have a small personal blog fed by markdown? Check.

Already use WordPress but want to run it in a low-consumption VM and make your front-end static and nimble? Use its API to query for content on builds.

Want to pull from a database to integrate with your product? Go ahead.

Or maybe you need a [headless CMS](http://headlesscms.org/) for clients to manage content effortlessly? You got it!

**✨ Real life reference:** [Gatsby](https://gatsbyjs.com) is a somewhat good example of this. Their source plugins make it really easy to pull-in data from wherever, from Wordpress to Slack, from Mailchimp to SQL databases and any headless CMS out there. However, their hammer that data into a GraphQL API layer that makes it hard for beginners and painful for complex, schema-less data to fit in. [Eleventy](http://11ty.dev/) has a simpler, more approachable data flow. I believe the balance is offering both sides :)

### 2-file minimal setup for an easy start, limitless depth for advanced use cases

A `package.json` file with the framework as a dependency and a markdown file to be transformed into a plain HTML file should be enough to get started. Just `npm init -y`, `npm install framework-name-here` and `npm run build`.

But if you want to build a 50.000 pages website with dozens of taxonomies and complex templates, it should allow for that, too.

**✨ Real life reference:** [Eleventy](http://11ty.dev/) is again a great example of this.

### Every appropriate performance optimization done out of the box

In the sake of productivity, error-proofing and making for a strong business case, we want to include every fitting optimization turned on by default:

- Lazy loaded images with responsive `srcset`s for adaptive loading
- Image compression, WebP and AVIF conversions
- Compression and tree-shaking of Javascript
- Warning on unused CSS
- Route-based style and script chunking - prevents having your `/blog` styles in your homepage, for example
- Javascript bundles for modern and old browsers (differential bundling) - this most users would avoid polyfills that make code more bloated and slower

BUT, we'd keep away from:

- Mindlessly pre-fetching content and new routes just to make the experience _seem_ faster for users.
  - JS libraries such as [GuessJS](https://github.com/guess-js/guess) have supposedly the good intention to help users, but they end-up using too much bandwidth by pre-fetching other pages assuming the user wants to navigate around the site.
- In-lining all things
  - Sure, it'll give you a better Lighthouse score and reduce the number of requests the browser makes, but then you miss the chance to cache static assets, which would be a waste when the user comes back to the site.

Also, when creating PRs that change code, it'd integrate with Google Lighthouse to run checks on performance and alert developers if there was a significant downgrade.

### Plugins

What optimizations we can't do automatically for every combination of template and data source could be outsourced to plugins.

Plugins could also help with integrating with data sources, introducing and extending template engines, etc.

How to design a plugin system that is safe, performant and maintainable is a million dollar question in itself. Some tools like [Metalsmith](https://metalsmith.io/) make the plugin ecosystem so loose that most are stale and hard to use.

Others try to centralize so much that they never end up moving. It's a tough balance, and I don't intend to solve it here.

### SEO and social display audits and helpers

By answering a simple prompt in their terminal or filling in a form on the web, developers would get a config file that the framework would use to provide all sorts of helpers:

- Integrated favicon generator
- Base meta-tags for accessibility and styling
  - `meta name="theme-color"`, `html lang`, etc.
- [Schema.org](https://schema.org)-compliant JSON-LD generators for richer search results
  - Breadcrumbs, ratings, recipes and many others could often benefit users searching and site owners, but they involve a lot of prior knowledge and custom work, we could help with that.
  - This would be as easy as setting up Schema WP plugin, which many already do
- Sitemaps and RSS feeds built out of the box
- Automatic generation of lightweight social sharing images (`og:image`) based on a page's information 
  - This would be done on-demand and heavily cached, of course
  - The goal would be to increase the chances that a user of social media apps go to your website
- Audits on PRs for common SEO errors with helpful reminders to take necessary non-code steps (creating the site on Google and Bing's search console, etc.)

### Accessibility (a11y) audits and easy internationalization (i18n)

If we can't reach all potential users, we're not realizing the potential of the product, being ineffective and maybe most importantly, we're excluding people.

Hence, providing an easy path for accessible, multi-lingual websites and apps is a must. Integration with linting tools such as [Webhint](https://webhint.io/) and accessibility testing such as [Axe](https://www.deque.com/axe/) with helpful CI tests would go a long way.

Besides that, we could provide a guided set-up of new languages as well as point to accessible components and starter code.

**✨ Real life references:**
- i18n
  - [Hugo](https://gohugo.io/) has one of the easiest i18n systems out there to get started, although it's too rigid and bound to the file system.
  - Metalsmith, Gatsby and other SSGs that allow you to build pages independently from the file system allow for any internationalization structure you'd like, but that's also overwhelming and hard to grasp.
  - An hybrid option where you can use a simple structure to get started and eject from it to go deeper would be ideal.
- a11y
  - Svelte has some accessibility checks built in, which is a good start
  - But a lot of it comes down to manual testing, which we could kindly suggest and guide developers to

### Developer productivity tools and directives

Again, each developer has their own carbon footprint. If we can minimize the time they spend learning and building with this framework, then we're reducing their creation's footprint.

- A lot of debugging options - ideally integrated with major editors such as VS Code and Atom
- Comprehensible and helpful errors
  - Rust and Elm are often praised for how they guide developers when things go wrong
- Easily installable
- GUI & dev tools for visualizing and making sense of the data flow, how templates relate to each-other, what are potentials bottlenecks for build speed, etc.
  - [Statecharts.io](https://statecharts.io/) and [Xstate visualizer](https://xstate.js.org/viz) are good references
- Hot reloading and hot module replacement (HMR) if applicable
  - [Browsersync](https://www.browsersync.io/) is a great option for when all rendering happens on the server
  - And [Vite](https://github.com/vitejs/vite) is a prime example of how HMR can speed up work
- Recipes and bite-sized templates for common use cases
  - Good examples include [Next.js's examples](https://github.com/vercel/next.js/tree/canary/examples) and [Gatsby recipes](https://www.gatsbyjs.com/docs/recipes/)

### Automated design and business suggestions

- Do you really need that carousel?
- What about that video bg?
- Is "uber for trading cards" a good business model that adds to people's lives?
- Email devs every 6 months to ask them if the website is still being used
- Maybe you could re-build the website only when the server's grid is cleanest?

### Actionable tips for sustainability in the docs

We could give a quick crash course on the impacts of our digital creations on the planet and then link those with what the framework does to help. This would create a stronger sense of meaning for each decision and illustrate how developers can take action.

Also, we could list green hosting options, point to curated resources and build a community around how to do good for the Earth using web development - or simply point to [ClimateAction.tech](https://climateaction.tech/).

### A sustainable governance system

Finally, we couldn't call this "sustainable" if it didn't include a way to sustain the development of the project itself. A solid governance system and funding mechanism would have to be figured out from day 1.

Be it from public grants to green initiative, partnerships with green hosts or the sponsoring from a parent company, funds would have to come in. And with money, comes attachment and political pressures, which doesn't have to be a scary, unwanted thing.

As long as this is planned and well established between all stakeholders, including the community, we should be able to cover most bases.

Worldbrain, the company behind [Memex](https://getmemex.com/), is a potential reference here. [Their decision to cap profits, not take venture capital and not look for an exit/sale](https://community.worldbrain.io/t/why-worldbrain-io-does-not-take-venture-capital/75) establishes boundaries to how the product will be developed. 

There are many more interesting approaches out there, but here I just want to draw attention to the fact that we can't deny a project like this would need to think about funding from day 0. Also, we can't rely on donations, Patreon and Open Collective - these are great complements, but fail to be enough on their own.

If you've made it this far into my ramblings, congratulations! Thank you for your time and I hope this was at least minimally inspiring. If you have a few more minutes, please leave your impressions on this repo's issues or [reach me out Twitter](https://twitter.com/hdorodev), it'd be very encouraging to this work!

## Contributing

I haven't gotten the chance to add a code of conduct and a contributing guide yet. As long as you're **inclusive and respectful**, please share your thoughts, suggestions, critiques and perspectives on the issues 😊

Remember, **there's no true sustainability with exclusion, racism, hate or division**. We can only escape the dire fate predicted by climate scientists if everyone has a seat at the table and if we account for the diverse mosaic of economic, cultural and social realities out there. For more on this, read [I’m a black climate expert. Racism derails our efforts to save the planet.](https://www.washingtonpost.com/outlook/2020/06/03/im-black-climate-scientist-racism-derails-our-efforts-save-planet/)