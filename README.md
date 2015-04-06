# Clip3d

__3D rendering with css:clip-path__

> clip3d is still in very very experimental stage__ 

__SnapShot.gif__

![snapshot](https://raw.githubusercontent.com/leeluolee/clip3d/master/assets/snapshot.gif)


## Introduction

This project is Absolutely inspired by famous [species-in-pieces](http://species-in-pieces.com/#), I find that we can use property `clip-path` for rendering triangle. So, It can also rendering 3d model too, beacuse 3d model can generated by triangles.


Demo below __doesn't use any css property for 3d transform__, It is all __clip-path__. the pipeline is very similiar with the common 3d model rendering.

```
model coord -> global coord -> camera coord -> projection(frustum) -> backface excluding/z-sorting/lighting. 

```

Beacuse of the restrictions of `clip-path` , This experiment is very weak for realworld 3d rendering. __If you have idea with clip3d, please create issue in this [repo](https://github.com/leeluolee/clip3d/issues)


[__Demo on Codepen__](http://codepen.io/leeluolee/pen/zxQqpm)


__source__

```js
var Render = clip3d.Render,
  Light = clip3d.Light,
  Camera = clip3d.Camera,
  vec3 = clip3d.vec3,
  mat4 = clip3d.mat4,
  _ = clip3d.util,
  color = clip3d.color;

var render = new Render({
  parent: document.getElementById("app"),
  camera: new Camera({
    eye: [4,4, -10]
  }),
  // simple point-lighting
  light: new Light({
    position: [ 0, 0, -1 ],
    color: [255, 255, 255,1]
  }),
  // http://learningwebgl.com/blog/?p=370 
  // entity form learning webgl
  entities: [
    {
      vertices: [ 
        0,  1,  0,
        -1, -1, 1,
        1, -1,  1,

        0,  1,  0,
        1, -1,  1,
        1, -1, -1,

        0,  1,  0,
        1, -1, -1,
        -1, -1, -1,

        0,  1,  0,
        -1, -1, -1,
        -1, -1,  1,

        // warning the squence
        1, -1,  1,
        -1, -1, 1,
        -1, -1, -1,

        1, -1, -1,
        1, -1,  1,
        -1, -1, -1,

      ],
      colors: [
      ]
    },
    {
      vertices: [ 
  // Front face
      -1.0, -1.0,  1.0,
       1.0, -1.0,  1.0,
       1.0,  1.0,  1.0,
      -1.0,  1.0,  1.0,

      // Back face
      -1.0, -1.0, -1.0,
      -1.0,  1.0, -1.0,
       1.0,  1.0, -1.0,
       1.0, -1.0, -1.0,

      // Top face
      -1.0,  1.0, -1.0,
      -1.0,  1.0,  1.0,
       1.0,  1.0,  1.0,
       1.0,  1.0, -1.0,

      // Bottom face
      -1.0, -1.0, -1.0,
       1.0, -1.0, -1.0,
       1.0, -1.0,  1.0,
      -1.0, -1.0,  1.0,

      // Right face
       1.0, -1.0, -1.0,
       1.0,  1.0, -1.0,
       1.0,  1.0,  1.0,
       1.0, -1.0,  1.0,

      // Left face
      -1.0, -1.0, -1.0,
      -1.0, -1.0,  1.0,
      -1.0,  1.0,  1.0,
      -1.0,  1.0, -1.0
      ],
      indices : [
        0, 1, 2,      0, 2, 3,    // Front face
        4, 5, 6,      4, 6, 7,    // Back face
        8, 9, 10,     8, 10, 11,  // Top face
        12, 13, 14,   12, 14, 15, // Bottom face
        16, 17, 18,   16, 18, 19, // Right face
        20, 21, 22,   20, 22, 23  // Left face
      ],
      itemSize: 3,
      // for simplify. one face only have one color
      colors: [
        [255, 0, 0, 1],
        [255, 0, 0, 1],
        [255, 255, 0, 1],
        [255, 255, 0, 1],
        [0, 255, 0, 1],
        [0, 255, 0, 1],
        [255, 120 , 255, 1],
        [255, 120 , 255, 1],
        [120, 255, 0, 1],
        [120, 255, 0, 1],
        [0, 255, 120, 1],
        [0, 255, 120, 1]
      ],
      matrix: mat4.createRotate([0,0,1], 30)
    }
  ]
})

var i = 0;



function run(){
  i = i + .8;
  // vec3.rotateY(render.camera.eye, 1);
  // render.camera.update();
  render.entities[0].matrix =  mat4.rotate(
      mat4.translate(
       mat4.createScale(.2),
       4,0,0), 
  [1,1,0], i*3)


  // render.light.position = vec3.rotateY(position,  i, []);
  // console.log(render.light.position, i, position)

  render.entities[1].matrix =  mat4.translate(mat4.createRotate([1,0,1], i*2), 2,2,0)
  render.render();
  // setTimeout(run, 1000)
  _.requestFrame(run)
}

run();



```

## Usage

1. __AMD__


2. __Commonjs__

3. __GLobal__





## Feature

1. basic vector3 and matrix4 operation, like rotate， lookAt, perspective
2. only fragment color is supported
3. simple light
4. simple backface excluding
5. simple z-sorting based on z-index



## WARNING




## License

MIT
