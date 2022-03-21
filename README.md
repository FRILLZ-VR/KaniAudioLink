# KaniAudioLink
Uses scripts to connect audio link to kanikama emissive renderers

### Usage
These scripts control a KanikamaUnityRenderer mesh by accessing the KanikamaStandard emissive color property. AudioLink's amplitude per band and delay functionality are then connected to the emissive color. This allows for an almost 1:1 connection between realtime emissive meshes and audio link.

**To use this package, you will need the following:**
- VRC3 World SDK
- Udon Sharp (https://github.com/MerlinVR/UdonSharp)
- Audio Link (https://github.com/llealloo/vrc-udon-audio-link)
- Kanikama GI (https://github.com/shivaduke28/kanikama)

## Setup
Install the required dependencies and then import the Unity package. Your scene should include an AudioLink prefab. Optionally, you can add an AudioLinkController prefab for traditional controls. Provided is a modified AudioLinkController that interfaces with the PulseMasterController script.

There are three scripts required for this system to work:

**PulseChild** requires a mesh renderer and KanikamaUnityRenderer component. You will need to connect these as you would any other Kanikama emissive renderer, which means it should be referenced by both the KanikamaSceneDescriptor and KanikamaMapArrayProvider scripts, otherwise your emissions will not be baked into the GI system. Clicking "Link all sound reactive objects to this AudioLink" on the main AudioLink prefab will connect these objects automatically. Additionally, the emission color on these meshes before baking the scene with Kanikama Bake should not be black (0, 0 , 0).

**PulseController** should be attached to an empty parent GameObject that holds each PulseChild as a child. This script collects each child in an array to control one "strip" of lights. This is almost identical to the AudioReactiveSurfaceArray prefab in AudioLink. The delay timing for each child in the array is set using the DelayStepTime field.

**PulseMasterController** holds each PulseController so each strip can be controlled from a single UI canvas. This script requires manually referencing each PulseController object, meaning you'll need to drag each PulseController object into this array field.

### Known Issues
Unity is not capable of rendering more than eight light sources on a single mesh. If you are using a lot of light sources in Kanikama with this system, you will need to divide your scene/meshes into separate meshes. For example, a single floor plane will only take eight light sources, which will disable any additional emissive renderer sources you have in the scene and only render the eight closest light sources to the mesh.


