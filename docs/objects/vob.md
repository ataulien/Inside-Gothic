# Vob (Virtual Object)

Whenever an object goes into the world, it is based on the `Vob` class.
The `Vob` class adds the following important properties to an object:

- Transform (Position and Rotation, no Scale)
- Visual (Mesh, Particle System, Animated Mesh)
- Frame Updates
- Physics
- AI
- Event-Queue

Not all of these features are used on every object. They are only
initialized when required. For example, most decorative objects use a
Mesh visual and physics for collision, but no AI.
