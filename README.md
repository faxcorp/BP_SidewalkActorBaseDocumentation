# BP_SidewalkActorBase Documentation

![preview](https://github.com/faxcorp/BP_SidewalkActorBaseDocumentation/assets/37246339/eaa3ee36-48c4-4c81-bf48-62291f6dd707)

The BP_SidewalkActorBase is a blueprint in Unreal Engine that allows you to easily create and customize sidewalks in your game or project. This tool provides a 4 sets of pre-made sidewalk pieces, base shapes to use as template to model your own pieces.

Buy on Unreal Marketplace : https://www.unrealengine.com/marketplace/en-US/product/adf0e75c72ec4eb198f404f84a0da1ad

## Important classes:

### BP_SidewalkActorBase
This is the main class where the magic happens. By default this class has no spline, but there is a Child Blueprint Class "BP_SidewalkActorBase_ChildSpline" with a spline, which you should probably use to make presets. The default class might suit someone who is generating splines procedurally or already have splines in the level. By default "BP_SidewalkActorBase" class looks for a spline first on a parent actor (if it has one), and then in itself. This is so that you can add this class as a Child Actor Component on another actor that has a spline and have it generate automatically.
### BP_SidewalkFloorBooleanActor
This actor acts as a boolean for the sidewalk floor. For example if you want to make an entrance to metro or cut out sidewalk from the boundary of a building. It has StaticMesh component, which could be any StaticMesh to make any shape of hole.
### BP_NoSidewalksActor
With the help of this actor you can define areas where the sidewalk pieces are not placed. For example if you want to make ramp for vehicles, or remove distributed street lights which overlap with some other important geometry. You can specifically define what can't be placed in certain volume. Corners, Straight pieces, Distributed Meshes and Distributed Actors.
### BP_SidewalkExperimentalWorldPartitionManager
This is an experimental actor, for scenarios when the Level has World Partition enabled and the sidewalk actor spline goes out of the bounds of a loaded tile. "BP_SidewalkExperimentalWorldPartitionManager" will constantly check if the sidewalk can be built, and when all needed tiles load, it will build the sidewalk. I haven't tested it enough to be sure that it works all the time, so use it at your own risk. This is only applicable when the "SnaptoGround?" is true.


## Important notes:

### "Geometry Script" Plugin is required
- Your project has to have "Geometry Script" Plugin enabled for this blueprint to function.
### Floor vertex colors
- When generating the floor you can set vertex color for the floor and edges. You can use this in a Material to do for example damages on the edges of the floor or water puddles.
### Floor UV Rotation
- UVs of the floor are aligned to the longest segment of the spline
### How to make a preset
- Right click on "BP_SidewalkActorBase_ChildSpline" in content browser and select "Create Child Blueprint Class". Open it up and change the "Class Defaults". This blueprint will be your preset.
![Screenshot 2024-05-02 211045](https://github.com/faxcorp/BP_SidewalkActorBaseDocumentation/assets/37246339/ff24381d-3484-4697-9c41-b90ae87f671a)

### Methods of generation
- You can drag/drop your preset from the content browser into the world and adjust the spline
- If you already have an actor with a spline, you can add a Child Actor Component to that actor with your preset and it will pick up its parent actor spline
- You can spawn BP_SidewalkActorBase or its child and call "ConstructFunc" and generate from an external spline
- You can enter Modeling Mode in Unreal, Choose "Draw Spline" tool, set  to draw sidewalk directly onto the level.
![Screenshot 2024-05-02 211548](https://github.com/faxcorp/BP_SidewalkActorBaseDocumentation/assets/37246339/eb38feeb-1220-4ba8-9a90-0a9ee40f9dad)

## Issues:

When the spline has inner corners, sometimes straight pieces overlap in each other and looks weird. I haven't found a solution to this, but with some imagination it could be hidden or avoided. 
If you do know or find a solution, please let me know!
In this example top is the issue, and on the bottom I used "BP_NoSidewalksActor" to remove one piece and just placed there a corner mesh.

![issue](https://github.com/faxcorp/BP_SidewalkActorBaseDocumentation/assets/37246339/2280d198-3b0c-49c4-88b8-9fc0bbaf21a6)


# Parameters descriptions:

![settings](https://github.com/faxcorp/BP_SidewalkActorBaseDocumentation/assets/37246339/3644ec79-6e40-4845-a08f-596c093c6d46)



## BP_SidewalkActorBase

### Control
Variable | Description
--- | ---
Construct? | You can Enable/Disable this to regenerate the sidewalk or if you already have a spline or generating it procedurally your child class can have this disabled and you can call "ConstructFunc" function and provide an external spline for generation
Construct Sidewalks? | Disable/Enable generation of sidewalk from the modular meshes, for example if you only want to generate the floor mesh
Add Floor? | Disable/Enable floor generation, if you only want modular pieces to be generated
Snap to Ground? | Enables or disables snapping to ground. Each modular piece and vertices of the floor mesh will be line traced on underlying geometry by channel set on "TraceChannel" variable
Main Mesh | This is main straight piece sidewalk mesh that will be used to segment your spline. If this mesh has for example 5 meter length, the spline will be segmented into 5 meter segments. You can force scale the segment length with "Width" variable
Alternate Meshes | These meshes will be selected randomly for randomization or by length if for example you have 5 meter MainMesh but you have a 1 meter segment on your spline, BP_SidewalkActorBase will try to find a mesh with the best fit from this array
Corner Meshes | Corner mesh count doesn't matter but they have to have equal interval angles between them and they have to be in the range from 1° to 180°. For example 1°, 5°, 10°, 15° ... 180°. The corners will not be placed perfectly if you have meshes like 1°, 3°, 18°, 20°, 42° etc...
Offset | This will offset sidewalks and the floor.
Spacing | This controls if sidewalk pieces overlap or have gaps. For example 0.99 will make meshes overlap a little bit, and 1.01 will make small gaps between them
Corner Mesh Scale | Scales corner meshes
Random Yaw | Randomize each piece yaw
Random Pitch | Randomize each piece pitch
Random Roll | Randomize each piece roll
Width | You can force splines to be segmented more or less. For example 1.2 will result in longer segments and 0.8 will result in shorter segments
Align Roll? | Align roll to unerlying surface (only when "SnapToGround?" is true). You can set this to false if you want your sidewalks to always be straight on one axis and not be sloped
Align Pitch? | You can disable pitch alignment for some reason
Knockout | This will remove random pieces from the generation, 0.2 value will remove 20% of the pieces
Random Seed? | You can use random seed everytime
In Collision Profile Name | Collision profile name that will be set for sidewalks and the floor
Trace Distance | This is how far from the spline downwards BP_SidewalkActorBase will look for ground (only when "SnapToGround?" is true)
Seed | Randomization seed
Draw Debug Type | You can change this to see the line traces when when "SnapToGround?" is true
Trace Channel | This is the object collision channel that the traces will be traced on (only when "SnapToGround?" is true)
Randomize Location | Randomize location of each piece
Add Relative Rotation | Add constant location to each piece

### Floor
Variable | Description
--- | ---
Inset | Lenth that will be inset from the spline, for example -50 value will extrude floor 0.5 meters outwards from the spline
Floor Offset Z | This will move floor up or down
Uniform Options Target Type | This controls how floor polycount gets defined. You can set either total Triange Count or Target Edge Length (triangles size)
Uniform Options Target Triangle Count | How much triangles floor should have when "UniformOptionsTargetType" is set to "Triangles"
Uniform Options Target Edge Length | This is triangle size when "UniformOptionsTargetType" is set to "Target Edge Length"
Options Num Smooth Iterations | How many iterations to smooth the floor after triangulation
Extrude Height Floor | This is how much to extrude the floor, upwards or downwards
Normals Smooth Edge Angle | Controls the angle at which the smoothed normals will be split (sharp edges)
Floor Material | Material to be used for the floor
UVRotation | Rotate UVs. The UVs always use the rotation of the longest segment in the spline. This variable adds rotation.
UVScale Scale | Scale of the generated UVs
UVTranslate | You can translate UVs too
Vertex Color | This is the base vertex color that will be set for all vertices
Edges Color | This color will be set only for the edges of the floor mesh
Vertex Color Blur Iterations | Blurring iterations for the vertex colors (edges will not be blurred)
Look for Booleans? | Should this actor look for "BP_SidewalkFloorBooleanActor" and use it to cut holes in the floor mesh
Bulge Smooth Iterations | Smooth after bulging (if "BulgeHeight" is more than 0)
Bulge Height | Elevate floor mesh but not the edges
Tessellation Level | You can tesselate the generated mesh if for example you want to apply VertexPositionOffset (Displacement) in the Material
Final Whole Mesh Smooth Iterations | Smooth the whole mesh after everything else. For example to avoid sharp corners

### Distribution

Variable | Description
--- | ---
Distribute Actors Classes | These actor classes will be chosen randomly to distribute along spline
Keep Distributed Actors Straight? | Having this disabled will align the distributed actor rotation to the spline and disabled will set Pitch and Roll to 0
Distributed Actor Offset from Spline | Offsets actors to outside or the inside of the spline
Knockout Distributed Actors | Randomly remove distributed actors, 0.2 value will remove 20% of the pieces
Distributed Actor Offset Location | Offset all actors location
Distance Between Distributed Actors | Distance between distributed actors
Add Random Rotation for Distributed Actors | Randomize rotation
Max Distributed Actor Count | This is used more as a safety mechanism if you set "DistanceBetweenDistributedActors" to 0 or something
Distribute Static Meshes |  These meshes will be chosen randomly to distribute along spline
Distance Between Distributed Static Meshes |  Distance between distributed meshes
Max Distributed Static Meshes | This is used more as a safety mechanism if you set "DistanceBetweenDistributedStaticMeshes" to 0 or something
Knockout Distributed Static Meshes | Randomly remove distributed meshes, 0.2 value will remove 20% of the pieces
Distributed Static Mesh Offset from Spline | Offsets meshes to outside or the inside of the spline
Distributed Static Mesh Collision Profile Name | Collision profile name for the distributed StaticMeshes
Keep Distributed Static Mesh Straight? | Having this disabled will align the distributed mesh rotation to the spline and disabled will set Pitch and Roll to 0
Add Random Rotation for Distributed Static Meshes |  Randomize rotation
Distributed Static Mesh Offset | Offset all meshes location

## BP_NoSidewalksActor
Variable | Description
--- | ---
Remove Corners? | If true, Every corner piece  will not be placed inside this volume
Remove Straight? | If true, Every straight piece will not be placed inside this volume
Remove Distributed Static Meshes? | If true, distributed StaticMeshes will not be placed inside this volume
Remove Distributed Actors? | If true, distributed Actor will not be placed inside this volume
