---
title: A Map of the Internet
layout: map-of-internet
permalink: /map-of-internet/
---



<div class="splash-container" id="container">
	<div class="splash" id="introduction">
		<h1 class="splash-head">Map of the Internet</h1>
		<p class="splash-subhead">
			You can find the controls to the visualisation in the top right corner. Hover over the about section and scroll down to see an overview of what and how things are represented on the map. 
		</p>
		<p>
			<a onclick="removeintro()" class="pure-button pure-button-primary">Explore!</a>
		</p>
	</div>
</div>

<div class="content-wrapper">
	<div class="content brightribbon">
		<h2 class="content-head is-center">vvv About Map & More vvv</h2>

		<div class="pure-g">
			<div class="l-box pure-u-1 pure-u-md-1-2 pure-u-lg-1-4">

				<h3 class="content-subhead">
					Points
				</h3>
				<p>
					Each point represents an Autonomous System - a network of addresses (i.e. an Internet Service Provider, a big company etc.). The colour of the point represents the network's relative importance, with pink being a busy central network that serves as a connecting point to a lot of other networks and cyan being relatively quiet networks with little to no information passing through (other than their own).
				</p>
			</div>
			<div class="l-box pure-u-1 pure-u-md-1-2 pure-u-lg-1-4">
				<h3 class="content-subhead">
					Lines
				</h3>
				<p>
					Each line represents a connection between two networks, if there is a line, that means that traffic may take that path along a longer journey. Individual lines are semi-transparent, hence the places where they are fully visible are busy junctions of the Internet. Do note that IPv6 lines are less transparent in order for you to be able to see them on the maps.
				</p>
			</div>
			<div class="l-box pure-u-1 pure-u-md-1-2 pure-u-lg-1-4">
				<h3 class="content-subhead">
					Settings
				</h3>
				<p>
					The settings allow you to tweak several things. You may switch between the available dates to observe how the networks developed in time. You can also try comparing the IPv4 and IPv6 networks. Furthermore if you are mostly interested in the networks and their locations instead of the connections, there is an additional edges option that allows you to view only the networks (points). 
				</p>
			</div>
			<div class="l-box pure-u-1 pure-u-md-1-2 pure-u-lg-1-4">
				<h3 class="content-subhead">
					Detailed Information
				</h3>
				<p>
					A full length explanation and an overview of how this visualisation was created is available in my dissertation. It will be added here later.
				</p>
			</div>
		</div>
	</div>

	<div class="content ribbon">
		<h2 class="content-head content-head-ribbon is-center">Other Visualisations</h2>
		<h4 class="content-subhead is-center">
					Note that the visualisations also change using the controls for the 3D visualisation.
		</h4>

		<div class="pure-g">
			<div class="l-box pure-u-1 pure-u-md-1-2">

				<h3 class="content-subhead">
					CAIDA-style visualisation
				</h3>
				<p>
					This visualisation uses the same data as the main visualisation, however it plots it using a different style. Here the angle from the centre is the longitude of where the network is located, however the distance from the centre represents how important the network is. When a network is close to the centre it means that many networks use it to communicate through - the more central it is. See the original by CAIDA <a href=https://www.caida.org/research/topology/as_core_network/2017>here</a>. 
				</p>
				<a href = "/assets/images/2020_12_08/ipv4_caida_plot.png" id="caidalink"><img alt="CAIDA-style visualisation" class="pure-img-responsive" src="/assets/images/2020_12_08/ipv4_caida_plot.png" id="caidafile" loading=lazy></a>

			</div>

			<div class="l-box pure-u-1 pure-u-md-1-2">
				<h3 class="content-subhead">
					Azimuthal projection of the map
				</h3>
				<p>
					This visualisation is just a flattened version of the main three dimensional visualisation. The same explanations from the about section apply here. We use the Azimuthal projection to get this flat representation of earth, however the lines follow the <a href="https://en.wikipedia.org/wiki/Great-circle_distance">great circles</a>. Click to view the full resolution image.
				</p>
				<a href="/assets/images/2020_12_08/ipv4_azimuthal.png" id="azimuthallink"><img alt="Azimuthal projection" class="pure-img-responsive" src="/assets/images/2020_12_08/ipv4_azimuthal.png" id="azimuthalfile" loading=lazy></a>
			</div>
		</div>
	</div>

	<div class="ribbon l-box-lrg pure-g">
		<!-- <div class="l-box-lrg is-center pure-u-1 pure-u-md-1-2 pure-u-lg-2-5">
			<img width="300" alt="File Icons" class="pure-img-responsive" src="/img/common/file-icons.png">
		</div> -->
		<div class="pure-u-1 pure-u-md-1-2 pure-u-lg-3-5">

			<h2 class="content-head content-head-ribbon">Acknowledgements</h2>

			<p>
				This visualisation uses <a href="https://dev.maxmind.com/geoip/geoip2/geolite2/"> GeoLite2 by MaxMind </a> to locate the networks. 
			</p>
			<p>
				This page is based on the landing page layout from <a href="https://purecss.io/">purecss.io</a>
			</p>
			<p>
				The rendering is made possible by <a href="threejs.org">three.js</a>
			</p>
			<p> 
				The data was collected using the BGPStream framework by <a href="https://www.caida.org/home/">CAIDA</a>
			</p>
			<p> 
				The data itself is available thanks to <a href="http://www.routeviews.org/routeviews/">routeviews</a> and<a href="https://www.ripe.net/analyse/internet-measurements/routing-information-service-ris"> RIPE RIS</a>
			</p>
		</div>
	</div>

	{%- include footer.html -%}
	
</div>

<script type="module">
	import * as THREE from 'https://unpkg.com/three@0.125.2/build/three.module.js';
	import { OrbitControls } from 'https://unpkg.com/three@0.125.2/examples/jsm/controls/OrbitControls.js';
	import { Stats } from "/assets/js/stats.min.js";
	var container, stats;
	var camera, scene, renderer;
	var group;

	var windowHalfX = window.innerWidth / 2;
	var windowHalfY = window.innerHeight / 2;

	init();
	animate();

	function init() {

		container = document.getElementById( 'container' );

		camera = new THREE.PerspectiveCamera( 60, window.innerWidth / window.innerHeight / 0.87, 1, 2000 );
		camera.position.z = 500;

		scene = new THREE.Scene();

		group = new THREE.Object3D();
		scene.add( group );

		// earth

		var loader = new THREE.TextureLoader();
		var mesh;
		loader.load( '../assets/images/2020_12_08/ipv4_platecarree.jpg', function ( texture ) {

			texture.needsUpdate = true;
			var geometry = new THREE.SphereGeometry( 200, 64, 64 );
			var material = new THREE.MeshBasicMaterial( { map: texture } );
			mesh = new THREE.Mesh( geometry, material );
			group.add( mesh );

		} );


		renderer = new THREE.WebGLRenderer( { antialias: true } );
		renderer.setClearColor( 0x333333 );
		renderer.setSize( window.innerWidth, window.innerHeight * 0.87);
		// renderer.domElement.classList.add('splash');
		container.appendChild( renderer.domElement );

		// controls
		const controls = new OrbitControls( camera, renderer.domElement);
		controls.minDistance = 225;
		controls.maxDistance = 500;
		controls.enablePan = false;



		// Performance overlay
		stats = new Stats();
		stats.domElement.style.position = 'absolute';
		stats.domElement.style.top = '0px';
		container.appendChild( stats.domElement );

		const params = {
			date: '2020_12_08/',
			ipv4: true,
			ipv6: false,
			edges: true,
		};

		//GUI
		const gui = new dat.GUI({autoPlace: false});
		gui.add(params, 'date', { "Dec 2020": '2020_12_08/', "July 2017": '2017_06_06/'} ).name('Date').onChange( changeTexture );
		gui.add(params, 'ipv4').name('IPv4').listen().onChange(function(){changeIPVersion("ipv4")});
		gui.add(params, 'ipv6').name('IPv6').listen().onChange(function(){changeIPVersion("ipv6")});
		gui.add(params, 'edges').name('Edges').onChange( changeTexture );
		gui.domElement.style.position = 'absolute';
		gui.domElement.style.top = '0px';
		gui.domElement.style.right = '0px'
		gui.domElement.style.float = 'right';
		container.appendChild(gui.domElement);

		function changeIPVersion(ver){
			params.ipv4 = false
			params.ipv6 = false
			params[ver] = true
			changeTexture()
		}

		function changeTexture(){
			var ver;
			var filename;
			if (params.ipv6) ver = "ipv6_";
			if (params.ipv4) ver = "ipv4_";
			if (params.edges) {
				filename = "platecarree.jpg";
			} else {
				filename = "platecarree_centroid.png";
			}
			loader.load( ['assets/images/', params.date, ver, filename].join('') , function ( texture ) {
				mesh.material.map = texture;
				mesh.material.needsUpdate = true;
			},
			xhr => {
				//Download Progress
				console.log((xhr.loaded / xhr.total) * 100 + "% loaded");
			} );

			var caidalink = document.getElementById("caidalink").setAttribute("href", ['assets/images/', params.date, ver, "caida_plot.png"].join(''));
			var caidafile = document.getElementById("caidafile").setAttribute("src", ['assets/images/', params.date, ver, "caida_plot.png"].join(''));



			var azimuthallink = document.getElementById("azimuthallink").setAttribute("href", ['assets/images/', params.date, ver, "azimuthal.png"].join(''));
			var azimuthalfile = document.getElementById("azimuthalfile").setAttribute("src", ['assets/images/', params.date, ver, "azimuthal.png"].join(''));


		}

		//

		window.addEventListener( 'resize', onWindowResize, false );

	}

	function onWindowResize() {

		windowHalfX = window.innerWidth / 2;
		windowHalfY = window.innerHeight / 2;

		camera.aspect = window.innerWidth / window.innerHeight / 0.87;
		camera.updateProjectionMatrix();

		renderer.setSize( window.innerWidth, window.innerHeight * 0.87);

	}


	function animate() {

		requestAnimationFrame( animate );

		render()

		stats.update();

	}

	function render() {

		renderer.render( scene, camera );

	}


</script>

<script type="text/javascript">
	function removeintro(){
		var obj = document.getElementById("introduction");
		obj.remove();
	}
</script>

