# WebGL & 3D Reference — Awwwards Skill

## When to Use 3D

**DO use 3D for:**
- Hero visuals that reinforce brand identity
- Interactive data visualization
- Product showcases (3D models, configurators)
- Background atmospherics (particles, gradients)
- Creative agency portfolios

**DON'T use 3D for:**
- Standard SaaS dashboards
- Content-heavy sites (blogs, docs)
- When load time < 2s is critical
- Mobile-first apps with data-heavy UIs
- Just to "look cool" without concept

## Three.js + React Three Fiber

### Setup
```bash
npm install three @react-three/fiber @react-three/drei
```

### Basic Scene
```jsx
import { Canvas } from '@react-three/fiber';
import { OrbitControls, Environment } from '@react-three/drei';

function Scene() {
  return (
    <Canvas
      camera={{ position: [0, 0, 5], fov: 45 }}
      dpr={[1, 2]}  // limit pixel ratio for performance
      gl={{ antialias: true, alpha: true }}
    >
      <Environment preset="city" />
      <OrbitControls enableZoom={false} autoRotate autoRotateSpeed={0.5} />
      <mesh>
        <sphereGeometry args={[1, 64, 64]} />
        <meshStandardMaterial color="#4a90d9" roughness={0.2} metalness={0.8} />
      </mesh>
    </Canvas>
  );
}
```

### Scroll-Linked 3D
```jsx
import { useScroll } from '@react-three/drei';
import { useFrame } from '@react-three/fiber';

function ScrollModel() {
  const meshRef = useRef();
  const scroll = useScroll();

  useFrame(() => {
    const offset = scroll.offset; // 0 to 1
    meshRef.current.rotation.y = offset * Math.PI * 2;
    meshRef.current.position.y = -offset * 3;
  });

  return <mesh ref={meshRef}>...</mesh>;
}

// Wrap in ScrollControls
<Canvas>
  <ScrollControls pages={3}>
    <ScrollModel />
  </ScrollControls>
</Canvas>
```

## Shader Basics

### Gradient Sphere (Custom Shader)
```glsl
// vertex.glsl
varying vec2 vUv;
varying vec3 vNormal;

void main() {
  vUv = uv;
  vNormal = normalize(normalMatrix * normal);
  gl_Position = projectionMatrix * modelViewMatrix * vec4(position, 1.0);
}

// fragment.glsl
uniform float uTime;
varying vec2 vUv;
varying vec3 vNormal;

void main() {
  float fresnel = pow(1.0 - dot(vNormal, vec3(0.0, 0.0, 1.0)), 2.0);
  vec3 color1 = vec3(0.2, 0.4, 0.8);
  vec3 color2 = vec3(0.8, 0.2, 0.6);
  vec3 color = mix(color1, color2, fresnel + sin(uTime * 0.5) * 0.2);
  gl_FragColor = vec4(color, 1.0);
}
```

### Using Custom Shaders in R3F
```jsx
import { shaderMaterial } from '@react-three/drei';
import { extend, useFrame } from '@react-three/fiber';

const GradientMaterial = shaderMaterial(
  { uTime: 0 },
  vertexShader,
  fragmentShader
);
extend({ GradientMaterial });

function GradientSphere() {
  const matRef = useRef();
  useFrame((_, delta) => {
    matRef.current.uTime += delta;
  });

  return (
    <mesh>
      <sphereGeometry args={[1, 64, 64]} />
      <gradientMaterial ref={matRef} />
    </mesh>
  );
}
```

## Particle Systems

### Simple Floating Particles
```jsx
import { useMemo, useRef } from 'react';
import { useFrame } from '@react-three/fiber';
import * as THREE from 'three';

function Particles({ count = 500 }) {
  const meshRef = useRef();

  const positions = useMemo(() => {
    const pos = new Float32Array(count * 3);
    for (let i = 0; i < count * 3; i++) {
      pos[i] = (Math.random() - 0.5) * 10;
    }
    return pos;
  }, [count]);

  useFrame((_, delta) => {
    meshRef.current.rotation.y += delta * 0.05;
  });

  return (
    <points ref={meshRef}>
      <bufferGeometry>
        <bufferAttribute
          attach="attributes-position"
          count={count}
          array={positions}
          itemSize={3}
        />
      </bufferGeometry>
      <pointsMaterial size={0.02} color="#ffffff" transparent opacity={0.6} />
    </points>
  );
}
```

## Post-Processing

### Setup
```bash
npm install @react-three/postprocessing postprocessing
```

### Bloom + Vignette
```jsx
import { EffectComposer, Bloom, Vignette } from '@react-three/postprocessing';

<Canvas>
  <Scene />
  <EffectComposer>
    <Bloom
      intensity={0.5}
      luminanceThreshold={0.8}
      luminanceSmoothing={0.9}
    />
    <Vignette eskil={false} offset={0.1} darkness={0.5} />
  </EffectComposer>
</Canvas>
```

## Lightweight Alternatives

### Spline (No-Code 3D)
```jsx
import Spline from '@splinetool/react-spline';

function Hero() {
  return (
    <Spline scene="https://prod.spline.design/xxx/scene.splinecode" />
  );
}
```

### OGL (Lightweight WebGL)
- ~30KB vs Three.js ~600KB
- Good for simple effects: gradients, particles, noise
- No ecosystem/helpers — bare metal

### CSS 3D Transforms (No WebGL)
```css
.card-3d {
  perspective: 1000px;
}
.card-3d-inner {
  transform-style: preserve-3d;
  transition: transform 0.6s var(--ease-out);
}
.card-3d:hover .card-3d-inner {
  transform: rotateY(15deg) rotateX(5deg);
}
```

## Performance Optimization

### Key Rules
1. **Budget:** Max 100K triangles, 10 draw calls for hero scenes
2. **Textures:** Compress with KTX2/Basis, max 2048px, use mipmaps
3. **Instancing:** Use `<instancedMesh>` for repeated geometry (particles, trees)
4. **LOD:** Use `<Detailed>` from drei for Level of Detail
5. **Mobile:** Reduce DPR to 1, lower geometry detail, skip post-processing
6. **Lazy load:** Don't load 3D scene until visible
7. **Dispose:** Clean up geometries, materials, textures on unmount

### Performance Detection
```js
// Detect GPU tier and adjust quality
import { getGPUTier } from 'detect-gpu';

const gpuTier = await getGPUTier();

const quality = {
  1: { dpr: 1, particles: 100, postprocessing: false },
  2: { dpr: 1.5, particles: 500, postprocessing: false },
  3: { dpr: 2, particles: 2000, postprocessing: true },
}[gpuTier.tier] || { dpr: 1, particles: 100, postprocessing: false };
```

### Canvas as Background (Common Pattern)
```jsx
<div className="relative h-screen">
  {/* 3D Background */}
  <div className="absolute inset-0 -z-10">
    <Canvas>
      <Scene />
    </Canvas>
  </div>

  {/* HTML Content Over 3D */}
  <div className="relative z-10 flex items-center justify-center h-full">
    <h1>Content over 3D</h1>
  </div>
</div>
```
