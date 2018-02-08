# Guide To Async Components

## JavaScript async components

Also known as [code splitting](https://webpack.github.io/docs/code-splitting.html), [dynamic import](https://github.com/tc39/proposal-dynamic-import) (TC39 proposal), "chunks" (which are loaded on demand), "layers", "rollups", or "fragments".

Async component (in React) is a technique (typically implemented as a Higher Order Component) for loading components with dynamic `import`.

See also: [Redux modules and code-splitting](http://nicolasgallagher.com/redux-modules-and-code-splitting/)

### Solutions

- [`react-loadable`](https://github.com/thejameskyle/react-loadable)
- [`loadable-components`](https://github.com/smooth-code/loadable-components)
- [`react-async-component`](https://github.com/ctrlplusb/react-async-component)
- [`react-code-splitting`](https://github.com/didierfranc/react-code-splitting)
- [react-loadable-visibility](https://github.com/stratiformltd/react-loadable-visibility)

### Strategies

- `Routes to chunks` - separate into "chunks" based on routes, possible to do automatically. Example: `Next.js`.
- `Components to chunks` - separate components as "chunks". Example: `react-loadable`.
- 🦄 UI tip (`components to chunks`): show `spinner` if content not loaded, but do not start spinner earlier then 200ms
- 🦄 UI tip (`routes to chunks`): do not show spinner in the center of your page between the content switch, use progress line in the top, like YouTube does. Example: [PACE](http://github.hubspot.com/pace/docs/welcome/).
- ⚡ Speed tip: prefer `component placeholder` over `spinner` if possible
- ⚡ Speed tip (`routes to chunks`): you can preload chunk (e.g. next page) on link hover
- 💣 Caveat (`components to chunks`): make sure you wait till all components loaded in case of prerendered pages (`SSR` or `snapshots`)
- 🎱 Trick (`components to chunks`): some components can be skipped in case of `SSR`, for example, Mapbox map

### Placeholders
- `Component placeholder`, also called `skeleton screens`. Use [react-content-loader](https://github.com/danilowoz/react-content-loader)
- `Spinner`. Use CSS3, like [spinkit](http://tobiasahlin.com/spinkit/), [loaders](https://connoratherton.com/loaders), [css-loaders](https://projects.lukehaas.me/css-loaders/)

See also:
- [A Bone to Pick with Skeleton Screens](https://www.viget.com/articles/a-bone-to-pick-with-skeleton-screens/)

## Media: images, video

### Strategies

- `Lazy load` - load images only when they appear on the screen. Use [Intersection Observer API](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API) or [react-waypoint](https://github.com/brigade/react-waypoint)
- `Load on demand`. Useful for slow networks or big files, like video or gif.
- 🦄 UI tip (`load on demand`): Use [download ⬇️](https://material.io/icons/#ic_file_download) icon for images and [play ▶️](https://material.io/icons/#ic_play_arrow) icon for animations.
- 🦄 UI tip: Show error state when image load failed, so the user can click to retry load. Use [error ⚠️](https://material.io/icons/#ic_error) or [repeat 🔁](https://material.io/icons/#ic_repeat) icons
- 🦄 UI tip: Do not try to download the image if the device is offline and image not in the cache. Use [cloud off 🚫](https://material.io/icons/#ic_cloud_off) icon
- 💣 UI caveat: make sure that icon and background are in good contrast, especially if you use not constant placeholders. For example, in case of solid color placeholder, you can make sure you pickup contrast color (this what Twitter does) or use a semitransparent gray overlay for bright backgrounds.
- ⚡ Speed tip: always use width and height. Use [image-size](https://www.npmjs.com/package/image-size), [probe-image-size](https://github.com/nodeca/probe-image-size)
- ⚡ Speed tip: use small placeholders (plus width and height)
- ⚡ Speed tip: optimize images. See [essential image optimization](https://images.guide/)
- ⚡ Speed tip: use MP4 without sound instead of GIF images
- ⚡ Speed tip: provide WebP images instead of JPG images

See also:
- [`<img lazyload>`](https://docs.google.com/document/d/1e8ZbVyUwgIkQMvJma3kKUDg8UUkLRRdANStqKuOIvHg/edit#heading=h.fuqo94v1qejx)

### Placeholders

- `Constant placeholder` - do not use it
- `Solid color` or dominant color - fastest and simplest placeholders. Use [color-thief](https://github.com/lokesh/color-thief) to get color info. Used by Twitter, Google, Pinterest
- `LQIP` or Low Quality Image Placeholder or Progressive image loading or "Blur-up" - better for perceptual speed than dominant color, but also have a bigger weight. Use [lqip](https://github.com/zouhir/lqip) or [sqip](https://github.com/technopagan/sqip). Used by Facebook and Medium.
- `SVG traced images`. There is [plugin for Gatsby](https://using-gatsby-image.gatsbyjs.org/traced-svg/).

See also:

- [Overview of different types of placeholders](https://medium.freecodecamp.org/using-svg-as-placeholders-more-image-loading-techniques-bed1b810ab2c)

### Responsive (resize) strategies

- `Fit to column width`, preserves ratio. Easy for images, for other media use something like [responsive-embed](https://foundation.zurb.com/sites/docs/responsive-embed.html)
- `Fit to vertical grid`, preserves ratio. Example: [Gutenberg](https://matejlatin.github.io/Gutenberg/)
- `Intelligent cropping` (doesn't preserve ratio) - crop to required size taking into account focal point. Example: [focal-point](https://github.com/adamdbradley/focal-point#readme), [focal point intelligent cropping](https://designshack.net/articles/mobile/focal-point-intelligent-cropping-of-responsive-images/).
- `Choose size based on viewport`, can change or preserve ratio. Examples: `<img>` with [`srcset`](https://caniuse.com/#search=srcset), [`<picture>`](https://caniuse.com/#search=picture) with `srcset` and `sizes` attributes, [CSS `@media` queries and `background-image`](https://www.smashingmagazine.com/2013/07/simple-responsive-images-with-css-background-images/#the-css-background-image-property)
- `Choose type of media based on viewport` or capabilities, can change or preserve ratio. Example: [Foundation interchange](https://foundation.zurb.com/sites/docs/v/5.5.3/components/interchange.html), [`<Media render/>`](https://github.com/jaredpalmer/react-fns#media-render), [`<source type>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/source).

See also:
- [The anatomy of responsive images](https://jakearchibald.com/2015/anatomy-of-responsive-images/)
- [responsiveimages.org](https://responsiveimages.org/)
- [picturefill](http://scottjehl.github.io/picturefill/)

### Solutions

(unsorted)

- [react-progressive-image](https://github.com/FormidableLabs/react-progressive-image)
- [react-lazyload](https://github.com/jasonslyvia/react-lazyload)
- [react-lazy-image](https://github.com/sergiodxa/react-lazy-image)
- [react-image](https://github.com/mbrevda/react-image)
- [react-lazy-load](https://github.com/loktar00/react-lazy-load)
- [react-graceful-image](https://github.com/linasmnew/react-graceful-image)
- [lazy-image](https://github.com/notwaldorf/lazy-image)

### Resizing images on the fly

#### Examples from real world

- [Real-time Resizing of Flickr Images Using GPUs](http://code.flickr.net/2015/06/25/real-time-resizing-of-flickr-images-using-gpus/)
- [How Discord Resizes 150 Million Images Every Day with Go and C++](https://blog.discordapp.com/how-discord-resizes-150-million-images-every-day-with-go-and-c-c9e98731c65d)

#### AWS Lambda + S3

- [Resize Images on the Fly with Amazon S3, AWS Lambda, and Amazon API Gateway](https://aws.amazon.com/blogs/compute/resize-images-on-the-fly-with-amazon-s3-aws-lambda-and-amazon-api-gateway/)

#### Golang + FaaS

Go is the perfect fit for this kind of task. [AWS Lambda now supports Go](https://aws.amazon.com/ru/about-aws/whats-new/2018/01/aws-lambda-supports-go/). Also [you can use Go with Google Cloud functions](https://github.com/GoogleCloudPlatform/cloud-functions-go).

- [Fast, simple, scalable, efficient HTTP microservice for high-level image processing with first-class support for Docker](https://github.com/h2non/imaginary)
- [Fast and secure standalone server for resizing and converting remote images](https://github.com/DarthSim/imgproxy)
- [Golang graphics](http://libs.club/golang/media/graphics)
- [Go Bindings for Vips (a super fast image processor)](https://github.com/DAddYE/vips)
- [caire](https://github.com/esimov/caire) content aware image resize library

## Infinite scroll
A special case of async components. Example: [`react-virtualized`](https://bvaughn.github.io/react-virtualized/#/wizard)
