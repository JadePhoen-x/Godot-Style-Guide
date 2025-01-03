# Godot-Style-Guide
My personal style guide for projects made in Godot while using GDScript. 
Heavily inspired/borrowing [Justin Wasilenko's Unity Style Guide](https://github.com/justinwasilenko/Unity-Style-Guide/blob/master/README.md?plain=1#cases) formatting and advice.
Additionally using the tips provided within the [Official Godot Syle Guide](https://docs.godotengine.org/en/stable/tutorials/scripting/gdscript/gdscript_styleguide.html) for GDScript.

<a name="toc"></a>
## Table of Contents

> 1. [Introduction](#introduction)
> 1. [Project Structure](#structure)
> 1. [Scripts](#scripts)
> 1. [Asset Naming Conventions](#anc)

<a name="introduction"></a>
## 1. Introduction

### Sections

> 1.1 [Style](#style)

> 1.2 [Important Terminology](#importantterminology)

<a name="style"></a>
### 1.1 Style

#### All structure, assets, and code in any project should look like a single person created it, no matter how many people contributed.
Moving from one project to another should not cause a re-learning of style and structure. Conforming to a style guide removes unneeded guesswork and ambiguities.

It also allows for more productive creation and maintenance as one does not need to think about style, simply follow instructions. This style guide is written with best practices in mind, meaning that by following this style guide you will also minimize hard to track issues.

#### Friends do not let friends have bad style.
If you see someone working either against a style guide or no style guide, try to correct them.

When working within a team or discussing within a community, it is far easier to help and to ask for help when people are consistent. Nobody likes to help untangle someone's spaghetti code or deal with assets with names they can't understand.

If you are helping someone who's work conforms to a different but consistent and sane style guide, you should be able to adapt to it. If they do not conform to any style guide, please direct them here.

<a name="importantterminology"></a>
### 1.2 Important Terminology

<a name="terms-scene"></a>
#### Scenes/Nodes
Scenes are fundamental building blocks for Godot projects. Think of a scene as a contained piece of the project. These can be components, entities, UI, or entire levels.
The finished game or project will be comprised of nested scenes within scenes to create functionality.

<a name="terms-component"></a>
#### Components
A component is a scripted Scene designed to fulfill a singular purpose. These do <b>NOT</b> perform functionality on their own, and are designed to be nested within a greater scene that provides the real functionality.
A component may have nested components within itself. Components should not require to be aware of its parent scene.

As an example, a Character scene might have a Velocity component that contains functions to adjust the player's velocity. 
The Character's script would then use the functions in the Velocity component along with player input to move itself within worldspace.

<a name="terms-level"></a>
#### Levels
Levels are Scenes that serve as the top level parent of a collection of other Scenes.

<a name="terms-cases"></a>
#### Cases
There are a few different ways you can name things. Here are some common casing types:

> ##### PascalCase
> Capitalize every word and remove all spaces, e.g. `DesertEagle`, `StyleGuide`, `ASeriesOfWords`.
> 
> ##### camelCase
> The first letter is always lowercase but every following word starts with uppercase, e.g. `desertEagle`, `styleGuide`, `aSeriesOfWords`.
>
> ##### lowercase
> All letters are lowercase, e.g. `deserteagle`, 
>
> ##### snake_case
> Words start lowercase and are separated by an underscore, e.g. `desert_eagle`, `style_guide`, `a_series_of_words`.

**[⬆ Back to Top](#table-of-contents)**

<a name="structure"></a>
## 2. Project Structure
The directory structure style of a project should be considered law. Asset naming conventions and content directory structure go hand in hand, and a violation of either causes unneeded chaos.

> Folder and file names are written in `snake_case`

> IMPORTANT: Development Assets (work in progress or testing assets contained in `_Dev`) should always be prefixed with a `_` to make it easy when configuring things in the inspector
<pre>
res://
  <a name="#structure-developers">_dev</a>(Use a `_`to keep this folder at the top)
      dev_name
          (Work in progress and testing assets)
  addons
  <a name="structure-top-level">project_name</a>
      material_library
          debug
          shaders
      resources
          audio
              music
              sfx
          fonts
          materials
          shaders
          themes
      scenes
          autoload
              game_events
          components
          gameplay
              characters
                  player
                      (Scripts, Models, Textures, Materials, Scene)
              equipment
          <a name="#structure-levels">levels</a>
              main_menu
              level_1
              level_2
          objects
              architecture (Single use large set pieces)
              props (Small repeating objects)
          ui
    scripts
</pre>

This structure is explained in the following sub-sections.

### Sections

> 2.1 [Folder Names](#structure-folder-names)

> 2.2 [Top-Level Folders](#structure-top-level)

> 2.3 [Developer Folders](#structure-developers)

> 2.4 [Levels](#levels)

> 2.5 [Define Ownership](#structure-ownership)

> 2.6 [`Assets` and `AssetTypes`](#structure-assettypes)

> 2.7 [Large Sets](#structure-large-sets)

> 2.8 [Material Library](#structure-material-library)

> 2.9 [Scene Structure](#scene-structure)

<a name="2.1"></a>
<a name="structure-folder-names"><a>
### 2.1 Folder Names
These are common rules for naming any folder in the content structure.

<a name="2.1.1"></a>
#### Always Use [snake_case](#terms-cases)
snake_case refers to starting a name with a lowercase letter and then instead of using spaces, we use `_`. For example, `desert_eagle`, `style_guide`, `a_series_of_words`.

<a name="2.1.2"></a>
#### Never Use Spaces
Re-enforcing [2.1.1](#2.1.1), never use spaces. Spaces can cause various engineering tools and batch processes to fail. Ideally your project's root also contains no spaces and is located somewhere such as `D:\Project` instead of `C:\Users\My Name\My Documents\Unity Projects` though this isn't required.

<a name="2.1.3"></a>
#### Never Use Unicode Characters And Other Symbols (except `_`)
If one of your game characters is named 'Zoë', its folder name should be `zoe`. Unicode characters can be worse than [Spaces](#2.1.2) for engineering tools and some parts applications don't support Unicode characters in paths either.

Related to this, if your project has and your computer's user name has a Unicode character (i.e. your name is `Zoë`), any project located in your `My Documents` folder will suffer from this issue. Often simply moving your project to something like `D:\Project` will fix these mysterious issues.

Using other characters outside `a-z`, `A-Z`, and `0-9` such as `@`, `-`, `,`, `*`, and `#` can also lead to unexpected and hard to track issues on other platforms, source control, and weaker engineering tools.
`_` is treated with exception in regard to Godot.

<a name="structure-no-empty-folders"></a>
#### No Empty Folders
There simply shouldn't be any empty folders. They clutter the content browser.

If you find that the content browser has an empty folder you can't delete, you should perform the following:
1. Be sure you're using source control.
1. Navigate to the folder on-disk and delete the assets inside.
1. Close the editor.
1. Make sure your source control state is in sync (i.e. if using Perforce, run a Reconcile Offline Work on your content directory)
1. Open the editor. Confirm everything still works as expected. If it doesn't, revert, figure out what went wrong, and try again.
1. Ensure the folder is now gone.
1. Submit changes to source control.

<a name="2.2"></a>
<a name="structure-top-level"><a>
### 2.2 Use A Top Level Folder For Project Specific Assets
All of a project's assets should exist in a folder named after the project. For example, if your project is named 'Generic Shooter', _all_ of it's content should exist in `Assets/GenericShooter`.

> The `_dev` folder is not for assets that your project relies on and therefore is not project specific. See [Developer Folders](#2.3) for details about this.

There are multiple reasons for this approach.

<a name="2.2.1"></a>
#### No Global Assets
Often in code style guides it is written that you should not pollute the global namespace and this follows the same principle. When assets are allowed to exist outside of a project folder it often becomes much harder to enforce a strict structure layout as assets not in a folder encourages the bad behavior of not having to organize assets.

Every asset should have a purpose, otherwise it does not belong in a project. If an asset is an experimental test and shouldn't be used by the project it should be put in a [`Developer`](#2.3) folder.

<a name="2.2.2"></a>
#### Reduce Migration Conflicts
When working on multiple projects it is common for a team to copy assets from one project to another if they have made something useful for both. 

By placing all project specific assets in a top level folder you reduce the chance of migration conflict when importing those assets into a new project.

<a name="2.2.2e1"></a>
##### Master Material Example
For example, say you created a master material in one project that you would like to use in another project so you migrated that asset over. If this asset is not in a top level folder, it may have a name like `Assets/MaterialLibrary/M_Master`. If the target project doesn't have a master material already, this should work without issue.

As work on one or both projects progress their respective master materials may change to be tailored for their specific projects due to the course of normal development.

The issue comes when, for example, an artist for one project created a nice generic modular set of static meshes and someone wants to include that set of static meshes in the second project. If the artist who created the assets used material instances based on `Assets/MaterialLibrary/M_Master` as they're instructed to, when a migration is performed there is a great chance of conflict for the previously migrated `Assets/MaterialLibrary/M_Master` asset.

This issue can be hard to predict and hard to account for. The person migrating the static meshes may not be the same person who is familiar with the development of both project's master material, and they may not be even aware that the static meshes in question rely on material instances which then rely on the master material. The Migrate tool requires the entire chain of dependencies to work however, and so it will be forced to grab `Assets/MaterialLibrary/M_Master` when it copies these assets to the other project and it will overwrite the existing asset.

It is at this point where if the master materials for both projects are incompatible in _any way_, you risk breaking possibly the entire material library for a project as well as any other dependencies that may have already been migrated, simply because assets were not stored in a top level folder. The simple migration of static meshes now becomes a very ugly task.

<a name="2.2.3"></a>
#### Samples, Templates, and 3rd Party Content Are Risk-Free
An extension to [2.2.2](#2.2.2), if a team member decides to add sample content, template files, or assets they bought from a 3rd party, it is guaranteed that these new assets will not interfere with the project in any way unless your project's top level folder is not uniquely named.

You can not trust 3rd party content to fully conform to the [top level folder rule](#2.2). There exist many assets that have the majority of their content in a top level folder but also have possibly modified Unity sample content as well as level files polluting the global `Assets` folder.

When adhering to [2.2](#2.2), the worst 3rd party conflict you can have is if two 3rd party assets both have the same sample content. If all your assets are in a project specific folder, including sample content you may have moved into your folder, your project will never break.

#### DLC, Sub-Projects, and Patches Are Easily Maintained
If your project plans to release DLC or has multiple sub-projects associated with it that may either be migrated out or simply not cooked in a build, assets relating to these projects should have their own separate top level content folder. This make cooking DLC separate from main project content far easier. Sub-projects can also be migrated in and out with minimal effort. If you need to change a material of an asset or add some very specific asset override behavior in a patch, you can easily put these changes in a patch folder and work safely without the chance of breaking the core project.

<a name="2.3"></a>
<a name="structure-developers"></a>
### 2.3 Use Developers Folder For Local Testing
During a project's development, it is very common for team members to have a sort of 'sandbox' where they can experiment freely without risking the core project. Because this work may be ongoing, these team members may wish to put their assets on a project's source control server. Not all teams require use of Developer folders, but ones that do use them often run into a common problem with assets submitted to source control.

It is very easy for a team member to accidentally use assets that are not ready for use which will cause issues once those assets are removed. For example, an artist may be iterating on a modular set of static meshes and still working on getting their sizing and grid snapping correct. If a world builder sees these assets in the main project folder, they might use them all over a level not knowing they could be subject to incredible change and/or removal. This causes massive amounts of re-working by everyone on the team to resolve.

If these modular assets were placed in a Developer folder, the world builder should never of had a reason to use them and the whole issue would never happen.

Once the assets are ready for use, an artist simply has to move the assets into the project specific folder. This is essentially 'promoting' the assets from experimental to production.

<a name="levels"></a>
### 2.4 All [Level](#terms-level-map) Files Belong In A Folder Called Levels
Level files are incredibly special and it is common for every project to have its own map naming system, especially if they work with sub-levels or streaming levels. No matter what system of map organization is in place for the specific project, all levels should belong in `//res:/project_name/levels`.

Being able to tell someone to open a specific map without having to explain where it is is a great time saver and general 'quality of life' improvement. It is common for levels to be within sub-folders of `levels`, such as `levels/campaign_1/` or `levels/arenas`, but the most important thing here is that they all exist within `//res:/project_name/levels`.

This also simplifies the job of cooking for engineers. Wrangling levels for a build process can be extremely frustrating if they have to dig through arbitrary folders for them. If a team's levels are all in one place, it is much harder to accidentally not cook a map in a build. It also simplifies lighting build scripts as well QA processes.

<a name="2.5"></a>
<a name="structure-ownership"></a>
### 2.5 Define Ownership
In teams of more than one, define ownership of zone/assets/features. Some assets like scenes do not handle simultaneous changes by multiple people very well, creating conflict. Having a single person who can change (or give the right to change) a given assets helps to avoid that problem.

<a name="2.6"></a>
<a name="structure-assettypes"></a>
### 2.6 Do Not Create Folders Called `Assets` or `AssetTypes`

<a name="2.6.1"></a>
#### Creating a folder named `Assets` is redundant.
All assets are assets.

<a name="2.6.2"></a>
#### Creating a folder named `Meshes`, `Textures`, or `Materials` is redundant.
All asset names are named with their asset type in mind. These folders offer only redundant information and the use of these folders can easily be replaced with the robust and easy to use filtering system the Content Browser provides.

Want to view only static mesh in `environment/rocks/`? Simply turn on the Static Mesh filter. If all assets are named correctly, they will also be sorted in alphabetical order regardless of prefixes. Want to view both static meshes and skeletal meshes? Simply turn on both filters. This eliminates the need to potentially have to `Control-Click` select two folders in the Content Browser's tree view.

> This also extends the full path name of an asset for very little benefit. The `sm_` prefix for a static mesh is only three characters, whereas `meshes/` is seven characters.

Not doing this also prevents the inevitability of someone putting a static mesh or a texture in a `materials` folder.

<a name="2.7"></a>
<a name="structure-large-sets"></a>
### 2.7 Very Large Asset Sets Get Their Own Folder Layout

This can be seen as a pseudo-exception to [2.6](#2.6).

There are certain asset types that have a huge volume of related files where each asset has a unique purpose. The two most common are Animation and Audio assets. If you find yourself having 15+ of these assets that belong together, they should be together.

For example, animations that are shared across multiple characters should lay in `characters/common/animations` and may have sub-folders such as `locomotion` or `cinematic`.

> This does not apply to assets like textures and materials. It is common for a `rocks` folder to have a large amount of textures if there are a large amount of rocks, however these textures are generally only related to a few specific rocks and should be named appropriately. Even if these textures are part of a [Material Library](#2.8).

<a name="2.8"></a>
<a name="structure-material-library"></a>
### 2.8 `MaterialLibrary`

If your project makes use of master materials, layered materials, or any form of reusable materials or textures that do not belong to any subset of assets, these assets should be located in `//res:/project_name/material_library`.

This way all 'global' materials have a place to live and are easily located.

> This also makes it incredibly easy to enforce a 'use material instances only' policy within a project. If all artists and assets should be using material instances, then the only regular material assets that should exist are within this folder. You can easily verify this by searching for base materials in any folder that isn't the `material_library`.

The `material_library` doesn't have to consist of purely materials. Shared utility textures, material functions, and other things of this nature should be stored here as well within folders that designate their intended purpose. For example, generic noise textures should be located in `material_library/utility`.

Any testing or debug materials should be within `material_library/debug`. This allows debug materials to be easily stripped from a project before shipping and makes it incredibly apparent if production assets are using them if reference errors are shown.


<a name="2.9"></a>
<a name="scene-structure"></a>
## 2.9 Scene Structure
Next to the project’s hierarchy, there’s also scene hierarchy. Use named Node/Node2D/Node3D nodes as scene folders.

> Nodes in a scene should be ordered within their respective group using the following order based on inheritance unless otherwise required: Node > Control > Node2D > Node3D

 - All empty objects should be located at 0,0,0 with default rotation and scale.
 - For empty objects that are only containers for scripts and are otherwise not a Component, use “@” as prefix – e.g. @Cheats
 - When you’re instantiating an object in runtime, make sure to put it in _Dynamic – do not pollute the root of your hierarchy or you will find it difficult to navigate through it.

> IMPORTANT: The following structure template mostly applies to Level scenes. However, the same principles apply to other scenes. Just like with the Directory, there should NEVER be any empty Nodes without a purpose.
<pre>
@System
@Debug
@Management
@UI
    Layouts
Cameras
Lights
    Volumes
Particles
Sound
World
    Global
    Room1
        Architecture
        Terrain
        Props
Gameplay
	Actors
	Items
	Triggers
	Quests
_Dynamic
</pre>

**[⬆ Back to Top](#table-of-contents)**

<a name="scripts"></a>

## 3. Scripts

This section will focus on C# classes and their internals. When possible, style rules conform to Microsoft's C# standard.

### Sections
> 3.1 [Class Organization](#classorganization)

> 3.2 [Compiling](#compiling)

> 3.3 [Variables](#variables)

> 3.4 [Functions](#functions)

<a name="classorganization"></a>
### 3.1 Class Organization
Source files should contain only one public type, although multiple internal classes are allowed.

Source files should be given the name of the public class in the file.

Organize namespaces with a clearly defined structure,

Class members should be alphabetized, and grouped into sections:
* signals
* enums
* constants
* exported variables
* public variables
* private variables
* onready variables

* optional built-in virtual _init method
* built-in virtual _ready method
* remaining built-in virtual methods
* public methods
* private methods
* event methods

> For an example script using these principles, refer to the included script_example.gd within the repository.

#### All Public Methods Should Have A Summary
Any method intended to be used by another script should have a comment above its declaration that explains its purpose. While in C# XML summaries can be used, we simply make do with comments.

```
# Makes the player attack
func attack() -> void:
    # attack
```

#### Foldout Groups
If a class has only a small number of variables, Foldout Groups are not required.

If a class has a moderate amount of variables (5-10), all [Serializable](#serializable) variables should have a non-default Foldout Group assigned. A common category is `Config`.

To create Foldout Groups, use `@export_group()` above the variables to be grouped.

#### Commenting
Comments should be used to describe intention, algorithmic overview, and/or logical flow.
It would be ideal if from reading the comments alone someone other than the author could understand a function’s intended behavior and general operation.

While there are no minimum comment requirements and certainly some very small routines need no commenting at all, it is hoped that most routines will have comments reflecting the programmer’s intent and approach.

> Note, comments should not be used if the purpose of a method is explicitly obvious. Your code, except in certain cases such as those previously mentioned, should be able to be understood from being simply read.

##### Comment Style
Place the comment on a separate line, not at the end of a line of code.

Begin comment text with an uppercase letter.

End comment text with a period.

Insert one space between the comment delimiter (#) and the comment text, as shown in the following example.

Where ever possible, place comments above the code instead of beside it. Here are some examples:
```
        # Sample comment above a variable.
        var my_int := 5;
```

#### Spacing
Do use a single space after a comma between function arguments.

Example: `example.example_method(my_int, 0, 1);`
* Do not use a space after the parenthesis and function arguments.
* Do not use spaces between a function name and parenthesis.
* Do not use spaces inside brackets.

#### Indentation
Each indent level should be one greater than the block containing it.
Use 2 indent levels to distinguish continuation lines from regular code blocks.
Exceptions to this rule are arrays, dictionaries, and enums. Use a single indentation level to distinguish continuation lines.

#### Blank Lines
Surround variable declaration with two blank lines, additionally space each group of variable declarations with a single blank line.
Surround functions and class definitions with two blank lines.
Use on blank line inside functions to seperate logical sections.

```
@export my_exported_var
@export my_second_exported_var

var my_var
var my_second_var


func example_function_1():
    # Code block.


func example_function_2():
    # Code block.
```

<a name="compiling"></a>
### 3.2 Compiling
All scripts should compile with zero warnings and zero errors. You should fix script warnings and errors immediately as they can quickly cascade into very scary unexpected behavior.

Do *not* submit broken scripts to source control. If you must store them on source control, shelve them instead.

<a name="variables"></a>
### 3.3 Variables
The word `variable` refers to internal data of a class or script.

#### Variable Naming

##### Nouns
All non-boolean variable names must be clear, unambiguous, and descriptive nouns. 

##### Case
All variables use snake_case.

##### Considered Context
All variable names must not be redundant with their context as all variable references in the class will always have context.

###### Considered Context Examples:
Consider a Class called `PlayerCharacter`.

**Bad**

* `player_score`
* `player_kills`
* `my_target_player`
* `my_character_name`
* `character_skills`
* `chosen_character_skin`

All of these variables are named redundantly. It is implied that the variable is representative of the `PlayerCharacter` it belongs to because it is `PlayerCharacter` that is defining these variables.

**Good**

* `score`
* `kills`
* `target_player`
* `name`
* `skills`
* `skin`

#### Typed Variables
ALL variables without exception should be typed. This should be done using one of the appropriate following methods.
Variables with initial values should have a single space on either side of `:=` and use `as` to type the variable when not representing an int, float, string, or bool.
Float variables should always be defined with a 0 when not being initialized with a decimal value.

```
var my_variable: Node
var my_float := 1.0
var my_boolean := false
@onready var my_onready_node := $OnreadyNode as Node
```

<a name="privatevariables"></a>
##### Private Variables
Private variables should have a prefix with an underscore, such as `_my_variable`.

##### Do _Not_ use Hungarian notation
Do _not_ use Hungarian notation or any other type identification in identifiers
```
# Good
var counter: int;
var name: string;
 
# Bad
var iCounter: int;
var strName: string;
```

##### Variable Slide and Value Ranges
All exported variables should make use of slide and value ranges if there is ever a value that a variable should _not_ be set to.

Example: A script that generates fence posts might have an editable variable named `posts_count` and a value of -1 would not make any sense. Use the range fields `@export_range(min, max, increment)` to mark 0 as a minimum.

If an editable variable is used in a Construction Script, it should have a reasonable Slider Range defined so that someone can not accidentally assign it a large value that could crash the editor.

#### Variable Types

##### Booleans

###### Boolean Prefix
All booleans should be named in PascalCase but prefixed with a verb.

Example: Use `isDead` and `hasItem`, **not** `Dead` and `Item`.

###### Boolean Names
All booleans should be named as descriptive adjectives when possible if representing general information.

Try to not use verbs such as `isRunning`. Verbs tend to lead to complex states.

###### Boolean Complex States
Do not use booleans to represent complex and/or dependent states. This makes state adding and removing complex and no longer easily readable. Use an enumeration instead.

Example: When defining a weapon, do **not** use `isReloading` and `isEquipping` if a weapon can't be both reloading and equipping. Define an enumeration named `WeaponState` and use a variable with this type named `WeaponState` instead. This makes it far easier to add new states to weapons.

##### Enums
Enums use PascalCase and use UPPERCASE singular names for enums and their values.

Example: 
```
enum WeaponType
{
    KNIFE,
    GUN
}

var weapon: WeaponType
```

###### Global Enums
Enums defined in global space are given their own file. The enum uses the file's class name rather than its own, and can only contain 1 type of enum.

Example:
```
class_name Fruit

enum {
    APPLE,
    ORANGE,
    PEAR
}
```

##### Arrays
Arrays follow the same naming rules as above, but should be named as a plural noun.

Example: Use `targets`, `hats`, and `enemy_players`, not `target_list`, `hat_array`, `enemy_player_array`.

<a name="functions"></a>
### 3.4 Functions, Events, and Event Dispatchers
This section describes how you should author functions, events, and event dispatchers. Everything that applies to functions also applies to events, unless otherwise noted.

#### Function Naming
The naming of functions, events, and event dispatchers is critically important. Based on the name alone, certain assumptions can be made about functions. For example:

* Is it a pure function?
* Is it fetching state information?
* Is it a handler?
* What is its purpose?

These questions and more can all be answered when functions are named appropriately.

<a name="function-verbrule"></a>
#### All Functions Should Be Verbs
All functions and events perform some form of action, whether its getting info, calculating data, or causing something to explode. Therefore, all functions should start with verbs. They should be worded in the present tense whenever possible. They should also have some context as to what they are doing.

Good examples:

* `fire` - Good example if in a Character / Weapon class, as it has context. Bad if in a Barrel / Grass / any ambiguous class.
* `jump` - Good example if in a Character class, otherwise, needs context.
* `explode`
* `receive_message`
* `sort_player_array`
* `get_coordinates`
* `update_transforms`
* `is_enemy` - ["Is" is a verb.](http://writingexplained.org/is-is-a-verb)

Bad examples:

* `dead` - Is Dead? Will deaden?
* `rock`
* `process_data` - Ambiguous, these words mean nothing.
* `player_state` - Nouns are ambiguous.
* `color` - Verb with no context, or ambiguous noun.

#### Functions Returning Bool Should Ask Questions
When writing a function that does not change the state of or modify any object and is purely for getting information, state, or computing a yes/no value, it should ask a question. This should also follow [the verb rule](#function-verbrule).

This is extremely important as if a question is not asked, it may be assumed that the function performs an action and is returning whether that action succeeded.

Good examples:

* `is_dead`
* `is_on_fire`
* `is_alive`
* `is_speaking`
* `has_weapon` - ["Has" is a verb.](http://grammar.yourdictionary.com/parts-of-speech/verbs/Helping-Verbs.html)
* `was_charging` - ["Was" is past-tense of "be".](http://grammar.yourdictionary.com/parts-of-speech/verbs/Helping-Verbs.html) Use "was" when referring to 'previous frame' or 'previous state'.
* `can_reload` - ["Can" is a verb.](http://grammar.yourdictionary.com/parts-of-speech/verbs/Helping-Verbs.html)

Bad examples:

* `fire` - Is on fire? Will fire? Do fire?
* `on_fire` - Can be confused with event dispatcher for firing.
* `dead` - Is dead? Will deaden?
* `visibility` - Is visible? Set visibility? A description of flying conditions?

#### Event Handlers and Dispatchers Should Start With `On`
Any function that handles an event or dispatches an event should start with `_on` and continue to follow [the verb rule](#function-verbrule).

Good examples:

* `_on_death` - Common collocation in games
* `_on_pickup`
* `_on_receive_message`
* `_onmessage_received`
* `_on_target_changed`
* `_on_click`
* `_on_leave`

Bad examples:

* `_on_data`
* `_on_target`

**[⬆ Back to Top](#table-of-contents)**
<a name="anc"></a>
<a name="4"></a>

## 4. Asset Naming Conventions
Naming conventions should be treated as law. A project that conforms to a naming convention is able to have its assets managed, searched, parsed, and maintained with incredible ease.

Most things are prefixed with the prefix generally being an acronym of the asset type followed by an underscore.

**Assets use [snake_case](#cases)**

<a name="base-asset-name"></a>
<a name="4.1"></a>
### 4.1 Base Asset Name - `Prefix_BaseAssetName_Variant_Suffix`
All assets should have a _Base Asset Name_. A Base Asset Name represents a logical grouping of related assets. Any asset that is part of this logical group 
should follow the the standard of  `prefix_baseAssetName_variant_suffix`.

Keeping the pattern `prefix_baseAssetName_variant_suffix` in mind and using common sense is generally enough to warrant good asset names. Here are some detailed rules regarding each element.

`prefix` and `suffix` are to be determined by the asset type through the following [Asset Name Modifier](#asset-name-modifiers) tables.

`baseAssetName` should be determined by short and easily recognizable name related to the context of this group of assets. For example, if you had a character named Bob, all of Bob's assets would have the `BaseAssetName` of `bob`.

For unique and specific variations of assets, `variant` is either a short and easily recognizable name that represents logical grouping of assets that are a subset of an asset's base name. For example, if Bob had multiple skins these skins should still use `bob` as the `baseAssetName` but include a recognizable `variant`. An 'Evil' skin would be referred to as `bob_evil` and a 'Retro' skin would be referred to as `bob_retro`.

For unique but generic variations of assets, `variant` is a two digit number starting at `01`. For example, if you have an environment artist generating nondescript rocks, they would be named `rock_01`, `rock_02`, `rock_03`, etc. Except for rare exceptions, you should never require a three digit variant number. If you have more than 100 assets, you should consider organizing them with different base names or using multiple variant names.

Depending on how your asset variants are made, you can chain together variant names. For example, if you are creating flooring assets for an Arch Viz project you should use the base name `flooring` with chained variants such as `flooring_marble_01`, `flooring_maple_01`, `flooring_tile_squares_01`.

<a name="1.1-examples"></a>
#### Examples

##### Character

| Asset Type               | Asset Name   |
| ------------------------ | ------------ |
| Skeletal Mesh            | sk_bob       |
| Material                 | m_bob        |
| Texture (Diffuse/Albedo) | t_bob_d      |
| Texture (Normal)         | t_bob_n      |
| Texture (Evil Diffuse)   | t_bob_evil_d |

##### Prop

| Asset Type               | Asset Name   |
| ------------------------ | ------------ |
| Static Mesh (01)         | sm_rock_01   |
| Static Mesh (02)         | sm_rock_02   |
| Static Mesh (03)         | sm_rock_03   |
| Material                 | m_rock       |
| Material Instance (Snow) | mi_rock_snow |

<a name="asset-name-modifiers"></a>

<a name="asset-name-modifiers"></a>
### 4.2 Asset Name Modifiers

When naming an asset use the following table to help determine the prefix and suffix to use with an asset's [Base Asset Name](#base-asset-name).

> IMPORTANT: The following table is not all encompasing. Feel free to make suggestions as to possible prefixes to add.

#### Most Common Prefixes

| Asset Type              | Prefix     | Suffix     | Notes                            |
| ----------------------- | ---------- | ---------- | -------------------------------- |
| Level           	  |  *         |            | [Should be in a folder called Levels.](#levels) e.g. `Levels/A4_C17_Parking_Garage.unity` |
| Level (Persistent)      |            | _p         |                                  |
| Level (Audio)           |            | _audio     |                                  |
| Level (Lighting)        |            | _lighting  |                                  |
| Level (Geometry)        |            | _geo       |                                  |
| Level (Gameplay)        |            | _gameplay  |                                  |
| Trigger Area            |            | _trigger   |                                  |
| Material                | m_         |            |                                  |
| Static Mesh             | sm_        |            |                                  |
| Skeletal Mesh           | sk_        |            |                                  |
| Texture                 | t_         | _?         | See [Textures](#anc-textures)    |
| Visual Effects          | vfx_       |            |                                  |
| Particle System         | ps_        |            |                                  |
| Light                   | l_         |            |                                  |
| Characters    | ch_    |        |       |
| Vehicles      | vh_    |        |       |
| Weapons       | wp_    |        |       |
| Static Mesh   | sm_    |        |       |
| Skeletal Mesh | sk_    |        |       |
| Skeleton      | skel_  |        |       |
| Mesh          |        | _mesh_lod0* | Only use LOD suffix if model uses LOD's |
| Mesh Collider |        | _collider   |                                         |
| Animation Library | al_    |        |       |
| AI / NPC                | ai_        |  _npc          |   *Npc could be pawn of CH_ !AI_                          |
| Material          | m_     |        |       |
| Material Instance | mi_    |        |       |
| Texture                 | t_         |            |                                  |

<a name="anc-textures-packing"></a>

