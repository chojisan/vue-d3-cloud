<template>
	<div id="app">
	<header>
		<h1>vue-d3-cloud example</h1>
	</header>
	<main>
	<h2></h2>
	<cloud
		v-if="wordCount"
		class="cloud" 
		:data="wordCount" 
		:fontSizeMapper="fontSizeMapper" 
		:rotate="rotate" 
		:font="font" 
		:padding="padding" 
		:spiral="spiral" 
		:colors="colors"
		:coloring="coloring"/>
	</main>
	<div class="container">
		<div>
			<textarea v-model="words" cols="100" rows="20" />
		</div>
		<div>
			<span>Words will have a random rotation between</span>
			<br />
			<label for="minimum degree">Min Degree</label>
			<input v-model="min" type="number" name="minimum degree" min="0" :max="max-1" />
			<label for="maximum degree">Max Degree</label>
			<input v-model="max" type="number" name="maximum degree" :min="min+1" max="359s" />
			<br />
			<label for="padding">Padding</label>
			<input v-model="padding" type="number" name="padding" />
			<br />
			<label for="font">Font</label>
			<select v-model="font" name="font">
				<option>Serif</option>
				<option>Helvetica</option>
				<option>Arial</option>
				<option>Times</option>
				<option>Times New Roman</option>
				<option>Courier</option>
				<option>Impact</option>
				<option>Georgia</option>
			</select>
			<br />
			<label for="spiral">Spiral Style</label>
			<select v-model="spiral" name="spiral">
				<option>archimedean</option>
				<option>rectangular</option>
			</select>
			<br />
			<label for="spiral">Colors</label>
			[<span v-for="color in useColors" :key="color">
				{{color.toString()}},
			</span>]
			<button @click="addColor">Add Color</button>
			<button @click="removeColor">Remove Color</button>
			<br />
			<label for="coloring">Coloring By</label>
			<select v-model="coloring" name="coloring">
				<option>size</option>
				<option>random</option>
			</select>
		</div>
	</div>
	</div>
</template>

<script>
import Cloud from '../../src/components/Cloud.vue'

export default {
	name: 'app',
	
	data() {
		return {
			min: 0,
			max: 65,
			padding: 0,
			words: "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Maecenas sed cursus metus. Proin posuere, risus vestibulum malesuada consectetur, est justo vehicula eros, et hendrerit velit eros nec sem. Fusce lacinia ex et urna suscipit lobortis. Sed sollicitudin sodales felis, in tincidunt erat pretium eget. Integer at ligula rutrum, faucibus massa eget, porttitor sem. Ut non porttitor lorem. In luctus nec dui quis finibus. Vestibulum ante ipsum primis in faucibus orci luctus et ultrices posuere cubilia Curae; Donec ultrices leo vitae vehicula vehicula. Cras porttitor, quam eu lobortis luctus, nunc justo suscipit sem, sit amet semper velit ante quis turpis. Vestibulum mattis sollicitudin ullamcorper. Maecenas sit amet nulla vitae nisl blandit dictum. Proin pharetra eget nisl pharetra venenatis. Quisque interdum ullamcorper neque tincidunt ultricies.",
			fontSizeMapper: word => Math.log2(word.value*5) * 10,
			font: "Helvetica",
			spiral: "archimedean",
			colors: ['coral', 'blue', 'hotpink', 'peachpuff', 'green'],
			coloring: 'size',
			colorCount: 3,
		}
	},
	
	components: {
		Cloud,
	},
			
	computed: {

		rotate: function() {
			let newMin = this.min
			let newMax = this.max
			return () => Math.random() * (newMax - newMin) + newMin
		},

		wordCount: function() {
			if(!this.words)
				return []

			let occurences = this.words.split(' ').reduce((allNames, name) => { 
				if (name in allNames) {
					allNames[name]++;
				} else {
					allNames[name] = 1;
				}
				return allNames;
			}, {});
			
			let occurencesCount = []
			
			for(var text in occurences) {
				let obj = {}
				obj.text = text;
				obj.value = occurences[text]
				occurencesCount.push(obj)
			}

			return occurencesCount
		},

		useColors() {
			return this.colors.filter((color, index) => {
				if(index < this.colorCount) {
					return color
				}
			})
		}
	},

	methods: {
		addColor() {
			if(this.colorCount < 5) {
				this.colorCount++;
			}
		},
		removeColor() {
			if(this.colorCount > 1) {
				this.colorCount--;
			}
		}
	}
}
</script>

<style>
#app {
	font-family: 'Avenir', Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	color: #2c3e50;
	margin-top: 60px;
}

header {
	margin: auto;
	text-align: center;
}

textarea {
	display: block;
	margin-left: auto;
	margin-right: auto;
}

svg {
	display: block;
	margin-left: auto;
	margin-right: auto;
}

br {
	display: block;
    margin-bottom: 7px;
    font-size:2px;
    line-height: 30px;
}

.container {
	display: flex;
	flex-direction: row;
	justify-content: space-around;
}
</style>