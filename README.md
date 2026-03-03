# joy_twist

[![ROS](https://img.shields.io/badge/ROS-Melodic%20%7C%20Kinetic-blue?logo=ros)](http://wiki.ros.org/joy_twist)
[![Language](https://img.shields.io/badge/language-C%2B%2B-blue?logo=cplusplus)](src/joy_twist_node.cpp)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)
[![catkin](https://img.shields.io/badge/build-catkin-orange)](CMakeLists.txt)

> A ROS node that converts `sensor_msgs/Joy` messages into `geometry_msgs/Twist` commands — designed for ROV / ground robot teleoperation via joystick.

## How It Works

The node subscribes to `/joy` and re-maps joystick axes and buttons to a `Twist` message, which is published on `/cmd_vel`. A deadman switch (button 0) must be **released** for movement commands to pass through.

### Axis / Button Mapping

| Input | `Twist` field | Description |
|---|---|---|
| `axes[2]` | `linear.x` | Thrust (forward/back) |
| `axes[0]` | `linear.y` | Lateral |
| `axes[1]` | `linear.z` | Heave (up/down) |
| `axes[3]` | `angular.z` | Yaw |
| `buttons[3]` | `angular.y = +0.5` | Roll positive |
| `buttons[4]` | `angular.y = −0.5` | Roll negative |
| `buttons[0]` | deadman (active LOW) | Block all output when pressed |

## Topics

| Topic | Type | Direction |
|---|---|---|
| `/joy` | `sensor_msgs/Joy` | Subscribed |
| `/cmd_vel` | `geometry_msgs/Twist` | Published |

## Dependencies

- [ROS](http://www.ros.org/) (Kinetic / Melodic)
- [`joy`](http://wiki.ros.org/joy) — joystick driver package
- `roscpp`, `geometry_msgs`, `sensor_msgs`

## Build & Install

```bash
# Clone into your catkin workspace
cd ~/catkin_ws/src
git clone https://github.com/JuliusPinsker/joy_twist.git

# Build
cd ~/catkin_ws
catkin_make
source devel/setup.bash
```

## Usage

Launch both the joystick driver and the joy_twist node together:

```bash
roslaunch joy_twist joy_twist.launch
```

Or run the node directly:

```bash
rosrun joy_twist joy_twist_node
```

> Make sure your joystick device is accessible (default: `/dev/input/js0`). You can set the device path via the `joy_node` parameter `dev` if needed.

## License

MIT — see [LICENSE](LICENSE).
