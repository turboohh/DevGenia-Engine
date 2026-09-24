# Rendering and model import

The default renderer runs in Java and needs only JDK 17+. It supports Part shapes,
static imported meshes, perspective-correct UVs, directional/ambient lighting,
point lights, picking, transform handles and transparency. Point lights use
distance attenuation and face lighting; they do not cast shadows. Parent Model
transforms are applied to geometry, lights and transform tools.

## Optional OpenGL backend

`setup-gpu.ps1` downloads LWJGL **3.3.6** modules `lwjgl`, `lwjgl-glfw` and
`lwjgl-opengl`, plus the matching Windows x64 or ARM64 native libraries, from
Maven Central. Downloads have a 30-second timeout and are checked against the
repository's SHA-256 checksum. The ordinary Java build has no LWJGL dependency.

Run `run-gpu.bat` after setup, or enable the editor's GPU option when these jars
are on the Java classpath. The backend creates a hidden OpenGL 3.0 compatibility
context and an offscreen framebuffer. Geometry and GLSL texture/alpha shading
are rasterized by OpenGL; the finished image is read back into Swing. GPU
textures use bilinear/mipmap filtering. CPU triangle picking preserves Explorer
selection and the existing transform handles. This hybrid path is an initial
GPU backend, not a guarantee that every scene is faster than software rendering.

The reported `OpenGL · <driver renderer>` state is set only after a successful
native frame. Missing libraries, unsupported drivers and rendering errors produce
an explicit software-fallback reason. Native GL resources are released when the
renderer is disabled or the viewport is closed. Transparent surfaces use a sorted
front-face pass on OpenGL; overlapping/intersecting transparent meshes may differ
from the software renderer's per-pixel ordering.

Native verification requires the optional libraries and a working display driver:

```powershell
javac -encoding UTF-8 -d bin src\devgenia\*.java tests\devgenia\*.java
java -cp "bin;lib/*" devgenia.GpuRendererTests --require-gpu
```

Without LWJGL, `GpuRendererTests` exercises the working software fallback and
prints **GPU NOT VERIFIED**. Such a run is not a native GPU validation.

## Static glTF / GLB import

`GltfImporter.load(Path)` loads glTF 2.0 JSON files with local or base64 buffers,
and binary GLB version 2. It imports the default scene, applies node matrix/TRS
transforms and merges triangle primitives into immutable `MeshData`. Indexed and
non-indexed triangles, triangle strips/fans, and `TEXCOORD_0` are supported.
Positions are centered and normalized; original bounds become the Part's size.
Mesh bytes are embedded in project files, so the original import path is no longer
needed after saving. A planar mesh has zero extent on one axis; the Part inspector
uses its minimum supported size for that axis.

Limits are 32 MB per model/buffer, 64 MB combined buffers, 150,000 vertices,
50,000 triangles and a bounded node/JSON hierarchy. Out-of-range accessors,
non-finite coordinates, invalid indices and escaping/external buffer URLs are
rejected before use. Symlink-resolved local buffer files must remain within the
model directory.

This importer handles static geometry and UVs. It does not import glTF materials,
image textures, skeletal animation, morph targets, sparse accessors or compressed
extensions. Unsupported required geometry features fail with a specific message.
Assign a DevGenia texture through Part Properties after importing. Imported mesh
collision remains the engine's configured collider; it is not a triangle collider.

Format references: [Khronos glTF 2.0 specification](https://registry.khronos.org/glTF/specs/2.0/glTF-2.0.html),
[LWJGL guide](https://www.lwjgl.org/guide),
[GLFW offscreen contexts](https://www.glfw.org/docs/latest/context_guide.html).
