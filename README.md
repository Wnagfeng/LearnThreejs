# Three.JS学习笔记

### 1.创建几何体的方式总结

#### 1. 使用预定义几何体类

- **简介**：

  - 这是一种简单便捷的方式，通过使用 Three.js 提供的预定义几何体类（如 `SphereGeometry`、`BoxGeometry`、`ConeGeometry` 等）来创建常见形状的几何体。

- **优点**：

  - **简单易用**：可以快速创建几何体而无需详细设置顶点和面。
  - **参数化**：允许通过参数控制几何体的大小、分段等属性。

- **示例**：

  ```javascript
  const coneGeometry = new THREE.ConeGeometry(5, 20, 32); // 创建一个锥体
  ```

  DiffCopyInsert

#### 2. 使用 BufferGeometry

- **简介**：

  - `BufferGeometry` 是一种更底层的几何体表示方式，允许开发者灵活定义顶点、面、颜色、法线等属性，适合复杂的几何体和自定义形状。

- **优点**：

  - **性能优化**：`BufferGeometry` 更加高效，特别是在需要渲染大量几何体时，可以减少内存占用和提高渲染速度。
  - **灵活性**：开发者可以完全控制顶点的结构，自定义几何体的各个方面（顶点坐标、颜色、UV 坐标等）。

- **使用步骤**：

  1. 创建 `BufferGeometry` 对象。
  2. 设置顶点数据。
  3. （可选）设置颜色、法线等其他属性。
  4. 创建材质。
  5. 创建物体并将其加入场景。

- **示例**：

  ```javascript
  const geometry = new THREE.BufferGeometry();
  const vertices = new Float32Array([...]); // 自定义顶点数据
  geometry.setAttribute('position', new THREE.BufferAttribute(vertices, 3)); // 设置顶点属性
  ```

#### 3. 自定义几何体

- **简介**：
  - 利用 `BufferGeometry` 和自定义顶点数组，开发者可以创建完全符合需求的几何体，包括复杂形状和定制的外观。
- **优点**：
  - **极大的灵活性和控制能力**：可以精确地通过代码设置每个顶点和面的属性，适合需要高度定制的三维模型。
- **适用场景**：
  - 在需要实现独特形状和细节的时候，例如编写基于数学方程的几何体或从数据生成模型时。

### 总结

- **选择预定义几何体**适合快速开发和常规使用对象。
- **选择 BufferGeometry**适合性能要求高且需要灵活性的场景，能够实现自定义的几何体结构。
- 对于特殊的几何体需求，自定义几何体可以全面满足开发者的设计需求。

根据项目需求选择合适的几何体创建方式，可以大大提高开发效率，提升三维场景的表现效果。

### 2.不同材质总结

#### 1. MeshBasicMaterial

- **描述**：

  - `MeshBasicMaterial` 是最基本的材质之一，不受场景中光照的影响。
  - 适用于需要简单颜色或纹理显示的对象，不需要光照效果的情况。

- **特性**：

  - 不计算光照：该材质的任何颜色（或贴图）都会以原始形式渲染，不会随光源的变化而变化。
  - 性能良好：因其不进行光照计算，通常比其他材质渲染速度更快。
  - 适用场景：适合在场景中不需要真实反射或阴影的对象，例如界面元素、背景纹理或实时分辨率对性能有要求的模型。

- **示例代码**：

  ```javascript
  const material = new THREE.MeshBasicMaterial({
      map: texture, // 设置贴图
  });
  ```

#### 2. MeshStandardMaterial

- **描述**：

  - `MeshStandardMaterial` 是一种基于物理的材质，能够与光照系统交互，提供更真实的光照效果。
  - 适用于需要模拟现实世界材料（例如金属、塑料）反射性的对象。

- **特性**：

  - 受光照影响：能够根据环境光和方向光动态调整其外观，适应不同的光照条件。

  - Roughness 和 Metalness：可以通过

     

    ```
    roughness
    ```

     

    和

     

    ```
    metalness
    ```

     

    属性控制光照效果：

    - `roughness`：控制表面的粗糙度（0 表示光滑，1 表示粗糙）。
    - `metalness`：决定材料的金属特性（0 表示非金属，1 表示完全金属）。

  - 适用场景：适合要求光照效果的对象，诸如建筑、交通工具和各种真实物体。

- **示例代码**：

  ```javascript
  const material = new THREE.MeshStandardMaterial({
      map: texture,        // 设置贴图
      color: 0xffffff,     // 设置颜色
      roughness: 0.3,      // 设置粗糙度
      metalness: 0.5       // 设置金属度
  });
  ```

### 总结

- 选择建议

  ：

  - 使用 `MeshBasicMaterial` 时，当对象无需考虑光照影响时，适用于简单的视觉效果。
  - 使用 `MeshStandardMaterial` 时，当需要真实性、光照交互性和材质表现细腻时，更适合复合材质效果。

- 注意事项

  ：

  - 在使用 `MeshStandardMaterial` 时，确保场景中有适当的光源设置（如环境光和方向光），以便能够体验到不同的光照效果。

通过正确选择和应用不同的材质，可以极大地提高三维渲染场景的真实感和视觉吸引力。

### 3.贴图笔记

#### 1. 贴图的定义

贴图（Texture）是一种将图像应用于3D对象表面的技术，用以增加物体的视觉细节和真实感。通过贴图，我们可以模拟表面的细微特征，比如颜色、纹理、光泽、透明度、反射等。

#### 2. 贴图的类型

- **颜色贴图（Diffuse Map）**：
  - 主要用于定义物体表面的基本颜色。
  - 颜色贴图常用 `.jpg`、`.png` 格式。
- **高光贴图（Specular Map）**：
  - 用于定义物体表面的高光反射度，通常是灰度图。
  - 黑色区域表示低反射率，白色区域表示高反射率。
- **环境光遮蔽贴图（AO Map）**：
  - 增强光影效果，模拟物体在光照不足的地方的阴影。
  - 通常是灰度图，用于确定表面的阴影深度。
- **透明度贴图（Alpha Map）**：
  - 定义表面的透明度，常用于制作需半透明效果的物体（如窗户、叶子）。
  - 通常是灰度图，其中白色表示完全可见，黑色表示完全透明。
- **光照贴图（Light Map）**：
  - 用于确定物体表面在特定光源下的光照效果，增加光照细节。
  - 光照贴图通常在光照技术中使用，尤其是在静态场景中。
- **法线贴图（Normal Map）**：
  - 通过改变法线的方向来模拟表面的细微凹凸效果，而不需要增加多边形的数量。
  - 使用法线贴图可以给物体表面添加更多细节，使其看起来更复杂。

#### 3. 贴图的应用

- 在Three.js中，贴图可以通过`THREE.TextureLoader`加载，并将其应用到材质上。

- 贴图的不同类型可以结合使用，营造出更丰富的效果，例如：

  ```javascript
  const material = new THREE.MeshStandardMaterial({
      map: textureLoader.load('color.jpg'),   // 颜色贴图
      specularMap: textureLoader.load('specular.jpg'), // 高光贴图
      aoMap: textureLoader.load('ao.jpg'),     // AO贴图
      alphaMap: textureLoader.load('alpha.png'),   // 透明度贴图
  });
  ```

#### 4. 颜色空间

- **sRGB颜色空间**：用于处理颜色贴图，以确保颜色真实。
- 在使用纹理时，设置正确的颜色空间（`THREE.SRGBColorSpace`）可以提高渲染效果。

#### 5. 贴图的性能考虑

- 贴图的大小和分辨率会影响性能和渲染速度，因此应考虑使用合适的贴图尺寸。
- 使用Mipmap（多级渐远纹理）可以进一步提高性能和视觉质量。

### 总结

贴图是3D图形中的重要元素，极大地增加了物体的视觉效果和真实感。理解不同类型的贴图及其用途，以及如何在Three.js中正确加载和应用它们，是实现高质量3D渲染的关键。

### 4.Raycaster使用

```
本文将讲解 Raycaster 的作用 Raycaster 是 Three.js 中用来进行射线检测的工具。它可以通过摄像机和鼠标的当前位置发射一条射线，检测这条射线是否与场景中的物体相交。
```

```
用人话说就是 我们在开发3D网页的时候他所有的东西都是绘制在Canvas上，这时候我们如果想实现点击某个东西实现对应的操作我们不能向以前那样获取dom 给dom添加事件，然后处理事件，我们现在需要通过Raycaster来实现类似于dom点击事件的效果,这种点击效果的实现他需要你做一个转换，就是我们给dom添加事件我们获取的是屏幕的xy，我们在threejs中元素是绘制在xy轴上的，我们最终需要拿到一个xy坐标然后才能判断元素有没有被点击到。
```

这个公式的作用是将鼠标在屏幕中的二维位置转换成 Three.js 的标准化设备坐标系（NDC, Normalized Device Coordinates）。下面是详细的解释：

#### 1. **屏幕坐标系**：

- 当你在网页上点击时，鼠标位置的坐标是相对于整个屏幕的，`event.clientX` 和 `event.clientY` 分别表示点击事件在浏览器窗口的 X 和 Y 方向上的像素位置。
- 这些值的范围是从 `(0, 0)` 到 `(window.innerWidth, window.innerHeight)`，也就是说，左上角是 `(0, 0)`，右下角是 `(window.innerWidth, window.innerHeight)`。

#### 2. **Three.js 的标准化设备坐标系（NDC）**：

- Three.js 使用的坐标系统要求鼠标的位置在 [-1, 1] 的范围内，而不是屏幕像素值。这个坐标系叫做 NDC。
  - 在 NDC 中：
    - `x = -1` 是屏幕的最左边，`x = 1` 是最右边；
    - `y = 1` 是屏幕的顶部，`y = -1` 是屏幕的底部。

#### 3. **公式的推导**：

- **X 方向**：

  - 通过 

    ```
    event.clientX / window.innerWidth
    ```

    ，你将鼠标在屏幕上的横向位置转化为 [0, 1] 之间的值。

    - 例如，`event.clientX` 为 0 时，表示鼠标在屏幕最左边，对应的值为 0。
    - `event.clientX` 为 `window.innerWidth` 时，表示鼠标在屏幕最右边，对应的值为 1。

  - 但是，NDC 要求 X 方向的范围是 [-1, 1]，所以你需要将 [0, 1] 映射到 [-1, 1]。这就通过公式 

    ```
    (event.clientX / window.innerWidth) * 2 - 1
    ```

     来实现：

    - 当 `event.clientX = 0`，计算结果为 `-1`（屏幕最左边）；
    - 当 `event.clientX = window.innerWidth`，计算结果为 `1`（屏幕最右边）。

- **Y 方向**：

  - Three.js 中的 Y 轴方向与屏幕的 Y 轴方向是相反的。屏幕的 Y 轴是从上到下递增，而 Three.js 的 Y 轴是从下到上递增。

  - 因此，`event.clientY / window.innerHeight` 将屏幕上的纵向位置映射到 [0, 1]，但我们需要将它映射到 [-1, 1]，并且 Y 轴还要反向，所以需要加负号。

  - 这就通过公式 

    ```
    -((event.clientY / window.innerHeight) * 2 - 1)
    ```

     实现：

    - 当 `event.clientY = 0`，计算结果为 `1`（屏幕顶部）；
    - 当 `event.clientY = window.innerHeight`，计算结果为 `-1`（屏幕底部）。

#### 4. **总结公式**：

```
javascript复制代码mouse.x = (event.clientX / window.innerWidth) * 2 - 1;
mouse.y = -((event.clientY / window.innerHeight) * 2 - 1);
```

- `mouse.x`：将屏幕的 X 坐标映射到 [-1, 1]。
- `mouse.y`：将屏幕的 Y 坐标映射到 [-1, 1]，并且反向。

这个公式是将鼠标的像素坐标转换成 NDC，方便使用 Three.js 中的射线投射（Raycasting）等功能，以判断鼠标点击了哪个 3D 对象。

