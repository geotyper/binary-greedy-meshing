# Binary Greedy Meshing

Fast voxel meshing algorithm - creates 'greedy' meshes with support for voxel types, baked light & Ambient Occlusion.
UVs can easily be added but the vertex structure would have to be changed from a single integer.

## Installation example (visual studio)
```
git clone git@github.com:cgerikj/binary-greedy-meshing.git --recursive
cd binary-greedy-meshing
cmake -G "Visual Studio 16 2019" .
```

Open binaryMesher.sln

## Program usage

- Noclip: WASD
- Toggle wireframe: X
- Regenerate: Spacebar
- Cycle mesh type: Tab

Meshing duration is printed to the console.

## Algorithm usage
The mesher lives in src/mesher.h

Input data:
- std::vector<uint8_t> voxels (values 0-31 usable)
- std::vector<uint8_t> light  (values 0-15 usable)

The input data includes duplicate edge data from neighboring chunks which is used for visibility culling and AO.
Input data is ordered in YXZ and is 64^3 which results in a 62^3 mesh.

Output data:
- std::vector<uint32_t> of vertices in chunk-space.

## Mesh details

Vertex data is packed into one unsigned integer:
- x, y, z: 6 bit each (0-63)
- Type: 5 bit (0-31)
- Light: 4 bit (0-15)
- Normal: 3 bit (0-5)
- AO: 2 bit

Meshes can be offset to world space using a per-draw uniform or by packing xyz in gl_BaseInstance if rendering with glMultiDrawArraysIndirect.

## Screenshots
| Mesh                       | Wireframe                  |
| -------------------------- |:--------------------------:|
| ![](screenshots/cap1.png)  | ![](screenshots/cap2.png)  |
| ![](screenshots/cap7.png)  | ![](screenshots/cap8.png)  |
| ![](screenshots/cap3.png)  | ![](screenshots/cap4.png)  |
| ![](screenshots/cap5.png)  | ![](screenshots/cap6.png)  |

## Benchmarks
Average execution time running on Ryzen 3800x.

| Scene             | Milliseconds   | Vertices   |
| ----------------- |:--------------:|:----------:|
| 3d hills          | 1.139          | 46798      |
| Red sphere        | 1.108          | 71532      |
| White noise       | 33.315         | 2142608    |
| 3d checkerboard   | 36.832         | 4289904    |
| Empty             | 0.174          | 0          |
