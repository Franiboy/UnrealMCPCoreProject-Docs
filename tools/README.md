# Tool Reference

Full input/output documentation for all UnrealMCPCore tools, organized by category.

## Blueprint Tools (41)

### Read / Inspect

| Tool | Description |
| ---- | ----------- |
| [get_blueprint](blueprint/get_blueprint.md) | Full Blueprint introspection (variables, functions, components, graphs, class defaults) |
| [list_blueprints](blueprint/list_blueprints.md) | List/search Blueprints via Asset Registry |
| [get_blueprint_graph](blueprint/get_blueprint_graph.md) | Node-level graph data: pins, connections, positions, types |
| [get_blueprint_node](blueprint/get_blueprint_node.md) | Single-node deep inspection with reflection-based properties |
| [search_blueprint_nodes](blueprint/search_blueprint_nodes.md) | Search nodes by title, class, or comment across all graphs |
| [get_blueprint_diff](blueprint/get_blueprint_diff.md) | Compare two Blueprints structurally across all categories |

### Create / Manage

| Tool | Description |
| ---- | ----------- |
| [create_blueprint](blueprint/create_blueprint.md) | Create a Blueprint with configurable parent class |
| [delete_blueprint](blueprint/delete_blueprint.md) | Delete with reference checking and optional force mode |
| [rename_blueprint](blueprint/rename_blueprint.md) | Rename/move with automatic redirector creation |
| [duplicate_blueprint](blueprint/duplicate_blueprint.md) | Copy to a new name/path, preserving parent class |
| [reparent_blueprint](blueprint/reparent_blueprint.md) | Change parent class and recompile |

### Modify

| Tool | Description |
| ---- | ----------- |
| [add_blueprint_variable](blueprint/add_blueprint_variable.md) | Add a typed member variable with metadata (category, default, tooltip, flags) |
| [remove_blueprint_variable](blueprint/remove_blueprint_variable.md) | Remove a member variable by name, returns type and metadata for undo |
| [set_blueprint_variable_properties](blueprint/set_blueprint_variable_properties.md) | Change variable type, default, category, tooltip, flags, replication |
| [add_blueprint_function](blueprint/add_blueprint_function.md) | Create a function graph with typed inputs/outputs, return type, pure/const/access flags |
| [add_blueprint_macro](blueprint/add_blueprint_macro.md) | Create a macro graph with optional tunnel pins (supports exec pins for multi-output flow control) |
| [remove_blueprint_function](blueprint/remove_blueprint_function.md) | Remove a function graph by name, returns full signature for undo |
| [remove_blueprint_macro](blueprint/remove_blueprint_macro.md) | Remove a macro graph by name, returns tunnel pin metadata for undo |
| [add_blueprint_component](blueprint/add_blueprint_component.md) | Add a component with optional transform and parent attachment |
| [remove_blueprint_component](blueprint/remove_blueprint_component.md) | Remove a component by name, returns class/parent/transform for undo |
| [set_component_property](blueprint/set_component_property.md) | Set properties on a component template with nested dot-notation paths |
| [add_blueprint_interface](blueprint/add_blueprint_interface.md) | Implement an interface on a Blueprint (C++ or Blueprint Interface) |
| [remove_blueprint_interface](blueprint/remove_blueprint_interface.md) | Remove an interface from a Blueprint, deleting associated function graphs |
| [add_event_dispatcher](blueprint/add_event_dispatcher.md) | Add a multicast delegate (event dispatcher) with optional typed parameters |
| [remove_event_dispatcher](blueprint/remove_event_dispatcher.md) | Remove an event dispatcher by name, returns parameters for undo |
| [set_blueprint_defaults](blueprint/set_blueprint_defaults.md) | Modify CDO properties with nested dot-notation paths |
| [compile_blueprint](blueprint/compile_blueprint.md) | Single or batch compilation with error/warning collection |

### Graph / Node

| Tool | Description |
| ---- | ----------- |
| [add_node](blueprint/add_node.md) | Add a node to a graph by K2Node class (function calls, events, variables, flow control) |
| [remove_node](blueprint/remove_node.md) | Remove a node from a graph by node ID |
| [connect_pins](blueprint/connect_pins.md) | Wire two pins together (validates type compatibility via graph schema) |
| [disconnect_pins](blueprint/disconnect_pins.md) | Break a wire between two pins |
| [set_pin_default](blueprint/set_pin_default.md) | Set the default value on an input pin (string, bool, float, enum, struct literals) |
| [move_node](blueprint/move_node.md) | Reposition a node in the graph editor (cosmetic, no recompile) |
| [add_comment](blueprint/add_comment.md) | Add a comment box to a Blueprint graph |
| [add_custom_event](blueprint/add_custom_event.md) | Add a custom event node with optional typed output parameters |
| [add_function_call](blueprint/add_function_call.md) | Add a function call node (by ClassName::FunctionName or auto-discovery) |
| [add_variable_get](blueprint/add_variable_get.md) | Add a variable getter node (Blueprint or inherited property) |
| [add_variable_set](blueprint/add_variable_set.md) | Add a variable setter (Set) node to a graph |
| [collapse_to_function](blueprint/collapse_to_function.md) | Collapse selected nodes into a new function |
| [collapse_to_macro](blueprint/collapse_to_macro.md) | Collapse selected nodes into a new macro |
| [promote_to_variable](blueprint/promote_to_variable.md) | Promote a pin to a new member variable (getter or setter) |

## Asset Tools (14)

| Tool | Description |
| ---- | ----------- |
| [list_assets](asset/list_assets.md) | List/search all asset types via Asset Registry with class, path, name filters and pagination |
| [get_asset_info](asset/get_asset_info.md) | Detailed asset metadata: class, size, tags, referencers, dependencies |
| [rename_asset](asset/rename_asset.md) | Rename or move any asset type with automatic redirector creation |
| [move_asset](asset/move_asset.md) | Move any asset to a different content folder, keeping its name |
| [duplicate_asset](asset/duplicate_asset.md) | Duplicate any asset with a new name and optional destination folder |
| [delete_asset](asset/delete_asset.md) | Delete any asset with reference checking and optional force mode |
| [create_folder](asset/create_folder.md) | Create a content browser folder (with intermediate directories) |
| [find_references](asset/find_references.md) | Find all assets referencing a given asset (direct or recursive) |
| [find_dependencies](asset/find_dependencies.md) | Find all assets a given asset depends on (direct or recursive) |
| [fix_redirectors](asset/fix_redirectors.md) | Fix up and delete ObjectRedirectors in a content path |
| [import_asset](asset/import_asset.md) | Import external files (textures, meshes, audio, etc.) into the project |
| [export_asset](asset/export_asset.md) | Export an asset to an external file (PNG, FBX, WAV, etc.) |
| [save_asset](asset/save_asset.md) | Save a dirty asset's package to disk |
| [save_all](asset/save_all.md) | Save all dirty asset packages to disk in one call |

## Level / World Tools (12)

| Tool | Description |
| ---- | ----------- |
| [get_level_info](level/get_level_info.md) | Level name, bounds, streaming levels, actor count by class |
| [list_actors](level/list_actors.md) | List actors in the current level with class, tag, folder, and name filters |
| [get_actor_info](level/get_actor_info.md) | Detailed actor inspection: properties, components, transform, tags, layers |
| [spawn_actor](level/spawn_actor.md) | Spawn an actor from a C++ class or Blueprint asset with transform, label, folder, and tags |
| [delete_actor](level/delete_actor.md) | Remove an actor from the level by name or label |
| [set_actor_transform](level/set_actor_transform.md) | Move, rotate, and/or scale an actor with partial field updates |
| [set_actor_property](level/set_actor_property.md) | Set property values on an actor instance via reflection (dot-notation, arrays) |
| [get_world_settings](level/get_world_settings.md) | World settings: game mode, physics/gravity, kill Z, rendering, lightmass, tick |
| [set_world_settings](level/set_world_settings.md) | Modify world settings: physics, rendering, lightmass, tick via section.key paths |
| [get_streaming_levels](level/get_streaming_levels.md) | List streaming/sub-levels with state, priority, blocking flags, transform, actor count |
| [load_level](level/load_level.md) | Open a different level/map in the editor by path or name |
| [get_selected_actors](level/get_selected_actors.md) | Get currently selected actors in the editor viewport |

## Material Tools (17)

| Tool | Description |
| ---- | ----------- |
| [get_material_info](material/get_material_info.md) | Inspect material properties, parameters, expressions, and shading model |
| [list_material_parameters](material/list_material_parameters.md) | List all parameters of a material/instance by type with values and override status |
| [create_material](material/create_material.md) | Create a new material asset with domain, blend mode, shading model, two-sided |
| [create_material_instance](material/create_material_instance.md) | Create a material instance from a parent with optional parameter overrides |
| [set_material_parameter](material/set_material_parameter.md) | Set a scalar, vector, or texture parameter on a material instance |
| [set_material_properties](material/set_material_properties.md) | Modify base material properties (domain, blend mode, shading model, two-sided, etc.) |
| [assign_material](material/assign_material.md) | Assign a material to a mesh component slot on an actor in the current level |
| [compile_material](material/compile_material.md) | Force recompile a material and return compilation status and errors |
| [get_material_slots](material/get_material_slots.md) | Read material slot assignments from an actor's mesh components |
| [clear_parameter_override](material/clear_parameter_override.md) | Clear a parameter override on a material instance (revert to parent value) |
| [set_material_instance_parent](material/set_material_instance_parent.md) | Change the parent of a material instance |
| [set_static_switch_parameter](material/set_static_switch_parameter.md) | Set a static switch parameter on a material instance |
| [add_material_expression](material/add_material_expression.md) | Add a material expression node to a material graph |
| [remove_material_expression](material/remove_material_expression.md) | Remove a material expression node from a material graph |
| [set_material_expression_property](material/set_material_expression_property.md) | Set a property on a material expression node |
| [connect_material_expression](material/connect_material_expression.md) | Connect two material expression pins |
| [disconnect_material_expression](material/disconnect_material_expression.md) | Disconnect a material expression pin |

## DataTable / Struct Tools (9)

| Tool | Description |
| ---- | ----------- |
| [create_data_table](datatable/create_data_table.md) | Create a new DataTable asset with a specified row struct |
| [list_data_tables](datatable/list_data_tables.md) | List data tables and their row struct |
| [get_data_table_rows](datatable/get_data_table_rows.md) | Get all rows (or filtered) from a data table |
| [add_data_table_row](datatable/add_data_table_row.md) | Add a new row to a DataTable |
| [modify_data_table_row](datatable/modify_data_table_row.md) | Modify field values of an existing DataTable row |
| [delete_data_table_row](datatable/delete_data_table_row.md) | Remove a row from a DataTable by name |
| [create_user_defined_struct](datatable/create_user_defined_struct.md) | Create a new UserDefinedStruct asset with initial fields |
| [modify_user_defined_struct](datatable/modify_user_defined_struct.md) | Add, remove, rename, or change field types in a UserDefinedStruct |
| [get_struct_info](datatable/get_struct_info.md) | Get fields, types, and default values of any UStruct |

## Widget / UI Tools (23)

| Tool | Description |
| ---- | ----------- |
| [create_widget_blueprint](widget/create_widget_blueprint.md) | Create a new UMG Widget Blueprint asset |
| [list_widget_blueprints](widget/list_widget_blueprints.md) | List Widget Blueprints with path and name filtering |
| [list_widget_types](widget/list_widget_types.md) | List available widget classes for adding to Widget Blueprints |
| [get_widget_tree](widget/get_widget_tree.md) | Get the full widget hierarchy of a Widget Blueprint |
| [get_widget_properties](widget/get_widget_properties.md) | Get detailed properties of a specific widget |
| [add_widget](widget/add_widget.md) | Add a child widget to a panel |
| [remove_widget](widget/remove_widget.md) | Remove a widget from the hierarchy |
| [set_widget_property](widget/set_widget_property.md) | Modify widget properties (text, color, visibility, etc.) |
| [set_widget_slot](widget/set_widget_slot.md) | Modify slot/layout properties (anchors, offsets, padding, etc.) |
| [reparent_widget](widget/reparent_widget.md) | Move a widget to a different parent panel |
| [get_widget_animations](widget/get_widget_animations.md) | List all animations in a Widget Blueprint (name, duration, tracks) |
| [create_widget_animation](widget/create_widget_animation.md) | Create a widget animation within a Widget Blueprint |
| [add_animation_track](widget/add_animation_track.md) | Add a track to a widget animation |
| [delete_widget_animation](widget/delete_widget_animation.md) | Delete an animation from a Widget Blueprint |
| [remove_animation_track](widget/remove_animation_track.md) | Remove a property track from a widget animation |
| [get_animation_keyframes](widget/get_animation_keyframes.md) | Get all keyframes of a property track (time, value, interpolation) |
| [add_animation_keyframe](widget/add_animation_keyframe.md) | Add or update a keyframe on a property track |
| [remove_animation_keyframe](widget/remove_animation_keyframe.md) | Remove a keyframe from a property track at a specific time |
| [set_widget_animation_length](widget/set_widget_animation_length.md) | Change the duration of a widget animation |
| [rename_widget_animation](widget/rename_widget_animation.md) | Rename a widget animation |
| [get_widget_event_bindings](widget/get_widget_event_bindings.md) | Get all event bindings for a widget |
| [add_event_binding](widget/add_event_binding.md) | Bind a widget event to a Blueprint function |
| [remove_event_binding](widget/remove_event_binding.md) | Remove an event binding from a widget |

## Animation Tools (24)

### Read / Inspect

| Tool | Description |
| ---- | ----------- |
| [list_animation_assets](animation/list_animation_assets.md) | List AnimSequences, AnimMontages, BlendSpaces, and AnimBPs with filtering |
| [get_animation_info](animation/get_animation_info.md) | Detailed inspection of a single animation asset (timing, notifies, curves, sections) |
| [get_anim_blueprint_info](animation/get_anim_blueprint_info.md) | Deep AnimBP inspection: state machines, states, transitions, variables, graphs |
| [get_skeleton_info](animation/get_skeleton_info.md) | Skeleton bone hierarchy, sockets, virtual bones, slots, curves, blend profiles |

### Create / Manage

| Tool | Description |
| ---- | ----------- |
| [create_anim_montage](animation/create_anim_montage.md) | Create an AnimMontage from a skeleton with optional source AnimSequence and slot |
| [create_blend_space](animation/create_blend_space.md) | Create a BlendSpace (1D or 2D) with axis settings and target skeleton |
| [create_anim_blueprint](animation/create_anim_blueprint.md) | Create an Animation Blueprint for a target skeleton with optional parent class |

### AnimSequence / Montage Editing

| Tool | Description |
| ---- | ----------- |
| [add_anim_notify](animation/add_anim_notify.md) | Add a notify event to an AnimSequence or AnimMontage |
| [remove_anim_notify](animation/remove_anim_notify.md) | Remove a notify event by name or index |
| [add_anim_curve](animation/add_anim_curve.md) | Add a float curve to an AnimSequence |
| [remove_anim_curve](animation/remove_anim_curve.md) | Remove a float curve from an AnimSequence |
| [set_anim_curve_keys](animation/set_anim_curve_keys.md) | Set keyframes on a float curve |
| [set_anim_montage_sections](animation/set_anim_montage_sections.md) | Add, remove, or link montage sections |
| [set_anim_montage_slot](animation/set_anim_montage_slot.md) | Change the slot name on a montage track |

### BlendSpace Editing

| Tool | Description |
| ---- | ----------- |
| [set_blend_space_axis](animation/set_blend_space_axis.md) | Configure BlendSpace axis parameters (name, range, grid, snap, wrap) |
| [add_blend_space_sample](animation/add_blend_space_sample.md) | Add an AnimSequence sample to a BlendSpace at a position |
| [remove_blend_space_sample](animation/remove_blend_space_sample.md) | Remove a sample from a BlendSpace by index or animation |

### Animation Blueprint Editing

| Tool | Description |
| ---- | ----------- |
| [add_anim_state](animation/add_anim_state.md) | Add a state to a state machine in an Animation Blueprint |
| [remove_anim_state](animation/remove_anim_state.md) | Remove a state (and its transitions) from a state machine |
| [add_anim_transition](animation/add_anim_transition.md) | Add a transition between two states with crossfade and priority |
| [remove_anim_transition](animation/remove_anim_transition.md) | Remove a transition between two states |
| [set_anim_state_animation](animation/set_anim_state_animation.md) | Set the animation played by a state |
| [add_anim_bp_variable](animation/add_anim_bp_variable.md) | Add a variable to an Animation Blueprint |
| [set_anim_bp_variable](animation/set_anim_bp_variable.md) | Modify variable properties in an Animation Blueprint |

## PCG Tools (11)

### PCG Query (4)

| Tool | Description |
| ---- | ----------- |
| [list_pcg_graphs](pcg/list_pcg_graphs.md) | List PCG graph assets with path, name, node count, and generation settings |
| [get_pcg_graph_info](pcg/get_pcg_graph_info.md) | Detailed PCG graph inspection: nodes, pins, edges, hierarchical generation |
| [get_pcg_node](pcg/get_pcg_node.md) | Deep single-node inspection: settings, seed, pins with connections, overridable params |
| [get_pcg_component_info](pcg/get_pcg_component_info.md) | Inspect PCG component on an actor: graph reference, generation status, grid size |

### PCG Lifecycle (2)

| Tool | Description |
| ---- | ----------- |
| [create_pcg_graph](pcg/create_pcg_graph.md) | Create a new PCG Graph asset (saved to disk, registered with Asset Registry) |
| [add_pcg_component](pcg/add_pcg_component.md) | Add/update a PCG Component on an actor and optionally assign a graph |

### PCG Graph Editing (5)

| Tool | Description |
| ---- | ----------- |
| [add_pcg_node](pcg/add_pcg_node.md) | Add a node to a PCG Graph by settings class name |
| [remove_pcg_node](pcg/remove_pcg_node.md) | Remove a node from a PCG Graph |
| [connect_pcg_nodes](pcg/connect_pcg_nodes.md) | Wire two PCG nodes together (output pin to input pin) |
| [disconnect_pcg_nodes](pcg/disconnect_pcg_nodes.md) | Break a wire between two PCG nodes |
| [set_pcg_node_settings](pcg/set_pcg_node_settings.md) | Set settings/properties on a PCG node via reflection |

## Behavior Tree Tools (7)

| Tool | Description |
| ---- | ----------- |
| [list_behavior_trees](behaviortree/list_behavior_trees.md) | List Behavior Tree assets with path and name filtering |
| [get_behavior_tree_info](behaviortree/get_behavior_tree_info.md) | Inspect a BT: root node hierarchy, tasks, decorators, services, blackboard |
| [create_behavior_tree](behaviortree/create_behavior_tree.md) | Create a new BT asset with root composite (Selector, Sequence, SimpleParallel) |
| [add_bt_node](behaviortree/add_bt_node.md) | Add a node to a BT (Composite, Task, Decorator, Service) by path |
| [remove_bt_node](behaviortree/remove_bt_node.md) | Remove a node from a BT by path |
| [set_bt_node_property](behaviortree/set_bt_node_property.md) | Set a property on a BT node via reflection |
| [connect_bt_nodes](behaviortree/connect_bt_nodes.md) | Move/re-parent a BT node to a new parent composite |

## Blackboard Tools (6)

| Tool | Description |
| ---- | ----------- |
| [list_blackboards](blackboard/list_blackboards.md) | List Blackboard Data assets with path and name filtering |
| [get_blackboard_info](blackboard/get_blackboard_info.md) | Inspect a Blackboard: keys, parent chain, inherited keys, sync flag |
| [create_blackboard](blackboard/create_blackboard.md) | Create a new Blackboard Data asset with optional parent |
| [add_blackboard_key](blackboard/add_blackboard_key.md) | Add a key (Bool, Int, Float, String, Name, Vector, Rotator, Enum, Object, Class) |
| [remove_blackboard_key](blackboard/remove_blackboard_key.md) | Remove a key from a Blackboard by name |
| [set_blackboard_key_properties](blackboard/set_blackboard_key_properties.md) | Modify key properties (instance synced, description, category) |

## State Tree Tools (7)

| Tool | Description |
| ---- | ----------- |
| [list_state_trees](statetree/list_state_trees.md) | List State Tree assets with path and name filtering |
| [get_state_tree_info](statetree/get_state_tree_info.md) | Inspect a State Tree: states, transitions, tasks, conditions, evaluators |
| [create_state_tree](statetree/create_state_tree.md) | Create a new State Tree asset with configurable schema |
| [add_state_tree_state](statetree/add_state_tree_state.md) | Add a state (State, Group, Linked, Subtree) as root or child |
| [remove_state_tree_state](statetree/remove_state_tree_state.md) | Remove a state and its children from a State Tree |
| [add_state_tree_transition](statetree/add_state_tree_transition.md) | Add a transition between states (trigger, priority, delay) |
| [set_state_tree_task](statetree/set_state_tree_task.md) | Set/configure a task on a state (class, properties, enabled) |

## EQS Tools (5)

| Tool | Description |
| ---- | ----------- |
| [list_eqs_queries](eqs/list_eqs_queries.md) | List EQS Query template assets with path and name filtering |
| [get_eqs_query_info](eqs/get_eqs_query_info.md) | Inspect an EQS Query: generators, tests, options, weighting |
| [create_eqs_query](eqs/create_eqs_query.md) | Create a new EQS Query template asset |
| [add_eqs_generator](eqs/add_eqs_generator.md) | Add a generator to an EQS Query (Points Around, Actors Of Class, Grid, etc.) |
| [add_eqs_test](eqs/add_eqs_test.md) | Add a test to an EQS Query (Distance, Trace, Dot, PathFinding, etc.) |

## Smart Object Tools (5)

| Tool | Description |
| ---- | ----------- |
| [list_smart_object_definitions](smartobject/list_smart_object_definitions.md) | List Smart Object Definition assets with path and name filtering |
| [get_smart_object_definition_info](smartobject/get_smart_object_definition_info.md) | Inspect a Smart Object Definition: slots, offsets, tags, policies |
| [create_smart_object_definition](smartobject/create_smart_object_definition.md) | Create a new Smart Object Definition asset |
| [add_smart_object_slot](smartobject/add_smart_object_slot.md) | Add a slot to a Smart Object Definition (offset, rotation, tags, enabled) |
| [remove_smart_object_slot](smartobject/remove_smart_object_slot.md) | Remove a slot from a Smart Object Definition by index |

## Mesh Tools (12)

### Static Mesh (6)

| Tool | Description |
| ---- | ----------- |
| [get_static_mesh_info](mesh/get_static_mesh_info.md) | Inspect a Static Mesh: LODs, materials, sockets, bounds, Nanite, collision, lightmap |
| [set_static_mesh_property](mesh/set_static_mesh_property.md) | Set Static Mesh properties (lightmap_resolution, nanite_enabled, lod_group, min_lod) |
| [add_static_mesh_socket](mesh/add_static_mesh_socket.md) | Add a socket to a Static Mesh |
| [remove_static_mesh_socket](mesh/remove_static_mesh_socket.md) | Remove a socket from a Static Mesh by name |
| [set_static_mesh_collision](mesh/set_static_mesh_collision.md) | Configure collision settings (complexity, LOD for collision) |
| [set_static_mesh_material](mesh/set_static_mesh_material.md) | Assign a material to a Static Mesh material slot |

### Skeletal Mesh (3)

| Tool | Description |
| ---- | ----------- |
| [get_skeletal_mesh_info](mesh/get_skeletal_mesh_info.md) | Inspect a Skeletal Mesh: LODs, materials, skeleton, physics asset, morph targets, bounds, sockets |
| [set_skeletal_mesh_property](mesh/set_skeletal_mesh_property.md) | Set Skeletal Mesh properties (physics_asset, min_lod) |
| [set_skeletal_mesh_material](mesh/set_skeletal_mesh_material.md) | Assign a material to a Skeletal Mesh material slot |

### Mesh Operations (3)

| Tool | Description |
| ---- | ----------- |
| [merge_meshes](mesh/merge_meshes.md) | Merge multiple Static Mesh actors into a single Static Mesh asset |
| [generate_lods](mesh/generate_lods.md) | Auto-generate LODs for a Static Mesh or Skeletal Mesh |
| [generate_collision](mesh/generate_collision.md) | Auto-generate collision for a Static Mesh (box, sphere, capsule, convex) |

## Motion Design Tools (5)

| Tool | Description |
| ---- | ----------- |
| [get_motion_design_info](motiondesign/get_motion_design_info.md) | Inspect Cloners and Effectors, or list all Motion Design actors |
| [create_motion_design_actor](motiondesign/create_motion_design_actor.md) | Spawn a Cloner or Effector actor with optional properties |
| [set_motion_design_property](motiondesign/set_motion_design_property.md) | Set properties on a Cloner or Effector (enabled, seed, layout, magnitude, etc.) |
| [add_motion_design_effector](motiondesign/add_motion_design_effector.md) | Link an Effector to a Cloner |
| [add_motion_design_modifier](motiondesign/add_motion_design_modifier.md) | Set effector shape/type and mode |

## Enhanced Input Tools (7)

### Query

| Tool | Description |
| ---- | ----------- |
| [list_input_mapping_contexts](enhanced-input/list_input_mapping_contexts.md) | List Input Mapping Context assets with mapping counts |
| [get_input_mapping_context_info](enhanced-input/get_input_mapping_context_info.md) | Inspect an IMC: action mappings, keys, triggers, modifiers |

### Create

| Tool | Description |
| ---- | ----------- |
| [create_input_action](enhanced-input/create_input_action.md) | Create a new Input Action asset (Boolean, Axis1D, Axis2D, Axis3D) |
| [create_input_mapping_context](enhanced-input/create_input_mapping_context.md) | Create a new Input Mapping Context asset |

### Edit

| Tool | Description |
| ---- | ----------- |
| [add_input_mapping](enhanced-input/add_input_mapping.md) | Add an action mapping to a context (action, key, triggers, modifiers) |
| [remove_input_mapping](enhanced-input/remove_input_mapping.md) | Remove an action mapping from a context by index |
| [set_input_action_property](enhanced-input/set_input_action_property.md) | Set Input Action properties (value type, description, triggers, modifiers) |

## GAS — Abilities & Effects (13)

### Query

| Tool | Description |
| ---- | ----------- |
| [list_gameplay_abilities](gas/list_gameplay_abilities.md) | List Gameplay Ability Blueprint assets: tags, instancing policy, net execution policy |
| [get_gameplay_ability_info](gas/get_gameplay_ability_info.md) | Inspect a Gameplay Ability: policies, all tag containers, cost/cooldown effects |
| [list_gameplay_effects](gas/list_gameplay_effects.md) | List Gameplay Effect Blueprint assets: duration, modifiers, stacking, tags |
| [get_gameplay_effect_info](gas/get_gameplay_effect_info.md) | Inspect a Gameplay Effect: modifiers, duration, stacking, tags, cues, components |
| [list_attribute_sets](gas/list_attribute_sets.md) | List Attribute Set classes (C++ and Blueprint): name, origin, attribute count |
| [get_attribute_set_info](gas/get_attribute_set_info.md) | Inspect an Attribute Set: attributes with name, base value, current value |

### Lifecycle

| Tool | Description |
| ---- | ----------- |
| [create_gameplay_ability](gas/create_gameplay_ability.md) | Create a new Gameplay Ability Blueprint (custom parent class supported) |
| [create_gameplay_effect](gas/create_gameplay_effect.md) | Create a new Gameplay Effect Blueprint (Instant/Infinite/HasDuration) |
| [create_attribute_set](gas/create_attribute_set.md) | Create a new Attribute Set Blueprint with initial FGameplayAttributeData attributes |

### Edit

| Tool | Description |
| ---- | ----------- |
| [set_gameplay_effect_modifier](gas/set_gameplay_effect_modifier.md) | Add or edit a modifier on a Gameplay Effect (attribute, op, magnitude) |

### Gameplay Tags (extended)

| Tool | Description |
| ---- | ----------- |
| [add_gameplay_tag](gas/add_gameplay_tag.md) | Add a new Gameplay Tag to the project INI (idempotent if tag exists) |
| [remove_gameplay_tag](gas/remove_gameplay_tag.md) | Remove a Gameplay Tag from the project INI |
| [rename_gameplay_tag](gas/rename_gameplay_tag.md) | Rename a Gameplay Tag in the project INI (creates a redirector) |

## Audio / MetaSounds (19)

### Query

| Tool | Description |
| ---- | ----------- |
| [list_audio_assets](audio/list_audio_assets.md) | List Sound Waves, Sound Cues, MetaSounds, Sound Classes, Sound Mixes, Attenuation Settings |
| [get_sound_cue_info](audio/get_sound_cue_info.md) | Inspect a Sound Cue: nodes, connections, volume/pitch, attenuation, sound class, duration |
| [get_metasound_info](audio/get_metasound_info.md) | Inspect a MetaSound Source: metadata, inputs, outputs, graph pages, nodes, edges, dependencies |

### Lifecycle

| Tool | Description |
| ---- | ----------- |
| [create_sound_cue](audio/create_sound_cue.md) | Create a new Sound Cue asset (optional volume/pitch multipliers) |
| [create_metasound](audio/create_metasound.md) | Create a new MetaSound Source asset with default graph |
| [create_sound_class](audio/create_sound_class.md) | Create a new Sound Class asset (optional volume/pitch) |
| [create_sound_mix](audio/create_sound_mix.md) | Create a new Sound Mix asset |
| [create_sound_attenuation](audio/create_sound_attenuation.md) | Create a new Sound Attenuation Settings asset (optional shape, radius, falloff) |

### Sound Cue Editing

| Tool | Description |
| ---- | ----------- |
| [add_sound_cue_node](audio/add_sound_cue_node.md) | Add a node to a Sound Cue graph (WavePlayer, Mixer, Random, Modulator, Delay, etc.) |
| [remove_sound_cue_node](audio/remove_sound_cue_node.md) | Remove a node from a Sound Cue graph by index |
| [connect_sound_cue_nodes](audio/connect_sound_cue_nodes.md) | Wire two Sound Cue nodes together (parent -> child) |
| [set_sound_cue_node_property](audio/set_sound_cue_node_property.md) | Set type-specific properties on a Sound Cue node |

### MetaSound Editing

| Tool | Description |
| ---- | ----------- |
| [add_metasound_node](audio/add_metasound_node.md) | Add a node to a MetaSound graph by class name (e.g. UE.Add.Float) |
| [remove_metasound_node](audio/remove_metasound_node.md) | Remove a node from a MetaSound graph by GUID |
| [connect_metasound_nodes](audio/connect_metasound_nodes.md) | Wire two MetaSound nodes together (output -> input) |
| [set_metasound_input](audio/set_metasound_input.md) | Set the default value of a MetaSound node input |

### Audio Configuration

| Tool | Description |
| ---- | ----------- |
| [set_sound_class_properties](audio/set_sound_class_properties.md) | Set Sound Class properties (volume, pitch, LPF, attenuation scale, flags) |
| [set_sound_mix_properties](audio/set_sound_mix_properties.md) | Set Sound Mix properties (EQ, timing, sound class effect overrides) |
| [set_sound_attenuation](audio/set_sound_attenuation.md) | Set Sound Attenuation settings (shape, distance, spatialization, occlusion) |

## Landscape / Terrain Tools (9)

### Landscape Query

| Tool | Description |
| ---- | ----------- |
| [get_landscape_info](landscape/get_landscape_info.md) | Inspect a Landscape actor: size, components, layers, material, LOD, collision, Nanite, edit layers |
| [list_landscape_layers](landscape/list_landscape_layers.md) | List Landscape Layer Info assets: layer name, hardness, physical material, weight-blend mode |

### Landscape Lifecycle

| Tool | Description |
| ---- | ----------- |
| [create_landscape](landscape/create_landscape.md) | Create a new flat Landscape actor with configurable grid size, section layout, position, and scale |
| [create_landscape_layer_info](landscape/create_landscape_layer_info.md) | Create a new Landscape Layer Info asset for weight-based painting layers |

### Landscape Editing

| Tool | Description |
| ---- | ----------- |
| [set_landscape_material](landscape/set_landscape_material.md) | Assign a material (and optional hole material) to a Landscape actor |
| [set_landscape_property](landscape/set_landscape_property.md) | Set Landscape properties (LOD, collision, navigation, Nanite, streaming) |
| [export_landscape_heightmap](landscape/export_landscape_heightmap.md) | Export the Landscape heightmap to a raw uint16 binary file |
| [import_landscape_heightmap](landscape/import_landscape_heightmap.md) | Import a raw uint16 heightmap file onto an existing Landscape |
| [import_landscape_layer_weight](landscape/import_landscape_layer_weight.md) | Import a raw uint8 weight map for a paint layer |

## Physics Material Tools (4)

| Tool | Description |
| ---- | ----------- |
| [list_physics_materials](physics/list_physics_materials.md) | List Physical Material assets with name, friction, restitution, density, surface type |
| [get_physics_material_info](physics/get_physics_material_info.md) | Inspect a Physical Material: friction, restitution, density, surface type, combine modes, sleep thresholds, strength |
| [create_physics_material](physics/create_physics_material.md) | Create a new Physical Material asset with optional initial properties |
| [set_physics_material_property](physics/set_physics_material_property.md) | Set Physical Material properties (friction, restitution, density, surface type, strength, etc.) |

## Physics Asset Tools (8)

| Tool | Description |
| ---- | ----------- |
| [get_physics_asset_info](physics/get_physics_asset_info.md) | Inspect a Physics Asset: bodies (bone, shape, mass, physics type), constraints (limits, motion types) |
| [create_physics_asset](physics/create_physics_asset.md) | Create a new empty Physics Asset |
| [add_physics_body](physics/add_physics_body.md) | Add a physics body with collision shape (sphere, box, capsule) to a Physics Asset |
| [remove_physics_body](physics/remove_physics_body.md) | Remove a physics body by bone name or index (cascades constraint removal) |
| [set_physics_body_property](physics/set_physics_body_property.md) | Set body properties (mass, damping, physics type, simulate, gravity, collision profile) |
| [add_physics_constraint](physics/add_physics_constraint.md) | Add a constraint between two bodies with swing/twist limits |
| [remove_physics_constraint](physics/remove_physics_constraint.md) | Remove a constraint from a Physics Asset by index |
| [set_physics_constraint_property](physics/set_physics_constraint_property.md) | Set constraint properties (motion types, limits, stiffness, damping, collision) |

## Foliage Tools (7)

### Query (3 tools)

| Tool | Description |
| ---- | ----------- |
| [list_foliage_types](foliage/list_foliage_types.md) | List Foliage Type assets with optional path/name filtering and summary info |
| [get_foliage_type_info](foliage/get_foliage_type_info.md) | Detailed Foliage Type inspection: mesh, painting, placement, instance settings, scalability |
| [get_foliage_instances](foliage/get_foliage_instances.md) | Query foliage instances in the current level with optional type and bounding box filters |

### Create / Edit (4 tools)

| Tool | Description |
| ---- | ----------- |
| [create_foliage_type](foliage/create_foliage_type.md) | Create a new Foliage Type (Instanced Static Mesh) from a Static Mesh with optional properties |
| [set_foliage_type_property](foliage/set_foliage_type_property.md) | Set Foliage Type properties: density, scaling, placement, shadows, culling, mobility |
| [add_foliage_instances](foliage/add_foliage_instances.md) | Programmatically place foliage instances with location, rotation, and scale |
| [remove_foliage_instances](foliage/remove_foliage_instances.md) | Remove foliage instances by type, bounding box area, or all |

## World Partition Tools (7)

### Query (3 tools)

| Tool | Description |
| ---- | ----------- |
| [get_world_partition_info](worldpartition/get_world_partition_info.md) | Inspect World Partition settings: enabled state, streaming, grid info, HLOD defaults, data layer summary |
| [list_data_layers](worldpartition/list_data_layers.md) | List all Data Layers with name, type, initial runtime state, visibility, and hierarchy |
| [get_hlod_info](worldpartition/get_hlod_info.md) | Get HLOD settings: default layer, all HLOD layer assets with configuration |

### Create / Edit (4 tools)

| Tool | Description |
| ---- | ----------- |
| [create_data_layer](worldpartition/create_data_layer.md) | Create a new Data Layer asset and register it with the world's data layer system |
| [set_data_layer_state](worldpartition/set_data_layer_state.md) | Set Data Layer state: editor visibility, loaded-in-editor, initial runtime state |
| [set_actor_data_layer](worldpartition/set_actor_data_layer.md) | Add or remove an actor from a Data Layer |
| [set_world_partition_settings](worldpartition/set_world_partition_settings.md) | Configure World Partition settings: streaming, default HLOD layer |

## Control Rig Tools (6)

### Query (2 tools)

| Tool | Description |
| ---- | ----------- |
| [list_control_rigs](controlrig/list_control_rigs.md) | List Control Rig Blueprint assets with optional name/path filtering |
| [get_control_rig_info](controlrig/get_control_rig_info.md) | Inspect hierarchy elements (bones, controls, nulls, curves), transforms, and control settings |

### Create / Edit (4 tools)

| Tool | Description |
| ---- | ----------- |
| [create_control_rig](controlrig/create_control_rig.md) | Create a new Control Rig Blueprint, optionally from a Skeleton |
| [add_control_rig_element](controlrig/add_control_rig_element.md) | Add an element (Bone, Control, Null, Curve) to a Control Rig's hierarchy |
| [remove_control_rig_element](controlrig/remove_control_rig_element.md) | Remove an element from a Control Rig's hierarchy |
| [set_control_rig_element_property](controlrig/set_control_rig_element_property.md) | Set properties on a hierarchy element: transform, parent, shape, color |

## Rendering Configuration Tools (6)

| Tool | Description |
| ---- | ----------- |
| [get_rendering_settings](rendering/get_rendering_settings.md) | Get global rendering settings |
| [set_rendering_settings](rendering/set_rendering_settings.md) | Modify global rendering settings |
| [get_post_process_settings](rendering/get_post_process_settings.md) | Get Post Process Volume settings |
| [set_post_process_settings](rendering/set_post_process_settings.md) | Set Post Process Volume settings |
| [set_nanite_settings](rendering/set_nanite_settings.md) | Configure Nanite per-mesh or global settings |
| [set_lumen_settings](rendering/set_lumen_settings.md) | Configure Lumen GI and reflection settings |

## Curve Asset Tools (6)

| Tool | Description |
| ---- | ----------- |
| [list_curve_assets](curve/list_curve_assets.md) | List Curve Float, Vector, LinearColor, and CurveTable assets |
| [get_curve_asset_info](curve/get_curve_asset_info.md) | Inspect a Curve asset: keys, ranges, type, optional evaluation |
| [create_curve_asset](curve/create_curve_asset.md) | Create a new Curve asset (Float, Vector, LinearColor) with optional keys |
| [set_curve_keys](curve/set_curve_keys.md) | Set keyframes on a Curve asset (replace all or add/update) |
| [create_curve_table](curve/create_curve_table.md) | Create a new empty Curve Table asset |
| [add_curve_table_row](curve/add_curve_table_row.md) | Add a named row (curve) to a Curve Table |

## Project / Editor Info Tools (17)

### Project & Plugin Metadata

| Tool | Description |
| ---- | ----------- |
| [get_project_info](project/get_project_info.md) | Project name, engine version, target platforms, enabled plugins, maps, project modules |
| [list_plugins](project/list_plugins.md) | List all discovered plugins with version, description, category, type, and enabled status |
| [list_modules](project/list_modules.md) | List all modules with loaded status, game module flag, and file path |

### Type Introspection

| Tool | Description |
| ---- | ----------- |
| [get_class_hierarchy](project/get_class_hierarchy.md) | Inheritance chain for any UClass (from given class up to UObject) |
| [get_class_info](project/get_class_info.md) | Properties and functions of a native or Blueprint class |
| [get_enum_values](project/get_enum_values.md) | Values and display names of a UEnum |
| [list_asset_types](project/list_asset_types.md) | List all registered UClass types that can exist as assets |

### Editor State & Configuration

| Tool | Description |
| ---- | ----------- |
| [get_editor_state](project/get_editor_state.md) | PIE status, loaded level, selected actors, editor mode, viewport camera, dirty packages |
| [get_editor_preferences](project/get_editor_preferences.md) | Read editor INI config settings by section and key |
| [set_editor_preferences](project/set_editor_preferences.md) | Write editor INI config settings by section and key |

### Project Settings

| Tool | Description |
| ---- | ----------- |
| [get_project_settings](project/get_project_settings.md) | Read project configuration from Default*.ini files |
| [set_project_settings](project/set_project_settings.md) | Write/modify project configuration in Default*.ini files |
| [get_collision_profiles](project/get_collision_profiles.md) | List collision presets and channels with object types and responses |
| [list_gameplay_tags](project/list_gameplay_tags.md) | List all gameplay tags defined in the project with hierarchy info |
| [list_input_actions](project/list_input_actions.md) | List Enhanced Input Actions and Input Mapping Contexts with key bindings |

### Console & Log Access

| Tool | Description |
| ---- | ----------- |
| [execute_console_command](project/execute_console_command.md) | Run an Unreal console command and capture the output |
| [get_log_output](project/get_log_output.md) | Retrieve recent Output Log entries with category and verbosity filtering |

## PIE / Testing Tools (10)

| Tool | Description |
| ---- | ----------- |
| [start_pie](pie/start_pie.md) | Start a Play In Editor session |
| [stop_pie](pie/stop_pie.md) | Stop the current PIE session |
| [is_pie_running](pie/is_pie_running.md) | Check PIE session state |
| [pause_pie](pie/pause_pie.md) | Pause or unpause PIE |
| [take_screenshot](pie/take_screenshot.md) | Capture viewport screenshot |
| [inject_input](pie/inject_input.md) | Simulate keyboard/mouse input |
| [get_fps](pie/get_fps.md) | Frame rate and frame time stats |
| [get_viewport_info](pie/get_viewport_info.md) | Editor viewport camera info |
| [set_viewport_camera](pie/set_viewport_camera.md) | Move editor viewport camera |
| [run_automation_test](pie/run_automation_test.md) | List/run automation tests |

## Infrastructure Tools (1)

| Tool | Description |
| ---- | ----------- |
| [get_request_log](infrastructure/get_request_log.md) | Query the history of all MCP tool calls with filtering and detail options |

## Error Convention

| Scenario           | `isError` | Detail                                      |
| ------------------ | --------- | ------------------------------------------- |
| Total failure      | `true`    | Asset not found, invalid args, all ops fail |
| Partial success    | `false`   | Check `success_count` / `fail_count`        |
| Full success       | `false`   | All operations succeeded                    |
