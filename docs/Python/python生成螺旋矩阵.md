---
title: 生成螺旋矩阵
date: 2021-10-31 13:38:25
permalink: /pages/8b831c/
categories:
  - Python
tags:
  - 
---
## 螺旋矩阵生成器
[ 描述 ]  
``` 
按顺序生成如下位姿矩阵 
5 <- 4 <- 3
|        |
6 -> 1 -> 2
|
7 -> 8 -> 9 -> ...
```
[ 实现 ]
```py
def generate_n_points(initial_pose, step, loops=1):
    """

    :param initial_pose: translate + euler/quaternion
    :param step: number
    :param loops: 1-3x3, 2-5x5
    :return: {}
    """
    visited_mat = []

    def _move(cur_pose, _step, max_distance):
        if cur_pose not in visited_mat:
            visited_mat.append(cur_pose)
        cx, cy, cz = cur_pose[:3]
        # right -> left -> up -> bottom
        forwards = [(1, 0), (-1, 0), (0, 1), (0, -1)]
        for forward in forwards:
            next_x = cx + (_step * forward[0])
            next_y = cy + (_step * forward[1])
            next_pose = [next_x, next_y] + cur_pose[2:]
            if -max_distance <= next_x <= max_distance and \
                    -max_distance <= next_y <= max_distance and \
                    next_pose not in visited_mat:
                _move(next_pose, _step, max_distance)

    for loop in range(1, loops + 1):
        _initial_pose = visited_mat[-1] if len(visited_mat) else initial_pose
        _move(_initial_pose, step, step * loop)

    return visited_mat


ret = generate_n_points([0,0,0,0,0,0], 100, 3)
print(ret)
print(len(ret))

```