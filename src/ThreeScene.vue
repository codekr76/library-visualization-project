<template>
	<div ref="container" class="three-container"></div>
</template>

<script setup lang="ts">
	import { onMounted, ref, onBeforeUnmount } from "vue";
	import * as Three from "three";
	import { GLTFLoader } from "three/addons/loaders/GLTFLoader.js";
	import { OrbitControls } from "three/addons/controls/OrbitControls.js";

	const container = ref<HTMLDivElement | null>(null);

	let renderer: Three.WebGLRenderer | null = null;

	onMounted(() => {
		if (!container.value) return;

		// 1. Scene 생성
		const scene = new Three.Scene();
		scene.background = new Three.Color(0xf0f0f0);

		// 2. Camera 생성
		const camera = new Three.PerspectiveCamera(
			75,
			container.value.clientWidth / container.value.clientHeight,
			0.1,
			1000,
		);

		camera.position.set(0, 0, 4);
		camera.lookAt(0, 0, 0);

		// 3. Renderer 생성
		renderer = new Three.WebGLRenderer({ antialias: true });

		renderer.setSize(
			container.value.clientWidth,
			container.value.clientHeight,
		);

		container.value.appendChild(renderer.domElement);

		// OrbitControls 생성하기
		// 마우스로 카메라 조작하기
		const controls = new OrbitControls(camera, renderer.domElement);

		// 부드러운 움직임 활성화
		controls.enableDamping = true;

		// 부드러운 움직임 정도
		controls.dampingFactor = 0.05;

		// 조명 추가
		const light = new Three.HemisphereLight(0xffffff, 0x444444, 2);

		scene.add(light);

		// GLB 로더 생성
		const loader = new GLTFLoader();

		// GLB 파일 불러오기
		loader.load(
			"/models/bookshelf.glb",

			// ① 로딩 성공
			(gltf) => {
				const model = gltf.scene;

				// 1. 모델의 실제 크기 확인
				const box = new Three.Box3().setFromObject(model);
				const size = box.getSize(new Three.Vector3());
				console.log("모델 크기:", size);

				// 2. 모델 크기 자동 조정
				const maxDimension = Math.max(size.x, size.y, size.z);

				if (maxDimension > 0) {
					model.scale.setScalar(4 / maxDimension);
				}

				// 3. 모델의 중심 위치 계산
				const scaledBox = new Three.Box3().setFromObject(model);

				const center = scaledBox.getCenter(new Three.Vector3());

				// 4. 모델 중심을 원점(0, 0, 0)에 배치
				model.position.sub(center);

				// 5. scene에 모델 추가
				scene.add(model);
			},

			// ② 로딩 진행 상황
			(progress) => {
				console.log(
					"로딩 진행률:",
					(progress.loaded / progress.total) * 100,
					"%",
				);
			},

			// ③ 로딩 실패
			(error) => {
				console.error("3D 모델 로딩 실패:", error);
			},
		);

		const animate = () => {
			requestAnimationFrame(animate);

			controls.update();

			// 모델이 추가된 후 화면 출력
			renderer?.render(scene, camera);
		};

		animate();
	});

	onBeforeUnmount(() => {
		renderer?.dispose();
	});
</script>

<style scoped>
	.three-container {
		width: 100%;
		height: 500px;
	}
</style>
