# html5-animation-video-renderer

A Node.js script that renders an HTML5-based animation (\*) into a high-quality video.
It renders the animation frame-by-frame using Puppeteer without using screen capturing, so no frameskips!

It works by opening a headless browser and calls `seekToFrame(frameNumber)` for each frame of your animation.
Your web page is expected to display that frame on the screen.
It then captures a screenshot for that frame.
Each frame is then sent to `ffmpeg` to encode the video without needing to save temporary files to disk.
Because it works by capturing the page screenshot, it can render:

- HTML elements
- SVG elements
- Canvas
- WebGL

However, the renderer needs to get the webpage to display a specific frame before rendering. Therefore, it does not support:

- CSS animations and transitions
- Nondeterministic animation

Features:

- Works with GSAP.
- It can run multiple instances of headless Chrome to speed up rendering.

## Examples

| Example                                                          | Result                                                                                                                                          |
| ---------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| [examples/gsap-hello-world.html](examples/gsap-hello-world.html) | [🎬 View](https://latest-circleci-artifacts.lovely.workers.dev/github/dtinth/html5-animation-video-renderer/master/output/gsap-hello-world.mp4) |

## Creating your HTML5 animation

Your HTML5-based animation can be created using any tool, so long as the webpage has these properties:

- The webpage should contain a `#scene` element in the DOM, positioned at `top: 0; left: 0;`.
  The dimensions of the `#scene` element will be the video’s dimensions.
  You can use the provided [lib/style.css](lib/style.css) as a starting point.

- The webpage should contain these global JavaScript functions:

  - **`getInfo()`** should return an object with the following properties:

    - `fps` The video frame rate.
    - `numberOfFrames` The number of frames to render.

  - **`seekToFrame(frameNumber)`** should display the frame at the specified `frameNumber`.
    This function may return a promise, in this case the renderer will wait for it to resolve.
    Please make sure that all assets (such as images/fonts) are already loaded.

  See an example at [examples/gsap-hello-world.html](examples/gsap-hello-world.html).

## Install the prerequisites

Install Node.js 12 and Yarn, then install the project dependencies with:

```
yarn install
```

## Running the renderer

```
node render --help
```

## Demo

TODO add video

It has been used to render a 4-minute long 1080p60 video. That’s 15000 frames.
The speed on my Late 2013 MacBook Pro is around 4.5 frames per second.

```
frame=15000 fps=4.5 q=-1.0 Lsize=  644441kB time=00:04:09.98 bitrate=21118.5kbits/s speed=0.0758x
video:644374kB audio:0kB subtitle:0kB other streams:0kB global headers:0kB muxing overhead: 0.010475%
```
