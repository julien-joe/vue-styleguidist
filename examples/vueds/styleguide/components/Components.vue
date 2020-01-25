<template>
	<div class="component-status">
		<ul class="status-list">
			<li>
				<Icon name="ready" fill="#7cb518" size="small" />
				<p>Ready</p>
			</li>
			<li>
				<Icon name="review" :fill="tokens.color_ucla_gold.value" size="small" />
				<p>Under review</p>
			</li>
			<li>
				<Icon name="deprecated" :fill="tokens.color_vermilion.value" size="small" />
				<p>Deprecated</p>
			</li>
			<li>
				<Icon name="prototype" :fill="tokens.color_bleu_de_france.value" size="small" />
				<p>Prototype</p>
			</li>
			<li>
				<span>—</span>
				<p>Not applicable</p>
			</li>
		</ul>
		<table>
			<thead>
				<tr>
					<th v-if="show === 'all'">Component Name</th>
					<th v-if="show === 'elements'">Element Name</th>
					<th v-if="show === 'patterns'">Pattern Name</th>
					<th v-if="show === 'templates'">Template Name</th>
					<th>Released in</th>
					<th>Status</th>
				</tr>
			</thead>
			<tbody>
				<tr v-for="(component, index) in components" :key="index" class="component">
					<td v-if="component.name">
						<code class="name">{{component.name}}</code>
					</td>
					<td v-else>N/A</td>
					<td v-if="component.release">{{component.release}}</td>
					<td v-else>N/A</td>
					<td v-if="component.status">
						<Icon v-if="component.status === 'ready'" name="ready" fill="#7cb518" size="small" />
						<Icon
							v-if="component.status === 'under-review' || component.status === 'review'"
							name="review"
							:fill="tokens.color_ucla_gold.value"
							size="small"
						/>
						<Icon
							v-if="component.status === 'prototype'"
							name="prototype"
							:fill="tokens.color_bleu_de_france.value"
							size="small"
						/>
						<Icon
							v-if="component.status === 'deprecated'"
							name="deprecated"
							:fill="tokens.color_vermilion.value"
							size="small"
						/>
					</td>
					<td v-else>—</td>
				</tr>
			</tbody>
		</table>
	</div>
</template>

<script>
// If you want to use your own tokens here, change the following line to:
// import designTokens from "@/assets/tokens/tokens.raw.json"
import orderBy from 'lodash/orderBy'
export default {
	name: 'Components',
	methods: {
		getComponents: function() {
			let context = require.context('../../src/', true, /\.vue$/)
			return context.keys().map(key => context(key).default)
		},
		orderData: function(data) {
			return orderBy(data, 'name', 'asc')
		}
	},
	data() {
		return {
			components: this.orderData(this.getComponents()),
			tokens: designTokens.props
		}
	}
}
</script>
