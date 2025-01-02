# MetaWorld Camera Wrapper

This repository provides a versatile camera wrapper for the MetaWorld environment, enabling dynamic camera control modes such as **shake**, **translation**, **zoom**, **rotation**, and **action-control**. The wrapper is designed to enhance the visual experience and adaptability of the MetaWorld tasks by allowing flexible camera configurations.

<div style="display: flex; justify-content: space-between; align-items: center;">
    <div style="text-align: center;">
        <img src="assests\metaworld-button-press-rotation-strong-expert.gif" alt="Image 1" style="width: 200px; height: auto;">
        <p>Rotation</p>
    </div>
    <div style="text-align: center;">
        <img src="assests\metaworld-button-press-shake-strong-expert.gif" alt="Image 2" style="width: 200px; height: auto;">
        <p>Shake</p>
    </div>
    <div style="text-align: center;">
        <img src="assests\metaworld-button-press-translation-strong-expert.gif" alt="Image 3" style="width: 200px; height: auto;">
        <p>Translation</p>
    </div>
    <div style="text-align: center;">
        <img src="assests\metaworld-button-press-zoom-strong-expert.gif" alt="Image 4" style="width: 200px; height: auto;">
        <p>Zoom</p>
    </div>
</div>

## Features

- **Five Camera Control Modes**:
  - **Shake**: Simulates camera shaking with varying intensity (weak, medium, strong).
  - **Translation**: Moves the camera along the lookat axis with smooth transitions.
  - **Zoom**: Adjusts the camera distance dynamically, creating a zoom-in/zoom-out effect.
  - **Rotation**: Rotates the camera around the azimuth axis.
  - **Action-Control**: Allows precise control of the camera parameters via sensory actions.

- **Customizable Camera Parameters**:
  - Distance, azimuth, elevation, and lookat can be adjusted within predefined ranges.
  - Supports weak, medium, and strong randomization for each mode.

- **Expert Trajectories**:
  - Pre-recorded expert trajectories for various MetaWorld tasks are stored in the `data` folder.
  - Trajectories are available for different camera modes, providing a rich dataset for training and analysis.

## Usage

### Installation

Ensure you have the required dependencies installed:

```bash
pip install gymnasium metaworld numpy matplotlib imageio
```

### Example Code

Here’s how to set up and use the camera wrapper in your MetaWorld environment:

```python
from metaworld_env_wrapper import setup_metaworld_env
import imageio

# Set up the environment with a specific camera mode
viewpoint_mode = 'controlled'  # Options: 'shake', 'translation', 'zoom', 'rotation', 'controlled'
viewpoint_randomization_type = 'strong'  # Options: 'weak', 'medium', 'strong'
env = setup_metaworld_env('button-press-v2-goal-observable', seed=10, size=64, viewpoint_mode=viewpoint_mode, viewpoint_randomization_type=viewpoint_randomization_type)

# Reset the environment
obs, _ = env.reset()

# Record a GIF of the environment with the selected camera mode
T = 100
with imageio.get_writer(uri=f'assets/metaworld-button-press-{viewpoint_mode}-{viewpoint_randomization_type}.gif', mode='I', fps=10) as writer:
    for i in range(T):
        _ = env.step(env.env.action_space.sample())
        img_array = env.render(sensory_action=np.random.uniform(-1, 1, size=6))
        writer.append_data(img_array)
```

### Camera Configuration

The camera configuration can be dynamically adjusted during rendering. The following parameters are available:

- **Distance**: Controls how far the camera is from the target.
- **Azimuth**: Rotates the camera around the target.
- **Elevation**: Adjusts the vertical angle of the camera.
- **Lookat**: Specifies the point the camera is focused on.

### Expert Trajectories

The `data` folder contains expert trajectories for various MetaWorld tasks, recorded under different camera modes. These trajectories can be used for imitation learning, benchmarking, or analysis.

| Task                   | Camera Mode      | Randomization Type | Success Rate | Episodic Return |
|:------------:|:-------------:|:-------------:|:-------------:|:-----------:|
| button-press  | rotation            | strong               | 0.40       |     6671.8     |
| button-press  | shake            | strong               | 0.40       |    6649.4      |
| button-press  |    translation         | strong               | 0.40       |    6671.8      |
| button-press  | zoom         | strong               | 0.40       |     6671.8     |
| door-close  | rotation            | strong               | 1.00       |     9547.2     |
| door-close  | shake            | strong               | 1.00       |    9554.9      |
| door-close  |    translation         | strong               | 1.00       |    9547.2      |
| door-close  | zoom         | strong               | 1.00       |      9547.2     |

## Future Works

- **Enhanced Randomization**: Implement more sophisticated randomization strategies for camera parameters.
- **Multi-Modal Trajectories**: Record trajectories under multiple camera modes simultaneously, providing richer data for analysis.
- **Support for More Environments**: Extend the wrapper to support more simulators.


## Contributing

Contributions are welcome! If you have suggestions for improvements or new features, feel free to open an issue or submit a pull request.

## License

This project is licensed under the MIT License. See the [LICENSE](./LICENSE) file for details.

---

This wrapper is designed to make MetaWorld tasks more visually dynamic and adaptable, providing researchers and developers with a powerful tool for experimentation and analysis. Enjoy exploring the MetaWorld with enhanced camera control!