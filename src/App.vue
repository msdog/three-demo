<template>
  <div id="app"></div>
</template>

<script>
import * as THREE from "three";
import CameraControls from "camera-controls";
CameraControls.install({ THREE });
export default {
  name: "App",
  mounted() {
    const { innerWidth, innerHeight } = window;
    const renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
    renderer.setPixelRatio(devicePixelRatio);
    renderer.setSize(innerWidth, innerHeight);
    this.$el.appendChild(renderer.domElement);
    console.log(this.$el);

    renderer.sortObjects = false;
    const scene = new THREE.Scene();

    const camera = new THREE.PerspectiveCamera(
      80,
      innerWidth / innerHeight,
      0.001,
      3000
    );
    camera.position.set(10, 10, 10);
    let mesh = new THREE.Mesh(
      new THREE.Geometry(),
      new THREE.MeshBasicMaterial({ color: 0x99cc00 })
    );
    console.log(mesh);
    mesh.position.set(1, 1, 1);
    scene.add(mesh);
    scene.add(new THREE.Mesh(new THREE.BoxGeometry(1, 1, 1)));

    mesh.geometry.groups.push(...this.createMesh());

    const controls = new CameraControls(camera, renderer.domElement);
    const clock = new THREE.Clock();
    const anim = () => {
      const delta = clock.getDelta();
      const hasControlsUpdated = controls.update(delta);
      requestAnimationFrame(anim);
      if (hasControlsUpdated) {
        renderer.render(scene, camera);
      }
    };

    anim();
    function render() {
      requestAnimationFrame(render);
      renderer.render(scene, camera);
    }
    render();
    window.addEventListener("resize", () => {
      camera.aspect = innerWidth / innerHeight;
      camera.updateProjectionMatrix();
      renderer.setSize(innerWidth, innerHeight);
    });
  },
  methods: {
    createMesh() {
      let arr = [];
      for (let index = 0; index < 2; index++) {
        arr.push(new THREE.BoxGeometry(1, 1.1));
      }
      return arr;
    },
  },
};
</script>

<style>
#app {
  height: 100vh;
  width: 100vw;
  background: #ccc;
}
* {
  padding: 0;
  margin: 0;
}
</style>
