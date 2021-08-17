# vue-waterball
Vue module for creating chart like echarts-liquidfill by SVG in your application.

![](https://img.shields.io/travis/Sunflowersmile/vue-waterball)
![](https://img.shields.io/npm/v/vue-waterball)
![](https://img.shields.io/github/package-json/v/Sunflowersmile/vue-waterball)
![](https://img.shields.io/bundlephobia/min/vue-waterball)
![](https://img.shields.io/bundlephobia/minzip/vue-waterball)
![](https://img.shields.io/npm/dw/vue-waterball)
![](https://img.shields.io/npm/l/vue-waterball)
![](https://img.shields.io/github/last-commit/Sunflowersmile/vue-waterball)

## Examples
![](./public/images/GIF.gif)
![](./public/images/GIF2.gif)
![](./public/images/GIF3.gif)

## Requirements
Vue.js 2.x


## Installation
### npm
```
$ npm i vue-waterball -S
```

### yarn
```
$ yarn add vue-waterball
```
## Usage
main.js
```
import Vue from 'vue';
import WaterBall from 'vue-waterball';

Vue.component('WaterBall', WaterBall);
```
template:
```
<WaterBall />
```
## Attributes
| Attribute        | Description                                                                | Type   | Accepted Values           | Default    |
| ---------------- | -------------------------------------------------------------------------- | ------ | ------------------------- | ---------- |
| size             | width and height of component                                              | number | -                         | 200        |
| percentage       | value from 0 to 1                                                          | number | 0 to 1                    | 0.5        |
| border-width     | width of border                                                            | number | -                         | 5          |
| border-color     | color of border                                                            | string | -                         | '#F00'     |
| padding          | the distance between border and water                                      | number | -                         | 2          |
| background-color | background color of component                                              | string | -                         | '#FFF'     |
| water-color      | color of water                                                             | string | -                         | '#F00'     |
| dur              | on animation cycle duration, millisecond                                   | number | -                         | 5000       |
| amplitude        | times of half size, from 0 to 1, bigger number represents bigger amplitude | number | -                         | 0.3        |
| wave-length      | wave length, just use when type is 'horizontal'                            | number | -                         | 200        |
| font-size        | font size                                                                  | number | -                         | 20         |
| font-color       | font color                                                                 | string | -                         | '#000'     |
| type             | wave direction                                                             | string | 'horizontal' / 'vertical' | 'vertical' |

## Slot
default slot will replace the center text.