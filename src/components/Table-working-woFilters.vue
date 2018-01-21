<template>
	<div>
		<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u" crossorigin="anonymous">
		<table class="table-striped table-bordered table-hover">
			<thead>
				<tr>
					<th v-for="key in sort_column" @click="sortBy(key)" :class="{ active: sortKey == key }">{{ key | capitalize }}
					<span class="arrow"></span>
					</th>
				</tr>
				<!--
				<tr>
					<th @click="sortBy('name')">Name</th>
					<th @click="sortBy('name')">Age</th>
				</tr>
				-->
			</thead>
			<tbody>
				<tr v-for="row in table_data">
					<td v-for="key in sort_column">
						{{ row[key] }}
					</td>
					<!--
					<td>{{ row.name }}</td>
					<td>{{ row.age }}</td>
					-->
				</tr>
			</tbody>
		</table>
		<br>
		<input id="search" v-model="search_text" placeholder="Search" />
		<br><br>
		<a @click="prevPage">Prev</a>
		<a @click="nextPage">Next</a>
		<div>{{ search_text }}</div>
	</div>
</template>

<script>
	export default {
		name: 'TableData',
		data () {
			return {
				table_data: [
					{name: 'John', 'age': 24},
					{name: 'Robbie', 'age': 30},
					{name: 'Richard', 'age': 31},
					{name: 'Tim', 'age': 28}
				],
				//text to search on
				search_text: "",

				//column to sort by
				sort_column: ['name', 'age'],

				//current page number
				page_num: 0
			}
		},
		filters: {
			capitalize: function (str) {
      			return str.charAt(0).toUpperCase() + str.slice(1)
    		}
		}
	}
</script>

<style>
	table {
		font-size: 16px;
		font-family: Helvetica Neue, Arial, sans-serif;
		border: 2px solid #42b983;
		background-color: #fff;
		border-radius: 3px;
	}
	th {
		background-color: #42b983;
	  	color: rgba(255,255,255,0.66);
	  	cursor: pointer;
	  	user-select: none;
	}
	th, td {
  		min-width: 120px;
  		padding: 10px 20px;
	}
	tr {
		padding: 50px;
	}
	th.active {
  		color: #fff;
	}
</style>