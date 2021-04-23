# Scripting in Unity

This is a tiny text that I'm writing as a document which I can
consult whenever I forget something related to scripting in Unity.
Its purpose is purely for looking up a reference for some specific
thing, not for learning how to do scripting in Unity, there are other
materials for that.

## Basic script structure

Every script in Unity is basically just a class that inherits from
`MonoBehaviour`. Otherwise, it's just a normal C# function. That means
there is no special syntax to follow.
The only exception are methods that actually interact with the engine
and the entities the scripts(components) are attached to.
The most basic of those are outlined below.

```cs
using UnityEngine;
using System.Collections;

public class Script : MonoBehaviour {
    void Awake() {
        // This is called when the entity is created,
        // even if the component is disabled.
    }

    void Start() {
        // This function is called when the object this is attached to enters a scene
        // and the component is enabled. It's called after Awake(),
        // right before the first Update().

        // This is a function for logging inside the unity editor
        Debug.Log("Some output ...");
    }

    void Update() {
        // This function is called every frame.
        // It's not called on fixed intervals(frame times may differ).
    }

    void FixedUpdate() {
        // Called every physics step.
        // Intervals between calls are consistent.
    }
}
```

## MonoBehaviour scripting wizard

In Visual Studio, there is a special wizard
to help with script creation.
It's invoked by pressing `Ctrl+Shift+M`.
Methods created by the wizard will be placed
where the cursor is pointing.

## Enabling/Disabling Components

A component, which has a reference in the script
can be enabled or disabled through the `enabled`
flag.

Basic example:
```cs
private SomeComponent component;

void Update() {
    if(someCondition) component.enabled = !component.enabled;
}

```

## Activating GameObjects

A `GameObject` can be activated or deactivated using the `SetActive()`
method. Its state can be checked via the `activeSelf` and `activeInHierarchy`
attributes(getters).

## Translations, Rotations, LookAt

Moving and rotating `GameObjects` in space is done through translations and rotations.
Every `GameObject` has a transform, which has the `Translate` and `Rotate` methods.
`Translate` takes one argument, which is a `Vector3`. Rotate takes two arguments,
the axis of rotation and the angle by which it the object should be rotated.

Next, we also have the `LookAt` method.
It's used to point a `GameObject` at some transform.
`LookAt` takes a single parameter, which is the `Transform`
that it should point to.

Example:
```cs
using UnityEngine;
using System.Collections;

public class Transforms : MonoBehaviour
{
    public float moveSpeed = 10f;
    public float turnSpeed = 50f;
    
    
    void Update ()
    {
        if(Input.GetKey(KeyCode.UpArrow))
            transform.Translate(Vector3.forward * moveSpeed * Time.deltaTime);
        
        if(Input.GetKey(KeyCode.DownArrow))
            transform.Translate(-Vector3.forward * moveSpeed * Time.deltaTime);
        
        if(Input.GetKey(KeyCode.LeftArrow))
            transform.Rotate(Vector3.up, -turnSpeed * Time.deltaTime);
        
        if(Input.GetKey(KeyCode.RightArrow))
            transform.Rotate(Vector3.up, turnSpeed * Time.deltaTime);
    }
}
```

## Linear Interpolation

Unity contains a function called `Mathf.Lerp()` which takes
three parameters. The first parameter is the starting point of the interpolation.
The second is the end point of the interpolation and the third one
specifies where exactly between the two points we want to be.

It's also present as `Vector3.Lerp()`, which takes two `Vector3` parameters
and a float, which again, specified the position between the two points
provided. This is useful for linear interpolation in 3D space.

## Destroy

`Destroy` is a method that can be used for removing `GameObject`s, while
the game is running. For example, in the `Update` method we can write:

```cs
if (Input.GetKey(KeyCode.Space) Destroy(gameObject);
```

## GetKey and GetButton

Unity provides ways to read user input from the keyboard through
`GetKeyDown`, `GetKey` and `GetKeyUp` methods of the `Input` class.
Each of these takes one parameter, which is the key code of the key
we want to register, contained in the `KeyCode` enum(?).

It's also possible to use the `GetButton` equivalents of these previous
methods, where instead of the `KeyCode` a custom name of a key or a button
is provided as a parameter. 
These custom names can be set in the `Edit > Project Settings > Input` menu.

## OnMouseDown

`OnMouseDown` is one of the methods that the class in a script can implement.
It takes no parameters and doesn't return anything.
It iscalled, when and object is clicked on with the mouse at runtime.

## GetComponent

It is possible to get and modify an instance of one script with another script,
even from another `GameObject`. As scripts are just components, they can be accessed
with the `GetComponent` method of a `GameObject`.
To get an instance of a script(component) from a `GameObject`, `GetComponent`
is called with a type specifier(of the class in the script).

## DeltaTime

It's useful to have access to the time that has elapsed between the last `Update`
call and the current `Update` call. That is possible with the `Time.deltaTime`
attribute.

## Instantiate

It's used to clone instances of `GameObject`s.
It can be used in many ways. In the simplest form, it just takes a Rigidbody and creates a new one.

## Invoke

`Invoke` is used to call a function with a time delay.
It takes two parameters, first the function name and second the delay
with which the function should be called.

`InvokeRepeating` takes a third parameter, which specifies the delay between each call
after the first.

Note: Only functions that take no parameters and return void can be called by `Invoke`.


