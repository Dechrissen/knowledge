# Intro to Godot

## Godot's key concepts

There are 4 main building blocks in Godot: scenes, nodes, scene tree, signals.

### Scenes

A collection of one or more nodes (organized into a tree).

### Nodes

The smallest building block of your game. There are many types of nodes that can come together to create more powerful ones.

### Scene tree

A collection of Scenes (also organized into a tree). It helps to think of your game as a collection of scenes (characters, weapons, doors, menus, etc.)

### Nodes

Nodes emit signals when some event occurs, and can communicate with other nodes. This gives you a lot of flexibility in structuring your game.

There are built-in signals that can tell you when some objects collided, when an enemy enters a certain room, etc.

## Godot's editor

The main window is the _Project Manager_. It's where you can open or make a new project. The _Asset Library Projects_ window is where you can search for community-created projects, templates, and demos.

When you open a project, the main area features the _viewport with toolbar_ in the center. The toolbar is where you find tools to move, scale, or lock the scene's nodes.

### Docks

Docks have various helpful tools: file system navigation, scene selector, node properties.

### The 4 main screens

- 2D screen: for 2D games and menus
- 3D screen: for 3D games
- Script: a complete code editor with debugger, auto-complete, and built-in code reference
- AssetLib: library of FOSS scripts, add-ons, and assets to use for any project

### How to use the built-in class reference

- Press `F1`
- "Search Help" button in top right
- Hold `Ctrl` while clicking a class, function, or built-in variable in the editor

Any of these will bring up a search window. Type something or browse here. If you click something, a page opens in the script main screen. (This is the Godot scripting API)

## Godot's design philosophy

Godot is object-oriented at its core. e.g. create a BlinkingLight scene, and a BrokenLantern scene that uses (inherits?) BlinkingLight. Then fill a City scene with a bunch of BrokenLanterns. Then, if you change the color property of BlinkingLight, all the BrokenLanterns will update instantly.

## Other resources

- [Godot docs](https://docs.godotengine.org/en)
- [Class reference](https://docs.godotengine.org/en/stable/classes/index.html#toc-class-ref)
- [Godot Q&A site](https://godotengine.org/qa/)
- [Godot tutorials](https://docs.godotengine.org/en/stable/community/tutorials.html#doc-community-tutorials)
