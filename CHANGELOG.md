2.21.0
    Adoption by Linuxgurugamer
    Added InstallChecker and AssemblyVerison.tt
    Fix bug where a null entry in the renderers would throw an exception
    Thanks to github user @Scialytic for this:
        PR originally submitted to original repo:
            QoL improvements for parts with a large number of subtypes
    Thanks to github user @StonesmileGit for this:
        PR originally submitted to original repo:
            Handle ModuleFuelTanks nodes

2.20.0.0
    Placeholder

### v2.20.0

* Recompile against KSP 1.12.3
* Add new `allowSwitchFromInFlight` option to subtypes, if a subtype
  with this set to `true` is selected it can't be switched off of in
  flight
* Add Italian localization

### v2.19.0

* Recompile against KSP 1.12.2

### v2.18.0

* Recompile against KSP 1.11.2
* Fix modules getting force-enabled when new data is loaded
* Change remote AVC reference so that it notifies even if you change KSP
  versions
* Prevent exceptions caused by zombie parts

### v2.17.0

* Recompile against KSP 1.10.1
* Update Spanish localization, add German and Chinese localization

### v2.16.0

* Fix description of attach node mover (`SUBTYPE` -> `NODE` ->
  `positionOffset`) for error messages
* Allow node size to be modified
  * `SUBTYPE` -> `NODE` -> `size`
  * Takes an integer
  * Will scale with TweakScale (and round to the nearest integer)

* Allow more flexible name matching in many places
  * If it starts and ends with `/`, treat it as a regular expression
  * If it contains `*` or `?`, treat those as wildcards (anything or one
    character respectively)
  * Otherwise treat it as a normal string
    * If it starts with `\`, the next character is `/`, and it ends with
      `/`, eliminate the leading `\`
  
  * Implemented in the following places:
    * attach node modifier node name (`SUBTYPE` -> `NODE` -> `name`)
    * transform toggle name (`SUBTYPE` -> `transform`)
    * node toggler name (`SUBTYPE` -> `node`)
    * material modifier transform names (`SUBTYPE` -> `MATERIAL` ->
      `transform`/`baseTransform`)
    * texture modifier transform names (`SUBTYPE` -> `TEXTURE` ->
      `transform`/`baseTransform`)
    * module modifier name (`SUBTYPE` -> `MODULE` -> `IDENTIFIER` ->
      `name`)
    * transform modifier transform names (`SUBTYPE` -> `TRANSFORM` ->
      `name`)

* Implement custom handling for `ModuleRCSFX`
  * Disable unused effects when switching based on `runningEffectName`

* Fix transforms not getting shown/hidden properly after another module
  updates the model
* Allow `ModuleB9PartSwitch` to have its fields and events placed in a
  group:
  * `uiGroupName` - unique identifier of the group
  * `uiGroupDisplayName` - human readable name of the group to show in
    the UI

* New module for assigning PAW groups on other modules:
  `ModuleB9AssignUiGroups`
  * takes one or more `MODULE` nodes that each identify a module to have
    its UI group assigned
    * each one must have an `IDENTIFIER` node to identify the module
      * it must have a `name` which is the name of the module (wildcards
        and regex are allowed)
      * it can have any other fields that uniquely identify the module
      * This is the same as the `IDENTIFIER` in a module switcher
  
  * `uiGroupName` - unique identifier of the group
  * `uiGroupDisplayName` - human readable name of the group to show in
    the UI
  * only applies to fields/events that don't already have a group
  * Cannot apply to `ModuleB9PartSwitch`, `ModuleB9PartInfo`,
    `ModuleB9AssignUiGroups` (itself), or
    `ModuleSimpleAdjustableFairing`

### v2.15.2

* Fix `ModuleJettison` shrouds disappearing in flight (again)

### v2.15.1

* Preserve drag cube weights when recalculating drag cubes
* Don't call drag cube update methods that will be called by the flight
  integrator anyway
* Ensure drag cubes aren't recalculated in flight regardless of
  configuration (it breaks things)

### v2.15.0

* Recompile against KSP 1.9.1

### v2.14.0

* Fix non-unique aspects complaining when present on more than one
  module
* Generic material modifiers
  * `MATERIAL` nodes on subtypes
    * `transform` defines GameObjects on which to use renderers
    * `baseTransform` defines GameObjects and children on which to use
      renderers
    * `FLOAT` - modifies a float property
      * `shaderProperty` - name of the shader property to modify
      * `value` - float to set the value to
    
    * `COLOR` - modifies a color property
      * `shaderProperty` - name of the shader property to modify
        (default `_Color`)
      * `color` - color to set the property to, can be specified in any
        of the regular formats (name, hex, RGB(A) list)
    
    * `TEXTURE` modifies a texture property (same functionality as
      `TEXTURE` nodes directly on the subtype)
      * `currentTexture` - name of current texture to match when
        building (not full path) (optional)
      * `texture` - path to new texture to switch to
      * `isNormalMap` - whether to access the texture as a normal map or
        not (default false)
      * `shaderProperty` - shader property to modify the color on
        * Default `_MainTex` if `isNormalMap = false`
        * Default `_BumpMap` is `isNormalMap = true`

* Listen for `OnPartModelChanged` event to reinitialize model
* Send `ModuleDataChanged` to modules that have had their data changed
  * Include two attributes in the event details,
    `requestNotifyFARToRevoxelize` and `requestRecalculateDragCubes`,
    which can be used to request FAR/drag cubes updates at the end of
    the cycle

* Transform move/rotation/scale now affect drag cubes/FAR
* Send/listen for `DragCubesWereRecalculated` and
  `FarWasNotifiedToRevoxelize` to make sure actions are only done once
  per cycle
* Fix drag cube updates possibly not actually being used

### v2.13.0

* Support changing `ModuleDeployableSolarPanel` `chargeRate`
* Get rid of some useless debug messages related to UI prefabs
* Fix UI breaking on switchers with tech restrictions if subtypes are
  unlocked out of order
* Fix texture switches breaking on inactive subtypes when a part is
  duplicated

### v2.12.1

* Recompile against KSP 1.8.1

### v2.12.0

* Recompile against KSP 1.8.0
* Stop moving the switcher to the end of the part action window
* Remove `bottomOfWindow` paramter

### v2.11.1

* Fix default subtype not activating on the icon

### v2.11.0

* Allow switching UI to not be moved to the end of the part action
  window
  * ModuleB9PartSwitch now accepts `bottomOfWindow = false` which will
    leave it in place
  * Modules without this new parameter are unaffected

* Allow transforms to be scaled
  * `TRANSFORM {}` nodes now accept a `scaleOffset` which multiplies the
    transform's local scale
  * `scaleOffset` either accepts a single number for all 3 axes or 3
    numbers for x, y, z which can be separated by spaces, tabs, or
    commas

* Fix plume switching for ModuleEnginesFX
  * Still doesn't support switching in flight

* Fix Texture switches getting stuck on copied parts
* Fix node offsets not respecting part rotation when attempting to move
  the part with the switch

### v2.10.1

* Add missing Russian localization keys
* Fix surface attach node modifiers not actually doing anything
* Fix surface attach node modifiers checking whether the part allows
  surface attach to it rather than to parent part

### v2.10.0

* Use funds symbol for cost in tooltips
* Fix vessel size including disabled objects
* add new `upgradeRequired` field to `SUBTYPE`s
  * References the name of a `PARTUPGRADE` require do unlock the subtype
  * At least one subtype on every switcher must have no tech restriction
    (i.e. unlocks with the part), otherwise it will complain and remove
    the restriction from the first subtype
  * All subtypes are unlocked in sandbox regardless of whether upgrades
    are applied
  * Warning if the upgrade doesn't exist
  * If you attempt to load a craft with a locked subtype you get a
    warning that it was replaced with the highest priority unlocked
    subtype

* Add `defaultSubtypePriority` to `SUBTYPE`s
  * Number (float) that determines a subtype's priority as the "default"
    subtype (i.e. the one that is chosen when you freshly add the part).
  * The subtype with the highest priority that is also unlocked will be
    chosen
  * If two subtypes have the same priority and both are unlocked, it
    will choose the first
  * The default value is zero.

* Add basic implementation of module switching
  * HIGHLY EXPERIMENTAL
  * Subtypes now accept a `MODULE` node
    * inside is an `IDENTIFIER` node which is used to identify the
      module
    * it must have a `name` which is the same as the module
      * it can have any other fields that are used to identify the
        module
        * e.g. `engineID` on `ModuleEngines`
      
      * Identifying the module by nodes is not currently supported
    
    * It accepts a `DATA` node which provides new data to be loaded into
      the module
    * It accepts a `moduleActive = false` value which causes the module
      to be disabled
  
  * Not everything will work initially, custom handling will have to be
    added for some modules
  * Some modules are blacklisted for loading new data and disabling.
    This list is subject to change.
    * `ModulePartVariants`
    * `ModuleB9PartSwitch`
    * `ModuleB9PartInfo`
    * `ModuleB9DisableTransform`
    * `FSfuelSwitch`
    * `FSmeshSwitch`
    * `FStextureSwitch`
    * `FStextureSwitch2`
    * `InterstellarFuelSwitch`
    * `IntersteallarMeshSwitch`
    * `InterstellarTextureSwitch`

### v2.9.0

* Implement new switching UI based on the stock variant switcher
* Have subtype switching buttons show some info about the subtype being
  switched to in a tooltip
  * By default shows resources (including parent), mass, cost, max
    temperature, max skin temperature, crash tolerance
  * Also shows `descriptionSummary` and `descriptionDetail` from
    subtype, before and after auto-generated info respectively, if
    present

* 4 new fieds on `SUBTYPE`
  * `descriptionSummary` - any info here will be put in the subtype
    switching tooltip before the auto-generated info - make it brief
  * `descriptionDetail` - any info here will be put in the subtype
    switching tooltip after the auto-generated info - go nuts
  * `primaryColor` - color to use in the left part of the switching
    button
    * if not specified, use the tank type's primaryColor
    * if that's not specified, use white
  
  * `secondaryColor` - color to use in the right part of the switching
    button
    * if not specified, use the tank's secondaryColor
    * if that's not specified, use the subtype's primaryColor
    * if that's not specified, use the tank's primaryColor
    * if that's not specified, use gray

* 2 new fields on `B9_TANK_TYPE`
  * `primaryColor` - color to use in the left part of the switching
    button i they subtype does not specify one. If not specified, common
    resource combinations will be used.
  * `secondaryColor` - color to use in the right part of the switching
    button i they subtype does not specify one. If not specified, common
    resource combinations will be used.

* add default colors for common resources
  * `ResourceColorLiquidFuel`
  * `ResourceColorLqdHydrogen`
  * `ResourceColorLqdMethane`
  * `ResourceColorOxidizer`
  * `ResourceColorMonoPropellant`
  * `ResourceColorXenonGas`
  * `ResourceColorElectricChargePrimary`
  * `ResourceColorElectricChareSecondary`
  * `ResourceColorOre`

* Automatically apply resource colors to common resource combinations in
  tanks (if colors are not specified by the tank or subtype):
  * LiquidFuel
  * LiquidFuel/Oxidizer
  * LqdHydrogen
  * LqdHydrogen/Oxidizer
  * LqdMethane
  * LqdMethane/Oxidizer
  * Oxidizer
  * MonoPropellant
  * XenonGas
  * Ore
  * ElectricCharge

### v2.8.1

* Recompile against KSP 1.7.3

### v2.8.0

* Recompile against KSP 1.7.1
* Fix part action window showing removed resources in KSP 1.7.1
* Add Russian localization

### v2.7.1

* Fix part into button being shown when there's no info to display
* Provide more context for subtype initialization errors in the warning
  dialog

### v2.7.0

* Compile for KSP 1.7.0
* Remove `ModuleB9PropagateCopyEvents` from parts since KSP handles this
  correctly now
  * Leave empty class so that KSP doesn't complain when loading
    craft/vessels

* All initialization errors now warn the user but allow the game to
  continue
* Add fuzzy matching for attach node toggling
  * `?` will match any one character, `*` will match anything (or
    nothing)
  * All matching nodes will be switched

* Allow moving and rotation of transforms
  * Subtypes can now have `TRANSFORM` nodes
    * Each one should nave a `name` which is the name of the transform
    * Each one can have a `positionOffset = x, y, z` which is a local
      offset for that transform
      * Any number of modules can modify a transform's position (it's
        additive)
    
    * Each one can have a `rotationOffset = x, y, z` which is a local
      rotation offset
      * Only one module can modify a transform's position

* Remove KSP localization debug logging
* Add Brazilian Portuguese localization
* Localize switch subtype button
* Fix texture switches incorrectly saying the current texture wasn't
  found when really the new texture wasn't found
* Use more correct part names in some log messages
* Allow subtypes to specify a mirror symmetry counterpart
  * Subtypes now accept a `mirrorSymmetrySubtype` value which is the
    subtype name of the mirror symmetry subtype
  * When placing the part in mirror symmetry, the symmetry counterpart
    will use this mirror symmetry subtype, otherwise it will use the
    normal subtype

### v2.6.0

* Recompile against KSP 1.6.1
* Fix misspellings in fatal error and serious warning handlers

### v2.5.1

* Moved stack nodes now respect `scale`, `rescaleFactor`, and TweakScale
* Moved surface attach node now respects `scale` and `rescaleFactor`
* When only one subtype is present, disable switching GUI and display
  subtype title as non-interactable string
* Downgrade incompatible resource switching module to a warning and
  disable B9 resource switching in that case
* French localization

### v2.5.0

* Allow moving stack nodes
  * Within a `SUBTYPE`, `NODE` nodes take a `name` (node ID) and a
    `position` (x, y, z position of the node)

* Fix log message for duplicated subtype names
* Fix texture switching behaving weirdly when copying a part in the
  editor

### v2.4.5

* Fix issues with resource switching and stock delta-v simulation code
  * Exception when copying a part in the editor
  * Delta-v simulation was probably off as well

### v2.4.4

* Recompile against KSP 1.5.1
* Downgrade certain fatal errors to warnings
  * The user will still get an on-screen message but it can be dismissed
    without closing the game
  * Duplicate subtype names is now only a serious warning
  * Subtype without a name is now only a serious warning

### v2.4.3

* Fix .version file again again

### v2.4.2

* Fix .version file again

### v2.4.1

* Fix .version file still listing KSP 1.4.x

### v2.4.0

* Recompile against KSP 1.5
* Provide better context for fatal exceptions
* A few incompatibilities that previously silently disabled
  functionality are now fatal errors
* Add Spanish translation of built-in strings
* Fire onPartResourceListChange when changing resources

### v2.3.3

* Recompile against KSP 1.4.5

### v2.3.2

* Recompile against KSP 1.4.4

### v2.3.1

* Fix ModuleJettison shroud disappearing in flight if used with a
  ModuleB9PartSwitch that affects drag cubes
* Don't destroy info module in flight since that messes with module
  order

### v2.3.0

* Recompile against KSP 1.4.3
* Remove a couple of hacky workarounds as fixes/improvements were added
  in KSP 1.4.3
* Use resource display names rather than identifiers in module
  description
* Extract all hard-coded UI strings into localization table

### v2.2.2

* Fix texture replacements getting locked in when loading a craft in the
  editor if a part up the hierarchy renders procedural drag cubes

### v2.2.1

* Recompile against KSP 1.4.2
* Fix transforms incorrectly being disabled in the part icon if subtypes
  are in a particular order
* Fix `transform` in a `TEXTURE` node looking for renderers in child
  transforms too

### v2.2.0

* Recompile for KSP 1.4.1

### v2.1.1

* Fix texture replacements being reset when drag cubes are rendered
* Fix battery tank type having 100x too much electric charge, bring mass
  in-line with stock

### v2.1.0

* Add texture switching
  * Each subtype can now have `TEXTURE` nodes which take the following
    fields:
    * `texture` (required) - path to the texture you want to use, e.g.
      `MyMod/Parts/SomePart/texture`
    * `currentTexture` (optional) - name of the current texture (just
      the filename excluding the extension, not the full path). Anything
      that does not have this as the current texture will be ignored.
    * `isNormalMap` (optional, default false) - whether the texture is a
      normal map or not (necessary due to KSP treating normal maps
      differently when they are loaded)
    * `shaderProperty` (optional) - name of the shader property that the
      texture sits on. Default is `_MainTex` if `isNormalMap = false` or
      `_BumpMap` if `isNormalMap = true`. For an emissive texture you
      would want `_Emissive`
    * `transform` (optional, can appear more than once) - names of
      transforms to apply the texture switch to
    * `baseTransform` (optional, can appear more than once) - names of
      transforms where the texture switch should be applied to them and
      all of their children
  
  * If no `transform` or `baseTransform` is specified, it will look for
    textures to switch on the entire part

### v2.0.0

* Only match on exact attach node id
* When switching in flight, resources should always start empty
* Allow individual subtypes to not allow switching in flight via
  `allowSwitchInFlight` field
* Allow `ModuleB9PartSwitch` to have its GUI hidden if it has
  `advancedTweakablesOnly = true` and advanced tweakables are disabled
* Better error handling if resource of tank type does not exist (show
  error dialog in game and force the user to quit)
* Fix .version file not being able to be parsed by KSP-AVC
* Move remote .avc file from bintray to s3
* Add back assembly guid (accidentally removed a while ago)

### v1.10.0

* Add new GUI that allows selecting subtype from a list
* Allow switching in flight via switchInFlight parameter (uses new GUI)

### v1.9.0

* Add stackSymmetry part field to subtypes

### v1.8.1

* Fix drag cubes being overwritten with defaults on root part in flight
  scene
* Fix vessel disappearing from map view if root part has a switcher that
  affects drag cubes

### v1.8.0

* Recompile for KSP 1.3
* Drag cube re-rendering now supports IMultipleDragCubes

### v1.7.1

* Fix an occasional NRE when building part info

### v1.7.0

* Allow "child" part switch modules to modify volume of "parent" module
* Allow multiple modules to manage the same transform or node, only
  enable it if they all agree

### v1.6.1

* Switch percentFilled priority to resource -> subtype -> tank type ->
  100% since resources can be overridden on individual subtypes now

### v1.6.0

* Allow tanks to be partially filled - percentFilled can be defined on
  the subtype, resource, or tank type (in decreasing order of priority),
  defaulting to completely full
* Allow toggling resource tweakability in the editor -
  resourcesTweakable can be defined on the subtype or tank type (subtype
  takes priority), default is whatever the standard is for that resource
* Allow RESOURCE nodes directly on the subtype
  * If the resource already exists on the tank, values defined here will
    override what is already on the tank (won't affect other subtypes
    using the same tank)
  * If it isn't already on the tank, it will be added (won't affect
    other subtypes using the same tank)

* Add ModuleB9DisableTransform to remove unused transforms on models
* Major internal changes

### v1.5.3

* Recompile against KSP 1.2.2
* Remove useless warnings in the log
* A few internal changes

### v1.5.2

* Recompile against KSP 1.2.1

### v1.5.1

* Fix resource amounts displaying incorrectly in part tooltip
* Reformat module title in part list tooltip a bit
* Hopefully reduce GC some more

### v1.5.0

* Update for KSP 1.2
* Add CoMOffset, CoPOffset, CoLOffset, CenterOfBuoyancy,
  CenterOfDisplacement to editable part fields
* Hopefully reduce GC allocation a little bit

### v1.4.3

* Recompile against KSP 1.1.3
* Remove some code which is unnecessary in KSP 1.1.3

### v1.4.2

* Fix TweakScale interaction - resource amounts did not account for
  scaling (broken since v1.4.0)

### v1.4.1

* Fix bug where we were setting maxTemp when we should have been setting
  skinMaxTemp or crashTolerance

### v1.4.0

* Find best subtype intelligently
  * If subtype name was previously set, use it to find the correct
    subtype (allows subtypes to be reordered without breaking craft)
  * If name was not previously set or not found, but index was, use it
    (this allows transitioning from current setup and renaming subtypes
    if necessary)
  * If index was not previously set, try to infer subtype based on
    part's resources (this allows easy transitioning from a
    non-switching setup)
  * Finally, just use first subtype

* Add unit testing for subtype finding
* Get rid of some unnecessary logging in debug mode
* Refactor part switching a bit

### v1.3.1

* Fix bug where having ModuleB9PartInfo on a root part would cause
  physics to break due to an exception (really a stock issue but no
  sense waiting for a fix)

### v1.3.0

* Do not destroy incompatible fuel switchers. Instead, disable fuel
  switching
* Allow part's crash tolerance to be edited
* Add info module to display changes to part in the info window. Only
  displays things that can be changed.
* Various internal changes

### v1.2.0

* Support TweakScale integration
* Allow plural switcher description (in part catalog) to be edited)
* Disable changing surface attach node size (problematic with
  Tweakscale)

### v1.1.4

* Don't remove FSfuelSwitch or InterstellarFuelSwitch if
  ModuleB9PartSwitch doesn't manage resources
* Defer rendering drag cubes until part has been attached (fixes
  flickering in editor)
* Avoid firing events multiple times
* Various internal changes

### v1.1.3

* Recompile against KSP 1.1.2
* Simplify part list info a bit
* Hopefully make some error messages clearer
* Various internal refactors and simplifications

### v1.1.2

* Remove FSmeshSwitch and InterstellarMeshSwitch from incompatible
  modules
* Recompile against KSP 1.1.1

### v1.1.1

* Fix resource cost not accounting for units per volume on tank type

### v1.1

* KSP 1.1 compatibility
* Fixed bug where having part switching on the root part would cause
  physics to break
* Moved UI controls to UI\_ChooseOption
* Adjust default Monopropellant tank type to be closer to (new) stock
  values
* Use stock part mass modification
* Hopefully fix incompatible module checking
* Various refactors and simplifications which might improve performance
  a bit

### v1.0.1

* Fix NRE in flight scene

### v1.0.0

* Initial release
