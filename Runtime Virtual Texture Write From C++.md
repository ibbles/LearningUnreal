[[Runtime Virtual Texture]].

# Research Notes

This section contains my research notes gathered while writing the sections above.


## Bing Chat

Prompt:
> In Unreal Engine, how do I write to a runtime virtual texture from C++?

Reply
>To write to a runtime virtual texture from C++, you need to create a Runtime Virtual Texture Asset in the Content Browser[1][2]. This asset links all the components (Actors, RVT Volume, and RVT Materials) together to render the runtime virtual texture.
> 
>Then, you need to implement a Runtime Virtual Texture Producer component in C++[1]. This component defines how to write data to the runtime virtual texture. You can use the URuntimeVirtualTextureComponent class as a base class for your custom component.
> 
>Finally, you need to add your custom component to an Actor and assign it to the Virtual Texture property of the RVT Volume[1]. This way, your component will be used to write data to the runtime virtual texture when it is rendered.
> 
> - [1] https://docs.unrealengine.com/en-US/Engine/Rendering/VirtualTexturing/Runtime/index.html
> - [2] https://docs.unrealengine.com/4.26/en-US/RenderingAndGraphics/VirtualTexturing/Runtime/QuickStart/
> - [3] https://docs.unrealengine.com/5.0/en-US/virtual-texturing-in-unreal-engine/


Short-form:
- Create a Runtime Virtual Texture asset in the Content Browser.
	- This makes sense. A [[Runtime Virtual Texture]] is an asset so by creating one we get an object to write into.
- Using a [[Runtime Virtual Texture]] include [[Actor|Actors]], [[Runtime Virtual Volume]] (Name?), and [[Runtime Virtual Material|Runtime Virtual Materials]] (Name?).
- Implement a Runtime Virtual Texture Producer in C++ by inheriting from `URuntimeVirtualTextureComponent`.
	- I assume this is where we define what color each texel should be.
- Add the Runtime Virtual Texture Produce to an [[Actor]].
- Assign the [[Actor]] to the Virtual Texture property of the [[Runtime Virtual Volume]].

Let's sanity check this.

Does the `URuntimeVirtualTextureComponent` class exists?
Yes, see the [documentation](https://docs.unrealengine.com/5.1/en-US/API/Runtime/Engine/Components/URuntimeVirtualTextureComponent/).
> Component used to place a `URuntimeVirtualTexture` in the world.
This sounds more like something that a Runtime Virtual Volume would use, not the texel data producer.


Continuing the Bing Chat conversation

Prompt:
>Can you show me an example implementation in C++ of a Runtime Virtual Texture Producer?

Reply:
>I’m sorry but I couldn’t find any example implementation in C++ of a Runtime Virtual Texture Producer on the web.😞
>
>You may want to check the Unreal Engine documentation123 or the Unreal Engine forums4 for more information.
>
>Alternatively, you can try to create your own component by following the steps in the documentation1 and using the URuntimeVirtualTextureComponent class as a reference.
>
>I apologize for any inconvenience this may cause.😔
> 
> - [1] https://docs.unrealengine.com/en-US/Engine/Rendering/VirtualTexturing/Runtime/index.html
> - [2] https://docs.unrealengine.com/5.0/en-US/virtual-texturing-in-unreal-engine/
> - [3] https://docs.unrealengine.com/4.27/en-US/RenderingAndGraphics/VirtualTexturing/Runtime/QuickStart/
> - [4] https://docs.unrealengine.com/en-US/Engine/Rendering/VirtualTexturing/Runtime/index.html
> - [5] https://docs.unrealengine.com/5.0/en-US/virtual-texturing-in-unreal-engine/
> - [6] https://docs.unrealengine.com/4.26/en-US/RenderingAndGraphics/VirtualTexturing/Runtime/QuickStart/
> - [7] https://docs.unrealengine.com/en-US/Engine/Rendering/VirtualTexturing/Runtime/index.html
> - [8] https://docs.unrealengine.com/5.0/en-US/virtual-texturing-in-unreal-engine/
> - [9] https://docs.unrealengine.com/4.26/en-US/RenderingAndGraphics/VirtualTexturing/Runtime/QuickStart/

It's duplicating references a bit here.

Prompt:
>Can you show me an example implementation of a URuntimeVirtualTextureComponent?

Reply:
>Sure, I can show you an example of a URuntimeVirtualTextureComponent.😊
>
>According to the Unreal Engine documentation12, a URuntimeVirtualTextureComponent is a component used to place a URuntimeVirtualTexture in the world. A URuntimeVirtualTexture is an asset that defines the properties and settings of a runtime virtual texture.
>
>To use a URuntimeVirtualTextureComponent, you need to create a URuntimeVirtualTexture asset in the Content Browser and assign it to the Virtual Texture property of the component3. Then, you need to add the component to an Actor and position it in the scene where you want to render the runtime virtual texture3.
>
>Here is an example of how to create and use a URuntimeVirtualTextureComponent in C++:

```cpp
// Include the header file for URuntimeVirtualTextureComponent
#include "Components/RuntimeVirtualTextureComponent.h"

// Create a new Actor class that inherits from AActor
class ARVTActor : public AActor
{
    // Declare a constructor
    ARVTActor();

    // Declare a pointer to a URuntimeVirtualTextureComponent
    UPROPERTY()
    URuntimeVirtualTextureComponent* RVTComponent;
};

// Define the constructor
ARVTActor::ARVTActor()
{
    // Create a new URuntimeVirtualTextureComponent and attach it to the root component of the Actor
    RVTComponent = CreateDefaultSubobject<URuntimeVirtualTextureComponent>(TEXT("RVTComponent"));
    RVTComponent->SetupAttachment(RootComponent);

    // Load a URuntimeVirtualTexture asset from the Content Browser
    // Note: You need to replace "MyRVT" with the name of your asset
    URuntimeVirtualTexture* RVTAsset = LoadObject<URuntimeVirtualTexture>(nullptr, TEXT("/Game/MyRVT.MyRVT"));

    // Assign the asset to the Virtual Texture property of the component
    RVTComponent->SetVirtualTexture(RVTAsset);
}
```

> - [1] https://docs.unrealengine.com/en-US/API/Runtime/Engine/Components/URuntimeVirtualTextureComponent/index.html
> - [2] https://docs.unrealengine.com/4.27/en-US/API/Runtime/Engine/Components/URuntimeVirtualTextureComponent/
> - [3] https://docs.unrealengine.com/4.26/en-US/RenderingAndGraphics/VirtualTexturing/Runtime/

Prompt:
>Can you add a member function to the Actor that writes a checkerboard pattern to the Runtime Virtual Texture asset?

Reply:
>Sure, I can try to generate some code for you.😊
>
>To write a checkerboard pattern to the Runtime Virtual Texture asset, you need to implement a custom Runtime Virtual Texture Producer component that overrides the PopulateVirtualTexture method. This method is called by the engine when it needs to write data to the runtime virtual texture.
>
>Here is an example of how to create and use a custom Runtime Virtual Texture Producer component in C++:
```cpp
// Include the header file for URuntimeVirtualTextureProducer
#include "VT/RuntimeVirtualTextureProducer.h"

// Create a new component class that inherits from URuntimeVirtualTextureProducer
class UCheckerboardRVTProducer : public URuntimeVirtualTextureProducer
{
    // Declare a constructor
    UCheckerboardRVTProducer();

    // Override the PopulateVirtualTexture method
    virtual void PopulateVirtualTexture(URuntimeVirtualTexture* InRuntimeVirtualTexture, const FRuntimeVirtualTextureContext& InContext) override;
};

// Define the constructor
UCheckerboardRVTProducer::UCheckerboardRVTProducer()
{
    // Set the output type of the component to BaseColor
    // Note: You can change this to match your runtime virtual texture asset settings
    OutputType = ERuntimeVirtualTextureMaterialType::BaseColor;
}

// Define the PopulateVirtualTexture method
void UCheckerboardRVTProducer::PopulateVirtualTexture(URuntimeVirtualTexture* InRuntimeVirtualTexture, const FRuntimeVirtualTextureContext& InContext)
{
    // Get the size of the runtime virtual texture in pixels
    int32 RVTWidth = InRuntimeVirtualTexture->GetWidth();
    int32 RVTHeight = InRuntimeVirtualTexture->GetHeight();

    // Get the size of each tile in pixels
    int32 TileWidth = InContext.TileSize.X;
    int32 TileHeight = InContext.TileSize.Y;

    // Get the coordinates of the top-left corner of the tile in pixels
    int32 TileX = InContext.TilePosition.X * TileWidth;
    int32 TileY = InContext.TilePosition.Y * TileHeight;

    // Loop through each pixel in the tile
    for (int32 Y = 0; Y < TileHeight; Y++)
    {
        for (int32 X = 0; X < TileWidth; X++)
        {
            // Calculate the coordinates of the pixel in the runtime virtual texture
            int32 RVTX = TileX + X;
            int32 RVTY = TileY + Y;

            // Calculate the color of the pixel based on a checkerboard pattern
            // Note: You can change this to any pattern you want
            bool IsBlack = ((RVTX / 8) % 2) == ((RVTY / 8) % 2);
            FLinearColor Color = IsBlack ? FLinearColor::Black : FLinearColor::White;

            // Write the color to the runtime virtual texture
            InRuntimeVirtualTexture->WriteToChannel(InContext, ERuntimeVirtualTextureMaterialType::BaseColor, Color);
        }
    }
}

// Modify the ARVTActor class to use the custom component instead of URuntimeVirtualTextureComponent
class ARVTActor : public AActor
{
    // Declare a constructor
    ARVTActor();

    // Declare a pointer to a UCheckerboardRVTProducer component
    UPROPERTY()
    UCheckerboardRVTProducer* RVTProducer;
};

// Define the constructor
ARVTActor::ARVTActor()
{
    // Create a new UCheckerboardRVTProducer component and attach it to the root component of the Actor
    RVTProducer = CreateDefaultSubobject<UCheckerboardRVTProducer>(TEXT("RVTProducer"));
    RVTProducer->SetupAttachment(RootComponent);

    // Load a URuntimeVirtualTexture asset from the Content Browser
    // Note: You need to replace "MyRVT" with the name of your asset
    URuntimeVirtualTexture* RVTAsset = LoadObject<URuntimeVirtualTexture>(nullptr, TEXT("/Game/MyRVT.MyRVT"));

    // Assign the asset to the Virtual Texture property of the component
    RVTProducer->SetVirtualTexture(RVTAsset);
}
```

This looks promising, but there is no class named `URuntimeVirtualTextureProducer` in Unreal Engine, and no function named `PopulateVirtualTexture`.
At least the documentation search function on [docs.unrealengine.com](docs.unrealengine.com) doesn't find any.
But is also doesn't find [`URuntimeVirtualTexture`](https://docs.unrealengine.com/5.1/en-US/API/Runtime/Engine/VT/URuntimeVirtualTexture/), which do exists, so I don't know anymore.
I don't trust anything anymore.
Except maybe for `grep` and a working copy of the Unreal Engine source code.