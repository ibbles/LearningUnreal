[[Material]]
[[Landscape Material]]
[[Landscape Blend Layers Material]]

When in the [[Landscape Mode]], select the Paint tab and then the Paint tool.

![[Landscape_Mode_Paint_Tab_Paint_Tool.jpg]]

A Landscape consists of layers where each layer has its own appearance.
The number of layers a [[Landscape]] has depend on the [[Landscape Blend Layers Material]] assigned to it.


# Layer Info

Each layer in the [[Landscape Blend Layers Material]] must have an associated Layer Info set on the [[Landscape]].
They Layer Info is created by clicking the + button on the layer in [[Landscape Mode]] > Landscape panel > Target Layers > Layers.
Each layer can be either weight-blended or not.
It should match the `Blend Type` set for that layer in the [[Landscape Material#Layer Blending|Layer Blend node]] in the [[Landscape Material]].
The Layer Info object will appear as an asset in the Content Browser.
We can chose where to save it, but Unreal Editor will provide a default.

The Layer Info asset cannot be shared by multiple Landscapes, each Landscape must have its own.

Once we have a Layer Info for the first layer in the [[Landscape Material]] that layer will be applied to the entire [[Landscape]].

Create Layer Info objects for all layers in the [[Landscape Material]].

# Painting
To paint a layer onto the [[Landscape]], select the layer to paint from the Landscape Mode panel > Target Layers > Layers, make sure the Paint tool is selected in the Tool Bar, and then click-and-drag on the Landscape in the [[Viewport]].

It helps to disable the world grid with Viewport > top-left > Show > Uncheck "Grid".

There are a number of settings one can tweak on the Paint tool to get the wanted result.




[UE4: Step-by-Step to Your First Landscape Material for Beginners (Day 2/3: 3-Day Tutorial Series) @ youtube.com by WorldofLevelDesign](https://www.youtube.com/watch?v=cWOlIvq0Etg)

# Troubleshooting

## Landscape Is Black

Make sure a Layer Info has been set for each layer listed under [[Landscape Mode]] > Landscape panel > Target Layers > Layers.
Make sure the Layer Info assets are used by a single [[Landscape]] only.
