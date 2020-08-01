## Nothing especially dramatic. Just trying my hands at the Three JS library. 

I plan to get started with this before checking out WebGL. I have a little bit of experience with 3d, so I understand how shaders, renders, cameras, scenes (and lighting) work enough not to get too lost. 

I suspect the real complexity would be understanding axes from a purely math perspective without the visual axes that come with 3d apps.

### While writing the code, I also added in-line comments mostly so I can do what I'm about to do: use the code in the readme as a sort of how-to: 

//1. Creating scene
        var scene = new THREE.Scene();


        //2. Create Perspective camera (which takes four variables namely: field of view (FOV), aspect ratio, near and fair planes)
        var camera = new THREE.PerspectiveCamera(
            75,
            window.innerWidth/window.innerHeight,
            10,
            1000
        )
        camera.position.set(-10,10,300); //zooms in and out. the smaller, the closer

        //3. Set up your render (could be a WebGL render, a CSS2D render, or an SVG renderer). WebGL is the most advanced.
        var renderer = new THREE.WebGLRenderer({antialias: true});
        renderer.setClearColor('#ffffff'); //background color of renderer
        renderer.setSize(window.innerWidth, window.innerHeight);

        //4. Append the renderer to the window DOM element
        document.body.appendChild(renderer.domElement);

        //On the display, now what you see is a black screen. It isn't responsive, but takes the shape of the viewport at the time of refreshing. Let's fix that: 

        window.addEventListener('resize', ()=>{
            renderer.setSize(window.innerWidth, window.innerHeight);
            //Also, adjust the aspect ratio with the resize
            camera.aspect = window.innerWidth/window.innerHeight;
            camera.updateProjectionMatrix(); //dunno what this means!
        })

        // 5. Notice the blackness? Remove it by doing this:

        
        //and now we have the scene and the camera rendered!

        //Note: when working with a 3d element, there are two things to consider: the physical shape/form, and the material. A glass cup (physical shape) has a glossy material for example. You define these two things in three JS. 

        var geometry = new THREE.BoxGeometry(1,1,1);
        var material = new THREE.MeshLambertMaterial({color: 0xFFCC00});
        // var mesh = new THREE.Mesh(geometry, material) //combine geometry + material into one mesh

       

        //To move items around in space, move the mesh
        
        

        

        //Now, you see an ugly sphere. Let there be light!
        var light = new THREE.AmbientLight(0xFFFEE0, 3, 100);
        light.position.set(10,0,25);
        scene.add(light);

        

        var spotlight = new THREE.PointLight( 0xFFFEE0, 100, 60 );
        spotlight.position.set( 5, 50, 2 );
        scene.add( spotlight );

        var loader = new THREE.GLTFLoader();

        //Add/import object

        const mainOBJ = './object/scene.gltf';


        loader.load( mainOBJ, function ( gltf ) {
            scene.add( gltf.scene );
            car = gltf.scene.children[0];
            animate();

        });
        // scene.add('./scene.gltf'); //add mesh to the scene

        function animate(){
            requestAnimationFrame(animate);
            car.rotation.z+=0.015;
            // car.scale.z+=0.1;
            // car.rotation.y+=0.01;
        }

        var render = function() {
            requestAnimationFrame(render);
            renderer.render(scene, camera); 

        }
        
        render();
        //render scene (should be usually at the bottom of everything)