

#  shader 

> Shaders 也是一系列的指令，但是这些指令会对屏幕上的每个像素同时下达。也就是说，你的代码必须根据像素在屏幕上的不同位置执行不同的操作。

[**shader 示例网站**](https://www.shadertoy.com/)

## 着色器

```glsl
#ifdef GL_ES
precision mediump float;
#endif

uniform float u_time;

void main() {
	gl_FragColor = vec4(1.0,0.0,1.0,1.0);
}
```

## Uniforms(统一值)

> ​	由于每个线程和其他线程之间不能有数据交换，但我们能从 CPU 给每个线程输入数据。因为显卡的架构，所有线程的输入值必须**统一**（uniform），而且必须设为**只读**。也就是说，每条线程接收相同的数据，并且是不可改变的数据。

**这些输入值叫做 `uniform` （统一值）**

​		它们的数据类型通常为：`float`, `vec2`, `vec3`, `vec4`, `mat2`, `mat3`, `mat4`, `sampler2D` an和`samplerCube`.

你可以把 uniforms 想象成连通 GPU 和 CPU 的许多小的桥梁。虽然这些 uniforms 的名字千奇百怪，但是在这一系列的例子中我一直有用到：`u_time` （时间）, `u_resolution` （画布尺寸）和 `u_mouse` （鼠标位置）。按业界传统应在 uniform 值的名字前加 `u_` ，

### **如何定义uniform**

```GLSL
#ifdef GL_ES
precision mediump float;
#endif

uniform vec2 u_resolution; // 画布尺寸（宽，高）
uniform vec2 u_mouse;      // 鼠标位置（在屏幕上哪个像素）
uniform float u_time;     // 时间（加载后的秒数）
```

#### 注意

> uniform 值需要数值类型前后一致。且在 shader 的开头，在设定精度之后，就对其进行定义。

### test

```glsl
// u_time 加上一个 sin 函数
#ifdef GL_ES
precision mediump float;
#endif

uniform float u_time;
void main() {
	gl_FragColor = vec4(abs(sin(u_time)),0.0,0.0,1.0);
}
```

#### 提示



## shader 内置变量

### `gl_FragColor`

>`gl_FragColor`内置变量主要用来设置**片元像素**的颜色，出现的位置是片元着色器语言的main函数中。内置变量`gl_Position`的值是四维向量`vec4`(r,g,b,a),前三个参数表示片元像素颜色值`RGB`，第四个参数是片元像素透明度A，1.0表示不透明,0.0表示完全透明。

### `gl_FragCoord`

> 就像 `GLSL `有个默认输出值 `vec4 gl_FragColor` 一样，它也有一个默认输入值（ `vec4 gl_FragCoord` ）。`gl_FragCoord`存储了活动线程正在处理的**像素**或**屏幕碎片**的坐标。有了它我们就知道了屏幕上的哪一个线程正在运转。为什么我们不叫 `gl_FragCoord` uniform （统一值）呢？因为每个像素的坐标都不同，所以我们把它叫做 **varying**（变化值）。
```glsl
//gl_FragCoord 代表当前像素相对于屏幕的位置，屏幕的左下角
//u_resolution 是当前图像的分辨率； 
//用 gl_FragCoord 除以 u_resolution 得到的结果 fragcoord 就是归一化的屏幕坐标。
//由于已经归一化了那么 fragcoord 的值就在 [0,1] 的闭区间内。
```

## 造型函数
### `GLSL`内置函数
> `GPU` 的硬件加速支持我们使用角度，三角函数和指数函数。这里有一些`GLSL`内置函数的介绍：[`sin()`](https://thebookofshaders.com/glossary/?search=sin), [`cos()`](https://thebookofshaders.com/glossary/?search=cos), [`tan()`](https://thebookofshaders.com/glossary/?search=tan), [`asin()`](https://thebookofshaders.com/glossary/?search=asin), [`acos()`](https://thebookofshaders.com/glossary/?search=acos), [`atan()`](https://thebookofshaders.com/glossary/?search=atan), [`pow()`](https://thebookofshaders.com/glossary/?search=pow), [`exp()`](https://thebookofshaders.com/glossary/?search=exp), [`log()`](https://thebookofshaders.com/glossary/?search=log), [`sqrt()`](https://thebookofshaders.com/glossary/?search=sqrt), [`abs()`](https://thebookofshaders.com/glossary/?search=abs), [`sign()`](https://thebookofshaders.com/glossary/?search=sign), [`floor()`](https://thebookofshaders.com/glossary/?search=floor), [`ceil()`](https://thebookofshaders.com/glossary/?search=ceil), [`fract()`](https://thebookofshaders.com/glossary/?search=fract), [`mod()`](https://thebookofshaders.com/glossary/?search=mod), [`min()`](https://thebookofshaders.com/glossary/?search=min), [`max()`](https://thebookofshaders.com/glossary/?search=max) 和 [`clamp()`](https://thebookofshaders.com/glossary/?search=clamp)。

下面进行一简单的介绍
* `vec4 texture2D(sampler2D sampler, vec2 coord)`
   
   > 使用纹理坐标 `coord`，从当前绑定到 sampler 的二维纹理中读取相应的纹素。
* `radians(x)`
   
   > 将角度转化为弧度值，即 PI*x/180。
* `degrees(x)`
  
  >将弧度值转化为角度，即 180*x/PI。
* `sin(x)`
  
  >三角正弦函数，返回值区间为 [-1,1]。
* `cos(x)`
  
  >三角余弦函数，返回值区间为 [-1,1]。
* `pow(x, n)`
  
  >返回 x 的 n 次幂。
* `abs(x)`
  
>返回 x 的无符号绝对值。
  
* `sign(x)`
  
>如果 x>0 返回1.0，如果 x=0 返回0.0，否则返回 -1.0。
  
* `ceil(x)`
  
  >对 x 向上取整。
* `floor(x)`
  
  >对 x 向下取整。
* `mod(x, n)`
  
  >取模，即 x 除以 n 的余数。
* `min(x1, x2)`
  
  >返回 x1 和 x2 的较小值。
* `max(x1, x2)`
  
  >返回 x1 和 x2 的较大值。
* `fract(x)`
  
  >返回 x 的小数部分。
* `clamp(x, minVal, maxVal)`
  
> 把 x 的值限制在 minVal 和 maxVal 之间，即返回 min(max(x,minVal),maxVal)。
  
* `mix(x, y, a)`
  
>返回 x 和 y 的线性混合，即 x*(1-a)+y*a
  
* `distance(p0, p1)`
  
>返回 p0 和 p1 之间的距离，即 length(p0-p1)
  
* `dot(x, y)`
  
>返回 x 和 y 的点积，对于 vec2 就是 x[0]*y[0]+x[1]*y[1]
  
* `cross(x, y)`
  >返回 x 和 y 的叉积，对于 vec3 就是
  >result[0] = x[1]*y[2] - y[1]*x[2]
  >result[1] = x[2]*y[0] - y[2]*x[0]
  >result[2] = x[0]*y[1] - y[0]*x[1]
* `normalize(x)`
  
  >对 x 进行归一化，保持矢量方向不变但长度为 1，即 x/length(x)

#### Step和Smoothstep

> 属于GLSL中的独特插值函数

##### Step

> [`step(edge, x)`](https://thebookofshaders.com/glossary/?search=step) 插值函数需要输入两个参数。第一个是极限或阈值，第二个是我们想要检测或通过的值。对任何小于阈值的值，返回 `0.0`，大于阈值，则返回 `1.0`。据两个数值生成阶梯函数，即 x<edge 则返回 0.0，否则返回 1.0。


```glsl
  float pct = plot(st,value);
```

##### Smoothstep

>  [`smoothstep(edge0, edge1, x)`](https://thebookofshaders.com/glossary/?search=smoothstep) 插值函数当给定一个范围的上下限和一个数值，这个函数会在已有的范围内给出插值。前两个参数规定转换的开始和结束点，第三个是给出一个值用来插值。如果 x<=edge0 则返回 0.0，如果 x>=edge1 则返回 1.0，否则按照如下方法插值并返回



## 颜色

>
> 本节内容主要通过变量和色彩主题来了解向量类型

`shader` 采用一种类似C的 `struct`的方式访问向量数据的内部分量

```glsl
vec3 red = vec3(1.0,0.0,0.0);
red.x = 1.0;
red.y = 0.0;
red.z = 0.0;
```

采用x,y,z来定义颜色会有点奇怪。所以`shader` 有其他的方式访问这些变量，`.x`, `.y`, `.z`也可以被写作`.r`, `.g`, `.b` 和 `.s`, `.t`, `.p`。（`.s`, `.t`, `.p`通常被用做后面章节提到的贴图空间坐标）也可以通过使用索引位置`[0]`, `[1]` 和 `[2]`来访问向量.

```glsl
vec4 vector;
vector[0] = vector.r = vector.x = vector.s;
vector[1] = vector.g = vector.y = vector.t;
vector[2] = vector.b = vector.z = vector.p;
vector[3] = vector.a = vector.w = vector.q;
```

GLSL中向量类型的另一大特点是可以用你需要的任意顺序简单地投射和混合（变量）值。

```glsl
vec3 yellow, magenta, green;
// Making Yellow
yellow.rg = vec2(1.0);  // Assigning 1. to red and green channels
yellow[2] = 0.0;        // Assigning 0. to blue channel
// Making Magenta
magenta = yellow.rbg;   // Assign the channels with green and blue swapped
// Making Green
green.rgb = yellow.bgb; // Assign the blue channel of Yellow (0) to red and blue channels
```

### 混合颜色

>通过GLSL 中的[`mix()`](https://thebookofshaders.com/glossary/?search=mix)，这个函数让你以百分比混合两个值。百分比的取值范围为 0到1！

示例 ：

> 用随时间变化的sin绝对值来混合 `colorA` 和 `colorB`。

```glsl
#ifdef GL_ES
precision mediump float;
#endif

uniform vec2 u_resolution;
uniform float u_time;

vec3 colorA = vec3(0.149,0.141,0.912);
vec3 colorB = vec3(1.000,0.833,0.224);

void main() {
    vec3 color = vec3(0.0);
    float pct = abs(sin(u_time));
    //pct 是一个 0-1的值
    // Mix uses pct (a value from 0-1) to
    // mix the two colors
    color = mix(colorA, colorB, pct);
    gl_FragColor = vec4(color,1.0);
}
```

> 缓动函数与混合颜色组合
> 给颜色赋予一个有趣的过渡。想想某种特定的感情。哪种颜色更具代表性？他如何产生？又如何褪去？再想想另外的一种感情以及对应的颜色。然后改变上诉代码中的代表这种情感的开始颜色和结束颜色。`Robert Penner `开发了一些列流行的计算机动画塑形函数，被称为[缓动函数](http://easings.net/)。你可以研究这些[例子](https://thebookofshaders.com/edit.php#06/easing.frag)并得到启发，但最好你还是自己写一个自己的缓动函数。

## 形状
> 使用GLSL的类型和函数，画简单的图形
### 长方形
> 想像在大小为10 x 10方格纸画一个8 x 8的正方形。你会怎么做？

>方格纸上的每个小方形格点就是一个线程（一个像素）。每个格点有它的位置，就想棋盘上的坐标一样。在之前的章节我们将x和y映射到rgb通道，并且我们学习了如何将二维边界限制在0和1之间。我们如何用这些来画一个中心点位于屏幕中心的正方形？

```glsl
if ( (X GREATER THAN 1) AND (Y GREATER THAN 1) )
        paint white
    else
        paint black
```

现在我们有个更好的主意让这个想法实现，来试试把if语句换成step（），并用0到1代替10 x 10的范围。

```glsl
uniform vec2 u_resolution;

void main(){
    vec2 st = gl_FragCoord.xy/u_resolution.xy;
    vec3 color = vec3(0.0);

    // Each result will return 1.0 (white) or 0.0 (black).
    float left = step(0.1,st.x);   // Similar to ( X greater than 0.1 )
    float bottom = step(0.1,st.y); // Similar to ( Y greater than 0.1 )

    // The multiplication of left*bottom will be similar to the logical AND.
    color = vec3( left * bottom );

    gl_FragColor = vec4(color,1.0);
}
```

### 圆形
> 我们把圆规展开到半径的长度，把一个针脚戳在圆圆心上，旋转着把圆的边界留下来。这个过程翻译给 shader 意味着纸上的每个方形格点都会隐含着问每个像素（线程）是否在圆的区域以内。我们通过计算像素到中心的距离来实现（这个判断）。

**有几种方法来计算距离。**
* 最简单的是用distance()函数，这个函数其实内部调用 length()函数，计算不同两点的距离（在此例中是像素坐标和画布中心的距离）。
* length（）函数内部只不过是用平方根(sqrt())计算斜边的方程。

```glsl
#ifdef GL_ES
precision mediump float;
#endif

uniform vec2 u_resolution;
uniform vec2 u_mouse;
uniform float u_time;

void main(){
	vec2 st = gl_FragCoord.xy/u_resolution;
    float pct = 0.0;

    // a. The DISTANCE from the pixel to the center
    pct = distance(st,vec2(0.5));

    // b. The LENGTH of the vector
    //    from the pixel to the center
    // vec2 toCenter = vec2(0.5)-st;
    // pct = length(toCenter);

    // c. The SQUARE ROOT of the vector
    //    from the pixel to the center
    // vec2 tC = vec2(0.5)-st;
    // pct = sqrt(tC.x*tC.x+tC.y*tC.y);

    vec3 color = vec3(pct);

	gl_FragColor = vec4( color, 1.0 );
}
```
#### 距离场
> 把它当做海拔地图（等高线图）——越黑的地方意味着海拔越高。想象下，你就在圆锥的顶端，那么这里的渐变就和圆锥的等高线图有些相似。到圆锥的水平距离是一个常数0.5。这个距离值在每个方向上都是相等的。通过选择从那里截取这个圆锥，你就会得到或大或小的圆纹面。