<template>
	<main>
		<section>
			<img src="../assets/face.jpg" id="image" alt="face" title="face" />
		</section>
		<section>
			<div v-if="resultPrediction.length > 0">
				<div v-for="(res, idx) in resultPrediction" :key="idx">
					Label: {{ res.label }}<br/>
					Confidence: {{ res.confidence }} %
				</div>
			</div>
			<div v-else>
				computing, please wait...
			</div>
		</section>
	</main>
</template>

<script>
export default {
	data () {
		return {
			classifier: {},
			resultPrediction: []
		}
	},
	created () {
		this.classifier = ml5.imageClassifier('MobileNet')
	},
	mounted () {
		// separate this, so the imageClassifier model can be loaded and initialized properly
		this.classifier
			.classify(document.getElementById('image'))
			.then(result => {
				result.map(r => {
					this.resultPrediction.push({
						label: r.label,
						confidence: (r.confidence * 100).toFixed(2)
					})
				})
			})
	}
}
</script>

<style lang="scss" scoped>
main {
	display: grid;
	grid-template-columns: repeat(2, minmax(200px, 1fr));
}
</style>
