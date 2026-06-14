# Progress post Weeks-7-10

### Overview
This is my second progress post on Worldbuilding Simulator. As a brief overview: this project focuses on allowing users to work from planet-scale to city-scale and use procedural simulation to create maps. I'm working with Zoe Hammer, Jacob Fifield and Ken Cage. My role has primarily been creating the UI elements, and recently I have been working on the shader for the globe.

### Radius selection menu
<img width="337" height="301" alt="image" src="https://github.com/user-attachments/assets/b8816194-0248-4a85-a739-89cfaca3fa54" />  

As of the last progress post, the radius selection menu was drawing on the screen but didn't have much functionality. Now, you can open the selector with 'z' for radius and 'x' for intensity and drag with the mouse to change the parameter. Releasing the key closes the selector and emits a signal with the new radius/intensity. The outer circle represents the brush radius, and the inner circle represents the intensity or where the brush will start to taper off. The intensity cannot exceed the radius, and dragging the the radius below zero will just go with the absolute value. In retrospect, I think it might have been better to make the radius stop at 0 since I could see some users wanting it to stop there instead of flip so that they could reach small values. I'm not sure though, that might be something to get feedback on from users.

### Vertex shader
<img width="1553" height="891" alt="image" src="https://github.com/user-attachments/assets/a9f5cb99-f763-4350-b702-4e5e8b1c0aa0" />  

After finishing the radius selection I started working on this vertex shader, which I think was the most fun part of the project so far. This was the first step in the larger task of combining the globe view and the whiteboard view into one main scene. I had previously made a few basic fragment shaders in godot's GDShader code but hadn't done any 3D vertex shaders or used the visual shader editor. In non-technical terms: fragment shaders manipulate the way pixels look through code (like changing color), and vertex shaders manipulate the points in 3D space that make up a mesh (like changing shape). The visual shader editor abstracts the code out into nodes that you can drag around and visually wire the inputs to the outputs. I found [this tutorial by Bonkahe](https://www.youtube.com/watch?v=uDzimK9L5y4) very helpful for setting it up and used [free textures from Poly Haven](https://polyhaven.com/a/rocky_terrain_03) to test. I got it to where it would extrude the vertices based on where there was red values in the displacement texture, without the shader this sphere is smooth and the rocks are all flat. 

### Main scene
<img width="1911" height="1037" alt="image" src="https://github.com/user-attachments/assets/c26fb153-e4fa-4c48-9480-55ec8828dd10" />  

<img width="1916" height="1037" alt="image" src="https://github.com/user-attachments/assets/07bc1ffb-9eed-417d-b59c-a43f4f06b581" />

The next step was applying the shader to the globe in the main scene and pulling the displacement texture from the whiteboard and placing it all in the main scene. This part combined code from everyone, Jacob and Zoe had been working on the whiteboard and polygon system, Ken made the globe view, and I made the UI scene. I started by just placing the two subviewports side by side in the main scene but ended up putting one view at a time on the whole screen with a toggle at the top. The challenge was getting the displacement texture from the whiteboard to the globe. The subviewport in the whiteboard scene has a `get_texture()` method, I sent the subviewport's texture through the event bus and the globe scene receives it, updating the texture. I don't think this is the best way to update the globe in the long run. Eventually we want to sample the whiteboard view with a different level of detail depending on the current scale.


### Challenges
The main challenge this time around was combining the multiple views into one main scene. The whole time I had been working in GDScript with the UI, I found it much easier to connect signals and change UI elements in GDScript. It seems like all the while everyone else had been working mostly in C#, so connecting the two was a little challenging. I feel like collaborating on a project in a game engine is harder in some ways than other more common types projects. Keeping track of dependencies is confusing when there are different types of components that may have some unseen interactions with the engine. Our approach so far has been to each keep our own directories with our own scenes on our own branches, but eventually when they have to come together you need to figure out where the dependencies are.

### Lessons learned
Throughout this project I learned about planning the architecture for tools like Worldbuilding Simulator. I'm not entirely sure if Godot was the best choice for it? I don't think we had anywhere near enough time to fully accomplish all the tasks we set out to accomplish, this project is definitely a lot larger in scope than we thought. I also think that the way I see the overall structure of the project has changed drastically from what I thought it was when we were filling out the design document.
