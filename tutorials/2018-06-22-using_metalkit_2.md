---
layout: tutorial
title: Using MetalKit part 2
image_url: 
date: 2018-06-21
blurb: A recreation of a tutorial series by Marius Horga of metalkit.org
tags: [MetalKit]
author: Marius Horga
group: Using MetalKit
---

In the first part of this series we introduced the MetalKit framework. Let’s reuse the project from `part 1` and pick up where we left off. If you look again at the **render()** function, it was looking like this:

{% highlight swift %}
func render() {
    let device = MTLCreateSystemDefaultDevice()!
    self.device = device
    let rpd = MTLRenderPassDescriptor()
    let bleen = MTLClearColor(red: 0, green: 0.5, blue: 0.5, alpha: 1)
    rpd.colorAttachments[0].texture = currentDrawable!.texture
    rpd.colorAttachments[0].clearColor = bleen
    rpd.colorAttachments[0].loadAction = .Clear
    let commandQueue = device.newCommandQueue()
    let commandBuffer = commandQueue.commandBuffer()
    let encoder = commandBuffer.renderCommandEncoderWithDescriptor(rpd)
    encoder.endEncoding()
    commandBuffer.presentDrawable(currentDrawable!)
    commandBuffer.commit()
}
{% endhighlight %}

Let’s improve this code a bit. First, since our class subclasses **MTKView**, it already has its own `device` so there is no need to declare another one. This lets us reduce the first two lines to just one:

{% highlight swift %}
device = MTLCreateSystemDefaultDevice()
{% endhighlight %}

Second, last week we said that we should make sure **currentDrawable** and **currentRenderPassDescriptor** are not nil or the app will crash. For the sake of simplicity, we have not done that in part one, so let’s do that now. This will also help us get rid of a couple more lines of code. The final version of the function will look like this:


{% highlight swift %}
func render() {
    device = MTLCreateSystemDefaultDevice()
    if let rpd = currentRenderPassDescriptor, drawable = currentDrawable {
        rpd.colorAttachments[0].clearColor = MTLClearColorMake(0, 0.5, 0.5, 1.0)
        let command_buffer = device!.newCommandQueue().commandBuffer()
        let command_encoder = command_buffer.renderCommandEncoderWithDescriptor(rpd)
        command_encoder.endEncoding()
        command_buffer.presentDrawable(drawable)
        command_buffer.commit()
    }
}
{% endhighlight %}

You might wonder what **colorAttachments[0]** means. To set the `rendering pipeline state`, the `Metal` framework provides **3** types of attachments that we can write to:

* colorAttachments
* depthAttachmentPixelFormat
* stencilAttachmentPixelFormat

We are only interested in storing color data for now and `colorAttachments` is an array of textures that hold drawings results and display them on the screen. We currently only have one such texture - the first element (at index `0`) of the array. Ok, now is a good time to run the app and make sure you are still seeing the same colored background we saw last time. So much better! With only 9 lines of code we can get safe `Metal` code running on our GPU. Not too shabby.

So far so good! Next, let’s dive into a new `Metal` topic - drawing geometry on the screen. All graphics tutorials such as those about `OpenGL` start with a `Hello, Triangle` type of program because a triangle is the simplest form of geometry that can be drawn on screen. It is a **2D graphics** basic element and all the other objects in the world of graphics are composed of triangles, so this makes it a great place to start. Imagine the screen coordinate system having its axes running through the center of the screen which would have the coordinates **(0, 0)**. The edges of the screen would have values of **-1** and **1** respectively. Let’s create an array of floats and a buffer to hold the vertex values for our triangle. Insert these lines right after initializing the `device`:

{% highlight swift %}
let vertex_data:[Float] = [-1.0, -1.0, 0.0, 1.0,
                            1.0, -1.0, 0.0, 1.0,
                            0.0,  1.0, 0.0, 1.0]
let data_size = vertex_data.count * sizeof(Float)
let vertex_buffer = device!.newBufferWithBytes(vertex_data, length: data_size, options: [])
{% endhighlight %}

The vertices above are located in order: bottom left, bottom right and top center. We notice that each vertex uses **4** floats for its coordinate. The first two are the **x** and **y** axes. The ones we do not use this time are: the third one is the `depth (Z-axis)` and the fourth one is the `W coordinate` making our coordinates `homogeneous`. We will talk about them in our next episode. Then we compute the size of this array to simply be the size of **12** floats and finally we create the buffer based on the array and its size. Now that we have our vertexes stored, we need a way to send them to the `GPU` so it can display them on the screen. Let’s look at the entire process (`pipeline`) that facilitates drawing graphics on the screen:

![Diagram]({{ "/images/using_metal_kit/chapter03-1.png" | absolute_url }})

We have completed the first stage so far, storing the vertexes. You notice that the next stages require that we have a new construct named **shader**. A `shader` is where programmers are allowed to interfere in the graphics pipeline with their custom functions. `Metal` provides a few types of shaders, however, today we only look at two of them: the **vertex shader** which is responsible for the **location** of our point, and the **fragment shader** which is responsible for the **color** of our point.

The `Metal` framework provides a function that we can call on the `device` to create a **Library** of functions (`shaders`), so let’s create it:

{% highlight swift %}
let library = device!.newDefaultLibrary()!
let vertex_func = library.newFunctionWithName("vertex_func")
let frag_func = library.newFunctionWithName("fragment_func")
{% endhighlight %}

We create two new **Functions** and point them to their corresponding shaders (which we will create later). The next step is to create a **Render Pipeline Descriptor** which needs to know about our shaders:

{% highlight swift %}
let rpld = MTLRenderPipelineDescriptor()
rpld.vertexFunction = vertex_func
rpld.fragmentFunction = frag_func
rpld.colorAttachments[0].pixelFormat = .BGRA8Unorm
{% endhighlight %}

You might wonder what **.BGRA8Unorm **means. This setting configures the pixel format so that everything that goes through the render pipeline conforms to the same order (in this case `Blue`, `Green`, `Red`, `Alpha`) of color components as well as size (in this case an `8-bit` color value goes from `0` to `255`). The last step is to create a R**ender Pipeline State** based on the above `descriptor`:

{% highlight swift %}
let rps = try! device!.newRenderPipelineStateWithDescriptor(rpld)
{% endhighlight %}

Finally, we only need to let the command `encoder` know about our triangle, so add the following lines right after creating the `encoder`:

{% highlight swift %}
command_encoder.setRenderPipelineState(rps)
command_encoder.setVertexBuffer(vertex_buffer, offset: 0, atIndex: 0)
command_encoder.drawPrimitives(.Triangle, vertexStart: 0, vertexCount: 3, instanceCount: 1) 
{% endhighlight %}

Now let’s get back to the two `shaders` we promised to create when we created the `Library`. For this, we need to create a new file in `Xcode`. Choose the **Metal File** type, name it **Shaders.metal** or something similar and click `Create`. You will immediately notice that the code does not resemble `Swift` much, and that is because the `Metal shading language` is based on `C++.` Let’s add the code below:

{% highlight c++ %}
#include <metal_stdlib>

using namespace metal;

struct Vertex {
    float4 position [[position]];
};

vertex Vertex vertex_func(constant Vertex *vertices [[buffer(0)]], uint vid [[vertex_id]]) {
    return vertices[vid];
}

fragment float4 fragment_func(Vertex vert [[stage_in]]) {
    return float4(0.7, 1, 1, 1);
}
{% endhighlight %}

The code is pretty straightforward. We first create a `struct` named **Vertex** that has only one member - an array of position arrays. We notice that the array is **float4** which in the shading language is the same as the vertexes we created earlier with **4** floats each. We leave the explanation for the **[[…]]** syntax for next time. Then we have the **vertex_func** shader which returns the **location** of the current vertex, and the **fragment_func** shader which returns the **color** of the current vertex. We hardcoded a particular color value, but we could have added a `color` struct member to `Vertex` and set the color separately for each vertex.

If you run the app, you should see a triangle like this:

![SampleWindow]({{ "/images/using_metal_kit/chapter03-2.png" | absolute_url }})

In the next part we will learn more about the `Metal shading language` as well as how `3D graphics` is rendered on the `GPUs`. The [source code](https://github.com/MetalKit/metal) is posted on Github as usual.

Until next time!
