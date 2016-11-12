<template>
	<ul class="page-break text-center">
		<li>
			<button class="btn btn-default" @click.prevent="goPrevious">
				<i class="glyphicon glyphicon-chevron-left"></i>
			</button>
		</li>
		<li v-if="showFirst">
			<button class="btn btn-default" @click.prevent="goSpecific(first)"> {{first}} </button>
		</li>
		<li v-if="showFirst">...</li>

		<li v-for="page in pages">
			<button class="btn btn-default"
					:class="page.isActive ? 'highlight' : ''"
					v-if="page.number >= first && page.number <= last"
					@click.prevent="goSpecific(page.number)">{{page.number}}
			</button>
		</li>

		<li v-if="showLast">...</li>
		<li v-if="showLast">
			<button class="btn btn-default" @click.prevent="goSpecific(last)"> {{last}} </button>
		</li>
		<li>
			<button class="btn btn-default" @click.prevent="goNext">
				<i class="glyphicon glyphicon-chevron-right"></i>
			</button>
		</li>
	</ul>
</template>

<script>
	export default {
		name: 'page',
		props: {
			first: {
				type: Number,
				default: 1
			},
			last: {
				type: Number,
				required: true
			},
			settings: {
				type: Number,
				default: 5
			}
		},
		created () {
			this.arrayFrom();
		},
		data () {
			return {
				current: this.first,
				pages: [],
				showFirst: false,
				showLast: false
			}
		},
		watch: {
			'last' () {
				this.updateUI();
			}
		},
		methods: {
			goSpecific (number) {
				this.current = number;
				this.arrayFrom();
				this.$emit('page', this.current);
			},
			goPrevious () {
				if(this.current > this.first){
					this.goSpecific(this.current - 1);
				}
			},
			goNext () {
				if(this.current < this.last){
					this.goSpecific(this.current + 1);
				}
			},
			updateUI () {
				this.showFirst = this.pages[0].number > this.first;
				this.showLast = this.pages[this.pages.length - 1].number < this.last;
			},
			arrayFrom () {
				var start = this.current - parseInt((this.settings - 1) / 2);
				this.pages = [];
				for(var i = 0; i < this.settings; i++){
					this.pages.push({
						isActive: start === this.current,
						number: start++
					});
				}
				this.updateUI();
			}
		}
	}
</script>

<style>
	.page-break li {
		display: inline-block;
		margin: 2px;
	}

	.highlight,
	.highlight:hover {
		background-color: #5cb85c;
		color: #ffffff;
	}
</style>
