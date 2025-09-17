---
{"dg-publish":true,"permalink":"/projects/modeling/juniper-design-team-introduction/","noteIcon":"","created":"2025-09-17T08:49:19.443-05:00"}
---

Date: [[-Daily Activity Log-/2025 09-September 17\|2025 09-September 17]]

### A New Way to Create Plant Graphics

The goal of this project is to create a new graphics pipeline that allows us to design modern, high-quality Ovation graphics using tools and data that are far more advanced than the native Ovation Graphics Builder. Instead of using the old, proprietary editor, we'll create immersive, beautiful graphics in a modern design environment and then translate them into the simple Ovation format. This will allow us to move around the virtual plant, almost like a video game, with views from different angles and levels of detail.

---

### The Workflow

This project is a three-step process that uses modern design tools and your skills as a graphic designer.

#### 1. Design and Export Your Assets

You'll start by taking raw data from different sources and bringing it into a design environment.

- **Source Assets:** We will use CAD, P&ID (Piping and Instrumentation Diagrams), and GIS (Geographic Information System) files. The plan is to create a "single source of truth" by linking all of this information in a central database or file1111.
    
- **The Design Tool:** You will use a JavaScript library called **D3.js** in a web browser to turn this raw data into graphics2222. This is where your design skills will shine. You'll apply advanced techniques like
    
    **isometric projection** to create angled, three-dimensional-looking views of the plant3333.
    
- **Output:** The final output of this step will be an **SVG (Scalable Vector Graphics)** file. SVG is a great format for this because it's vector-based, just like the Ovation graphics language, and you can embed special information (like the names of Ovation points) directly into the file4444.
    

#### 2. The Python Translator

This is the "technical" part of the project, which you'll learn as we go. We'll use a Python script to do the conversion.

- **Input:** The script will read the SVG file you created in the first step5.
    
- **Process:** The Python script will read the shapes and special information from the SVG file and translate them into the proprietary Ovation graphics language6. It will also scale the coordinates to fit the Ovation system's specific grid7777.
    
- **Output:** The script will generate a `.src` file, which is a human-readable text file that defines the Ovation graphic8.
    

#### 3. Import and Deploy

The last step is to get your new graphic onto the Ovation system.

- **Import:** We'll use the Ovation Developer Studio to import the `.src` file9.
    
- **Compile:** The system will then compile the file, turning it into a `.diag` file that can be displayed on the operator screens10.
    

---

### What to Keep in Mind: The Ovation System's Limits

As you design, it's important to remember that Ovation isn't a modern web browser. It has some key constraints:

- **No Free-Form Animation:** Ovation doesn't support complex, timeline-based animations like the ones you might create in Blender or with CSS. Instead, its "animations" are tied to specific plant data11111111. For example, you can make a shape change color when a value is high, or blink faster as a motor's speed increases121212121212121212.
    
- **Limited Interaction:** User interaction is not as flexible as a web page. We can't have a custom `onclick` event on every object. Instead, we have to define specific, invisible clickable areas that trigger predefined actions, like changing to a different screen or opening a control window13131313.
    
- **State-Driven Design:** The key is to design based on "states." Instead of animating a valve opening, you'll design two different states for the valve: one for "open" and one for "closed." Our Python script will then translate these states into Ovation's conditional logic14.
    

The project vision is to make your design work easier and more powerful by treating Ovation as a final destination for our designs, not the starting point15151515. By using modern tools, we can create graphics that are far more sophisticated and useful than anything we could have made with the old methods.

---

# The Asset Translation Process
The JavaScript library D3.js acts as the crucial bridge between the structured data from your source assets and the final SVG graphics file1. It takes a structured data format, like the JSON output from

`1_asset_extractor.py`, and programmatically renders it as a visual SVG scene2.

Here is a breakdown of how different types of users would interact with this process.

### Super User Functions (CLI-based)

This approach is for a user who is comfortable with scripting and wants to automate the graphics generation process without a graphical interface.

- **Process:** The super user would operate this part of the pipeline as a command-line tool. The `2_svg_renderer.js` script would be run using a JavaScript runtime like Node.js, not in a web browser.
    
- **Input:** The script would read the JSON file generated by the `1_asset_extractor.py` script3.
    
- **Code-Centric:** The user would primarily interact with the D3.js script itself. They would modify the code to define how different asset types are drawn (e.g., pumps as circles, valves as rectangles) and how they are positioned based on the 2D coordinates in the JSON data4.
    
- **Automation:** This method allows for batch generation. The user could write a simple loop to process multiple JSON files, each representing a different plant view, and automatically generate a corresponding SVG file for each.
    
- **Metadata Integration:** The user would write the code to explicitly "bind process point information and interaction metadata" directly to the SVG elements5. This is done by setting
    
    `data-ovation-*` attributes on the SVG shapes, which is the key to telling the Python translator how to handle the object666.
    

### Powerful, Streamlined Interface (Browser-based)

This is the recommended approach for a graphic designer, as it leverages the visual power of a web browser while hiding the underlying complexity of the code.

- **Process:** The user would open a simple HTML file (like the provided `svg_renderer.html` 7) in a web browser.
    
- **Automatic Rendering:** When the page loads, the D3.js script inside the page automatically fetches the JSON data from the `1_asset_extractor.py` script and renders the plant view8888. The user immediately sees a live, visual preview of the graphic9.
    
- **Interactive Controls:** While the provided demo is simple, a more advanced version of this interface would provide user-friendly controls. For example, instead of editing a JSON file, the user could:
    
    - **Drag-and-Drop:** Visually adjust the positions of elements on the canvas.
        
    - **Sliders and Inputs:** Use sliders to change a "camera angle" or "zoom level," with the D3.js script dynamically re-rendering the isometric projection in real-time.
        
    - **Property Panels:** Click on a shape and use a graphical panel to enter the `data-ovation-*` metadata (e.g., the point name or animation type) without ever touching the code10.
        
- **Output:** Once the user is happy with the visual design, they simply click a button on the webpage to download the final SVG file11111111. This SVG is the input for the next step of the pipeline, the Python translator12.
    

This dual-path approach allows the project to be accessible to a new user for design while still offering a powerful, automated workflow for a developer or "super user." Both paths are built on the same core D3.js logic, which transforms raw data into a visually rich and metadata-laden SVG.

---
# Asset Formats
Based on the project analysis and a general understanding of industry standards, here are the acceptable formats for GIS, P&ID, and CAD files. The goal is to use formats that are either open-source, widely supported, or can be easily converted to a format that can be processed by our Python and JavaScript tools.

### CAD Formats

For CAD files, the most important distinction is between "native" and "neutral" formats.1 Native formats are tied to a specific software package (like `.DWG` for AutoCAD) and may contain proprietary data that is difficult to extract.2 Neutral formats are designed for interoperability between different systems.3

The best formats to use for this project are:

- **DXF (Drawing Exchange Format)**: This is a widely supported open standard from AutoCAD.4 It's excellent for 2D geometry and is the de-facto standard for exchanging vector graphics between different CAD programs.5
    
- **DWG (AutoCAD Drawing)**: While proprietary, it's so common that most tools can read it. It is a good option if you have an AutoCAD license and need to retain 3D solid model information.
    
- **STEP (`.stp`, `.step`)**: The most popular neutral CAD format.6 It's a standard for exchanging 3D product data and is ideal for solid models and surfaces.
    
- **IGES (`.igs`, `.iges`)**: An older standard, primarily used for surface geometry.7 It's still widely supported and a good fallback for surface-based models.
    

### P&ID Formats

P&ID (Piping and Instrumentation Diagrams) formats are less standardized than CAD or GIS, often depending on the specific software used to create them (e.g., Microsoft Visio, AutoCAD P&ID).8 The key is to get the diagram into a format from which you can easily extract the geometry and the associated data (e.g., equipment names, point tags).

- **Vector Formats (Preferred)**: If possible, a vector format that retains the shapes and text as separate, editable objects is ideal. This is because the Python script needs to read and interpret these individual objects.
    
    - **PDF**: A PDF that retains vector data is excellent. Tools can be used to extract the vector paths and text, which can then be converted to a more usable format like JSON.
        
    - **SVG (Scalable Vector Graphics)**: This is the target output format for your D3.js work, so if you can get source P&IDs as SVG, it would streamline the process.
        
- **Image Formats (Less Ideal)**: P&IDs are often provided as static images (e.g., `JPG`, `PNG`, `TIFF`) to protect intellectual property.9 For these, a separate step using Optical Character Recognition (OCR) and computer vision would be required to digitize the symbols and text.10
    

### GIS Formats

GIS data is typically easier to work with because many of its formats are open-source and designed for interoperability.11

- **Shapefile (`.shp`, `.shx`, `.dbf`)**: This is a very common and widely supported format for storing vector data (points, lines, and polygons) and their associated attributes.12 You need all the accompanying files for it to work.
    
- **GeoJSON (`.geojson`)**: An open standard that encodes geographic data structures using JavaScript Object Notation (JSON).13 This is an excellent format for this project because it is human-readable and integrates directly into the JavaScript/D3.js workflow.
    
- **GeoTIFF (`.tif`, `.tiff`)**: A standard for georeferenced raster (image) data.14 If you're working with aerial imagery or satellite photos as part of the project, this format would be the standard.
    

For a summary of various file formats and their characteristics, you can refer to the video "How to Convert Piping and Instrumentation Diagrams (P&ID) to CAD".

---
# CAD Drawing in Blender and FreeCAD
Creating dimensionally accurate assets requires a shift in mindset from artistic, free-form modeling to a more technical, constraint-based approach. While your experience in Blender is a great starting point, the focus now is on precision rather than aesthetics.

Here are guidelines for using both Blender and FreeCAD to create assets that are suitable for a CAD-like workflow.

### Using Blender for Dimensional Accuracy

While Blender is not a parametric CAD tool, you can enforce dimensional accuracy by adopting a specific workflow. This is a good way to get started using a tool you already know.

**1. Set Up Your Scene Properly**

- **Units:** The very first step is to set up real-world units. Go to `Scene Properties > Units` and change the Unit System to **Metric** or **Imperial**. Set the Unit Scale to `1.0` and the Length to a convenient unit like `Meters` or `Centimeters`. This ensures that every measurement you make in Blender corresponds to a real-world dimension.
    
- **Grid:** Adjust the grid scale to match your working units for visual reference.
    

**2. Focus on Precision Modeling Tools**

- **Numerical Input:** Instead of eyeballing a measurement, use numerical input whenever possible. For example, after initiating a move (`G`), extrude (`E`), or scale (`S`) operation, simply type in a number to specify the exact distance.
    
- **Snapping:** Use Blender's `Snap` feature (hold `Ctrl` or click the magnet icon) extensively. Set the snapping mode to `Vertex`, `Edge`, or `Grid` to align new geometry perfectly with existing points.
    
- **Boolean Operations:** For creating clean cuts and holes, the Boolean Modifier is your best friend. Instead of manually cutting faces, use simple primitive shapes (like a cylinder for a hole) to subtract from your main model. This is a common practice in CAD modeling.
    

**3. Maintain Clean Geometry for Export**

- **Manifold Meshes:** Before exporting, ensure your meshes are "manifold." This means they have no open edges, internal faces, or disconnected vertices. A clean mesh is essential for reliable export to CAD-like formats.
    
- **Use the 3D Print Toolbox:** Blender has a built-in add-on for this. Go to `Edit > Preferences > Add-ons` and search for **3D Print Toolbox**. This tool can analyze your mesh and report issues like non-manifold edges, which can then be fixed.
    

### Using FreeCAD for True Parametric Design

FreeCAD is a completely different kind of program from Blender. It is a "parametric" CAD tool, which means every change you make is recorded and can be edited later. This is the ideal tool for creating assets that serve as a "single source of truth" for dimensional data.

**1. Understand the Parametric Workflow**

- **Start with a Sketch:** The core of FreeCAD is the **Part Design Workbench**. You always start by creating a 2D sketch on a plane. This sketch contains the fundamental geometry of your object.
    
- **Add Constraints:** This is the most crucial step. Instead of drawing freehand, you will use **constraints** to define every aspect of your sketch.
    
    - **Dimensional Constraints:** Add constraints to lock down the exact length, width, or radius of a shape.
        
    - **Geometric Constraints:** Use constraints to force lines to be perpendicular, parallel, or tangent to each other.
        
- **Extrude to 3D:** Once your sketch is fully constrained and turns green, you can use a tool like **Pad** to extrude it into a 3D solid.
    

**2. Learn Key Workbenches**

- **Part Design:** Your primary workbench for creating solid models. It's built for creating single, cohesive objects.
    
- **Draft:** Contains tools for 2D drafting and snapping, similar to AutoCAD's command-line interface. It's useful for importing and working with 2D DXF files.
    

**3. The Power of Iteration**

- The biggest advantage of FreeCAD is that you can go back and change a single dimension in your sketch, and the entire 3D model will automatically update. This is perfect for when you receive new measurements or need to adjust a design.
    

### Recommendation for Your Project

- **For New, Dimensionally Critical Assets:** Use **FreeCAD**. The assets you create in FreeCAD will be the most reliable and easiest to integrate into a CAD or GIS-based workflow. It will save you time in the long run by ensuring precision from the beginning.
    
- **For Artistic or Conversion Work:** Use **Blender**. If you need to import a CAD file to add stylized details, or to convert between different mesh formats, Blender's tools and your existing experience will be very valuable.