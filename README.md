# nanovg-vulkan-glfw-integration-demo

**What is it** - this project show Vulkan NanoVG integration to custom render pass, **using glfw**. 

NanoVG Vulkan port github link - [nanovg_vulkan](https://github.com/danilw/nanovg_vulkan)

### Remember this is not GUI example, NanoVG is UI rendering library

### Contact: [**Join discord server**](https://discord.gg/JKyqWgt)
___


To launch - copy builded binary to *example* folder(or launch when this folder is current as on build example commands below). Because required fonts and images/shaders to load.
___

# Example description:

*MAX_FRAMES_IN_FLIGHT* - **supported** and nanovg rendered in multiple frames, screenshot with 600+fps from 2 frames in flight.

**example_vulkan_glfw_integration.cpp** - this is example code from [Vulkan-tutorial Depth buffering](https://vulkan-tutorial.com/Depth_buffering) modified adding NanoVG integration. 

### **Look at [this commit 5bdb27f](https://github.com/danilw/nanovg-vulkan-glfw-integration-demo/commit/5bdb27f8de07192a2ffd17d9730aeb66af28f1f3) to see code changes from original C++ tutorial code**. 

Hotkey 3 to show/hide nanovg for example.

About *RenderPass integration* - copy paste code from *integration* examples above after your `vkCmdEndRenderPass` (or before `vkCmdBeginRenderPass`) and everything should work.
___

# Build:
(use cmake to build on Windows, Windows/Linux supported and tested)

```
git clone https://github.com/danilw/nanovg-vulkan-glfw-integration-demo
cd nanovg-vulkan-glfw-integration-demo/example
mkdir build
cd build
cmake ../
make
cd ../
./build/example-vk_glfw_integration
```
___

# Screenshot:

![nvgvk](https://danilw.github.io/GLSL-howto/vulkan_sh_launcher/nvg_integr_cpp.png)
___

