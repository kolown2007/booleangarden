<script lang="ts">
import { onMount } from 'svelte';
import { Engine, Scene, ArcRotateCamera, Vector3, HemisphericLight, MeshBuilder, Color4, ParticleSystem, Texture } from '@babylonjs/core';

let canvas: HTMLCanvasElement;

onMount(async () => {
    const { Inspector } = await import('@babylonjs/inspector');

    const engine = new Engine(canvas, true);
    const scene = new Scene(engine);
    scene.clearColor = new Color4(0, 0, 0, 1);

    const camera = new ArcRotateCamera("Camera", Math.PI / 2, Math.PI / 2, 2, Vector3.Zero(), scene);
    camera.attachControl(canvas, true);

    const light1 = new HemisphericLight("light1", new Vector3(1, 1, 0), scene);
    const sphere = MeshBuilder.CreateSphere("sphere", { diameter: 1 }, scene);

    // Create a particle system
    const particleSystem = new ParticleSystem("particles", 2000, scene);
    particleSystem.particleTexture = new Texture("textures/circle.png", scene); // Update texture to a simple circle
    particleSystem.emitter = sphere; // the starting object, the emitter
    particleSystem.minEmitBox = new Vector3(-1, 0, 0); // Starting all from
    particleSystem.maxEmitBox = new Vector3(1, 0, 0); // To...

    // Configure particle size
    particleSystem.minSize = 0.01;
    particleSystem.maxSize = 0.05;

    particleSystem.start();

    // 

    engine.runRenderLoop(() => {
        scene.render();
    });

    window.addEventListener("resize", () => {
        engine.resize();
        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;
    });

    // Hide/show the Inspector
    window.addEventListener("keydown", (ev) => {
        if (ev.shiftKey && ev.ctrlKey && ev.altKey && ev.key === 'I') {
            if (scene.debugLayer.isVisible()) {
                scene.debugLayer.hide();
            } else {
                scene.debugLayer.show();
            }
        }
    });
});
</script>


<svelte:head>
    <title>boolean_garden</title>
</svelte:head>

<canvas bind:this={canvas} style="width: 100%; height: 100%;"></canvas>




<style> 
canvas {
    display: block;
    width: 100%;
    height: 100%;
}
</style>