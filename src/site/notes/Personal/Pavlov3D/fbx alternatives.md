---
{"dg-publish":true,"permalink":"/personal/pavlov3-d/fbx-alternatives/","noteIcon":"","created":"2025-09-03T22:48:24.954-05:00"}
---

Date: [[-Daily Activity Log-/2025 09-September 03\|2025 09-September 03]]

Python 3.10 is required, last I checked

so
# CG 0

Ah — now it makes sense. You want to **replace your FBX export with GLB export** so that you can distribute 3D assets without forcing users to deal with Python 3.10 and `fbxsdkpy`.

Here’s the breakdown of how you can approach this:

---

### 1️⃣ Why GLB is easier than FBX for distribution

- **FBX**: Closed format, requires Autodesk FBX SDK (`fbxsdkpy`) — heavy dependency, often Python-version locked.
    
- **GLB**: Open, binary variant of glTF, widely supported in web, mobile, and desktop.
    
- Can be written **pure Python**, often using libraries that don’t require compiling C++ extensions.
    

---

### 2️⃣ Python Libraries for GLB Export

You have a few good options:

1. **`pythreejs` or `trimesh` + `panda3d`**
    
    - `trimesh` can export 3D meshes to glTF/GLB:
        
    
    ```python
    import trimesh
    
    mesh = trimesh.load("my_scene.fbx")  # you can also create it programmatically
    mesh.export("my_scene.glb")
    ```
    
    - Supports pure Python meshes, no FBX SDK needed.
        
    - Works for multiple meshes and scenes.
        
2. **`pygltflib`**
    
    - Lower-level GLB writer, you manually create buffers, scenes, nodes.
        
    - Pure Python, lightweight, good if you already have mesh data in arrays.
        
3. **`assimp` via `pyassimp`**
    
    - Can read FBX and write GLB:
        
    
    ```python
    import pyassimp
    
    scene = pyassimp.load("my_scene.fbx")
    pyassimp.export(scene, "my_scene.glb", file_type="glb")
    pyassimp.release(scene)
    ```
    
    - Caveat: `pyassimp` sometimes needs C/C++ dependencies; better for controlled environments.
        

---

### 3️⃣ Recommended Approach

Since your goal is **maximum portability**:

1. Use **`trimesh`** if your Python code already constructs meshes and curves.
    
2. Programmatically generate mesh/scene data, then export to **GLB**:
    

```python
import trimesh
import numpy as np

# Example: create a simple curve in 3D
points = np.array([[0,0,0], [1,2,0], [2,3,1]])
lines = np.array([[0,1], [1,2]])

curve = trimesh.load_path(points, lines)
curve.export("curve.glb")
```

- Pure Python. No `fbxsdkpy`.
    
- Works on Termux, Windows, Linux, anywhere Python + numpy + trimesh runs.
    

---

### 4️⃣ Migration Strategy from FBX → GLB

1. Identify all FBX export points in your code.
    
2. Replace `fbxsdkpy` calls with `trimesh` GLB export (or `pygltflib` if you need low-level control).
    
3. Keep original mesh/scene logic; only the **export step** changes.
    
4. Test in your web viewer or 3D app.
    

---

If you want, I can **rewrite your current FBX export function** to a **GLB-exporting function** using `trimesh`, keeping all curves and stacking logic intact.

Do you want me to do that?