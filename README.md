# MyCobot Pro 600 Maze Solver (MATLAB)

Team project (RAS) in which a myCobot arm draws a path through a maze from a photo.

## Pipeline (`gikmazefinal.m`)

1. Load a maze image, find the largest bright region and crop to it.
2. Mask out the red and green markers and build a safe-region mask that avoids walls and image edges.
3. Click a start and goal; each is snapped to the nearest safe pixel.
4. Find a path with Dijkstra on the safe mask, smooth it, and resample it to 20 points with `interparc`.
5. Convert image coordinates (14 cm square maze) to robot coordinates (mm) and add entry/exit moves.
6. Solve joint angles for each point with MATLAB `generalizedInverseKinematics` on `myCobot28.urdf`.
7. Send the angles to the robot over TCP as `set_angles(...)` commands (`test_tcp.m` is a minimal example).

## Files

- `gikmazefinal.m`: main script.
- `interparc.m`: curve interpolation by John D'Errico (MATLAB File Exchange), not original work; see its header for the license.
- `myCobot28.urdf`, `myCobot28*.slx`: robot model and Simulink models (digital twin, FK, IK).
- `maze*.jpg`, `new_maze.jpg`: test images.
- `*axiscobot.mat`: saved axis data.
- `RAS Final Project Team 12.zip`: full project folder including the `meshes/` STL files needed by the URDF.

## Before you run it

- Needs MATLAB with Image Processing, Robotics System, and Statistics/Signal toolboxes as used by the script (`regionprops`, `importrobot`, `smoothdata`).
- Unzip the project zip to get the `meshes/` folder, then change the `MeshPath` in `gikmazefinal.m`. It currently points to a local Windows path (`C:\Users\nitin\Downloads\New folder (7)\meshes`).
- Set `server_ip` and `server_port` to your robot's address. The IP in the script is a private LAN address.
- The image file name (`maze8.jpg`) is set at the top of the script.
