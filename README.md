# pixi-live2d-display

![GitHub package.json version](https://img.shields.io/github/package-json/v/guansss/pixi-live2d-display?style=flat-square)
![Cubism version](https://img.shields.io/badge/Cubism-2/3/4-ff69b4?style=flat-square)
![GitHub Workflow Status](https://img.shields.io/github/actions/workflow/status/guansss/pixi-live2d-display/test.yml?style=flat-square)

English | [中文](README.zh.md)

Live2D integration for [PixiJS](https://github.com/pixijs/pixi.js) v7.

This project aims to be a universal Live2D framework on the web platform. While the official Live2D framework is just
complex and problematic, this project has rewritten it to unify and simplify the APIs, which allows you to control the
Live2D models on a high level without the need to learn how the internal system works.

#### Feel free to support the Maintainer:
<a href="https://www.buymeacoffee.com/RaSan147" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" alt="Buy Me A Coffee" style="height: 60px !important;width: 217px !important;" ></a>

#### Features

- Supports all versions of Live2D models
- Supports PIXI.RenderTexture and PIXI.Filter
- Pixi-style transform APIs: position, scale, rotation, skew, anchor
- Automatic interactions: focusing, hit-testing
- Enhanced motion reserving logic compared to the official framework
- Loading from uploaded files / zip files (experimental)
- Fully typed - we all love types!
- Live Lipsync
- Play multiple motions simultaneously

#### Requirements

- PixiJS: 7.x
- Cubism core: 2.1 or 4
- Browser: WebGL, ES6

#### Demos

- [Basic demo](https://codepen.io/guansss/pen/oNzoNoz/left?editors=1010)
- [Interaction demo](https://codepen.io/guansss/pen/KKgXBOP/left?editors=0010)
- [Render texture & filter demo](https://codepen.io/guansss/pen/qBaMNQV/left?editors=1010)
- [Live2D Viewer Online](https://guansss.github.io/live2d-viewer-web/)

#### Documentations

- [Documentation](https://guansss.github.io/pixi-live2d-display)
- [API Documentation](https://guansss.github.io/pixi-live2d-display/api/index.html)
- [Development Notes](DEVELOPMENT.md)

## Cubism

Cubism is the name of Live2D SDK. There are so far three versions of it: Cubism 2.1, Cubism 3 and Cubism 4; where Cubism
4 is backward-compatible with Cubism 3 models.

This plugin supports all variants of Live2D models by using Cubism 2.1 and Cubism 4.

#### Cubism Core

Before using the plugin, you'll need to include the Cubism runtime library, aka Cubism Core.

For Cubism 4, you need `live2dcubismcore.min.js` that can be extracted from
the [Cubism 4 SDK](https://www.live2d.com/download/cubism-sdk/download-web/), or be referred by
a [direct link](https://cubism.live2d.com/sdk-web/cubismcore/live2dcubismcore.min.js) (_however the direct link is quite
unreliable, don't use it in production!_).

For Cubism 2.1, you need `live2d.min.js`. It's no longer downloadable from the official
site [since 2019/9/4](https://help.live2d.com/en/other/other_20/), but can be
found [here](https://github.com/dylanNew/live2d/tree/master/webgl/Live2D/lib), and with
a [CDN link](https://cdn.jsdelivr.net/gh/dylanNew/live2d/webgl/Live2D/lib/live2d.min.js) that you'll probably need.

#### Individual Bundles

The plugin provides individual bundles for each Cubism version to reduce your app's size when you just want to use one
of the versions.

Specifically, there are `cubism2.js` and `cubism4.js` for respective runtime, along with an `index.js` that includes
both of them.

Note that if you want both the Cubism 2.1 and Cubism 4 support, use `index.js`, but _not_ the combination
of `cubism2.js` and `cubism4.js`.

To make it clear, here's how you would use these files:

- Use `cubism2.js`+`live2d.min.js` to support Cubism 2.1 models
- Use `cubism4.js`+`live2dcubismcore.min.js` to support Cubism 3 and Cubism 4 models
- Use `index.js`+`live2d.min.js`+`live2dcubismcore.min.js` to support all versions of models

## Installation

#### Via npm

```sh
npm install pixi-live2d-display-mulmotion
```

```js
import { Live2DModel } from 'pixi-live2d-display-mulmotion';
// or import { Live2DModel } from 'pixi-live2d-display-mulmotion'; // i didn't test this

// if only Cubism 2.1
import { Live2DModel } from 'pixi-live2d-display-mulmotion/cubism2';
// or import { Live2DModel } from 'pixi-live2d-display-mulmotion/cubism2';

// if only Cubism 4
import { Live2DModel } from 'pixi-live2d-display-mulmotion/cubism4';
// or import { Live2DModel } from 'pixi-live2d-display-mulmotion/cubism4';
```

#### Via CDN (lipsync patched / mulmotion not added)

```html

<!-- Load Cubism and PixiJS -->
<script src="https://cubism.live2d.com/sdk-web/cubismcore/live2dcubismcore.min.js"></script>
<script src="https://cdn.jsdelivr.net/gh/dylanNew/live2d/webgl/Live2D/lib/live2d.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/pixi.js@7.x/dist/pixi.min.js"></script>


<!-- if support for both Cubism 2.1 and 4 -->
<script src="https://cdn.jsdelivr.net/gh/RaSan147/pixi-live2d-display@v0.5.0-ls-8/dist/index.min.js"></script>

<!-- if only Cubism 2.1 support-->
<script src="https://cdn.jsdelivr.net/gh/RaSan147/pixi-live2d-display@v0.5.0-ls-8/dist/cubism2.min.js"></script>

<!-- if only Cubism 4 support-->
<script src="https://cdn.jsdelivr.net/gh/RaSan147/pixi-live2d-display@v0.5.0-ls-8/dist/cubism4.min.js"></script>
```

In this way, all the exported members are available under `PIXI.live2d` namespace, such as `PIXI.live2d.Live2DModel`.

## Basic usage

See here for basic usage: [pixi-live2d-display-lipsync](https://github.com/RaSan147/pixi-live2d-display)

## play multiple motions in parallel

```ts
model.parallelMotion([
  {group: motion_group1, index: motion_index1, priority: MotionPriority.NORMAL},
  {group: motion_group2, index: motion_index2, priority: MotionPriority.NORMAL},
]);
```

If you need to synchronize the playback of expressions and sounds, please use`model.motion`/`model.speak` to play one of the motions, and use `model.parallelMotion` to play the remaining motions. Each item in the motion list has independent priority control based on its index, consistent with the priority logic of `model.motion`.


# See here for more Documentation: [Documentation](https://guansss.github.io/pixi-live2d-display/)


