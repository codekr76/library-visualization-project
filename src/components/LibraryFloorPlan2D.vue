<script setup lang="ts">
	import { ref, computed } from "vue";

	// 서가 데이터
	const shelves = [
		{ id: "A01", zone: "A구역", x: 80, y: 120 },
		{ id: "A02", zone: "A구역", x: 140, y: 120 },
		{ id: "A03", zone: "A구역", x: 200, y: 120 },

		{ id: "B01", zone: "B구역", x: 360, y: 120 },
		{ id: "B02", zone: "B구역", x: 420, y: 120 },
		{ id: "B03", zone: "B구역", x: 480, y: 120 },

		{ id: "C01", zone: "C구역", x: 640, y: 120 },
		{ id: "C02", zone: "C구역", x: 700, y: 120 },
		{ id: "C03", zone: "C구역", x: 760, y: 120 },
	];

	// 선택된 서가
	const selectedShelf = ref<string | null>(null);

	// 서가 클릭 함수
	const selectShelf = (shelfId: string) => {
		selectedShelf.value = shelfId;
	};

	// 선택된 서가 정보
	const selectedShelfInfo = computed(() => {
		return shelves.find((shelf) => shelf.id === selectedShelf.value);
	});
</script>

<template>
	<div class="library">
		<h1>도서관 평면도</h1>
		<p>Library Floor Plan</p>

		<svg viewBox="0 0 900 500">
			<!-- 도서관 전체 영역 -->
			<rect
				x="20"
				y="20"
				width="860"
				height="460"
				fill="#f8fafc"
				stroke="#334155"
				stroke-width="4"
			/>

			<!-- A구역 -->
			<rect
				x="50"
				y="60"
				width="240"
				height="360"
				fill="#dbeafe"
				stroke="#3b82f6"
				stroke-width="2"
			/>

			<text x="150" y="90" class="zone-label">A구역</text>

			<!-- B구역 -->
			<rect
				x="330"
				y="60"
				width="240"
				height="360"
				fill="#dcfce7"
				stroke="#16a34a"
				stroke-width="2"
			/>

			<text x="430" y="90" class="zone-label">B구역</text>

			<!-- C구역 -->
			<rect
				x="610"
				y="60"
				width="240"
				height="360"
				fill="#fef3c7"
				stroke="#d97706"
				stroke-width="2"
			/>

			<text x="710" y="90" class="zone-label">C구역</text>

			<!-- 서가 반복 생성 -->
			<g v-for="shelf in shelves" :key="shelf.id">
				<rect
					:x="shelf.x"
					:y="shelf.y"
					width="40"
					height="220"
					rx="3"
					:class="{
						'shelf-selected': selectedShelf === shelf.id,
					}"
					class="shelf"
					@click="selectShelf(shelf.id)"
				/>

				<text :x="shelf.x + 20" :y="355" class="shelf-label">
					{{ shelf.id }}
				</text>
			</g>

			<!-- 출입구 -->
			<text x="420" y="455" class="entrance">출입구</text>
		</svg>

		<!-- 선택된 서가 정보 -->
		<div v-if="selectedShelfInfo" class="iinfo-panel">
			<h2>선택된 서가</h2>

			<p>서가 ID: {{ selectedShelfInfo?.id }}</p>
			<p>구역: {{ selectedShelfInfo?.zone }}</p>

			<p>전체 도서: 150권</p>
			<p>정상 도서: 145권</p>
			<p>오배열 도서: 5권</p>
		</div>

		<div v-else class="info-panel">서가를 클릭해 주세요.</div>
	</div>
</template>

<style scoped>
	.library {
		padding: 30px;
		text-align: center;
	}

	.library h1 {
		font-size: 28px;
		margin-bottom: 5px;
	}

	.library p {
		color: #64748b;
		margin-bottom: 25px;
	}

	svg {
		width: 100%;
		max-width: 900px;
		height: auto;
	}

	.zone-label {
		font-size: 22px;
		font-weight: bold;
		text-anchor: middle;
		fill: #1e293b;
	}

	.shelf {
		fill: #64748b;
		cursor: pointer;
		transition: fill 0.2s;
	}

	.shelf:hover {
		fill: #3b82f6;
	}

	.shelf-selected {
		fill: #2563eb;
		stroke: #1e40af;
		stroke-width: 3;
	}

	.shelf-label {
		font-size: 12px;
		text-anchor: middle;
		fill: #334155;
	}

	.entrance {
		font-size: 18px;
		font-weight: bold;
		fill: #334155;
	}

	.info-panel {
		max-width: 400px;
		margin: 20px auto;
		padding: 20px;
		background: #f1f5f9;
		border-radius: 10px;
		text-align: left;
	}

	.info-panel h2 {
		font-size: 20px;
	}
</style>
