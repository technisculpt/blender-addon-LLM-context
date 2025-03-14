# blender-addon-LLM-context
relevant info regarding blender python addon dev to prepend to the context window of a LLM prompt

Blender Add-on Development Context

Add-on Structure:
Every Blender add-on requires standard structure:
Must have bl_info dictionary at top of main file
Needs register and unregister functions
Should follow modular design for maintainability

Essential bl_info Dictionary:
bl_info = {
    "name": "My Add-on Name",
    "author": "Your Name",
    "version": (1, 0, 0),
    "blender": (3, 6, 0),
    "location": "View3D > Sidebar > My Tab",
    "description": "Brief description of what the add-on does",
    "warning": "",
    "doc_url": "",
    "category": "3D View"
}

Registration Framework:
def register():
    bpy.utils.register_class(MyOperator)
    bpy.utils.register_class(MyPanel)
    
def unregister():
    bpy.utils.unregister_class(MyPanel)
    bpy.utils.unregister_class(MyOperator)

if __name__ == "__main__":
    register()

Common Blender Python Classes:
bpy.types.Operator: For implementing commands/tools
bpy.types.Panel: For creating UI panels
bpy.types.PropertyGroup: For organizing related properties
bpy.types.Menu: For creating menus

Key Blender Python Modules:
bpy: Main Blender Python API with submodules context, data, ops
bpy.context: Access current context like active/selected objects, modes, areas, regions
bpy.context.scene: Access scene properties, render settings, timeline
bpy.data: Access all blend file data including objects, materials, meshes, textures
bpy.ops: Execute operators programmatically for modifying objects, rendering, etc.
bmesh: Advanced mesh editing API for creating/editing mesh geometry directly
mathutils: Math utilities with Vector, Matrix, Quaternion, Euler for 3D transformations

Building Operators:
class MyOperator(bpy.types.Operator):
    bl_idname = "object.my_operator"
    bl_label = "My Operator"
    bl_options = {'REGISTER', 'UNDO'}
    
    my_float: bpy.props.FloatProperty(
        name="Float Value",
        default=1.0,
        min=0.0, max=10.0,
        description="A float property"
    )
    
    @classmethod
    def poll(cls, context):
        return context.active_object is not None
    
    def execute(self, context):
        self.report({'INFO'}, "Operator executed")
        return {'FINISHED'}
    
    def invoke(self, context, event):
        return self.execute(context)
    
    def draw(self, context):
        layout = self.layout
        layout.prop(self, "my_float")

Building UI Panels:
class MyPanel(bpy.types.Panel):
    bl_idname = "VIEW3D_PT_my_panel"
    bl_label = "My Panel"
    bl_space_type = 'VIEW_3D'
    bl_region_type = 'UI'
    bl_category = "My Tab"
    
    def draw(self, context):
        layout = self.layout
        
        row = layout.row()
        row.operator("object.my_operator")
        
        obj = context.active_object
        if obj:
            row = layout.row()
            row.prop(obj, "location")

Property Types:
FloatProperty, IntProperty, BoolProperty
StringProperty, EnumProperty 
PointerProperty for references to other data
CollectionProperty for collections of properties

Key Blender Data Paths:
bpy.context.active_object: Currently active object
bpy.context.selected_objects: All selected objects
bpy.context.scene: Current scene
bpy.data.objects: All objects in the file
bpy.data.materials: All materials
bpy.data.meshes: All mesh data

Addon Installation & Testing:
Save as .py file or folder with __init__.py
Install via Edit > Preferences > Add-ons > Install
Enable the add-on by checking its box
For development use Blender's Reload Scripts feature (F3 search)

Best Practices:
Follow PEP 8 style guidelines
Use consistent naming conventions
Document your code with docstrings
Handle errors gracefully with try/except
Support Undo/Redo with the 'UNDO' operator option
Check context before operations to prevent crashes
Use poll methods to determine when operators are available
