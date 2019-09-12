<template>
  <div>
    <canvas id="application"></canvas>
  </div>
</template>

<style>
</style>

<script>
  import pc from './playcanvas-stable.js';
  export default {
    data() {
      return({
        title: 'ビッグカツがくるくる回るだけのページ',
      });
    },
    mounted: function() {
      // create a PlayCanvas application
      const canvas = document.getElementById('application');
      const app = new pc.Application(canvas, { });
      app.start();

      // fill the available space at full resolution
      app.setCanvasFillMode(pc.FILLMODE_FILL_WINDOW);
      app.setCanvasResolution(pc.RESOLUTION_AUTO);

      // ensure canvas is resized when window changes size
      window.addEventListener('resize', function() {
        app.resizeCanvas();
      });

      // create box entity
      const cube = new pc.Entity('cube');
      cube.addComponent('model', {
        type: 'box'
      });

      // create camera entity
      const camera = new pc.Entity('camera');
      camera.addComponent('camera', {
        clearColor: new pc.Color(0.1, 0.1, 0.1)
      });

      // create directional light entity
      const light = new pc.Entity('light');
      light.addComponent('light');

      // add to hierarchy
      //app.root.addChild(cube);
      app.root.addChild(camera);
      app.root.addChild(light);

      // set up initial positions and orientations
      camera.setPosition(0, 30, 0);
      camera.setEulerAngles(-90, 180, 0);

      // 
      light.setPosition(0, 30, 0);
      light.setEulerAngles(0, 0, 0);

      // load model
      app.assets.loadFromUrl('./BigKatsu/BigKatsu.json', 'model', function(error, asset) {
        // Create the entity called apple
        const appleEntity = new pc.Entity('BigKatsu');
        appleEntity.addComponent('model');
        appleEntity.model.asset = asset;
        app.root.addChild(appleEntity);
      });

      // register a global update event
      app.on('update', function (deltaTime) {
        if(app.root.findByPath('BigKatsu')) {
          app.root.findByPath('BigKatsu').rotate(10 * deltaTime, 20 * deltaTime, 30 * deltaTime);
        }
      });
    },
  }
</script>