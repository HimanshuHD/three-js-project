<template>
  <div ref="canvasContainer"></div>
</template>

<script>
import { ref, onMounted } from 'vue';
import * as THREE from 'three';

export default {
  setup() {
    const canvasContainer = ref(null);

    onMounted(() => {
      // Create a Three.js scene, camera, and renderer
      const scene = new THREE.Scene();
      const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
      const renderer = new THREE.WebGLRenderer();

      renderer.setSize(window.innerWidth, window.innerHeight);
      canvasContainer.value.appendChild(renderer.domElement);

      // Create a cube and add it to the scene
      const geometry = new THREE.BoxGeometry();
      const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
      const cube = new THREE.Mesh(geometry, material);
      scene.add(cube);

      // Set the camera position
      camera.position.z = 5;

      // Animation loop
      const animate = () => {
        requestAnimationFrame(animate);

        // Rotate the cube
        cube.rotation.x += 0.01;
        cube.rotation.y += 0.01;

        renderer.render(scene, camera);
      };

      animate();
    });

    return { canvasContainer };
  },
};
</script>

<style>
/* Add any CSS styles here if needed */
</style>
