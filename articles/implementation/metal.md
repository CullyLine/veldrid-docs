# Metal Backend

The Metal backend is an Apple-specific backend implemented using the Metal graphics API. A Metal [GraphicsDevice](xref:Veldrid.GraphicsDevice) can be created with a pointer to an [NSWindow](https://developer.apple.com/documentation/appkit/nswindow). That window will be used to create the device's main swapchain.

## API Concept Map

| Veldrid Concept | Metal Concept | Notes |
| --------------- | --------------| ----- |
| [GraphicsDevice](xref:Veldrid.GraphicsDevice) | [MTLDevice](https://developer.apple.com/documentation/metal/mtldevice), [MTLCommandQueue](https://developer.apple.com/documentation/metal/mtlcommandqueue) | |
| [CommandList](xref:Veldrid.CommandList) | [MTLCommandBuffer](https://developer.apple.com/documentation/metal/MTLCommandBuffer), [MTLRenderCommandEncoder](https://developer.apple.com/documentation/metal/MTLRenderCommandEncoder), [MTLBlitCommandEncoder](https://developer.apple.com/documentation/metal/MTLBlitCommandEncoder), [MTLComputeCommandEncoder](https://developer.apple.com/documentation/metal/MTLComputeCommandEncoder) | |
| [DeviceBuffer](xref:Veldrid.DeviceBuffer) | [MTLBuffer](https://developer.apple.com/documentation/metal/MTLBuffer) | |
| [BufferUsage](xref:Veldrid.BufferUsage) | None | MTLBuffers are not specialized -- any buffer can be used for any operation. |
| [Texture](xref:Veldrid.Texture) | [MTLTexture](https://developer.apple.com/documentation/metal/MTLTexture) | |
| [TextureUsage](xref:Veldrid.TextureUsage) | [MTLTextureUsage](https://developer.apple.com/documentation/metal/mtltextureusage) | |
| [TextureView](xref:Veldrid.TextureView) | [MTLTexture](https://developer.apple.com/documentation/metal/MTLTexture) | Similar to OpenGL, there is no dedicated TextureView object. "Subset" views can be generated by creating a new MTLTexture from an existing one. |
| [Sampler](xref:Veldrid.Sampler) | [MTLSamplerState](https://developer.apple.com/documentation/metal/mtlsamplerstate) | |
| [Pipeline](xref:Veldrid.Pipeline) | [MTLRenderPipelineState](https://developer.apple.com/documentation/metal/mtlrenderpipelinestate), [MTLComputePipelineState](https://developer.apple.com/documentation/metal/mtlcomputepipelinestate) | |
| [Blend State](xref:Veldrid.BlendStateDescription) | [MTLRenderPipelineColorAttachmentDescriptor](https://developer.apple.com/documentation/metal/mtlrenderpipelinecolorattachmentdescriptor) | |
| [Depth Stencil State](xref:Veldrid.DepthStencilStateDescription) | [MTLDepthStencilState](https://developer.apple.com/documentation/metal/mtldepthstencilstate) | |
| [Rasterizer State](xref:Veldrid.RasterizerStateDescription) | [MTLRenderCommandEncoder](https://developer.apple.com/documentation/metal/MTLRenderCommandEncoder) functions: `setCullMode`, `setFrontFacing` | |
| [PrimitiveTopology](xref:Veldrid.PrimitiveTopology) | [MTLPrimitiveType](https://developer.apple.com/documentation/metal/mtlprimitivetype) | |
| [Vertex Layouts](xref:Veldrid.VertexLayoutDescription) | [MTLVertexDescriptor](https://developer.apple.com/documentation/metal/mtlvertexdescriptor) | |
| [Shader](xref:Veldrid.Shader) | [MTLLibrary](https://developer.apple.com/documentation/metal/mtllibrary), [MTLFunction](https://developer.apple.com/documentation/metal/mtlfunction) | |
| [ShaderSetDescription](xref:Veldrid.ShaderSetDescription) | None | |
| [Framebuffer](xref:Veldrid.Framebuffer) | [MTLRenderPassDescriptor](https://developer.apple.com/documentation/metal/mtlrenderpassdescriptor) | |
| [ResourceLayout](xref:Veldrid.ResourceLayout) | None | The Metal backend uses the information from a ResourceLayout to determine which buffer, texture, and sampler slots to bind resources to. |
| [ResourceSet](xref:Veldrid.ResourceSet) | None | The Metal backend simply uses the individual resources contained in a ResourceSet to bind to appropriate shader stages. The slots are determined using information from the associated ResourceLayout. |
| [Swapchain](xref:Veldrid.GraphicsDevice#Veldrid_GraphicsDevice_SwapchainFramebuffer) | [CAMetalLayer](https://developer.apple.com/documentation/quartzcore/cametallayer), [CAMetalDrawable](https://developer.apple.com/documentation/quartzcore/cametaldrawable) | |

## Notes

Metal is a high-performance, modern graphics API developed by Apple. It is well-supported and well-implemented on newer devices, and as such it should be the preferred backend on those systems. Unlike OpenGL, it supports modern functionality like Compute shaders, and is also receiving continued investment and updates from Apple.

### Metal Interop

Metal is provided as an Objective-C API and is therefore much more difficult to interoperate with from .NET. The Veldrid.MetalBindings assembly contains a minimal set of helpers and function imports that are needed to communicate with Metal. It is possible that a non-negligible amount of overhead is caused by the complexity of this Objective-C interop layer, but it is unavoidable.

### Staging Textures

As with the Vulkan backend, Metal staging Textures are implemented using an MTLBuffer, for simplicity.