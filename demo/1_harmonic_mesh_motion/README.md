## Harmonic Mesh Deformation ##

### 1. Problem statement

Consider a square domain with vertices (0, 0) -- (0, 1) -- (1, 1) -- (1, 0) as shown below. We also call this domain as reference domain and corresponding mesh as reference mesh.

* **Reference domain**:

![alt text](https://github.com/niravshah241/MDFEniCSx/blob/main/demo/1_harmonic_mesh_motion/mesh_data/domain.png)

* **Reference mesh and reference boundaries**: 

1, 5: Bottom boundaries ($\Gamma_1, \Gamma_5$)
9, 12: Top boundaries ($\Gamma_9, \Gamma_{12}$)
4, 10: Left boundaries ($\Gamma_4, \Gamma_{10}$)
6, 11: Right boundaries ($\Gamma_6, \Gamma_{11}$)

![alt text](https://github.com/niravshah241/MDFEniCSx/blob/main/demo/1_harmonic_mesh_motion/mesh_data/boundaries.png)

We define below mesh **displacements**:

$$\text{On } \Gamma_1 \cup \Gamma_5: \ (0., 0.2 * sin(2 \pi x))$$

$$\text{On } \Gamma_9 \cup \Gamma_{12}: \ (0., 0.1 * sin(2 \pi x))$$

$$\text{On } \Gamma_4 \cup \Gamma_{10} \cup \Gamma_6 \cup \Gamma_{11}: \ (0., 0.)$$

### 2. Implementation

The mesh file is given in ```mesh_data/mesh.py``` which stores the mesh in same directory. The harmonic mesh motion implementation is given in ```harmonic_mesh_motion.py```. We print first few mesh points to observe an important difference. When the code is run with ```mpiexec -n 1 python3 harmonic_mesh_motion.py```, following output is produced.

```
Mesh points before deformation
[[0.     0.     0.    ]
 [0.125  0.     0.    ]
 [0.     0.125  0.    ]
 [0.125  0.125  0.    ]
 [0.0625 0.     0.    ]
 [0.     0.0625 0.    ]
 [0.125  0.0625 0.    ]]
Mesh points after first deformation
[[0.         0.         0.        ]
 [0.125      0.14142136 0.        ]
 [0.         0.125      0.        ]
 [0.125      0.18971448 0.        ]
 [0.0625     0.07653669 0.        ]
 [0.         0.0625     0.        ]
 [0.125      0.15808655 0.        ]]
Mesh points after exit from first deformation context
[[0.     0.     0.    ]
 [0.125  0.     0.    ]
 [0.     0.125  0.    ]
 [0.125  0.125  0.    ]
 [0.0625 0.     0.    ]
 [0.     0.0625 0.    ]
 [0.125  0.0625 0.    ]]
Mesh points after second deformation
[[0.         0.         0.        ]
 [0.125      0.14142136 0.        ]
 [0.         0.125      0.        ]
 [0.125      0.18971448 0.        ]
 [0.0625     0.07653669 0.        ]
 [0.         0.0625     0.        ]
 [0.125      0.15808655 0.        ]]
Mesh points after exit from second deformation context
[[0.         0.         0.        ]
 [0.125      0.14142136 0.        ]
 [0.         0.125      0.        ]
 [0.125      0.18971448 0.        ]
 [0.0625     0.07653669 0.        ]
 [0.         0.0625     0.        ]
 [0.125      0.15808655 0.        ]]
```

As can be observed, after first mesh deformation, the mesh returns to the reference state after exit from the mesh deformation context. While, after the second mesh deformation, the mesh remains deformed after exit from the mesh deformation context. This difference can be explained by keyword argument ```reset_reference```.

When ```reset_reference=True```, the mesh returns to reference mesh configuration upon exit from the mesh deformation context. Instead, when ```reset_reference=False```, the mesh remains deformed and does not return to the reference mesh configuration upon exit from the mesh deformation context.