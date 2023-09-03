<template>
    <div id="three-js-container" ref="container"></div>
  </template>
  
  <script>
  import { ref, onMounted, onUnmounted } from 'vue';
  import * as THREE from 'three';
  import { FontLoader } from 'three/addons/loaders/FontLoader.js';
  import { TextGeometry } from 'three/addons/geometries/TextGeometry.js';
  
  export default {
    setup() {
      const scene = new THREE.Scene();
      const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
      const renderer = new THREE.WebGLRenderer();
      const shapes = [];
  
      const container = ref(null);
  
      onMounted(() => {
        // Set up renderer
        renderer.setSize(window.innerWidth, window.innerHeight);
        container.value.appendChild(renderer.domElement);
  
        // Set up camera position
        camera.position.z = 5;
  
        // Create text geometry
        const fontLoader = new FontLoader();
        fontLoader.load('https://cdn.rawgit.com/mrdoob/three.js/r124/examples/fonts/helvetiker_regular.typeface.json', function (font) {
          const textGeometry = new TextGeometry('Hello, Three.js!', {
            font: font,
            size: 0.5,
            height: 0.1,
          });
          const textMaterial = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
          const textMesh = new THREE.Mesh(textGeometry, textMaterial);
          textMesh.position.set(-2, 1, 0);
          scene.add(textMesh);
          shapes.push(textMesh);
        });
  
        // Create geometric figures
        const geometry1 = new THREE.BoxGeometry();
        const material1 = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
        const cube = new THREE.Mesh(geometry1, material1);
        cube.position.set(2, 0, 0);
        scene.add(cube);
        shapes.push(cube);
  
        const geometry2 = new THREE.SphereGeometry();
        const material2 = new THREE.MeshBasicMaterial({ color: 0xff0000 });
        const sphere = new THREE.Mesh(geometry2, material2);
        sphere.position.set(0, -2, 0);
        scene.add(sphere);
        shapes.push(sphere);
  
        const geometry3 = new THREE.CylinderGeometry();
        const material3 = new THREE.MeshBasicMaterial({ color: 0x0000ff });
        const cylinder = new THREE.Mesh(geometry3, material3);
        cylinder.position.set(-2, -2, 0);
        scene.add(cylinder);
        shapes.push(cylinder);
  
        // Create animation loop
        const animate = () => {
          requestAnimationFrame(animate);
  
          // Rotate shapes
          shapes.forEach((shape) => {
            // console.log(shape.position)
            shape.position.x += 0.01;
            shape.position.y += 0.01;
          });
  
          renderer.render(scene, camera);
        };
  
        animate();
      });
  
      onUnmounted(() => {
        // Clean up Three.js resources
        shapes.forEach((shape) => {
          shape.geometry.dispose();
          shape.material.dispose();
        });
        renderer.dispose();
      });
  
      return { container };
    },
  };
  </script>
  
  <style scoped>
  #three-js-container {
    position: absolute;
    top: 0;
    left: 0;
  }
  </style>
  